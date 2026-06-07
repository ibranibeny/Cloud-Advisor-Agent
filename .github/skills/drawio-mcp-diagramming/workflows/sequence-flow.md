# Sequence and Flow Diagram Patterns

Use this workflow for diagrams that show **temporal flows** — what happens in order — rather than infrastructure topology. No cloud icon catalog lookup is required.

## When to Apply

| Diagram type | Keywords | Layout |
|---|---|---|
| Auth / authorisation flow | OAuth, OIDC, JWT, SSO, login, token exchange, Entra, Cognito | Swimlane interaction flow |
| API / microservice call chain | REST, GraphQL, request/response, service-to-service, API gateway | Swimlane or vertical flowchart |
| CI/CD pipeline | pipeline, build, deploy, release, GitHub Actions, Azure DevOps, approval gate | Horizontal pipeline flowchart |

## Layout Approach

**Swimlane interaction flow** (auth / API flows with 2-5 actors):
- Represent each actor as a labelled header rectangle at the top, with a matching full-height light-coloured background column below it
- Steps flow top-to-bottom within each column; number them (`1.`, `2.`, `3.`) in the label so execution order is unambiguous
- All step boxes and edges live at `parent="1"` (root) — no nested swimlane cell geometry required
- Edges cross between columns with `edgeStyle=orthogonalEdgeStyle;`
- Canvas: `pageWidth="1400" pageHeight="900"` for 3 actors; add ~420 px width per additional actor

**Horizontal pipeline flowchart** (CI/CD):
- Stages flow left-to-right: Source → Build → Test → Staging → Approval → Production
- Use `rounded=1` rectangles for stages, `rhombus` shape for gate / decision points
- Colour-code each stage box using the Stage Colours table below
- Failure branch goes downward from the gate with a red edge to a Rollback/Notify step
- Canvas: `pageWidth="1700" pageHeight="600"`

## Colour Conventions

### Edge Colours (consistent with topology palette)

| Meaning | `strokeColor` | Style |
|---|---|---|
| Primary request / call | `#0078D4` Azure blue | solid, `strokeWidth=2` |
| Success response / return | `#00897B` Teal | solid, `strokeWidth=2` |
| Token / credential / redirect | `#F57C00` Amber | `dashed=1`, `strokeWidth=2` |
| Async / event-driven call | `#5C6BC0` Indigo | `dashed=1`, `strokeWidth=2` |
| Error / rejection / rollback | `#C62828` Red | solid, `strokeWidth=2` |
| Optional / conditional | `#666666` Grey | `dashed=1`, `strokeWidth=1` |

### Participant Lane Colours (swimlane header + column background at `opacity=30`)

| Actor type | `fillColor` | `strokeColor` |
|---|---|---|
| User / browser / client | `#dae8fc` | `#6c8ebf` |
| Identity provider (Entra, Cognito, Okta) | `#e6f4ea` | `#82b366` |
| API / backend service | `#fff3e0` | `#e6821e` |
| Database / data store | `#f5f5f5` | `#666666` |
| Cloud managed service (Key Vault, S3, etc.) | `#f3e5f5` | `#7B1FA2` |

### Stage Fill Colours (CI/CD pipeline)

| Stage | `fillColor` | `fontColor` |
|---|---|---|
| Source / Trigger | `#0078D4` | `#ffffff` |
| Build | `#00897B` | `#ffffff` |
| Test / Quality Gate | `#F57C00` | `#ffffff` |
| Deploy to Staging | `#5C6BC0` | `#ffffff` |
| Approval Gate | `#795548` | `#ffffff` |
| Deploy to Production | `#43A047` | `#ffffff` |
| Rollback / Failure | `#C62828` | `#ffffff` |

## Flow Animation

`flowAnimation=1;` works on sequence/flow edges exactly as in topology diagrams. Apply to primary call paths or pipeline stage transitions. Always ask the user before applying.

## Checklist (Sequence/Flow Diagrams)

- [ ] Diagram type identified (auth flow / API flow / CI/CD pipeline)
- [ ] Actors / participants labelled clearly
- [ ] Steps numbered in execution order
- [ ] Edge colours consistent with conventions above
- [ ] Error / failure paths shown in red
- [ ] Animation preference confirmed with user before generating
- [ ] Canvas sized appropriately for participant count and step depth
