# Sales DAX documentation

[Measures.DAX](Measures.DAX) contains the measure definitions supplied from the Sales Analysis Power BI model, grouped by reporting purpose.

The `M_` prefix is the project's measure-naming convention. The file presents individual Power BI measure definitions for reference.

## Core measures

| Measure | Definition |
|---|---|
| `M_Gross Sales` | Sum of `FACT_SALES[Gross Sales]`. |
| `M_Discounts` | Sum of `FACT_SALES[Discounts]`. |
| `M_Net Sales` | Sum of `FACT_SALES[Sales]`, the sales-after-discounts field. |
| `M_Cost Of Goods` | Sum of `FACT_SALES[COGs]`. |
| `M_Profit` | Net sales minus cost of goods. |
| `M_Profit Margin` | Profit divided by net sales, with an error fallback of zero. |
| `M_Units Sold` | Sum of `FACT_SALES[Units Sold]`. |

Profit in this report means net sales minus cost of goods. Operating expenses and tax are outside this calculation.

## Coverage measures

| Measure | Definition |
|---|---|
| `M_# of Distinct Countries` | Distinct countries in the current filter context. |
| `M_# of Distinct Segments` | Distinct customer segments in the current filter context. |

## Time comparison

| Measure | Definition |
|---|---|
| `M_Profit CY` | Profit filtered to `DIM_DATE[Year Rank] = "1"`. |
| `M_Profit PY` | Profit filtered to `DIM_DATE[Year Rank] = "2"`. |
| `M_Units Sold CY` | Units sold filtered to year rank `"1"`. |
| `M_Units Sold PY` | Units sold filtered to year rank `"2"`. |
| `M_Profit YOY%` | Difference between current- and prior-year profit, divided by the absolute value of prior-year profit, with the filter on `DIM_DATE[Year]` removed. |

The dataset covers September 2021–December 2022. Because the full dataset contains four months of 2021 and twelve months of 2022, the case study does not present the full-period comparison as like-for-like annual growth.

The current YoY expression uses an `IFERROR` fallback of `1`. If an error is caught, this value displays as 100%; it is a fallback rather than a calculated growth result.

See the [validation notes](../Docs/Validation.md) for the scope of the recorded checks.

## Presentation helpers

| Measure | Purpose |
|---|---|
| `M_Date Selection` | Formats the minimum and maximum selected reporting dates as `MMM/YY - MMM/YY`. |
| `M_Date/Time Last Refreshed` | Displays the refresh value as `Last Refreshed: mm.dd.yy`. |
| `M_Transparent` | Returns `RGBA(255,255,255,0)` for transparent formatting. |
| `M_Welcome_All` | Returns white when the selected manager ID resolves to blank; otherwise returns a transparent color. |
| `M_User Greeting` | Builds the welcome message using the selected manager's first name. |
| `M_User Greeting Help` | Builds the help message using the selected manager's first name. |
| `M_Profit Margin Formatted` | Displays margin as a whole percentage, with a plus sign for positive values. |
| `M_Profit YOY% Formatted` | Displays the formatted comparison percentage with a `YOY: ` prefix. |
| `M_Discount Band Colors` | Assigns colors to discount-band selections. |
| `M_100% Minus Profit Margin` | Calculates one minus profit margin for report presentation, with an error fallback of zero. |

### Discount-band colors

| Selection | Color |
|---|---|
| High | `#e92f42` |
| Medium | `#ffbe0b` |
| Low | `#049570` |
| None | `#808080` |
| Default | `#07345F` |

### Display behavior

- Formatted percentage measures return text. The underlying numeric measures support calculations.
- Manager greetings use `SELECTEDVALUE` to retrieve a first name. They do not define a separate greeting for multiple or no selected first names.
- The refresh label uses `VALUES` and expects a single distinct refresh value in the current context.

## Model dependencies

| Table | Fields referenced by the measures |
|---|---|
| `FACT_SALES` | Gross Sales, Discounts, Sales, COGs, Units Sold, Country, Segment, Discount Band |
| `DIM_DATE` | Reporting Period, Year, Year Rank |
| `DIM_MANAGERS` | Manager ID, First Name |
| `LAST_REFRESHED` | Date Last Refreshed |

The DAX file documents measure expressions. Table relationships, Power Query transformations, and date-table generation are separate parts of the Power BI model.

## Related documentation

- [Project overview](../README.md)
- [Data dictionary](../Data_Dictionary.md)
- [Model explanation](../Model/README.md)
- [Validation notes](../Docs/Validation.md)