# Quickstart: Microsoft Cloud Advisor Agent

## Prerequisites

- VS Code 1.100+ with GitHub Copilot extension
- Node.js 18+ (for npx MCP server commands)
- Windows (required for Excel MCP; other capabilities work cross-platform)

## Setup (5 minutes)

### 1. Clone the repository

```bash
git clone https://github.com/<org>/microsoft-cloud-advisor.git
cd microsoft-cloud-advisor
```

### 2. Configure MCP servers

Create `.vscode/mcp.json` in the workspace root:

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
    "excel": {
      "command": "npx",
      "args": ["-y", "excel-mcp-server@latest"]
    },
    "markitdown": {
      "command": "npx",
      "args": ["-y", "@microsoft/markitdown-mcp@latest"]
    },
    "aws": {
      "command": "npx",
      "args": ["-y", "@aws/mcp@latest"]
    }
  }
}
```

### 3. Reload VS Code

Press `Ctrl+Shift+P` → "Developer: Reload Window"

### 4. Verify agent is available

Open Copilot Chat and type:

```
@microsoft-cloud-advisor Hello
```

The agent should respond with the L-level selection prompt.

## Usage

### Architecture advisory

```
@microsoft-cloud-advisor Design a hub-spoke network for 3 spoke VNets with shared services
```

### Migration planning

```
@microsoft-cloud-advisor We have 50 on-prem VMs running SQL Server and .NET apps. Help me plan migration to Azure.
```

### Cost calculator

```
@microsoft-cloud-advisor Generate an Azure cost calculator for a lift-and-shift of 20 VMs to East US 2
```

### Multi-cloud comparison

```
@microsoft-cloud-advisor Compare Azure Container Apps vs AWS ECS vs GCP Cloud Run for a microservices workload
```

### Document processing

Attach a Word/PDF/PPT file and ask:

```
@microsoft-cloud-advisor Analyze this architecture document and suggest Azure modernization opportunities
```

### Presentation generation

```
@microsoft-cloud-advisor Create a 10-slide executive presentation on our Azure migration roadmap at L100
```

## Validation Checklist

Manual acceptance tests mapped to success criteria SC-001 through SC-008.

### SC-001: Architecture Advisory (US1)
- [ ] Ask: `@microsoft-cloud-advisor Design a 3-tier web app architecture for Azure`
- [ ] Response includes a Mermaid architecture diagram
- [ ] Response includes a service mapping table (requirement → Azure service)
- [ ] WAF pillar considerations are mentioned
- [ ] L-level depth matches requested level (default L200)

### SC-002: Migration Planning (US2)
- [ ] Ask: `@microsoft-cloud-advisor Help me migrate 10 on-prem VMs to Azure`
- [ ] 6R strategy options are presented before generating a plan
- [ ] After selecting a strategy, a phased migration plan is produced
- [ ] Gantt-style timeline is included (Mermaid gantt or table)
- [ ] Cost estimation is referenced or offered

### SC-003: Cost Calculator (US2)
- [ ] Ask: `@microsoft-cloud-advisor Generate an Azure cost calculator for 5 D4s VMs in East US 2`
- [ ] Excel workbook is created with multiple sheets (Summary, Compute, etc.)
- [ ] Formulas are present (SUMPRODUCT, growth projection)
- [ ] Currency formatting is applied

### SC-004: Multi-Cloud Comparison (US3)
- [ ] Ask: `@microsoft-cloud-advisor Compare AKS vs EKS vs GKE for container orchestration`
- [ ] Comparison table has minimum 3 criteria columns
- [ ] Both Azure and at least one other provider have data
- [ ] Honest recommendation is provided (may favor non-Azure)

### SC-005: Document Processing (US4)
- [ ] Attach a Word or PDF file and ask: `Analyze this document`
- [ ] Content is extracted and summarized
- [ ] Advisory is provided based on document content
- [ ] Unsupported formats produce a graceful error message

### SC-006: Presentation Generation (US5)
- [ ] Ask: `@microsoft-cloud-advisor Create a 5-slide presentation on Azure migration at L100`
- [ ] Output uses slide-separator format (`---`)
- [ ] Maximum 6 bullets per slide
- [ ] Speaker notes are included (`> Speaker notes:`)
- [ ] Mermaid diagrams are used for architecture/timeline slides

### SC-007: Data Platform Advisory (US6)
- [ ] Ask: `@microsoft-cloud-advisor Recommend a data platform for real-time IoT analytics`
- [ ] Data Platform Decision Tree is applied
- [ ] Fabric services are recommended where appropriate
- [ ] Architecture pattern is named (lakehouse, medallion, real-time, etc.)

### SC-008: Graceful Degradation
- [ ] Disconnect one MCP server (e.g., remove `azure` from mcp.json)
- [ ] Ask an architecture question
- [ ] Agent responds with available information (no crash/hang)
- [ ] Missing MCP is acknowledged with guidance to reconnect

## Troubleshooting

| Issue | Fix |
|-------|-----|
| Agent not found | Ensure `.github/agents/microsoft-cloud-advisor.agent.md` exists |
| MCP tools unavailable | Check `.vscode/mcp.json` exists; reload VS Code |
| Excel generation fails | Ensure Windows + Excel installed; Excel MCP is Windows-only |
| AWS/GCP tools missing | Package names may differ; check MCP server logs in Output panel |
