# Sales data dictionary

Field and measure definitions for the Sales Analysis Power BI report.

The report analyzes sales, discounts, cost of goods, profit, and units sold across reporting periods, customer segments, countries, products, and managers.

[Project overview](README.md) · [DAX documentation](Dax%20Measures/README.md) · [Model explanation](Model/README.md) · [Validation notes](Docs/Validation.md)

## FACT_SALES

The sales table contains source sales records and the attributes used to group and filter reporting results.

The available documentation indicates summarized sales records rather than individual orders. Uniqueness at a specific combination of month, segment, country, product, discount band, and manager has not been established.

| Field | Definition |
|---|---|
| Segment / Segment Short Name | Customer segment and its shortened display label. |
| Country / Country Short Name | Country and its shortened display label. |
| Product / Product Name (Short) | Product and its shortened display label. |
| Manager ID | Manager identifier used to associate sales records with the manager dimension. |
| Discount Band / Discount Band ID | Discount category and its identifier. |
| Units Sold | Quantity sold. |
| Sale Price (source) / Sales Price (model) | Unit selling-price field. The source examples show the price before separately recorded discounts. |
| Manufacturing Price | Manufacturing-price attribute supplied in the source, distinct from the recorded cost-of-goods amount. |
| Gross Sales | Sales amount before discounts. Source examples calculate this as units sold multiplied by sale price. |
| Discounts | Monetary discount amount. |
| Sales | Sales amount after discounts. This field is summed by `M_Net Sales`. |
| COGS (source) / COGs (model) | Recorded cost-of-goods amount. The measure expressions reference `FACT_SALES[COGs]`. |
| Profit | Source profit amount, calculated as sales after discounts minus cost of goods. |
| YearMonth | Period key used in the model. |

The source also includes Date, Month Number, Month Name, and Year fields. Their presence in the source does not establish that every field is retained in the final sales table.

### Financial definitions

- **Gross sales:** sales before discounts.
- **Net sales:** sales after discounts, represented by `FACT_SALES[Sales]`.
- **Profit:** net sales minus cost of goods.
- **Profit margin:** profit divided by net sales.

The report's profit calculation does not include operating expenses or tax.

`M_Net Sales` is a measure. Its expression does not depend on a separate fact-table column named Net Sales.

Amounts are displayed with a dollar symbol. A specific currency code is not established by formatting alone.

## DIM_DATE

The report author created the date table using the minimum and maximum dates. It provides reporting-period labels and attributes for filtering and time comparisons.

The dataset covers September 2021–December 2022.

| Field | Definition |
|---|---|
| Reporting Period | Date field used for reporting-period selection and date-range labels. |
| Month (Short) | Abbreviated month label with a trailing period, created using `FORMAT(DIM_DATE[Reporting Period], "mmm") & "."`. |
| Month Name | Month-name label. |
| Month Number | Month-number attribute used for ordering or grouping. |
| Quarter | Calendar-quarter attribute. |
| Year | Calendar year. |
| Year Rank | Attribute used by measures to select the current reporting year with `"1"` and the prior reporting year with `"2"`. |
| YearMonth | Period key used in the model. |

The measure definitions document how Year Rank is used. The date-table generation expression, rank-generation expression, and exact relationship endpoints are not included in the DAX measure file.

Creating a date table from minimum and maximum dates establishes its boundaries. It does not, by itself, establish the table's daily or monthly granularity or its marked-date-table configuration.

## DIM_MANAGERS

The manager dimension supports manager filtering and personalized report content.

| Field | Definition |
|---|---|
| Manager ID | Manager identifier used in the model relationship and selection-dependent formatting. |
| Manager Name | Full manager name. |
| First Name | First name used by the welcome and help messages. |
| Last Name | Manager surname. |
| Territory | Assigned territory. Source examples combine customer segment and country. |
| Image URL | Hosted manager image used in the report. |

Manager names, territory assignments, and image URLs were supplied with the course materials.

## Utility tables

| Table | Purpose |
|---|---|
| LAST_REFRESHED | Stores the `Date Last Refreshed` value used by the refresh label. |
| *Measures | Organizes the report's measures. |

