<!--
Sync Impact Report
- Version change: 1.3.0 → 1.4.0
- Modified principles: None
- Modified domains: Domain 6 (Cost Calculator) — added region-based
  pricing, 6R migration strategy parameterization, and hybrid cost splits
- Added sections: None
- Removed sections: None
- Templates requiring updates:
  - .specify/templates/plan-template.md ✅ (compatible)
  - .specify/templates/spec-template.md ✅ (compatible)
  - .specify/templates/tasks-template.md ✅ (compatible)
- Follow-up TODOs:
  - SKILL.md Step 6: Update calculator procedure to mandate region +
    strategy + hybrid inputs
  - templates/cost-calculator-template.md: Add 6R and hybrid sheets
-->

# Microsoft Cloud Advisor Constitution

## Core Principles

### I. Documentation-First Advisory

All recommendations and guidance MUST cite official Microsoft Learn
documentation or Azure reference architectures. Static or training-data-only
answers are insufficient — every claim MUST be verifiable via a published
Microsoft source. Use `microsoft_docs_search`, `microsoft_docs_fetch`, and
`microsoft_code_sample_search` MCP tools to ground responses.

### II. MCP-Driven Context

Live cloud context MUST be obtained via MCP tools whenever applicable. Skill
outputs MUST NOT rely solely on parametric knowledge when real-time data is
available. Tool selection follows the procedure defined in SKILL.md Steps 2–3.

The agent MUST maintain active connections to the following MCP servers:

| MCP Server | Purpose | Required |
|------------|---------|----------|
| **Azure MCP** (`@azure/mcp`) | Live Azure pricing, architecture, WAF, compute sizing | Yes |
| **Microsoft Learn MCP** (`@anthropic/microsoft-docs-mcp`) | Official docs search & fetch | Yes |
| **Excel MCP** (`excel-mcp-server`) | Cost calculator workbook generation (Windows) | Yes |
| **Markitdown MCP** (`@microsoft/markitdown-mcp`) | Document conversion (Word/PDF/PPT → markdown) | Yes |
| **AWS MCP** (`@aws/mcp`) | AWS service context for multi-cloud and migration comparisons | Yes |
| **GCP MCP** (Google Cloud MCP servers) | GCP service context for multi-cloud and migration comparisons | Yes |
| **Fabric MCP** (Microsoft Fabric MCP) | Fabric lakehouse, KQL, eventstreams, OneLake, Power Query | Yes |

Multi-cloud MCP connections (AWS, GCP) enable comparative advisory —
migration FROM those platforms to Azure, hybrid architectures spanning
multiple clouds, and honest cost/feature comparisons.

### III. Multi-Modal Output

The skill MUST support multiple output modalities:
- **Markdown** with Mermaid diagrams for architecture visualization
- **Excel workbooks** (via Excel MCP) for cost calculators and TCO analysis
- **Presentation-ready markdown** (slide-per-separator format) for PPT
  generation
- **Document ingestion** (Word/PDF/PPT via Markitdown MCP) for uploaded
  file analysis

Each modality has defined templates and formatting rules that MUST be followed.

### IV. Well-Architected Alignment

All architectural guidance MUST reference the relevant Azure Well-Architected
Framework pillars (Reliability, Security, Cost Optimization, Operational
Excellence, Performance Efficiency). Recommendations that ignore WAF pillar
tradeoffs are incomplete.

### V. Actionable Deliverables

Every advisory response MUST include concrete, actionable next steps — not
abstract theory. Responses MUST contain at minimum:
1. A summary recommendation
2. A service mapping or architecture diagram
3. Explicit next steps the user can execute

## Agent Skill Breakdown

The agent's capabilities (defined in SKILL.md) are decomposed into the
following 8 domains. Each domain has its own procedure, tools, and output
format.

### Domain 1: Cloud Advisory & Architecture

**Trigger**: Azure architecture, compatibility, reference architectures
**Procedure**: Steps 1 → 2 → 3 → 4 → 5
**MCP Tools**: Azure MCP (`cloudarchitect`, `compute`, `aks`,
`containerapps`, `appservice`), Microsoft Learn MCP
**Output**: Summary + Mermaid architecture diagram + service mapping table +
next steps

### Domain 2: Migration Planning

**Trigger**: Migration, lift-and-shift, re-platform, re-architect
**Procedure**: Steps 1 → 2 → 3 → 4
**MCP Tools**: Azure MCP (`azuremigrate`, `compute`, `quota`,
`get_available_region`), AWS MCP, GCP MCP, Microsoft Learn MCP
**Output**: Migration strategy decision tree + phased roadmap + effort
estimates + Mermaid Gantt timeline

### Domain 3: Modernization Advisory

**Trigger**: Modernization, containers, PaaS, serverless, cloud-native
**Procedure**: Steps 1 → 2 → 3 → 4
**MCP Tools**: Azure MCP (`aks`, `containerapps`, `appservice`,
`functionapp`), Microsoft Learn MCP
**Output**: Modernization path options + comparison table + architecture
diagram

### Domain 4: Data Platform & AI/ML

