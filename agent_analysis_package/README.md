# Clean Spreadsheet Analysis Package

This package is a compact, source-like view of the sanitized business spreadsheets. It is not a database model and does not generate workflow, history, entity, or relationship rows.

## Files

- `RMS_clean.csv`: 224 combined RMS and RMS NFAR records using only the RMS column structure.
- `COI_clean.csv`: 52 COI records using the primary COI fields.
- `Events_clean.csv`: 9 event records using the primary event fields.
- `field_guide.csv`: field meanings, workflow order, possible values, codes, and explicit exclusions.
- `acronyms.csv`: acronyms detected in the clean files; undefined terms intentionally have blank definitions.

## Interpretation

1. In `RMS_clean.csv`, Final Status 3, 4, or 5 identifies an NFAR record. Final Status 1, 2, or blank identifies an RMS record. No generated state column is added.
2. Blank means the source did not record a value. It does not automatically mean false or not applicable.
3. Workflow information remains in the original wide columns. There is no generated workflow-step table.
4. Dates with known years use `YYYY-MM-DD`. A COI date shown as `MM-DD` had no year in the source and was not inferred.
5. Source `SR #` values restart between RMS and RMS NFAR and therefore are not globally unique.
6. NFAR-only columns and dated weekly-history columns are excluded. See the `EXCLUDED_FIELD` rows in `field_guide.csv`.
7. Free-text wording is trimmed and line breaks are removed, but wording is otherwise preserved to avoid changing meaning.
8. The confidential name mapping and Client Observations are not included.
