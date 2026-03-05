# ZeroCell Skill for OpenCode

Welcome to the ZeroCell Skill repository.

This repository contains the hardened, single-binary distribution of the ZeroCell Skill for OpenCode — injecting Excel AI matrix intelligence directly into your LLM workflow.

## Key Features

- **Direct Cell Patching**: Patch Excel files cell-by-cell natively (e.g. `{"Sheet1": {"A1": "New Value", "B2": "=SUM(A1:A2)"}}`) without hallucinating massive 2D arrays.
- **Native Formula Recalculation**: Force-evaluate complex spreadsheet models exactly like the Excel UI via the `recalculate_workbook` tool. 
- **Financial Validation & Inspection**: Validate formulas, extract parallel `values` and `formulas` matrices, and generate structural diffs before applying changes.
- **Enterprise Controls**: Includes telemetry tracking, rollback policies, and PII-redacted safe-read operations.

## Installation

Clone or download this repository:
```bash
git clone https://github.com/tachyonlabshq/zerocell-pub.git
cd zerocell-pub
```

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
