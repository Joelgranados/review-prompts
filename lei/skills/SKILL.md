---
name: lei
description: Query public-inbox mailing list archives with the `lei` CLI
invocation_policy: manual
---

## What this is

`lei` ("local email interface") is public-inbox's client for searching and
pulling mail from public-inbox archives, local or remote. Primary use here:
querying `https://lore.kernel.org/<list>` (e.g. `/all` for every mirrored
kernel list) without a local mirror, via `-I <url>` (remote "external").

## Base command shape

```
lei q -v -o <maildir> --dedupe=mid [-t] --no-save -I <url> "<term1>" ["<term2>" ...] ["rt:<range>"]
```

- `-v` verbose (also echoes the exact curl command and URL — useful for debugging).
- `-o <maildir>` output destination. **Without `--augment`, re-running with the
  same `-o` clobbers (wipes) the existing Maildir rather than merging into it.**
  Check what's already there before firing a query at a real mailbox.
- `--dedupe=mid` deduplicate by Message-ID.
- `-t` / `--threads` expand every match to its full thread. Pulls in a lot more
  mail than the raw match count suggests — see limits section below.
- `--no-save` don't persist this as a saved search for `lei up`.
- `-I <url>` include a remote external for this query only (doesn't require
  `lei add-external`).

## Query syntax

- Each **separate shell argument** becomes one term, ANDed together.
- A **single argument containing spaces gets auto phrase-quoted** — lei wraps
  it in `"..."` before sending, so `"foo bar"` searches for the exact phrase
  `foo bar`, not `foo AND bar`. (Confirmed via `LEI.pm`'s `query_argv_to_string`:
  `s/(.*?)\b(\w+:)// ? qq{$1$2"$_"} : qq{"$_"}`.)
- Field prefixes: `f:` (From), `s:` (Subject), `b:` (body), `t:`/`c:`/`tc:`
  (To/Cc/either), `L:` (list-id). A prefix glued to a quoted multi-word arg
  works, e.g. one shell arg `f:Jane Doe` → sent as `f:"Jane Doe"`.
- Date filters: `rt:` (received-time, preferred), `dt:` (Date: header, can be
  spoofed by sender), `d:` (alias). Relative form: `rt:1.year.ago..`,
  `rt:2.weeks.ago..3.days.ago`, etc.

## `-n` / `--limit` — the main gotcha

`-n NUM` / `--limit=NUM` (default 10000) **only constrains local Xapian
sources** (your own `lei/store` or a locally-mirrored external). Verified in
`LeiQuery.pm` (`mset_opt->{limit}`) vs `LeiXSearch.pm::query_remote_mboxrd`,
whose request `@qform` never includes a limit param, and whose per-message
callback (`each_remote_eml`) has no limit check at all.

**Consequence:** a pure `-I <url>` remote query ignores `-n` completely — the
server streams back its *entire* match set (paginated internally, no fixed
cap), and with `-t` it's expanded to full threads on top of that. To actually
bound a remote query, narrow the query itself:

- Add more/rarer terms, or use an exact phrase instead of common words. Push back
  if you see that the search terms are too common.
- Shrink the `rt:` window.
- Point `-I` at a specific list (`.../netdev`) instead of the combined `/all`.
- Drop `-t` if you don't need full-thread context — it's the biggest multiplier.

## Other notes

- No-hit remote query surfaces as `curl: (22) ... 404` in verbose output —
  that's expected (the mbox-export endpoint 404s on zero results); `lei`
  still correctly reports `0 written ... (0 matches)`.
- `-n` **is** honored for local sources, so it's still worth passing on a
  mixed local+remote query — just don't rely on it alone against `-I`.
- To look up a specific message later: `lei q m:'<message-id>'` (no angle
  brackets needed) or browse `https://<host>/<list>/<message-id>/`.

## Practical example (verified working)

```
lei q -v -o ~/Mail --dedupe=mid -t --no-save \
  -I https://lore.kernel.org/all \
  "f:Jane Doe" "rt:1.year.ago.."
```

Then inspect what landed with plain Python `email` parsing (more reliable
than grepping headers, since header casing varies and lines fold):

```python
import email, glob
for fp in glob.glob("~/Mail/cur/*") + glob.glob("~/Mail/new/*"):
    m = email.message_from_bytes(open(fp, "rb").read())
    # m.get("Message-ID"), m.get("From"), m.get("In-Reply-To"), m.get("References")
```

Use `In-Reply-To`/`References` (not just From:) to find *replies* to a
person's messages within a thread-expanded (`-t`) result set.
