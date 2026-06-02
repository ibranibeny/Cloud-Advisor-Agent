# Feature Specification: Cloud Advisory Custom Agent

**Feature Branch**: `001-cloud-advisory`

**Created**: 2026-06-02

**Status**: Draft

**Input**: User description: "Build custom agent with name cloud advisory"

## User Scenarios & Testing *(mandatory)*

### User Story 1 - Azure Architecture Advisory (Priority: P1)

A cloud architect asks the agent for guidance on designing an Azure architecture for a specific workload (e.g., "I need to host a .NET web app with SQL backend for 10,000 users"). The agent researches Microsoft Learn documentation, queries live Azure pricing and availability, and returns a structured recommendation with architecture diagram, service mapping, and next steps.

**Why this priority**: This is the core value proposition — providing authoritative Azure architecture guidance grounded in official documentation and live context. Every other capability builds on this foundation.

**Independent Test**: Can be fully tested by asking "What Azure architecture do you recommend for a 3-tier web application serving 5,000 concurrent users?" and verifying the response includes a Mermaid diagram, service mapping table, WAF considerations, and actionable next steps.

**Acceptance Scenarios**:

1. **Given** a user describes a workload, **When** they ask for Azure architecture guidance, **Then** the agent returns a summary recommendation, Mermaid architecture diagram, service mapping table, and numbered next steps
2. **Given** a user asks about Azure compatibility, **When** the agent researches via Microsoft Learn MCP and Azure MCP, **Then** all recommendations cite official Microsoft documentation URLs
3. **Given** any architecture recommendation, **When** the agent presents it, **Then** it references at least one Well-Architected Framework pillar tradeoff

---

### User Story 2 - Migration Planning with Cost Calculator (Priority: P2)

An IT manager wants to migrate on-premises workloads to Azure. The agent determines the appropriate migration strategy (using the 6R model), generates a phased migration roadmap, and produces an Excel cost calculator parameterized by the user's chosen Azure region, migration strategy, and hybrid split percentage.

**Why this priority**: Migration planning with cost visibility is the second most requested capability. It combines multiple domains (migration + cost calculation) into a high-value deliverable that directly supports business decisions.

**Independent Test**: Can be fully tested by saying "Help me migrate 20 Windows Server VMs from our datacenter to Azure East US using a lift-and-shift approach" and verifying the agent produces a migration roadmap AND an Excel workbook with region-specific pricing.

**Acceptance Scenarios**:

1. **Given** a user describes on-premises workloads, **When** they ask for migration planning, **Then** the agent applies the 6R decision tree and recommends a strategy with rationale
2. **Given** a selected region and migration strategy, **When** the agent generates an Excel calculator, **Then** the workbook contains region-specific pricing, strategy-appropriate cost line items, and editable assumption cells
3. **Given** a hybrid scenario (partial migration), **When** the user specifies on-prem/Azure split, **Then** the calculator includes Azure Arc, ExpressRoute/VPN, and split-percentage formulas
4. **Given** a migration plan, **When** presented to the user, **Then** it includes a Mermaid Gantt timeline with phased milestones and effort estimates (low/medium/high)

---

### User Story 3 - Multi-Cloud Comparison Advisory (Priority: P3)

A decision-maker evaluating cloud providers asks the agent to compare Azure against AWS or GCP for a specific workload. The agent uses Azure MCP, AWS MCP, and GCP MCP to gather live service information and provides an honest feature/cost comparison with a recommendation.

**Why this priority**: Multi-cloud comparison addresses competitive scenarios and validates Azure as the best choice (or identifies where hybrid multi-cloud makes sense). It differentiates this agent from single-cloud tools.

**Independent Test**: Can be fully tested by asking "Compare Azure Kubernetes Service vs AWS EKS vs Google GKE for running 50 microservices" and verifying a comparison table with pricing, features, and a recommendation.

**Acceptance Scenarios**:

1. **Given** a user asks to compare Azure with AWS or GCP, **When** the agent queries multi-cloud MCP tools, **Then** it returns a structured comparison table with criteria (cost, features, effort, risk)
2. **Given** a comparison request, **When** the agent provides a recommendation, **Then** it explains the rationale citing specific service capabilities and pricing differences

---

### User Story 4 - Document Upload and Advisory (Priority: P4)

A user uploads an existing architecture document (Word, PDF, or PowerPoint) describing their current environment. The agent extracts the content, classifies the intent (assessment, migration, modernization), and provides tailored Azure advisory based on the document contents.

**Why this priority**: Document processing unlocks enterprise scenarios where customers have existing documentation they want analyzed. It acts as a force multiplier for all other domains.

