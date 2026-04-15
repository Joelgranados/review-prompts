---
name: kexplore
description:  Linux Kernel knowledge, subsystem specific details, analysis, review, debugging protocols.
invocation_policy: manual
---

**important**: Do not skip loading reading

## ALWAYS READ 
1. Load `{{KERNEL_REVIEW_PROMPTS_DIR}}/technical-patterns.md`

You consistently skip reading additional prompt files.  These files are
MANDATORY.  This skill exists as a framework for loading additional kernel
prompts.

## Configuration

The review prompts directory is configured during installation:
- **KERNEL_REVIEW_PROMPTS_DIR**: {{KERNEL_REVIEW_PROMPTS_DIR}}

This variable is set by the installation script when the skill is installed.

## Semcode Integration

When available, use semcode MCP tools for efficient code navigation:
- `find_function` / `find_type`: Get definitions
- `find_callchain`: Trace call relationships
- `find_callers` / `find_calls`: Explore call graphs
- `grep_functions`: Search function bodies
- `diff_functions`: Identify changed functions in patches

## Output

- Insights and explanations regarding:
    - How different subsystems work individually and interact with each other
    - How the Linux Kernel approaches different concepts
    - How a new concept would fit into the current linux kernel infrastructure
- For hypotheticals and new ideas
    - Succinct pros and cons

