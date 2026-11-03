# Sales validation notes

This document records the evidence supporting the Sales Analysis case study, the scope of the checks performed, and relevant interpretation limits.

[Project overview](../README.md) · [Data dictionary](../Data_Dictionary.md) · [DAX documentation](../Dax%20Measures/README.md) · [Model explanation](../Model/README.md)

## Review scope

The November 2, 2026 review recorded checks against the published report, supplied source screenshots, saved report screenshots, model diagram, and available DAX documentation.

The November 3, 2026 documentation review incorporated the current measure expressions supplied by the report author and checked the saved Product screenshot against the documented product figures.

The November 3 update did not repeat the earlier live interaction checks. No PBIX or complete source workbook was inspected during these reviews.

## Displayed findings

The November 2 review recorded the following reporting context:

- Period: September 2021–December 2022.
- Manager selection: All.
- Discount-band filter: cleared.

| Metric | Recorded value |
|---|---:|
| Units sold | 1,125,788 |
| Gross sales | $127,931,599 |
| Discounts | $9,205,248 |
| Net sales | $118,726,350 |
| Cost of goods | $101,832,648 |
| Profit | $16,893,702 |
| Enterprise net sales | $19,611,694 |
| Enterprise profit | −$614,546 |
| USA net sales | $25,029,830 |
| USA profit | $2,995,541 |
| France net sales | $24,354,172 |
| France profit | $3,781,021 |
| Paseo net sales | $33,011,144 |
| Paseo profit | $4,797,438 |
| Amarilla net sales | $17,747,116 |
| Amarilla profit | $2,814,104 |

### Financial consistency

Displayed net sales minus cost of goods equals displayed profit:

`$118,726,350 − $101,832,648 = $16,893,702`

Displayed gross sales minus discounts is $1 higher than displayed net sales:

`$127,931,599 − $9,205,248 = $118,726,351`

Independent rounding of displayed amounts could explain this difference. The underlying unrounded amounts have not been reconciled, so the cause is not confirmed.

## Arithmetic supporting the case study

Ratios below are calculated from the recorded monetary totals and rounded to one decimal place.

| Calculation | Result |
|---|---:|
| Enterprise margin: −614,546 / 19,611,694 | −3.1% |
| USA margin: 2,995,541 / 25,029,830 | 12.0% |
| France margin: 3,781,021 / 24,354,172 | 15.5% |
| France profit minus USA profit: 3,781,021 − 2,995,541 | $785,480 |
| Paseo share of total profit: 4,797,438 / 16,893,702 | 28.4% |
| Paseo margin: 4,797,438 / 33,011,144 | 14.5% |
| Amarilla margin: 2,814,104 / 17,747,116 | 15.9% |

The [saved Product screenshot](Screenshots/05-Sales-Analysis-Product.png) supports the recorded Paseo and Amarilla amounts.

The report displays margins rounded to whole percentages. The case study uses ratios calculated from monetary totals to provide one-decimal precision.

The earlier documentation stated Amarilla's margin as 16.1%. This was corrected to 15.9% after checking the recorded amounts against the saved screenshot. This correction changes the documentation, not the report's calculations.

Recommendations in the case study are proposed follow-up analyses, not implemented business actions or measured business improvements.

## Recorded interaction checks

The following results were recorded during the November 2, 2026 review.

| Check | Recorded result |
|---|---|
| Home, Period, Segment, Country, and Product pages | Opened successfully. |
| High discount-band selection | Net sales changed to $37,372,487 and profit to $3,388,867, with a displayed margin of 9%. Reset restored the overall totals. |
| Bob Reyes selection | Greeting and photo changed. Net sales were $9,791,599 and profit was $2,039,159. Selection persisted from Period to Segment. |
| September 2021 profit tooltip | Displayed $763,603 with Top 5 Managers, matching the displayed period table. |
| Bob/Government profit drill-through | Retained $2,039,159, while the manager dropdown displayed All. |
| Enterprise units drill-through | Retained 168,549 units. |
| Detail-page return arrows | Both tested paths returned to the unfiltered Period page rather than the original view and filter context. |
| Help-page buttons | All three Watch Here buttons produced no visible response during the review. The cause was not established. |

All four drill-through destinations appeared in the segment context menu.

Net Sales and Cost of Goods detail pages and the remaining tooltips were not individually validated. Export behavior, social links, mobile layout, and security configuration were not tested.

Saved screenshots document particular report states; they do not establish that every live interaction has been tested.

## DAX documentation alignment

The updated DAX reference contains the 24 measure definitions supplied from the current Power BI model.

These definitions resolve the earlier documentation discrepancies:

- `M_Net Sales` sums `FACT_SALES[Sales]`.
- `M_Profit` references `M_Net Sales` and `M_Cost Of Goods`.
- `M_Profit Margin` uses a consistent measure name.
- `M_Date Selection` uses the minimum and maximum selected `DIM_DATE[Reporting Period]` values.
- `M_Transparent` returns `RGBA(255,255,255,0)`.

The expressions were grouped and formatted for readability without changing their calculation behavior.

This alignment updates the repository's description of the measures. It does not constitute independent execution testing against the PBIX or a change to the published report.

## Time-comparison interpretation

The dataset covers four months of 2021 and twelve months of 2022.

The current profit comparison:

- Selects current and prior reporting years using `DIM_DATE[Year Rank]`.
- Calculates `(CY − PY) / ABS(PY)`.
- Removes the filter on `DIM_DATE[Year]`.
- Uses an `IFERROR` fallback of `1`.

The full-period 236% badge recorded in the earlier review is excluded from the case-study findings because it has not been established as a comparison of equivalent reporting periods.

The error fallback formats as 100% if an error is caught. That fallback is not a measured growth result.

A like-for-like growth claim requires confirmed date coverage and equivalent periods. For this dataset, September–December in both years provides a candidate comparison window, subject to checking data completeness and filter behavior.

## Remaining verification scope

The reviews do not establish completion of the following checks:

| Area | Verification not established |
|---|---|
| Source reconciliation | Agreement between complete source workbooks, transformed data, and report totals using unrounded amounts. |
| Sales-record grain | Uniqueness of the proposed combination of reporting period and business attributes. |
| Relationships | Exact relationship endpoints, dimension-key uniqueness, and unmatched fact-table keys. |
| Data quality | Complete duplicate, missing-value, and invalid-value checks. |
| Date logic | Exact date-table generation, year-rank generation, and behavior under different date selections. |
| Calculation edge cases | Results for zero, missing, or negative comparison denominators and multiple refresh values. |
| Report interactions | Untested detail pages, tooltips, export behavior, and mobile layout. |
| Security | Any row-level security configuration or access-control implementation. |

The report author states that the date table was created using minimum and maximum dates. Its full generation expression was not included in this review.

## Business interpretation

Profit means net sales minus cost of goods. It does not represent verified net income after operating expenses and tax.

The figures describe an educational dataset. They do not establish causal relationships, realized revenue improvements, or cost savings.

Manager-based filtering and personalized greetings demonstrate report interaction. They do not, by themselves, demonstrate row-level security.