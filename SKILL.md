# ZeroCell Skill API Contract

## Scope

This document defines the stable contract for using ZeroCell as an AI skill through CLI and MCP.

## Version Contract

- Contract version: `2026.03`
- Tool schema version: `1.2.0`
- Minimum compatible tool schema version: `1.0.0`
- Patch schema version: `1.0`
- Compatibility model: additive fields in minor versions, breaking changes only in major versions.

## Stable Commands

- `read`
- `read-resilient`
- `read-safe`
- `context`
- `query-sheet`
- `inspect-workbook`
- `inspect-sheet`
- `propose-patch`
- `validate-patch`
- `apply-patch`
- `update`
- `schema-info`
- `skill-api-contract`
- `runtime-observability`
- `mcp-stdio`

## Stable MCP Tools

- `read_matrix`
- `read_matrix_resilient`
- `read_matrix_safe`
- `inspect_workbook`
- `inspect_sheet_context`
- `propose_patch`
- `validate_patch`
- `apply_patch`
- `query_sheet`
- `finance_analyze`
- `schema_info`
- `skill_api_contract`
- `runtime_observability`

## Patching Workflow (Propose -> Validate -> Apply)

To modify a workbook, follow this 3-step sequence:

1. **propose_patch**: Generate a structural patch from cell-level or matrix-level changes.
   - **Preferred input**: `{"workbook_path":"...","sheet_name":"Model","cell_map":{"A1":"New Value","B2":"=C2*1.1"}}`
   - **Legacy input (still supported)**:
     - `{"workbook_path":"...","payload":{"sheet":"Model","cell_map":{"A1":"New Value"}}}`
     - `{"workbook_path":"...","payload":{"sheet":"Model","matrices":{"values":[...],"formulas":[...]}}}`
   - Returns: A `WorkbookPatch` object.
2. **validate_patch**: (Optional but Recommended) Run the patch against a policy.
   - Requires: `workbook_path` and the `patch` from step 1.
3. **apply_patch**: Commit the changes to a new file.
   - Requires: `workbook_path`, `patch`, and `output_path`.

### Example Preferred Input
```json
{
  "workbook_path": "book.xlsx",
  "sheet_name": "Data Log",
  "cell_map": {
    "A8": "Division 99",
    "B8": "Special Ops",
    "C8": "=SUM(D1:D10)"
  }
}
```

### Response Status Guarantees

#### `read_matrix_resilient`

- Trigger: requested sheet not found.
- Behavior: returns `status: fallback_sheet_suggestion` and fallback metadata.
- Response guarantees:
  - `fallback.used = true`
  - `fallback.reason_code = worksheet_not_found`
  - `fallback.available_sheets` populated from workbook inspection
  - `fallback.suggested_sheet` best-effort match when possible

### `runtime_observability`

- Trigger: telemetry query requested.
- Behavior: returns telemetry summary with failure and fallback distribution.
- Failure mode: explicit IO diagnostic if telemetry file path does not exist.

## Requirements for Outputs

### All Excel files

- **Professional Font**: Use a consistent, professional font (e.g., Arial, Times New Roman) for all deliverables.
- **Zero Formula Errors**: Every Excel model MUST be delivered with ZERO formula errors (`#REF!`, `#DIV/0!`, `#VALUE!`, `#N/A`, `#NAME?`).
- **Preserve Existing Templates**: Exactly match existing format, style, and conventions when modifying templates. Never impose standardized formatting on files with established patterns.

### Formulas vs Hardcodes (CRITICAL RULE)

- **Always use Excel formulas instead of calculating values and hardcoding them.** This ensures the spreadsheet remains dynamic and updateable.
- Provide cell references instead of hardcoded values in formulas (e.g., use `=B5*(1+$B$6)` instead of `=B5*1.05`).
- If you must add hardcoded inputs based on research, document them! Comment or place notes beside the table: `"Source: [Document], [Date], [Reference]"`.

### Recalculating Formulas

- ZeroCell includes formula recalculation capabilities through the `recalculate_workbook` tool (via LibreOffice headless). You **MUST** run this tool after applying patches that inject new formulas to ensure the workbook evaluates correctly and there are no `#VALUE!` or `#REF!` errors.

### Financial Models

#### Color Coding Standards

- **Blue text**: Hardcoded inputs.
- **Black text**: ALL formulas and calculations.
- **Green text**: Links pulling from other worksheets within the same workbook.
- **Red text**: External links to other files.
- **Yellow background**: Key assumptions needing attention.

#### Number Formatting Standards

- **Years**: Format as text strings (e.g., `"2024"` not `"2,024"`).
- **Currency**: ALWAYS specify units in headers (`"Revenue ($mm)"`).
- **Zeros**: Use number formatting to make all zeros `-`, including percentages.
- **Percentages**: Default to `0.0%` format (one decimal).

## Changelog Policy Link

See [CHANGELOG_POLICY.md](/Users/michaelwong/Developer/ZeroCell/docs/CHANGELOG_POLICY.md).

## Release + Rollback Link

See [RELEASES_AND_ROLLBACK.md](/Users/michaelwong/Developer/ZeroCell/docs/RELEASES_AND_ROLLBACK.md).

## Migration + Compatibility Links

- Migration assistant: [CONTRACT_MIGRATION.md](/Users/michaelwong/Developer/ZeroCell/docs/CONTRACT_MIGRATION.md)
- Compatibility certification matrix outputs: `integration/reports/*/COMPATIBILITY_MATRIX.md`
- Single-binary skill bundle guide: [distribution/README.md](/Users/michaelwong/Developer/ZeroCell/distribution/README.md)
