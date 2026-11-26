# Model explanation

![Sales model diagram](Schema.png.png)

The diagram shows two active one-to-many relationships. DIM_DATE and DIM_MANAGERS each filter FACT_SALES in one direction. LAST_REFRESHED and the measures table appear disconnected.

Manager ID and YearMonth are the apparent relationship keys in the diagram. The course workbook contains 25 unique manager IDs, and all 700 sales rows match a manager. The diagram does not expose the exact relationship endpoint properties.

Country, segment, product, and discount attributes reside in FACT_SALES. The date dimension appears oriented around reporting months; do not assume a daily marked date table. Manager filtering customizes the report but does not demonstrate row-level security.

The fact table contains summarized source sales records, not verified individual orders. See the [data dictionary](../Data_Dictionary.md) and [validation checklist](../Docs/Validation.md).
