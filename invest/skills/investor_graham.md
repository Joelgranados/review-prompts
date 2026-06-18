---
name: investor_graham
description: Stock selection following Grahams "Defensive Investor"
invocation_policy: manual
---

# Grahams Defensive Strategy
This stock selection is all following the "Defensive Investor" concept in
Graham's "The intelligent investor"

  * Consider these 7 points when asked about an option in the stock market.
  * Use the "Measure" under each point to get a description of what to measure
  * Use the "Screener" to get more details of what to measure
    - You may be able to use the screener when looking for data in financial
      websites like finance.yahoo.com, nordnet.com
  * Use the "Screener" & "Manual Screener" to create a summary of how
    advantageous it is to buy a certain stock
  * Use the "Notes" to gain insight when analyzing each point.

.. note::
  * Manual screener to be used after Automatic screener

## 1. Adequate Size of Enterprise

Measure:
  * Market cap greater than a Constant

Screener:
  * stocks with market cap > 100 Billion (These are the biggest companies out
    there)
  * Stocks with market cap > 5 Billion (Claude suggested 3.5B after
    extrapolating, 5 after rounding).

Motivation:
  * Looking for Enterprises that:
    - Are capable of weathering troubled times
    - Have "Financial Stability"

Notes:
  * The money from investing in lower market caps is not
    for the "Defensive Investor"

## 2. Strong Financial Condition

Measure:
  1. (Current Assets) >= 2 * (Current Liabilities)
  2. ((Current Assets) - (Current Liabilities)) >= (Long Term Dept)

Screener:
  * Current Ratio >= 2

Motivation:
  * Conservatively financed Companies that can take a hit

Notes:
  * (Current Assets) - (Current Liabilities) = (Working Capital)
  * There is no (Long Term Dept) in Yahoo Finance.
  * **Current Ratio** = ((Current Assets) / (Current Liabilities))

## 3. Earnings Stability

Measure:
  * Positive Earnings for the past 10+ years

Manual Screener:
  * Net Income = (Earnings) - (taxes) - (Expenses) 
  * EPS (Earnings Per Share) = (Net Income) / (# Outstanding Shares)

Motivation:
  * Method to eliminate a "chronic" looser stock

Notes:
  * This is a manual check done in nordnet or yahoo Finance
  * Positive Earnings is the same as saying no earning deficit.
  * Yahoo has 4 years for (Net Income) and EPS
  * Nordnet has 10+ years for EPS. Does not have Net Income

## 4. Dividend Record

Measure:
  * Uninterrupted Dividends for the last 20 years

Manual Screener:
  * Consecutive years of dividend growth count at 5 to 10
  * Dividend Per Share (DPS) in Nordnet has a long past

Motivation:
  * It is a comforting sign

Notes:
  * This is a manual check done in nordnet or yahoo Finance
  * Denmark **taxes** dividends!!!!
  * consecutive growth is not uninterrupted dividen payments

## 5. Earnings Growth

Measure:
  * 1/3 (33.33%) increase in Earnings per Share (EPS) in the past 10 years

Screener:
  * 1 year % Change in EPS (Basic) > 5%

Manual Screener:
  * Calc ((Curr EPS) - (Prev EPS))/(Prev EPS) * 100 into the past

Notes:
  * Note that a 30%+ growth offer 10 years is a 3% yearly growth (approx)
  * I set it to 5 because it only counts the one year
  * Yahoo's 1yr % Change in EPS (Basic) is not long term (only one year)
  * Claude thinks the formula is ((Curr EPS) - (Prev EPS))/(Prev EPS) * 100
  * Save the EPS into a column do calculation automatically outside yahoo.

## 6. Price to Earnings (P/E)

Measure:
  * (Price per Share) / (Earnings per share) > 20

Screener:
  * Trailing P/E > 20

Motivation:
  * Based on experience from Graham and co.

Notes:
  * Yahoos screener is 1 year; grahams is 3 yearsa 
  * **This slashes most stocks** (Well known) and that makes sense as they are
    probably high valued
  * Trailing P/E in yahoo finance is calculated based on the past 12 months
  * P/E it is what the market thinks the company earnings are worth
  * P/E cannot be calculated when there is a earning deficit
  * >30 might mean that the Stock is overpriced
  * Graham identifies <15 as a good measure.

## 7. Price to Book P/B

Measure:
  * (Market Price Per Share) / (Book Value Per Share) < 1.5

Screener:
  * Yahoos "Price / Book Value (P/B)" < 2.5

Motivation:
  * Stock is "reasonably" priced

Notes:
  * Use 2.5 (greater than grahams 1.5) because todays companies are valued
     higher
  * (Book Value Per Share) = ((Current Assets) - (Current Liabilities)) / (# Outstanding Shares)

