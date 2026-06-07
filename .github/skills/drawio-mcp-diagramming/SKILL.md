---
name: drawio-mcp-diagramming
description: Create and edit architecture diagrams using Draw.io MCP (`drawio/create_diagram`) with reliable Azure and AWS icon rendering guidance and troubleshooting. Supports Azure2 and AWS4 icon libraries. Requires Python 3 and internet access to refresh icon catalogs (periodic, not per-run).
metadata:
  author: Thomas Thornton
  version: "1.1.0"
  last-updated: "2026-06-07"
---

# Draw.io MCP Diagramming Skill

> Vendored from [thomast1906/github-copilot-agent-skills](https://github.com/thomast1906/github-copilot-agent-skills/tree/main/.github/skills/drawio-mcp-diagramming) by Thomas Thornton. Refer to the source repository for licensing.

Use this skill to create or update diagrams through the Draw.io MCP tool and to avoid common Azure and AWS icon rendering problems.

See [references/REFERENCE.md](references/REFERENCE.md) for reference artifacts and refresh commands.

## When to Use

- The user asks to create or refine architecture diagrams (Azure, AWS, or multi-cloud)
- The user wants draw.io/diagrams.net output from an MCP workflow
- The user needs Azure or AWS service icons in diagrams
- The user reports that icons/shapes are not appearing
- The user asks for an auth/identity flow (OAuth, OIDC, JWT, SSO, Entra, Cognito)
- The user asks for an API/microservice interaction diagram
- The user asks for a CI/CD pipeline or deployment workflow diagram

## Required Tooling

- MCP tool: `drawio/create_diagram`
- Workspace MCP config should include a `drawio` server:

```json
{
  "servers": {
    "drawio": {
      "type": "http",
      "url": "https://mcp.draw.io/mcp"
    }
  }
}
```

## Workflow Router

| Diagram Type | Workflow File |
|--------------|--------------|
| Azure infrastructure / network topology | `workflows/azure-topology.md` |
| AWS infrastructure / network topology | `workflows/aws-topology.md` |
| Auth flow, API chain, CI/CD pipeline | `workflows/sequence-flow.md` |
| Icon lookup and validation (Azure2 / AWS4) | `workflows/icon-discovery.md` |

## Recommended Workflow

1. **Identify the cloud provider** — determine whether the diagram uses Azure, AWS, or both (multi-cloud)
2. **Verify icon paths from the static catalogs** — see `workflows/icon-discovery.md`:
   - Azure: grep `references/azure2-complete-catalog.txt`
   - AWS: grep `references/aws4-complete-catalog.txt`
3. If diagram is a **sequence or flow diagram** (auth, API, CI/CD): skip icon lookup, load `workflows/sequence-flow.md`
4. **For Azure infrastructure diagrams**: load `workflows/azure-topology.md`
5. **For AWS infrastructure diagrams**: load `workflows/aws-topology.md`
6. Build a valid `mxGraphModel` payload using verified icons when applicable
7. Call `drawio/create_diagram` with the XML
8. If user wants a file artifact, save as `.drawio` wrapped in `<mxfile><diagram>...</diagram></mxfile>`
9. Keep labels concise and explicit (service name + role)
10. For cloud-specific diagrams, prefer one icon per major service and use edges for flow semantics

For non-Azure/non-AWS diagrams, skip icon discovery and proceed directly to `drawio/create_diagram`.

## Visual Quality Guardrails

Apply these defaults unless the user explicitly asks for a dense/technical view:

- Use 3-4 major lanes/zones max (e.g., Source, Pipeline, Cloud target)
- Keep primary flow left-to-right with a single main path
- Use stage numbering (`1`, `2`, `3`, `4`) instead of many edge labels
- Keep one icon per major service; avoid icon-per-step layouts
- Limit cross-lane dashed lines to one security/auth line and one optional telemetry line
- Keep text concise (single purpose per box) and avoid multiline overload
- **Animated flow**: `flowAnimation=1;` on connectors renders a moving dot. Ask user before applying: *"Would you like any flow arrows animated? If so, which ones?"*
- Prefer a "clean" variant first; add detail only if requested

For worked examples of common layout problems, see [references/layout-antipatterns.md](references/layout-antipatterns.md).

## Exporting Diagrams

| Format | How | Notes |
|---|---|---|
| **SVG** | `File → Export As → SVG` | Preserves `flowAnimation` effects. Recommended. |
| **PNG** | `File → Export As → PNG` | Static snapshot. No animation. |
| **PDF** | `File → Export As → PDF` | Static. Best for print/docs. |
| **.drawio** | `File → Save As` | Preserves all XML for future editing. |

> `flowAnimation=1` is only visible in **draw.io desktop** or **SVG** — not in PNG or PDF.

## Troubleshooting Checklist

- Confirm MCP server appears in `MCP: List Servers`
- Run `MCP: Reset Cached Tools` if tool list is stale
- Ensure XML is well-formed (no malformed tags or invalid comments)
- **Azure**: Verify style uses `image=img/lib/azure2/...`
- **AWS**: Verify style uses `shape=mxgraph.aws4.*`
- Reopen diagram in web draw.io if VS Code extension rendering differs
- If icon paths look wrong, grep the relevant catalog for alternatives
- If catalogs appear stale, re-run refresh (see [references/REFERENCE.md](references/REFERENCE.md))

## Definition of Done

- All icon paths confirmed against relevant static catalog before calling `drawio/create_diagram`
- Diagram renders correctly; XML is valid and opens in draw.io
- Cloud resources identifiable via correct icons and clear labels
- All applicable checklist items passed (from relevant workflow file)
- Animation preference confirmed; `flowAnimation=1;` applied only to user-identified edges
- File artifact saved as `.drawio` (wrapped in `<mxfile>`) if requested
- Layout anti-patterns checked against [references/layout-antipatterns.md](references/layout-antipatterns.md)

## Reference Data

| Reference | Content |
|-----------|---------|
| `references/REFERENCE.md` | Full reference, prompt templates, refresh commands |
| `references/azure2-complete-catalog.txt` | 648 Azure2 icon paths |
| `references/aws4-complete-catalog.txt` | 1,037 AWS4 stencil shape names |
| `references/topology-patterns.md` | Complete topology diagram examples |
| `references/layout-antipatterns.md` | Common layout problems and fixes |
| `references/standalone-file-requirements.md` | .drawio file format requirements |
