# Agent Interface Contract: microsoft-cloud-advisor

**Phase**: 1 — Design & Contracts  
**Date**: 2026-06-02

## Overview

This contract defines the interaction interface between users and the
microsoft-cloud-advisor agent in VS Code Copilot Chat.

## Invocation

| Method | Format |
|--------|--------|
| Direct mention | `@microsoft-cloud-advisor <request>` |
| Agent mode routing | Automatic via trigger keywords in description |

## Input Contract

### Required First-Turn Inputs

The agent MUST collect these before generating advisory:

1. **Depth Level** (L100–L400)
   - Agent presents options table
   - User selects one
   - Default: L200 if no response

2. **6R Strategy** (migration domain only)
   - Agent presents 6 options with descriptions
   - User selects one
   - Required before generating migration plan or cost calculator

### Optional Inputs (detected from context)

| Input | Detection | Default |
|-------|-----------|---------|
| Azure region | User mentions region name | None (ask if needed for cost) |
| Hybrid split % | User mentions partial migration | 0% (full cloud) |
| Uploaded file | File attachment present | None |
| Comparison targets | "vs AWS", "vs GCP" mentioned | Azure-only |

## Output Contract

### Architecture Advisory Response

```yaml
structure:
  - summary: string (1 paragraph)
  - diagram: mermaid (architecture visualization)
  - service_mapping: table (requirement → Azure service → SKU)
  - waf_considerations: table (pillar → tradeoff → recommendation)
  - next_steps: list (numbered, actionable)
  - citations: list (Microsoft Learn URLs)
constraints:
  - L100: summary + 1 diagram + 3-5 benefit bullets + next steps
  - L200: full structure above
  - L300: full structure + networking detail + SKU justification + config snippets
  - L400: full structure + internals + edge cases + performance tuning + CLI commands
```

### Migration Plan Response

```yaml
structure:
  - strategy_rationale: string (why this 6R option)
  - phases: list (name, duration, effort, milestones)
  - gantt: mermaid (timeline diagram)
  - cost_reference: link (to generated Excel calculator)
  - next_steps: list (numbered, actionable)
constraints:
  - Each phase has effort estimate (low/medium/high)
  - Gantt uses dateFormat YYYY-MM
  - L100: phases only, no Gantt
  - L200+: full structure
```

### Cost Calculator Response

```yaml
structure:
  - confirmation: string ("Generated Excel workbook at: {path}")
  - summary_table: table (category, monthly, annual)
  - assumptions: list (region, strategy, hybrid %, growth rate)
constraints:
  - Excel file generated via Excel MCP (not inline markdown)
  - Formulas in cells (not static values)
  - Assumptions sheet with editable inputs
  - Charts on Summary sheet
```

### Multi-Cloud Comparison Response

```yaml
structure:
  - comparison_table: table (criteria × providers)
  - recommendation: string (with rationale)
  - citations: list (per-provider documentation URLs)
constraints:
  - Minimum 3 comparison criteria (cost, features, effort)
  - Data sourced from respective MCP servers
  - Honest assessment (may recommend non-Azure where appropriate)
```

### Presentation Response

```yaml
structure:
  - slides: markdown (separated by ---)
  - each_slide:
      - title: h1
      - content: bullets/table/diagram
      - speaker_notes: blockquote
constraints:
  - Max 6 bullets per slide
  - One key message per slide
  - Speaker notes on every slide
  - Mermaid diagrams where visual adds value
```

### Document Processing Response

```yaml
structure:
  - extraction_summary: string (what was found in document)
  - classification: string (architecture/migration/modernization/data/other)
  - advisory: <delegated to appropriate domain output contract>
constraints:
  - Single conversation turn (no multi-step extraction)
  - Supported formats: Word, PDF, PPT, Excel, images
  - Unsupported formats: reject with guidance listing supported formats
```

## Error Contract

### MCP Server Unavailable

```yaml
behavior: graceful_degradation
response:
  - warning: "⚠️ {server_name} is currently unavailable. {capability} is limited."
  - continue: Use remaining available tools
  - suggestion: "Run `npx -y {package}@latest` to install, then reload VS Code."
```

### All MCP Servers Unavailable

```yaml
behavior: full_health_check
response:
  - error: "⚠️ MCP servers are not running."
  - instructions: Full installation guide with npx commands
  - action: "After configuring, reload VS Code"
```

### Region Service Gap

```yaml
behavior: suggest_alternative
response:
  - warning: "{service} is not available in {region}."
  - suggestion: "Nearest region with full support: {alt_region}"
  - tradeoffs: table (latency, cost, compliance implications)
```

## Tool Access

The agent MUST have access to these tool families:

```yaml
tools:
  - read          # Workspace file access
  - search        # Workspace search
  - web           # Web fetch for pricing pages
  - edit          # File creation (Excel output path)
  - execute       # Terminal for npx commands
  - agent         # Sub-agent delegation
  - todo          # Task tracking
  - azure/*       # Azure MCP (all tools)
  - microsoft-lea/* # Microsoft Learn MCP
  - excel-mcp/*   # Excel MCP (Windows)
  - microsoft_mar/* # Markitdown MCP
  # Pending addition:
  # - aws/*       # AWS MCP
  # - gcp/*       # GCP MCP
  # - fabric/*    # Fabric MCP (or bundled via azure/*)
```
