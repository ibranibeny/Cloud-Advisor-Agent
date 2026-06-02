# Implementation Plan: Cloud Advisory Custom Agent

**Branch**: `001-cloud-advisory` | **Date**: 2026-06-02 | **Spec**: [spec.md](spec.md)

**Input**: Feature specification from `specs/001-cloud-advisory/spec.md`

## Summary

Build a VS Code custom agent (`.agent.md`) named "microsoft-cloud-advisor" that provides Azure architecture advisory, migration planning, cost calculator generation, multi-cloud comparison, document analysis, data platform guidance, WAF reviews, and presentation output — powered by 7 MCP server connections, with user-driven L-level depth and 6R strategy selection.

## Technical Context

**Language/Version**: Markdown/YAML (agent definition), Node.js 18+ (MCP runtime)

**Primary Dependencies**: 7 MCP servers (@azure/mcp, @anthropic/microsoft-docs-mcp, excel-mcp-server, @microsoft/markitdown-mcp, @aws/mcp, Google Cloud MCP, Microsoft Fabric MCP)

**Storage**: N/A (stateless agent — no persistent storage beyond conversation context)

**Testing**: Manual acceptance testing via Copilot Chat interactions; checklist-based validation

**Target Platform**: VS Code 1.100+ (Windows for Excel MCP; macOS/Linux for all other capabilities)

**Project Type**: VS Code custom agent (`.agent.md` + `.vscode/mcp.json` + skill/template files)

**Performance Goals**: N/A (response quality over speed; bounded by MCP server latency)

**Constraints**: Excel generation Windows-only (COM interop); MCP servers require network access; agent mode requires GitHub Copilot Chat

**Scale/Scope**: 8 capability domains, 17 functional requirements, single-user VS Code agent

## Constitution Check

*GATE: Must pass before Phase 0 research. Re-check after Phase 1 design.*

| Principle | Status | Notes |
|-----------|--------|-------|
| I. Documentation-First Advisory | ✅ PASS | Agent instructions mandate Microsoft Learn MCP citation in all responses |
| II. MCP-Driven Context | ✅ PASS | All 7 required MCP servers defined in Technical Context; .vscode/mcp.json will configure them |
| III. Multi-Modal Output | ✅ PASS | Plan covers Markdown+Mermaid, Excel workbooks, presentation markdown, document ingestion |
| IV. Well-Architected Alignment | ✅ PASS | FR-007 requires WAF pillar references in all architecture guidance |
| V. Actionable Deliverables | ✅ PASS | Every domain procedure ends with numbered next steps (per SKILL.md) |

**Gate Result**: PASS — no violations. Proceeding to Phase 0.

## Project Structure

### Documentation (this feature)

```text
specs/001-cloud-advisory/
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
├── agents/
│   └── microsoft-cloud-advisor.agent.md   # Primary agent definition
├── copilot-instructions.md                 # Agent context reference
└── skills/
    └── microsoft-cloud-advisor/
        └── SKILL.md                        # Skill procedure (existing)

.vscode/
└── mcp.json                                # MCP server configuration (7 servers)

templates/
├── cost-calculator-template.md             # Excel calculator sheet structure
└── ppt-slide-templates.md                  # Presentation formatting rules

skills/
└── microsoft-cloud-advisor/
    └── SKILL.md                            # Root-level skill copy (existing)
```

**Structure Decision**: This is a VS Code custom agent project — no compiled source code. The deliverables are configuration files (`.agent.md`, `mcp.json`), instruction files (SKILL.md), and templates (cost calculator, presentation). No src/ or tests/ directories needed.

## Complexity Tracking

No constitution violations. Table not applicable.
