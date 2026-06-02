# Quickstart: Hub-and-Spoke Networking Feature

## Prerequisites

- VS Code 1.100+ with GitHub Copilot Chat enabled
- Azure MCP server configured (for live pricing): `@azure/mcp`
- Microsoft Learn MCP server (for documentation grounding)
- The `cloud-advisor` agent available in VS Code chat

## Quick Test

1. Open VS Code Chat and select the `@cloud-advisor` agent
2. Ask: "What's the best practice for Azure networking?"
3. Verify the response includes:
   - Hub-spoke as the recommended topology
   - Named hub services (Firewall, Gateway, Bastion, DNS)
   - A Mermaid architecture diagram
   - Actionable next steps

## Key Scenarios to Test

| # | Prompt | Expected Behavior |
|---|--------|-------------------|
| 1 | "What's the best practice for Azure networking?" | Hub-spoke recommendation with diagram |
| 2 | "Compare cost of hub-spoke vs per-VNet firewall for 4 spokes" | Asks Firewall SKU → cost comparison table |
| 3 | "Should I use one spoke or multiple spokes?" | Decision matrix with 5+ factors |
| 4 | "Show me a hub-spoke architecture diagram at L300" | Mermaid with subnet-level detail |
| 5 | "How do I set up an Azure Landing Zone?" | Hub-spoke as networking foundation |
| 6 | "Compare hub-spoke vs Azure Virtual WAN" | Comparison table (FR-009) |

## Configuration

The feature is configured via:
- `.github/agents/cloud-advisor.agent.md` — Agent definition with hub-spoke triggers
- `.github/skills/microsoft-cloud-advisor/SKILL.md` — Skill procedure with hub-spoke domain
- `templates/hub-spoke-*.md` — Response templates for diagrams, cost tables, decision matrices

## Pricing Behavior

- Agent calls `mcp_azure_mcp_pricing` for live Firewall/Gateway/Bastion pricing
- If MCP unavailable: returns approximate estimates with disclaimer
- VNet peering: fixed $0.01/GB same-region (not queried from MCP)
- Firewall SKU: always asked — never assumed