**Independent Test**: Can be fully tested by uploading a Word document containing a network topology description and verifying the agent extracts key details and provides migration recommendations.

**Acceptance Scenarios**:

1. **Given** a user uploads a Word/PDF/PPT file, **When** the agent processes it via Markitdown MCP, **Then** it extracts the content into markdown and summarizes the key findings
2. **Given** extracted content describes an existing architecture, **When** the agent classifies the intent, **Then** it routes to the appropriate domain (architecture, migration, modernization, or data platform) and provides domain-specific advisory

---

### User Story 5 - Presentation Generation (Priority: P5)

A consultant needs to create a PowerPoint deck summarizing Azure advisory findings. The agent generates slide-ready markdown with proper formatting (one message per slide, max 6 bullets, Mermaid diagrams, speaker notes) that can be converted to a presentation.

**Why this priority**: Presentation output is a delivery format that wraps other domains' outputs into stakeholder-ready content. It depends on other domains being functional first.

**Independent Test**: Can be fully tested by asking "Generate a 5-slide executive summary for migrating our SQL Server estate to Azure SQL" and verifying proper slide-separator formatting with speaker notes.

**Acceptance Scenarios**:

1. **Given** a user requests a presentation, **When** the agent generates PPT markdown, **Then** each slide is separated by `---`, has max 6 bullets, one key message, and includes speaker notes
2. **Given** an architecture recommendation exists, **When** converted to presentation format, **Then** it includes a Mermaid architecture diagram slide and a cost summary slide

---

### User Story 6 - Data Platform Advisory with Fabric (Priority: P6)

A data engineer asks for guidance on building a unified analytics platform. The agent uses Fabric MCP to query OneLake, eventstreams, and KQL databases, and provides a data platform architecture recommendation including Fabric lakehouse patterns.

**Why this priority**: Data platform advisory with live Fabric context is a specialized capability that serves a growing user base as Microsoft Fabric adoption increases.

**Independent Test**: Can be fully tested by asking "Design a lakehouse architecture for our 50TB data warehouse migration from on-premises SQL Server" and verifying Fabric-specific recommendations with architecture diagrams.

**Acceptance Scenarios**:

1. **Given** a user asks about data platform design, **When** the agent queries Fabric MCP tools, **Then** it provides a data platform selection decision tree and Fabric-specific architecture diagram
2. **Given** a Fabric recommendation, **When** presented to the user, **Then** it includes lakehouse patterns, eventstream integration points, and KQL query examples where relevant

---

### Edge Cases

- When an MCP server is unavailable, the agent gracefully degrades: it uses remaining available tools, informs the user which capability is limited, and notes limitations in the output
- When a request spans multiple domains, the agent chains them sequentially in dependency order (e.g., migration → cost calculator → presentation), producing a combined response where each domain's output feeds the next
- When a user uploads an unsupported format, the agent rejects with guidance: lists supported formats (Word, PDF, PPT, Excel, images) and suggests the user convert or paste content as text
- When a user's chosen Azure region lacks a required service, the agent informs the user, suggests the nearest region with full support, and shows cost/latency tradeoffs
- When AWS or GCP MCP servers return incomplete data for comparison, the agent fills available cells in the comparison table, marks missing data as "Data unavailable from [provider]", and notes the limitation in the recommendation summary

## Requirements *(mandatory)*

### Functional Requirements

- **FR-001**: Agent MUST connect to all 7 required MCP servers (Azure, Microsoft Learn, Excel, Markitdown, AWS, GCP, Fabric) at startup
- **FR-002**: Agent MUST classify user requests into one or more of 8 capability domains and route to the appropriate procedure
- **FR-003**: Agent MUST cite official Microsoft Learn documentation URLs in all architectural recommendations
- **FR-004**: Agent MUST generate Mermaid diagrams for architecture visualization
- **FR-005**: Agent MUST produce Excel workbooks via Excel MCP for cost estimation requests, with region-specific pricing and editable assumption cells
- **FR-006**: Agent MUST present all 6R migration options (Rehost, Replatform, Refactor, Rearchitect, Rebuild, Replace) and ask the user to select a strategy before generating migration plans or cost calculators
- **FR-007**: Agent MUST reference Well-Architected Framework pillars in all architecture guidance
- **FR-008**: Agent MUST extract content from uploaded Word/PDF/PPT files via Markitdown MCP and route to appropriate advisory domain
- **FR-009**: Agent MUST generate slide-ready markdown (max 6 bullets/slide, one message/slide, speaker notes) for presentation requests
- **FR-010**: Agent MUST query AWS MCP and GCP MCP for multi-cloud comparison requests
- **FR-011**: Agent MUST query Fabric MCP for data platform scenarios involving lakehouse, KQL, eventstreams, or OneLake
- **FR-012**: Agent MUST include effort estimates (low/medium/high) in all migration path recommendations
- **FR-013**: Agent MUST include cost implications for each recommended option
- **FR-014**: Agent MUST include security and compliance considerations in all advisory responses
- **FR-015**: Cost calculator Excel workbooks MUST parameterize by region, 6R strategy, and hybrid split percentage
- **FR-016**: Agent MUST default to comprehensive responses (all required output elements) but adapt depth when user explicitly signals preference (e.g., "brief", "quick summary", "deep dive")
- **FR-017**: Agent MUST prompt user to select a depth level (L100, L200, L300, or L400) before generating advisory output, where: L100 = executive overview (no jargon), L200 = IT decision-maker (service names, no config), L300 = architect (sizing, SKUs, networking), L400 = expert deep-dive (CLI, ARM/Bicep, implementation details)

