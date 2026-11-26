# Sales validation notes

The course workbook reconciles to $118.73 million in net sales and $16.89 million in gross profit. This page gives the unrounded figures, matched-period growth calculation and results from testing the report.

[Project overview](../README.md) · [Data dictionary](../Data_Dictionary.md) · [DAX documentation](../Dax%20Measures/README.md) · [Model explanation](../Model/README.md)

## Evidence and dates

- November 2–3, 2026. Live report interactions, saved screenshots, model diagram and 24 DAX definitions.
- November 24, 2026. The original course workbook provided the 700 sales rows and 25 manager records used to check totals, keys and like-for-like growth.

## Displayed findings

The figures below use these selections in the published report.

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

The November 24 check summed the 700 sales rows in the original course workbook before rounding: gross sales of $127,931,598.50 minus discounts of $9,205,248.24 equals $118,726,350.26 in net sales. Subtracting $101,832,648.00 in cost of goods yields $16,893,702.26 in gross profit. Independent whole-dollar rounding explains the $1 display difference. All 25 manager IDs were unique in the manager table, and every sales row matched a manager.

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

Recommendations in the case study are proposed follow-up analyses, not implemented business actions or measured business improvements.

## Recorded interaction checks

These interactions were tested in the published report on November 2, 2026.

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

## DAX behind the displayed figures

The [24 measure definitions](../Dax%20Measures/Measures.DAX) include the following calculations.

- `M_Net Sales` sums `FACT_SALES[Sales]`.
- `M_Profit` references `M_Net Sales` and `M_Cost Of Goods`.
- `M_Profit Margin` uses a consistent measure name.
- `M_Date Selection` uses the minimum and maximum selected `DIM_DATE[Reporting Period]` values.
- `M_Transparent` returns `RGBA(255,255,255,0)`.

## Time-comparison interpretation

The dataset covers four months of 2021 and twelve months of 2022.

The current profit comparison:

- Selects current and prior reporting years using `DIM_DATE[Year Rank]`.
- Calculates `(CY − PY) / ABS(PY)`.
- Removes the filter on `DIM_DATE[Year]`.
- Uses an `IFERROR` fallback of `1`.

The full-period 236% badge compares twelve months of 2022 with four months of 2021.

The error fallback formats as 100% if an error is caught. That fallback is not a measured growth result.

The workbook contains each month from September 2021 through December 2022. Summing the same four calendar months in each year across all managers and discount bands gives:

| September–December | 2021 | 2022 | Change |
|---|---:|---:|---:|
| Net sales | $26,415,255.51 | $36,157,519.26 | +36.9% |
| Gross profit | $3,878,464.51 | $5,435,484.26 | +40.1% |

These are matched-period workbook results. The existing full-period dashboard badge covers a different date range.

## Remaining verification scope

| Area | Remaining check |
|---|---|
| Source reconciliation | The headline monetary totals reconcile to the workbook. Transformed rows and every report filter combination have yet to be reconciled individually. |
| Sales-record grain | Uniqueness of the proposed combination of reporting period and business attributes. |
| Relationships | Manager keys are unique and every sales row matches. Date-key coverage and the relationship properties inside the model remain unverified. |
| Data quality | Complete duplicate, missing-value, and invalid-value checks. |
| Date logic | Exact date-table generation, year-rank generation, and behavior under different date selections. |
| Calculation edge cases | Results for zero, missing, or negative comparison denominators and multiple refresh values. |
| Report interactions | Untested detail pages, tooltips, export behavior, and mobile layout. |
| Security | Any row-level security configuration or access-control implementation. |

The date table uses the minimum and maximum reporting dates. The repository documents its reporting fields and year-rank measures.

## Business interpretation

Profit means net sales minus cost of goods. It does not represent verified net income after operating expenses and tax.

The figures describe an educational dataset. They do not establish causal relationships, realized revenue improvements, or cost savings.

Manager-based filtering and personalized greetings demonstrate report interaction. They do not, by themselves, demonstrate row-level security.