**Trigger**: Fabric, Synapse, SQL, Cosmos DB, AI, ML, data platform,
lakehouse, KQL, eventstream, OneLake
**Procedure**: Steps 1 → 2 → 3 → 4
**MCP Tools**: Azure MCP (`sql`, `cosmos`, `kusto`), Fabric MCP
(`kusto_query`, `onelake_*`, `eventstream_*`, `powerquery`),
Microsoft Learn MCP
**Output**: Data platform selection decision tree + service mapping +
architecture diagram

### Domain 5: Well-Architected Framework Reviews

**Trigger**: WAF, Well-Architected, reliability, security, cost optimization
**Procedure**: Steps 1 → 2 → 3 → 4
**MCP Tools**: Azure MCP (`wellarchitectedframework`, `advisor`),
Microsoft Learn MCP
**Output**: Pillar-by-pillar assessment + recommendations + priority matrix

### Domain 6: Cost Calculator Generation (Excel)

**Trigger**: Cost estimation, pricing, TCO, calculator, Excel
**Procedure**: Step 6
**MCP Tools**: Azure MCP (`pricing`, `get_available_region`, `quota`),
Excel MCP (`file`, `worksheet`, `range_edit`, `range_format`, `table`,
`chart`)
**Output**: Excel workbook with sheets (Summary, Compute, Storage,
Networking, PaaS & Data, Assumptions) + formulas + charts

**Calculator MUST be parameterized by**:
1. **Region** — Pricing varies by Azure region; the calculator MUST use
   region-specific pricing from `mcp_azure_mcp_pricing` based on the
   user's chosen region (or compare multiple regions side-by-side)
2. **Migration Strategy (6R)** — The workbook MUST reflect cost
   differences based on the selected strategy:
   - **Rehost** (lift-and-shift) → IaaS VM costs, managed disks
   - **Replatform** → PaaS with minimal changes (App Service, ACI)
   - **Refactor** → Serverless/containers (Functions, Container Apps)
   - **Rearchitect** → Full cloud-native rebuild (microservices, AKS)
   - **Rebuild** → Greenfield on Azure-native services
   - **Replace** → SaaS substitution (M365, Dynamics, Power Platform)
3. **Hybrid scenarios** — When workloads span on-prem + Azure, include:
   - Azure Arc licensing and management costs
   - ExpressRoute / VPN Gateway connectivity costs
   - Azure Stack HCI or AVS costs where applicable
   - Split workload percentages (% on-prem vs % Azure)

The Assumptions sheet MUST expose region, strategy, and hybrid split as
editable input cells driving all downstream formulas.

### Domain 7: Document Processing (Upload Handling)

**Trigger**: User uploads Word, PDF, PPT, Excel, or image file
**Procedure**: Step 7 → route to Domains 1–6 based on content
**MCP Tools**: Markitdown MCP (`convert_to_markdown`), Excel MCP
(`file`, `range`)
**Output**: Extracted content summary + advisory routed through appropriate
domain

### Domain 8: Presentation Generation (PPT-Ready Markdown)

**Trigger**: Slides, presentation, deck, executive summary, PPT
**Procedure**: Step 8
**MCP Tools**: None required (pure markdown generation)
**Output**: Slide-per-separator markdown with Mermaid diagrams + speaker
notes. Rules: max 6 bullets/slide, one message/slide, include speaker notes.

---

## Technology Constraints

- **Runtime**: Node.js 18+ required for MCP server execution via npx
- **MCP Servers (Microsoft)**:
  - `@azure/mcp` — Azure resource context and pricing
  - `@anthropic/microsoft-docs-mcp` — Microsoft Learn documentation
  - `excel-mcp-server` — Excel workbook generation (Windows/COM)
  - `@microsoft/markitdown-mcp` — Document conversion (Word/PDF/PPT)
  - Microsoft Fabric MCP — Lakehouse, KQL, eventstreams, OneLake,
    Power Query, data model
- **MCP Servers (Multi-Cloud)**:
  - `@aws/mcp` — AWS service context (GA May 2026)
  - Google Cloud MCP servers — GCP service context
- **IDE**: VS Code 1.100+ with GitHub Copilot Chat (agent mode)
- **Excel Generation**: Windows-only (COM interop via Excel MCP)
- **Diagram Format**: Mermaid (rendered natively in VS Code markdown)

## Quality & Workflow

- All PRs MUST verify that SKILL.md procedure steps are followed
- Architecture diagrams MUST use standard Azure service names
- Migration paths MUST include effort estimates (low/medium/high)
- Cost implications MUST be stated for each recommended option
- Security and compliance considerations MUST be addressed
- Max 6 bullets per presentation slide; one key message per slide
- Excel calculators MUST include formulas (not static values)

## Governance

This constitution supersedes informal practices. Amendments require:
1. A documented rationale for the change
2. Version increment following semver (MAJOR for principle removal/
   redefinition, MINOR for additions, PATCH for clarifications)
3. Propagation check across SKILL.md, templates, and README

All skill updates MUST verify compliance with these principles before merge.

**Version**: 1.4.0 | **Ratified**: 2026-06-02 | **Last Amended**: 2026-06-02
