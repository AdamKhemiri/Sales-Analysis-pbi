# Data Dictionary

## FACT_SALES (fact, grain: order line)
| Column                | Type           | Description                                      |
|-----------------------|----------------|--------------------------------------------------|
| Country               | Text           | Country name                                     |
| Country Short Name    | Text           | Abbreviation                                     |
| Segment               | Text           | Customer segment                                 |
| Segment Short Name    | Text           | Abbreviation                                     |
| Product               | Text           | Product key/name                                 |
| Product Name (Short)  | Text           | Short label                                      |
| Manager ID            | Text           | Foreign key to DIM_MANAGERS                      |
| Units Sold            | Whole number   | Quantity sold                                    |
| Sales Price           | Currency       | Unit price after discount                        |
| Manufacturing Price   | Currency       | Unit cost                                        |
| Sales                 | Currency       | Base sales amount                                |
| Gross Sales           | Currency       | Units * List price                               |
| Discounts             | Currency       | Discount amount                                  |
| Net Sales             | Currency       | Gross Sales - Discounts                          |
| COGs                  | Currency       | Cost of goods sold                               |
| Profit                | Currency       | Net Sales - COGs                                 |
| Discount Band         | Text           | Discount band                                    |
| Discount Band ID      | Whole number   | Discount band identifier                         |
| YearMonth             | Text           | YYYYMM helper                                    |

## DIM_DATE (dimension)
| Column          | Type         | Description                          |
|-----------------|-------       |--------------------------------------|
| Reporting Period| Text         | September 2025                       |
| Month (Short)   | Text         | Jan, Feb, …                          |
| Month Name      | Text         | January, …                           |
| Month Number    | Text         | 01–12                                |
| Quarter         | Whole number | 1-2                                  |
| Year            | Whole number | Calendar year                        |
| Year Rank       | Text         | Sort helper                          |
| YearMonth       | Text         | YYYYMM                               |

## DIM_MANAGERS (dimension)
| Column       | Type     | Description                    |
|--------------|----------|--------------------------------|
| Manager ID   | Text     | Key                            |
| Manager Name | Text     | Full name                      |
| First Name   | Text     |                                |
| Last Name    | Text     |                                |
| Territory    | Text     | Region/country responsibility  |
| Image URL    | Text     | Photo for report               |

## LAST_REFRESHED (utility)
| Column              | Type | Description                  |
|---------------------|------|------------------------------|
| Date Last Refreshed | Date | ETL/refresh timestamp        |

## Core Measures (selection)
| Measure                 | Definition (plain)                       |
|-------------------------|------------------------------------------|
| Units Sold              | Sum of Units Sold                        |
| Gross Sales             | Sum of Gross Sales                       |
| Discounts               | Sum of Discounts                         |
| Net Sales               | Gross Sales - Discounts                  |
| COGS                    | Sum of COGs                              |
| Profit                  | Net Sales - COGS                         |
| Profit Margin %         | Profit / Net Sales                       |
| Profit/Units (optional) | Profit / Units Sold                      |
| Profit YOY %            | (Profit - Profit PY) / Profit PY         |
| Date/Time Refreshed     | Max Date Last Refreshed                  |
