# ZeroCell Skill API Contract

## Scope

This document defines the stable contract for using ZeroCell as an AI skill through CLI and MCP.

## Version Contract

- Contract version: `2026.03`
- Tool schema version: `1.9.0`
- Minimum compatible tool schema version: `1.0.0`
- Patch schema version: `1.0`
- Compatibility model: additive fields in minor versions, breaking changes only in major versions.

## Stable Commands

- `read`
- `read-resilient`
- `read-safe`
- `context`
- `query-sheet`
- `profile-sheet`
- `scan-sheet-anomalies`
- `scan-workbook-anomalies`
- `join-sources`
- `scan-agent-comments`
- `sync-agent-tasks`
- `set-agent-task-status`
- `resolve-agent-task-context`
- `inspect-workbook`
- `inspect-sheet`
- `propose-patch`
- `validate-patch`
- `apply-patch`
- `update`
- `doctor`
- `init`
- `schema-info`
- `skill-api-contract`
- `runtime-observability`
- `mcp-stdio`

## Stable MCP Tools

- `read_matrix`
- `read_matrix_resilient`
- `read_matrix_safe`
- `inspect_workbook`
- `profile_sheet`
- `scan_sheet_anomalies`
- `scan_workbook_anomalies`
- `join_sources`
- `inspect_sheet_context`
- `query_sheet`
- `scan_agent_comments`
- `sync_agent_tasks`
- `update_agent_task_status`
- `resolve_agent_task_context`
- `propose_patch`
- `validate_patch`
- `apply_patch`
- `finance_analyze`
- `doctor_environment`
- `schema_info`
- `skill_api_contract`
- `runtime_observability`

## Operational Notes

- Preferred query arguments remain `workbook_path`, `sheet_name`, and a narrow `range`.
- `query_sheet` clips omitted-range responses to a safe `50 x 26` default window.
- `profile_sheet` returns inferred headers, column types, confidence scores, and representative samples.
- `scan_sheet_anomalies` flags duplicate headers, mixed types, sparse columns, partial formula coverage, and numeric outliers.
- `scan_workbook_anomalies` aggregates sheet findings and cross-sheet type conflicts.
- `join_sources` aligns workbook sources by key. Sources may be sheet names or named ranges using `named:RangeName`.
- `scan_agent_comments` detects workbook comments/notes tagged with `@agent` and extracts explicit references.
- `sync_agent_tasks` and `resolve_agent_task_context` provide workbook-native orchestration state.
- `recalculate_workbook` relies on bundled helper scripts under `scripts/` and does not use `AF_UNIX` on Windows.

## Publish Notes

- This repository ships compiled binaries only.
- Export this file from the main `ZeroCell` repository instead of editing it directly in the public package.
