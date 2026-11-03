# Validation and improvement record

Reviewed November 2, 2026. Evidence: live published report interactions, supplied source screenshots, local model diagram, original DAX export, and documentation. No PBIX or complete source workbook was inspected. Existing screenshot files are illustrations of saved states, not new audit captures.

## Displayed findings

Context: September 2021–December 2022, Manager All, discount filter cleared.

| View | Observed value |
|---|---:|
| Overview units | 1,125,788 |
| Gross sales | $127,931,599 |
| Discounts | $9,205,248 |
| Net sales | $118,726,350 |
| Cost of goods | $101,832,648 |
| Profit | $16,893,702 |
| Enterprise net sales / profit | $19,611,694 / −$614,546 |
| USA net sales | $25,029,830 |
| France profit | $3,781,021 |
| Paseo profit | $4,797,438 |

Displayed net sales minus COGs equals displayed profit. Displayed gross sales minus discounts is $1 above displayed net sales; independently rounded cards can produce this, but unrounded amounts must be checked before concluding the cause.

## Interaction checks

### Arithmetic supporting the case study

Calculated from the displayed totals, rounded to one decimal place:

- Enterprise margin: −614,546 / 19,611,694 = −3.1%.
- USA margin: 2,995,541 / 25,029,830 = 12.0%.
- France margin: 3,781,021 / 24,354,172 = 15.5%.
- France minus USA profit: 3,781,021 − 2,995,541 = $785,480.
- Paseo share of total profit: 4,797,438 / 16,893,702 = 28.4%.
- Paseo margin: 4,797,438 / 33,011,144 = 14.5%.
- Amarilla margin: 2,814,104 / 17,747,116 = 16.1%.

The saved Product screenshot supports the Paseo/Amarilla comparison. The report rounds displayed margins to whole percentages, so README ratios use the monetary totals rather than copying rounded margin labels. Recommendations are follow-up analysis, not implemented business actions.

| Check | Observed result |
|---|---|
| Home, Period, Segment, Country, Product | Opened successfully. |
| High discount selection | Net sales $37,372,487; profit $3,388,867; displayed margin 9%. Reset restored totals. |
| Bob Reyes selection | Greeting/photo changed; net sales $9,791,599 and profit $2,039,159. Selection persisted from Period to Segment. |
| September 2021 profit tooltip | $763,603 with Top 5 Managers, matching the displayed period table. |
| Bob/Government profit drill-through | Retained $2,039,159, but manager dropdown displayed All. |
| Enterprise units drill-through | Retained 168,549 units. |
| Detail return arrows | Both tested paths returned to unfiltered Period, rather than the original view/context. |
| Help buttons | All three Watch Here buttons produced no visible response in the session. Cause not established. |

All four drill-through destinations appeared in the segment context menu. Net Sales and Cost of Goods detail pages and remaining tooltips were not individually validated. Screenshot availability does not establish live interaction correctness. Export, social links, mobile layout, and security configuration were not tested.

## Priority backlog

1. **Before claiming growth:** validate matching-period YoY and replace the error-to-100% fallback. Exclude 236% from public findings meanwhile.
2. **Before claiming a complete technical export:** reconcile undefined DAX dependencies, naming, and missing model-visible measures. Preserve original evidence.
3. **Before claiming source validation:** obtain complete workbooks/PBIX; test candidate grain, duplicate rows, dimension uniqueness, unmatched keys, nulls, unrounded financial identities, negative/zero denominators, and totals across slices.
4. **Terminology:** describe profit as net sales less cost of goods; clarify the live Net Profit label. Confirm currency and dataset attribution.
5. **Usability:** show inherited drill-through filters, preserve return context, repair Help links, label the manager selector, and widen truncated product labels.

The documentation copy addresses source/contribution transparency, grain uncertainty, field meanings, aggregate-detail wording, and file links. It does not alter or repair the published report.

## Evidence needed to finish validation

- Actual PBIX/PBIP or relationship-property export and complete current measures.
- Source workbooks and Power Query transformations, including date changes and Year Rank generation.
- Dataset origin/course identification and applicable asset/data reuse terms.
- Unrounded reconciliation results and screenshots with explicit filter context after any changes.

No revenue improvement, cost savings, causal discount effect, production deployment, or security implementation is claimed.
