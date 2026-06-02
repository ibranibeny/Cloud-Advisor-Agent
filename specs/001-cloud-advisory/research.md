# Research: Cloud Advisory Custom Agent

**Phase**: 0 — Outline & Research  
**Date**: 2026-06-02  
**Feature**: [spec.md](spec.md)

## Research Tasks

### 1. VS Code Custom Agent Format (`.agent.md`)

**Decision**: Use `.agent.md` file in `.github/agents/` directory

**Rationale**: 
- VS Code Copilot Chat recognizes `.agent.md` files as custom agent definitions
- YAML frontmatter defines metadata: `description`, `tools`, `model`, `argument-hint`
- Agent body is markdown with instructions, constraints, and procedure
- Already have a working agent at `.github/agents/microsoft-cloud-advisor.agent.md`

**Alternatives considered**:
- `.prompt.md` — lacks agent-level tool access and model pinning
- Custom VS Code extension — overkill for advisory skill; agent format sufficient
- External server — unnecessary; Copilot Chat's agent mode handles orchestration

### 2. MCP Server Configuration (`.vscode/mcp.json`)

**Decision**: Define all 7 MCP servers in `.vscode/mcp.json` using npx commands

**Rationale**:
- VS Code reads `.vscode/mcp.json` to discover and launch MCP servers
- npx ensures latest versions without global installs
- Each server maps to a tool family prefix (e.g., `azure/*`, `excel-mcp/*`)

**Current state** (4 servers configured in agent):
- `@azure/mcp` → `azure/*` tools
- `@anthropic/microsoft-docs-mcp` → `microsoft-lea/*` tools
- `excel-mcp-server` → `excel-mcp/*` tools
- `@microsoft/markitdown-mcp` → `microsoft_mar/*` tools

**Gap** (3 servers need addition):
- AWS MCP (`@aws/mcp`) → multi-cloud comparison
- GCP MCP (Google Cloud MCP) → multi-cloud comparison
- Fabric MCP (Microsoft Fabric) → data platform advisory

**Alternatives considered**:
- Docker-based MCP servers — adds friction, npx is simpler for dev machines
- Remote MCP endpoints — latency issues; local npx preferred

### 3. AWS MCP Integration

**Decision**: Add `@aws/mcp` to mcp.json for multi-cloud comparison capability

**Rationale**:
- AWS MCP provides service catalog, pricing, and architecture context
- Enables honest Azure-vs-AWS comparisons grounded in live data
- Required by FR-010 and User Story 3

**Integration pattern**:
- Tool prefix: TBD (depends on MCP server naming at install time)
- Use cases: service comparison tables, pricing lookups, migration source context
- Graceful degradation: if unavailable, agent notes limitation and provides Azure-only advisory

**Alternatives considered**:
- Web scraping AWS docs — unreliable, not structured
- Static pricing tables — stale data, maintenance burden

### 4. GCP MCP Integration

**Decision**: Add Google Cloud MCP servers to mcp.json for multi-cloud comparison

**Rationale**:
- GCP MCP provides service catalog and architecture context
- Enables Azure-vs-GCP comparisons
- Required by FR-010 and User Story 3

**Integration pattern**:
- Tool prefix: TBD (depends on Google Cloud MCP server naming)
- Use cases: GKE vs AKS, BigQuery vs Fabric, Cloud Run vs Container Apps
- Graceful degradation: if unavailable, agent notes limitation

**Alternatives considered**:
- Google Cloud CLI output parsing — fragile, requires auth
- Static documentation references — insufficient for live pricing

### 5. Fabric MCP Integration

**Decision**: Use Microsoft Fabric MCP for data platform advisory

**Rationale**:
- Fabric MCP exposes OneLake, KQL, eventstreams, Power Query, and data model tools
- Already available in workspace (tools prefixed `mcp_microsoft_fab_*`)
- Required by FR-011 and User Story 6

**Integration pattern**:
- Tool families: `kusto_query`, `onelake_*`, `eventstream_*`, `powerquery`
- Use cases: lakehouse architecture, KQL query examples, eventstream integration
- Agent instruction: query Fabric MCP when user asks about unified analytics or data platform

**Alternatives considered**:
- Direct REST API calls — MCP abstraction is cleaner
- Synapse-only advisory — Fabric is the strategic direction

### 6. L-Level Depth Prompting

**Decision**: Agent prompts for L-level (L100–L400) at start of every new advisory interaction

**Rationale**:
- Ensures output matches audience expectations
- Prevents over-engineering for executives or under-delivering for engineers
- Already implemented in existing agent definition (First Interaction Protocol)

**Implementation**:
- Present L-level table at first interaction
- Store selection for conversation duration
- Adapt response structure per L-stage (existing templates in agent)
- Default to L200 if user doesn't specify

### 7. 6R Strategy User Selection

**Decision**: Always present 6R options and ask user to choose before generating migration plans

**Rationale**:
- User control prevents wrong assumptions about migration approach
- Different strategies produce vastly different cost calculators and roadmaps
- Aligns with FR-006 clarification

**Implementation**:
- Present 6 options with brief description when migration intent detected
- After user selects, generate plan and calculator specific to that strategy
- Allow changing strategy mid-conversation

### 8. Graceful Degradation Pattern

**Decision**: When an MCP server is unavailable, use remaining tools and note limitations

**Rationale**:
- Blocking entirely on one server failure degrades user experience
- Most advisory can still provide value with partial tool access
- User is informed of what's missing so they can remediate

**Implementation**:
- If MCP tool call fails → catch error, inform user which capability is limited
- Continue advisory using available tools
- Suggest user check MCP server installation if pattern persists
- Exception: if ALL MCP servers are down, trigger full MCP Health Check Protocol

## Summary of Decisions

| # | Topic | Decision |
|---|-------|----------|
| 1 | Agent format | `.agent.md` in `.github/agents/` |
| 2 | MCP config | `.vscode/mcp.json` with 7 npx servers |
| 3 | AWS MCP | Add `@aws/mcp` for multi-cloud |
| 4 | GCP MCP | Add Google Cloud MCP for multi-cloud |
| 5 | Fabric MCP | Use existing Fabric MCP (`mcp_microsoft_fab_*`) |
| 6 | Depth prompting | L-level (L100–L400) asked at start |
| 7 | 6R selection | Always ask user before migration planning |
| 8 | Degradation | Graceful — use remaining tools, note gaps |

## Open Items (deferred to implementation)

- Exact GCP MCP package name and tool prefix (verify at install time)
- AWS MCP tool prefix naming convention (verify at install time)
- Whether Fabric MCP needs separate mcp.json entry or is bundled with Azure MCP