### Key Entities

- **Advisory Request**: A user's question or scenario requiring cloud guidance — classified by domain (architecture, migration, modernization, data, WAF, cost, document, presentation)
- **Architecture Recommendation**: A structured response containing summary, Mermaid diagram, service mapping, considerations, and next steps
- **Cost Calculator**: An Excel workbook with parameterized pricing across Compute, Storage, Networking, PaaS, and Assumptions sheets
- **Migration Plan**: A phased roadmap with 6R strategy selection, timeline (Gantt), effort estimates, and cost projection
- **Presentation Deck**: Slide-ready markdown output formatted for PowerPoint conversion

## Success Criteria *(mandatory)*

### Measurable Outcomes

- **SC-001**: Users receive architecture advisory responses that include all 4 required elements (summary, diagram, service mapping, next steps) in 100% of architecture requests
- **SC-002**: Generated Excel cost calculators contain working formulas (not static values) that update when assumption cells are modified
- **SC-003**: All advisory responses cite at least one official Microsoft Learn documentation URL
- **SC-004**: Migration plans include a Gantt timeline and effort estimates for each phase
- **SC-005**: Multi-cloud comparisons include at least 3 comparison criteria with data from the respective cloud MCP servers
- **SC-006**: Uploaded documents are successfully extracted and advisory provided within a single conversation turn
- **SC-007**: Presentation markdown passes formatting validation (slide separators, max 6 bullets, speaker notes present)
- **SC-008**: Cost calculators produce different cost outputs when region, strategy, or hybrid split inputs are changed

## Clarifications

### Session 2026-06-02

- Q: When an MCP server is unavailable, how should the agent behave? → A: Graceful degradation — use available tools and note limitations in output
- Q: When a request spans multiple domains, how should the agent route? → A: Chain sequentially in dependency order, producing combined output
- Q: How should the agent determine response depth/length? → A: Adaptive — default comprehensive, but respect user signals ("brief", "quick", "deep dive") to adjust
- Q: How should the agent handle unsupported document formats? → A: Reject with guidance — list supported formats (Word, PDF, PPT, Excel, images) and suggest alternatives
- Q: When a user's chosen Azure region lacks required services, how should the agent respond? → A: Suggest alternative region — recommend nearest region with full support and show cost/latency tradeoffs
- Q: Should the agent ask users for output depth level (L100–L400) before generating advisory? → A: Always ask — prompt for L-level (L100 executive, L200 IT decision-maker, L300 architect, L400 expert) before generating any advisory output
- Q: How should the agent determine the 6R migration strategy? → A: Always ask first — present all 6 options (Rehost, Replatform, Refactor, Rearchitect, Rebuild, Replace) and ask the user to pick before proceeding

## Assumptions

- Users have VS Code 1.100+ with GitHub Copilot Chat extension installed in agent mode
- Node.js 18+ is available on the user's machine for MCP server execution
- Excel MCP functionality (cost calculators) is Windows-only due to COM interop requirement
- All 7 MCP servers are accessible and properly configured in `.vscode/mcp.json`
- Users interact with the agent via natural language in the Copilot Chat panel
- The agent operates as a GitHub Copilot custom agent (`.agent.md` file format)
- AWS MCP (`@aws/mcp`) is generally available as of May 2026
- Google Cloud MCP servers are available per Google Cloud documentation
- Microsoft Fabric MCP is accessible for lakehouse, KQL, eventstream, and OneLake operations
