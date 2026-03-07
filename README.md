# ZeroCell Skill for OpenCode

Welcome to the ZeroCell Skill repository.

This repository contains the hardened, single-binary distribution of the ZeroCell Skill for OpenCode — injecting Excel AI matrix intelligence directly into your LLM workflow.

## Key Features

- **Direct Cell Patching**: Patch Excel files cell-by-cell natively (e.g. `{"Sheet1": {"A1": "New Value", "B2": "=SUM(A1:A2)"}}`) without hallucinating massive 2D arrays.
- **Native Formula Recalculation**: Force-evaluate complex spreadsheet models exactly like the Excel UI via the `recalculate_workbook` tool.
- **Bounded Sheet Queries**: `query_sheet` accepts `workbook_path`/`sheet_name` aliases (`path`, `workbook`, `sheet`, `worksheet`) and clips no-range responses to a safe default window so agents do not loop on oversized output.
- **Columnar Analytics Layer**: ZeroCell now uses Polars for bounded matrix analytics such as sparse extraction and inspection summaries, while keeping Excel read/write on the workbook-safe engine.
- **Financial Validation & Inspection**: Validate formulas, extract parallel `values` and `formulas` matrices, and generate structural diffs before applying changes.
- **Enterprise Controls**: Includes telemetry tracking, rollback policies, and PII-redacted safe-read operations.

## Installation

Clone or download this repository:
```bash
git clone https://github.com/tachyonlabshq/zerocell-pub.git
cd zerocell-pub
```

The distribution includes the helper scripts used by `recalculate_workbook` under `scripts/`. The binary resolves them relative to itself, so recalculation works even when the current working directory is a different project.

### macOS (Apple Silicon)

Add to your project's `opencode.json`:
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

Grant execute permission if needed:
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

> **Note for Windows users**: If SmartScreen warns you, click "More info" → "Run anyway".

## Available Binaries

| File | Platform |
|------|----------|
| `bin/zerocell-macos-arm64` | macOS Apple Silicon (M1/M2/M3/M4) |
| `bin/zerocell-macos-x64` | macOS Intel |
| `bin/zerocell-windows-x64.exe` | Windows x64 |
| `bin/zerocell-windows-arm64.exe` | Windows ARM64 (Copilot+ PCs) |

## Recalculation Notes

- `recalculate_workbook` requires LibreOffice plus Python.
- Override the helper script path with `ZEROCELL_RECALC_SCRIPT` if needed.
- Override the Python interpreter with `ZEROCELL_PYTHON_BIN` if needed.
- Windows recalculation uses the bundled helper path and does not rely on `AF_UNIX`.

## Query Tool Notes

- Preferred MCP arguments: `workbook_path`, `sheet_name`, and a narrow `range`.
- Accepted aliases: `workbook`, `path`, `file_path`, `sheet`, `worksheet`, `tab_name`.
- Nested argument objects under `request`, `payload`, `input`, or `query` are accepted.
- If `range` is omitted, `query_sheet` clips the response to a default `50 x 26` window and returns `bounded_by_default_window` plus `response_note`.
