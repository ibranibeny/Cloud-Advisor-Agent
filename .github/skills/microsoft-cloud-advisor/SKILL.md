---
name: microsoft-cloud-advisor
description: >
  Microsoft Cloud & Azure advisory skill for architecture advisory, migration planning,
  cost optimization, multi-cloud comparison, data platforms, hub-spoke networking,
  document processing, and presentation generation.
---

# Microsoft Cloud Advisor

Azure architecture advisory, migration planning, cost optimization, multi-cloud comparison,
data platform (Fabric/Synapse/SQL), hub-spoke networking, Well-Architected Framework reviews,
and document/presentation generation.

## When to Use

- Azure architecture questions at any depth (L100-L400)
- Migration strategy (6R classification, effort estimates)
- Cost estimation and Azure pricing calculator generation (Excel)
- Multi-cloud comparison (Azure vs AWS vs GCP)
- Data platform advisory (Fabric, Synapse, OneLake, SQL)
- Hub-spoke / Virtual WAN networking design and cost
- Processing uploaded documents (Word, PDF, PPT) for Azure advisory
- Generating presentation-style markdown decks

## L-Level Depth Convention

| Level | Audience | Depth |
|-------|----------|-------|
| L100 | Executive / non-technical | High-level overview, analogies |
| L200 | IT decision makers | Feature comparison, architecture diagrams |
| L300 | Architects / senior engineers | SKU details, IP ranges, config samples |
| L400 | Deep specialists | Threat models, compliance mapping, IaC |

Default to L200 unless user specifies otherwise or context implies L300+.

## Workflow Router

Based on user intent, load the appropriate workflow file:

| Domain | Workflow File |
|--------|--------------|
| Architecture advisory, Azure design questions | `workflows/architecture-advisory.md` |
| Migration planning, 6R, modernization strategy | `workflows/migration-planning.md` |
| Cost estimation, pricing, Excel calculator | `workflows/cost-calculator.md` |
| Multi-cloud comparison (vs AWS / vs GCP) | `workflows/multi-cloud-comparison.md` |
| Data platform (Fabric, Synapse, OneLake) | `workflows/data-platform-fabric.md` |
| Hub-spoke, VNet peering, VWAN, landing zone | `workflows/hub-spoke-networking.md` |
| Document processing (uploaded Word/PDF/PPT) | `workflows/document-processing.md` |
| Presentation / slide generation | `workflows/presentation-generation.md` |

## Reference Data

| Reference | Content |
|-----------|---------|
| `references/service-mapping-tables.md` | Cross-cloud service equivalents (Azure/AWS/GCP) |
| `references/mcp-tool-usage.md` | MCP tool families, when/how to call, priority order |
| `references/hub-spoke-patterns.md` | Mermaid templates by L-level, Firewall SKU table, cost formulas |

## Global Rules

1. **MCP First** -- Always attempt MCP tools before relying on training data
2. **Cite Sources** -- State whether info came from MCP (live) or training knowledge
3. **Graceful Degradation** -- If MCP unavailable, use estimates with verification link
4. **L-Level Respect** -- Never exceed requested depth; offer to go deeper
5. **Actionable Output** -- End every response with concrete next steps
