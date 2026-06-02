# Deployment Guide — Microsoft Cloud Advisor Skill

## Installation Methods

### Method 1: User-Level Installation (Recommended)

This makes the skill available across all your VS Code workspaces.

**Windows (PowerShell):**
```powershell
# Clone the repo
git clone https://github.com/bibrani/microsoft-cloud-advisor.git
cd microsoft-cloud-advisor

# Install to user-level skills directory
New-Item -ItemType Directory -Force -Path "$env:USERPROFILE\.copilot\skills\microsoft-cloud-advisor"
Copy-Item "skills\microsoft-cloud-advisor\SKILL.md" "$env:USERPROFILE\.copilot\skills\microsoft-cloud-advisor\SKILL.md"
```

**macOS / Linux:**
```bash
git clone https://github.com/bibrani/microsoft-cloud-advisor.git
cd microsoft-cloud-advisor

mkdir -p ~/.copilot/skills/microsoft-cloud-advisor
cp skills/microsoft-cloud-advisor/SKILL.md ~/.copilot/skills/microsoft-cloud-advisor/SKILL.md
```

### Method 2: Workspace-Level Installation

This makes the skill available only within a specific project.

```bash
# From your project root
mkdir -p .github/skills/microsoft-cloud-advisor
cp <path-to-repo>/skills/microsoft-cloud-advisor/SKILL.md .github/skills/microsoft-cloud-advisor/SKILL.md
```

### Method 3: VS Code Prompt Files (Alternative)

If you prefer using `.prompt.md` format:

```bash
# Copy to your VS Code user prompts folder
# Windows: %APPDATA%\Code\User\prompts\
# macOS: ~/Library/Application Support/Code/User/prompts/
# Linux: ~/.config/Code/User/prompts/
```

---

## MCP Server Setup

The skill requires MCP servers for full functionality. Add these to your VS Code configuration:

### Option A: User Settings (`settings.json`)

Press `Ctrl+Shift+P` → "Preferences: Open User Settings (JSON)" and add:

```json
{
  "mcp": {
    "servers": {
      "azure": {
        "command": "npx",
        "args": ["-y", "@azure/mcp@latest"]
      },
      "microsoft-docs": {
        "command": "npx",
        "args": ["-y", "@anthropic/microsoft-docs-mcp@latest"]
      },
      "excel-mcp": {
        "command": "npx",
        "args": ["-y", "excel-mcp-server@latest"]
      },
      "markitdown": {
        "command": "npx",
        "args": ["-y", "@microsoft/markitdown-mcp@latest"]
      }
    }
  }
}
```

### Option B: Workspace MCP Config (`.vscode/mcp.json`)

Create `.vscode/mcp.json` in your project:

```json
{
  "servers": {
    "azure": {
      "command": "npx",
      "args": ["-y", "@azure/mcp@latest"]
    },
    "microsoft-docs": {
      "command": "npx",
      "args": ["-y", "@anthropic/microsoft-docs-mcp@latest"]
    },
    "excel-mcp": {
      "command": "npx",
      "args": ["-y", "excel-mcp-server@latest"]
    },
    "markitdown": {
      "command": "npx",
      "args": ["-y", "@microsoft/markitdown-mcp@latest"]
    }
  }
}
```

> **Important**: Verify the exact npm package names from [VS Code MCP Servers Gallery](https://code.visualstudio.com/docs/copilot/chat/mcp-servers). Package names may change.

---

## Azure Authentication

The Azure MCP server requires authentication:

```powershell
# Login to Azure (if not already)
az login

# Verify subscription
az account show
```

The skill uses your active Azure CLI session for live pricing, resource queries, and architecture context.

---

## Verification

After installation, verify the skill is loaded:

1. Open VS Code
2. Open Copilot Chat (Agent mode: `Ctrl+Shift+I`)
3. Type: `"Design an Azure architecture for a web app"`
4. The skill should activate (you'll see it use MCP tools for docs/pricing)

### Troubleshooting

| Issue | Fix |
|-------|-----|
| Skill not triggering | Verify `SKILL.md` is in the correct path |
| MCP tools unavailable | Check MCP server config in settings |
| Azure tools failing | Run `az login` to refresh auth |
| Excel MCP failing | Ensure Excel desktop is installed (Windows only) |
| Document conversion failing | Check markitdown MCP is running |

---

## Uninstall

```powershell
# Windows
Remove-Item -Recurse "$env:USERPROFILE\.copilot\skills\microsoft-cloud-advisor"

# macOS/Linux
rm -rf ~/.copilot/skills/microsoft-cloud-advisor
```