The refresh-label expression expects a single distinct refresh value in the current filter context. The process that generates this value is separate from the display measure.

## Measures

The following 24 definitions match the supplied expressions in [Measures.DAX](Dax%20Measures/Measures.DAX).

### Core measures

| Measure | Definition |
|---|---|
| M_Gross Sales | Sum of `FACT_SALES[Gross Sales]`. |
| M_Discounts | Sum of `FACT_SALES[Discounts]`. |
| M_Net Sales | Sum of `FACT_SALES[Sales]`. |
| M_Cost Of Goods | Sum of `FACT_SALES[COGs]`. |
| M_Profit | `M_Net Sales` minus `M_Cost Of Goods`. |
| M_Profit Margin | Profit divided by net sales, with an `IFERROR` fallback of zero. |
| M_Units Sold | Sum of `FACT_SALES[Units Sold]`. |

### Coverage measures

| Measure | Definition |
|---|---|
| M_# of Distinct Countries | Distinct count of `FACT_SALES[Country]` in the current filter context. |
| M_# of Distinct Segments | Distinct count of `FACT_SALES[Segment]` in the current filter context. |

### Time-comparison measures

| Measure | Definition |
|---|---|
| M_Profit CY | Profit filtered to `DIM_DATE[Year Rank] = "1"`. |
| M_Profit PY | Profit filtered to `DIM_DATE[Year Rank] = "2"`. |
| M_Units Sold CY | Units sold filtered to `DIM_DATE[Year Rank] = "1"`. |
| M_Units Sold PY | Units sold filtered to `DIM_DATE[Year Rank] = "2"`. |
| M_Profit YOY% | `(CY - PY) / ABS(PY)`, evaluated with the filter on `DIM_DATE[Year]` removed. The `IFERROR` fallback is `1`. |

The full dataset contains four months of 2021 and twelve months of 2022. A full-period comparison therefore does not represent equivalent annual coverage.

The YoY error fallback formats as 100% if an error is caught. It is not a measured growth result. The case study does not use the displayed 236% badge as evidence of like-for-like growth.

### Reporting-period and refresh labels

| Measure | Definition |
|---|---|
| M_Date Selection | Formats the minimum and maximum selected `DIM_DATE[Reporting Period]` values as `MMM/YY - MMM/YY`. |
| M_Date/Time Last Refreshed | Formats the single distinct refresh value as `Last Refreshed: mm.dd.yy`. |

### Margin and comparison display helpers

| Measure | Definition |
|---|---|
| M_100% Minus Profit Margin | One minus profit margin, with an error fallback of zero. |
| M_Profit Margin Formatted | Whole-percentage text with a plus sign for positive margins. |
| M_Profit YOY% Formatted | Formatted comparison text prefixed with `YOY: `. |

Formatted measures return text for labels. The underlying numeric measures support calculations.

### Manager greetings and visibility

| Measure | Definition |
|---|---|
| M_User Greeting | Welcome message using the selected manager's first name. |
| M_User Greeting Help | Help message using the selected manager's first name. |
| M_Transparent | Returns `RGBA(255,255,255,0)`. |
| M_Welcome_All | Returns `#fff` when the selected manager ID resolves to blank; otherwise returns `RGBA(255,255,255,0)`. |

The greeting expressions use `SELECTEDVALUE` without a separate message for multiple or no selected first names.

### Discount-band colors

| Measure | Definition |
|---|---|
| M_Discount Band Colors | Returns a color based on the selected discount band, with a default color when no listed band resolves. |

| Selection | Color |
|---|---|
| High | `#e92f42` |
| Medium | `#ffbe0b` |
| Low | `#049570` |
| None | `#808080` |
| Default | `#07345F` |

## Documentation scope

The measure definitions reflect the expressions supplied from the current Power BI model.

Field descriptions draw on the supplied model information, source examples, and report documentation. They do not establish completed tests of source-row uniqueness, relationship keys, or full source-to-report reconciliation.

See the [validation notes](Docs/Validation.md) for recorded checks and remaining verification scope.