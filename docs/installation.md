---
layout: default
title: Installation
nav_order: 2
permalink: /installation/
---

# Installation
{: .no_toc }

1. TOC
{:toc}

---

## Prerequisites

| Requirement | Purpose |
|-------------|---------|
| VS Code 1.100+ | Host editor with GitHub Copilot |
| GitHub Copilot Chat (agent mode) | Agent runtime |
| Node.js 18+ | Required for `npx` install and MCP servers |
| Azure MCP Server | Live pricing, SKU availability, region checks |
| Microsoft Learn MCP | Documentation validation |
| Excel MCP Server | Cost-calculator workbook generation (Windows) |
| Markitdown MCP | Document conversion (Word / PDF / PPT → markdown) |

---

## Option A: Scaffold into an existing project (recommended)

Use `npx degit` to copy the agent files into your current workspace — no `git clone`, no `cd`, no git history:

```bash
npx degit ibranibeny/Cloud-Advisor-Agent
```

This drops `.github/agents/`, `.vscode/mcp.json`, and the skill files into the current folder.

---

## Option B: Clone as a standalone workspace

```bash
git clone https://github.com/ibranibeny/Cloud-Advisor-Agent.git
code Cloud-Advisor-Agent
```

---

## Install the MCP Servers

Run these one-time commands to install and verify the MCP servers the agent uses:

```bash
npx -y @azure/mcp@latest
npx -y @anthropic/microsoft-docs-mcp@latest
npx -y excel-mcp-server@latest
npx -y @microsoft/markitdown-mcp@latest
```

### Configure `.vscode/mcp.json`

```json
{
  "servers": {
    "azure":          { "command": "npx", "args": ["-y", "@azure/mcp@latest"] },
    "microsoft-lea":  { "command": "npx", "args": ["-y", "@anthropic/microsoft-docs-mcp@latest"] },
    "excel-mcp":      { "command": "npx", "args": ["-y", "excel-mcp-server@latest"] },
    "microsoft_mar":  { "command": "npx", "args": ["-y", "@microsoft/markitdown-mcp@latest"] }
  }
}
```

---

## Finish Setup

1. **Reload VS Code** so it picks up the agent and MCP config: `Ctrl+Shift+P` → **Developer: Reload Window**.
2. **Open Copilot Chat** in agent mode and invoke the advisor:

   ```
   @cloud-advisor Design a hub-spoke network for my Azure landing zone
   ```

{: .tip }
> Append an L-level (`L100`–`L400`) to any prompt to control depth — `L100` for executive summaries, `L400` for detailed architecture specs with subnet CIDRs and costed comparisons.

---

## Troubleshooting

| Symptom | Fix |
|---------|-----|
| MCP tools unavailable | Verify `node --version` ≥ 18, re-run the npx install commands |
| Agent not listed | Confirm `.github/agents/` exists in the workspace root, then reload VS Code |
| MCP server won't start | Check the Output panel (`Ctrl+Shift+U` → select **MCP**) for errors |
| Excel calculator fails | Excel MCP requires Windows (COM automation); close the file in the Excel desktop app |
