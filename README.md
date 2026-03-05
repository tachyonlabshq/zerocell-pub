# ZeroCell Skill for OpenCode

Welcome to the ZeroCell Skill repository. 

This repository contains the hardened, obfuscated, single-binary distribution of the ZeroCell Skill for OpenCode. It provides a massive leap in spreadsheet intelligence by injecting an Excel AI matrix directly into the context of your LLM workflow.

## Installation Instructions

To install the ZeroCell skill and MCP integration for OpenCode on macOS or Windows, follow these simple steps:

### 1. Download the Repository
Clone or download this repository to a secure location on your machine.
```bash
git clone https://github.com/tachyonlabshq/zerocell-pub.git
cd zerocell-pub
```

### 2. Configure OpenCode MCP
OpenCode connects to ZeroCell using the Model Context Protocol (MCP).

1. Open your OpenCode Settings.
2. Navigate to the **MCP Servers** configuration section.
3. Add a new MCP server configuration using the specific paths for your operating system based on where you downloaded this repository.

#### macOS Configuration
```json
{
  "$schema": "https://opencode.ai/config.json",
  "mcp": {
    "zerocell": {
      "type": "local",
      "command": ["/path/to/zerocell-pub/bin/zerocell-macos", "mcp"],
      "enabled": true
    }
  }
}
```

#### Windows Configuration
```json
{
  "$schema": "https://opencode.ai/config.json",
  "mcp": {
    "zerocell": {
      "type": "local",
      "command": ["C:\\path\\to\\zerocell-pub\\bin\\zerocell-windows.exe", "mcp"],
      "enabled": true
    }
  }
}
```

### 3. Grant Permissions
- **macOS Users:** You may need to grant execution permissions to the binary if it was downloaded via Safari or Mail.
  ```bash
  chmod +x bin/zerocell-macos
  ```
- **Windows Users:** If you receive a SmartScreen warning, click "More info" and "Run anyway".

### 4. Enable the Skill
Once the MCP server is connected, the `@ZeroCell` skill will be available in your OpenCode prompt. Use it to instantly decompose, analyze, and query massive Excel workbooks!
