# Sales data dictionary

Field and measure definitions for the Sales report. Source screenshots, the model diagram, and the supplied DAX are the reference materials. Remaining source and model checks are listed in Docs/Validation.md.

## FACT_SALES

Apparent grain: a summarized sales record by reporting month, segment, country, product, discount band, and manager assignment. Uniqueness of that combination is untested. No visible order identifier supports the former “order line” description.

| Field | Meaning and status |
|---|---|
| Segment / Segment Short Name | Customer segment and model display abbreviation. |
| Country / Country Short Name | Country and display abbreviation. |
| Product / Product Name (Short) | Product label and shortened display label. |
| Manager ID | Relationship key to the manager dimension; unmatched keys not yet checked. |
| Discount Band / Discount Band ID | Discount category and model category identifier; thresholds not supplied. |
| Units Sold | Quantity sold. |
| Sale Price (source); Sales Price (model) | Unit price before the separately deducted Discounts, based on visible source-row arithmetic. Confirm the source-to-model rename. |
| Manufacturing Price | Product manufacturing-price attribute supplied in the source; distinct from the recorded COGs amount. |
| Gross Sales | Sales amount before discounts: Units Sold × Sale Price in the source examples. |
| Discounts | Monetary discount amount, not percentage. |
| Sales | Sales after discounts: Gross Sales − Discounts. |
| COGS (source); COGs (model) | Source-provided cost of goods amount. Underlying costing method not supplied. |
| Profit | Source profit amount: Sales − COGS. |
| Date, Month Number, Month Name, Year | Visible source period fields. Do not assume they all remain in the final fact table. |
| YearMonth | Model period key. Confirm its exact format and type from the PBIX. |

Net Sales is a measure concept; the supplied diagram does not show a separate Net Sales fact column. Source files already contain monetary calculations. Dollar formatting does not establish currency code.

## DIM_DATE

| Field | Meaning |
|---|---|
| Reporting Period | Reporting month, e.g. September 2021; shown as a date field in the model. |
| Month (Short), Month Name, Month Number | Month labels and sorting attributes. |
| Quarter | Calendar quarter (1–4). |
| Year | Calendar year. |
| Year Rank | Exported DAX uses text values "1" for CY and "2" for PY. Rank-generation logic is not supplied. |
| YearMonth | Apparent month join key; confirm uniqueness and relationship endpoints. |

The diagram suggests a month-oriented dimension. A continuous daily date table has not been demonstrated.

## DIM_MANAGERS

| Field | Meaning |
|---|---|
| Manager ID | Intended unique manager key. |
| Manager Name, First Name, Last Name | Display and greeting fields. |
| Territory | Source examples combine segment and country (e.g. Channel Partners–Canada). |
| Image URL | Course-supplied hosted image used in report personalization. |

## Utility tables

`LAST_REFRESHED[Date Last Refreshed]` stores a refresh value. Its generation process is not provided. `*Measures` organizes calculations; it is not a business fact table.

## Measures

| Measure / concept | Definition and caveat |
|---|---|
| M_Units Sold | SUM of Units Sold. |
| M_Gross Sales | SUM of Gross Sales. |
| M_Discounts | SUM of Discounts. |
| M_Net Sales | Intended gross sales minus discounts; export references unprefixed measures not defined in that file. |
| M_Cost OF Goods | SUM of COGs, using the export's spelling. |
| M_Profit | Intended net sales minus COGs; export references unresolved unprefixed names. This is profit after cost of goods, not verified net income. |
| M_Profit Margin | Profit / Net Sales; exported IFERROR returns 0 on errors. The initial definition includes a trailing space in its name. |
| M_# of Distinct Countries / Segments | DISTINCTCOUNT of corresponding fact attribute in filter context. |
| M_Profit CY / PY | Profit filtered by Year Rank "1" / "2". |
| M_Profit YOY% | (CY − PY) / ABS(PY), evaluated with ALL(DIM_DATE[Year]); returns 1 on error. Unequal-period comparison is unresolved. |
| M_Units Sold CY / PY | Units filtered by the same year ranks. |
| M_100% Minus Profit Margin | 1 − margin, with error fallback 0; a display helper, not a separate business outcome. |
| M_Date/Time Last Refreshed | Formats VALUES of the refresh field; assumes a single value. |
| M_Profit YOY% Formatted | Prefixes the formatted YoY result with `YOY: `. |
| M_Profit Margin Formatted | Formats margin as a whole percentage and adds a plus sign for positive values. |
| M_User Greeting | Builds `Welcome <first name>!` from the selected manager. |
| M_User Greeting Help | Builds the Help-page greeting and assistance text from the selected manager. |
| M_Welcome_All | Returns `#fff` when SELECTEDVALUE(Manager ID) is blank; otherwise returns `RGBA(255,255,255,0)`. |
| M_Discount Band Colors | High: `#e92f42`; Medium: `#ffbe0b`; Low: `#049570`; None: `#808080`; default: `#07345F`. |
| Transparent | Supplied expression: `[Transparent] := 0`. Returns numeric zero; the visual property using it is not included in the export. |
| M_Transparent | Name shown in the model diagram. Whether this is the same measure as the exported Transparent needs confirmation. |
| M_Date Selection | Reporting-period label shown in the model and report, e.g. `Sep/21 - Dec/22`. Its expression is not included in the supplied Sales Measures.DAX. |
| Date/Time Last Refreshed | Supplied unprefixed helper: MAX of LAST_REFRESHED[Date Last Refreshed]. Separate from the formatted M_Date/Time Last Refreshed expression. |

The table includes all definitions supplied in the Sales DAX file plus the two model-visible helpers above. Formula availability is distinguished from a measure's presence in the report.

