# Implementation Plan: Hub-and-Spoke Network Architecture Skill

**Branch**: `002-hub-spoke-networking` | **Date**: 2026-06-02 | **Spec**: [spec.md](spec.md)

**Input**: Feature specification from `specs/002-hub-spoke-networking/spec.md`

## Summary

Add hub-and-spoke networking as a first-class capability of the `cloud-advisor` agent so that Azure best-practice and price-comparison questions default to a hub-spoke topology with centralized shared hub resources (Azure Firewall, VPN/ExpressRoute Gateway, Azure Bastion, DNS Private Resolver), VNet peering, and single- vs. multiple-spoke decision support. The work is delivered through VS Code agent-customization artifacts (markdown + YAML frontmatter): the `cloud-advisor` agent definition, the `microsoft-cloud-advisor` SKILL.md procedure, and the cost-calculator template. Cost analysis uses live region-specific pricing via the Azure Pricing MCP tool, asks the user for the Firewall SKU, target region, and per-spoke monthly data-transfer volume (with documented editable defaults of East US 2 and 1,000 GB/month/spoke when unknown), and presents Virtual WAN as a qualitative alternative unless a quantified comparison is explicitly requested.

## Technical Context

**Language/Version**: Markdown + YAML frontmatter (VS Code agent customization format); Mermaid for diagrams

**Primary Dependencies**: GitHub Copilot Chat (agent mode), Azure MCP (`pricing`, `cloudarchitect`, `get_available_region`), Microsoft Learn MCP (docs grounding), Excel MCP (cost workbook generation, Windows)

**Storage**: N/A — declarative agent/skill/template files committed to the repository

**Testing**: Manual scenario validation via the quickstart prompts (no compiled test harness for agent-customization artifacts)

**Target Platform**: VS Code 1.100+ with GitHub Copilot Chat; Excel generation is Windows-only (COM interop)

**Project Type**: Single project — VS Code agent customization (skill + agent + templates)

**Performance Goals**: N/A (advisory/generative skill; no runtime throughput target). Responses must complete within a single chat interaction.

**Constraints**: Region-specific pricing MUST come from the Azure Pricing MCP tool with graceful degradation to approximate estimates if unavailable; single-region topology only; up to 10 spokes rendered individually (beyond 10 summarized as "N spokes × unit cost"); both SKILL.md copies (`.github/skills/...` and public `skills/...`) MUST stay byte-identical.

**Scale/Scope**: 1 agent definition, 1 SKILL.md (2 synced copies), 1 cost-calculator template; 10 functional requirements, 5 user stories, 6 success criteria.

## Constitution Check

*GATE: Must pass before Phase 0 research. Re-check after Phase 1 design.*

| # | Principle / Domain | Requirement | Status |
|---|--------------------|-------------|--------|
| I | Documentation-First Advisory | All hub-spoke guidance cites official Microsoft Learn / Azure Architecture Center sources (research.md Decision 1 anchors to the canonical hub-spoke reference architecture). | PASS |
| II | MCP-Driven Context | Pricing sourced live via `mcp_azure_mcp_pricing`; docs via Microsoft Learn MCP; graceful degradation documented. | PASS |
| III | Multi-Modal Output | Mermaid diagrams (architecture), Excel cost workbook (Domain 6 template), markdown tables (decision matrix). | PASS |
| IV | Well-Architected Alignment | Hub-spoke guidance maps to WAF pillars (Security: centralized Firewall/NSG; Cost: shared-resource savings; Reliability: isolation; Operational Excellence: centralized management). | PASS |
| V | Actionable Deliverables | Every response yields a summary recommendation + diagram/table + explicit next steps (quickstart scenarios enforce this). | PASS |
| D6 | Cost Calculator (region + 6R + hybrid + AHB) | Cost analysis is region-parameterized (asks region, defaults East US 2 as editable assumption), exposes editable assumptions, and reuses the constitution-compliant cost-calculator template. | PASS |

**Result**: No violations. All gates pass before Phase 0 and remain passing after Phase 1 design (artifacts already reflect the latest clarifications). Complexity Tracking section intentionally left empty.

## Project Structure

### Documentation (this feature)

```text
specs/[###-feature]/
├── plan.md              # This file (/speckit.plan command output)
├── research.md          # Phase 0 output (/speckit.plan command)
├── data-model.md        # Phase 1 output (/speckit.plan command)
├── quickstart.md        # Phase 1 output (/speckit.plan command)
├── contracts/           # Phase 1 output (/speckit.plan command)
└── tasks.md             # Phase 2 output (/speckit.tasks command - NOT created by /speckit.plan)
```

### Source Code (repository root)

```text
.github/
├── skills/
│   └── microsoft-cloud-advisor/
│       └── SKILL.md            # Primary skill procedure (Step 6 cost + hub-spoke domain)
└── chatmodes/ or agents/
    └── cloud-advisor.agent.md  # Agent definition with hub-spoke section (US7)

skills/
└── microsoft-cloud-advisor/
    └── SKILL.md                # Public byte-identical copy (synced LAST)

templates/
└── cost-calculator-template.md # Excel cost-calculator structure (region/6R/hybrid/AHB)
```

**Structure Decision**: Single-project VS Code agent-customization layout. The feature is implemented entirely through declarative markdown/YAML artifacts — no application source tree or test harness. The two SKILL.md copies MUST remain byte-identical (sync the public `skills/` copy last and verify with grep).

## Complexity Tracking

> **Fill ONLY if Constitution Check has violations that must be justified**

No constitutional violations. This feature is delivered through declarative agent-customization artifacts that reuse the existing constitution-compliant cost-calculator template and MCP-driven pricing; no additional projects, abstractions, or deviations are introduced.

## Phase 0 & Phase 1 Artifacts

| Artifact | Path | Status |
|----------|------|--------|
| Research | [research.md](research.md) | Complete \u2014 all NEEDS CLARIFICATION resolved |
| Data Model | [data-model.md](data-model.md) | Complete \u2014 HubVNet, SpokeVNet, VNetPeering, SharedHubResources |
| Contracts | [contracts/agent-response-contracts.md](contracts/agent-response-contracts.md) | Complete \u2014 agent response shapes |
| Quickstart | [quickstart.md](quickstart.md) | Complete \u2014 6 validation scenarios |

All design artifacts reflect the latest clarifications (peering data-transfer default, pricing region default, Virtual WAN cost-comparison scope).
