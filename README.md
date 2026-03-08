# ZeroCell Skill for OpenCode

ZeroCell is the hardened public distribution of the Rust-based Excel skill used by AI agents.

## Current capability set

- Direct workbook-safe read, patch, validate, and apply flows
- Native formula recalculation via headless LibreOffice
- Bounded `query_sheet` responses for safer agent loops
- Polars-backed schema inference and column profiling
- Sheet and workbook anomaly scanning
- Workbook-native `@agent` comment/task discovery
- Task-state sync and task-context resolution
- Source joins across sheets and named ranges
- Runtime telemetry, environment doctoring, and project bootstrap

## Contract

- Contract version: `2026.03`
- Tool schema version: `1.9.0`
- Minimum compatible tool schema version: `1.0.0`

## Installation

Clone or download this repository:

```bash
git clone https://github.com/tachyonlabshq/zerocell-pub.git
cd zerocell-pub
```

The distribution includes the helper scripts used by `recalculate_workbook` under `scripts/`. The binary resolves them relative to itself, so recalculation works even when the current working directory is a different project.

### macOS (Apple Silicon)

```json
{
  "$schema": "https://opencode.ai/config.json",
  "mcp": {
    "zerocell": {
      "type": "local",
      "command": ["/path/to/zerocell-pub/bin/zerocell-macos-arm64", "mcp-stdio"],
      "enabled": true
    }
  }
}
```

```bash
chmod +x bin/zerocell-macos-arm64
```

### macOS (Intel)

```json
{
  "$schema": "https://opencode.ai/config.json",
  "mcp": {
    "zerocell": {
      "type": "local",
      "command": ["/path/to/zerocell-pub/bin/zerocell-macos-x64", "mcp-stdio"],
      "enabled": true
    }
  }
}
```

```bash
chmod +x bin/zerocell-macos-x64
```

### Windows (x64)

```json
{
  "$schema": "https://opencode.ai/config.json",
  "mcp": {
    "zerocell": {
      "type": "local",
      "command": ["C:\\path\\to\\zerocell-pub\\bin\\zerocell-windows-x64.exe", "mcp-stdio"],
      "enabled": true
    }
  }
}
```

### Windows (ARM64)

```json
{
  "$schema": "https://opencode.ai/config.json",
  "mcp": {
    "zerocell": {
      "type": "local",
      "command": ["C:\\path\\to\\zerocell-pub\\bin\\zerocell-windows-arm64.exe", "mcp-stdio"],
      "enabled": true
    }
  }
}
```

## Binary inventory

- `bin/zerocell-macos-arm64`
- `bin/zerocell-macos-x64`
- `bin/zerocell-windows-x64.exe`
- `bin/zerocell-windows-arm64.exe`

## Notes

- `query_sheet` accepts aliases such as `path`, `workbook`, `sheet`, and `worksheet`, but the stable contract remains `workbook_path` + `sheet_name`.
- `profile_sheet`, `scan_sheet_anomalies`, `scan_workbook_anomalies`, and `join_sources` are the recommended planning tools before structural edits.
- `join_sources` accepts sheet names directly and named ranges via `named:RangeName`.
- `doctor` validates Python, LibreOffice, helper scripts, temp-dir access, and telemetry paths.
- `init` generates local MCP config and a project-scoped skill stub under `.zerocell/`.
- Windows recalculation does not rely on `AF_UNIX`.
- Windows ARM64 binary has not been rebuilt in this publish step; it remains the previous artifact until the missing cross compiler is installed.
