# Sales DAX documentation

`Measures.DAX` is preserved byte-for-byte from the supplied repository. It documents existing exported expressions; it is not a complete, tested import script. It mixes `[Name] :=` notation with `Name =` definitions.

## Dependencies to reconcile against Power BI

- M_Net Sales references `[Gross Sales]` and `[Discounts]`, although this export defines M_Gross Sales and M_Discounts.
- M_Profit references `[Net Sales]` and `[COGS]`, which are not defined here.
- The first margin definition has a trailing space in `[M_Profit Margin ]`, while later references omit it.
- The diagram includes M_Date Selection and M_Transparent; the export omits the former and instead includes `[Transparent] := 0` for the latter concept. Do not assume numeric zero means transparent color.
- Full Power Query steps and Year Rank generation are absent.

These findings establish an incomplete/inconsistent export, not that the published report necessarily uses broken formulas. Re-export the actual measures before changing the model.

## Proposed core cleanup — not implemented or tested

After verifying model names and dependencies, consistent definitions could be:

```dax
M_Net Sales = [M_Gross Sales] - [M_Discounts]
M_Profit = [M_Net Sales] - [M_Cost Of Goods]
M_Profit Margin = DIVIDE([M_Profit], [M_Net Sales])
```

These are individual measure definitions, not a bulk import script. The proposed margin returns blank for an undefined denominator rather than presenting an error as zero. Confirm this business rule before implementation and recheck dependent visuals.

## Time comparison

The existing profit YoY measure selects ranked years, removes the Year filter, uses ABS(PY), and returns 100% on error. It does not explicitly restrict the comparison to months present in both years. The report covers September 2021–December 2022, so the displayed 236% should not be used as matched-period growth.

First compare September–December 2022 against September–December 2021. Validate both profit totals against the source and document the denominator convention, especially if prior profit is negative. Return blank or an explicit not-comparable label when prior profit is zero/missing. A model-specific replacement cannot be certified without confirming the date dimension and rank logic.

## Presentation helpers

The supplied export includes the following helpers. Their expressions are retained in Measures.DAX.

| Helper | Purpose / expression |
|---|---|
| Transparent | `[Transparent] := 0`; numeric helper. The export does not identify its visual-property binding. |
| M_Welcome_All | White (`#fff`) when no single manager ID is selected; transparent RGBA color otherwise. |
| M_User Greeting | Welcome text using the selected manager's first name. |
| M_User Greeting Help | Help text using the selected manager's first name. |
| Date/Time Last Refreshed | MAX refresh date. |
| M_Date/Time Last Refreshed | Refresh date formatted as `Last Refreshed: mm.dd.yy`. |
| M_Profit Margin Formatted | Percentage label with a plus sign for positive margins. |
| M_Profit YOY% Formatted | Percentage label prefixed with `YOY: `. |
| M_Discount Band Colors | Color mapping for High, Medium, Low, None, and default selection. |
| M_100% Minus Profit Margin | Complement used alongside margin in the report's display. |

**M_Date Selection** is present in the model diagram and the live reporting-period label. The supplied Sales DAX file does not include its formula. **M_Transparent** also appears in the diagram, while the file names the zero-valued helper **Transparent**. These are documentation gaps to reconcile, not grounds to remove the features from the project description.

The Airline file's M_Date Selection references Calendar[Date]. That is not the Sales date table, so copying it here would introduce an incorrect dependency. Obtain the Sales expression from its model before adding executable code under that name.

The single-manager greeting has no explicit All/multiple-selection fallback. Refresh formatting assumes VALUES returns one value. Formatted margins are text for display; numeric measures remain appropriate for calculations and sorting.
