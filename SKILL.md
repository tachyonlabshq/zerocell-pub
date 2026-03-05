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

## Fallback Policies

### `read_matrix_resilient`

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

## Changelog Policy Link

See [CHANGELOG_POLICY.md](/Users/michaelwong/Developer/ZeroCell/docs/CHANGELOG_POLICY.md).

## Release + Rollback Link

See [RELEASES_AND_ROLLBACK.md](/Users/michaelwong/Developer/ZeroCell/docs/RELEASES_AND_ROLLBACK.md).

## Migration + Compatibility Links

- Migration assistant: [CONTRACT_MIGRATION.md](/Users/michaelwong/Developer/ZeroCell/docs/CONTRACT_MIGRATION.md)
- Compatibility certification matrix outputs: `integration/reports/*/COMPATIBILITY_MATRIX.md`
- Single-binary skill bundle guide: [distribution/README.md](/Users/michaelwong/Developer/ZeroCell/distribution/README.md)
