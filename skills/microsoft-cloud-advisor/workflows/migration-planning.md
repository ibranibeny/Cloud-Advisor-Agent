# Migration Planning Workflow

## Trigger
Request contains: migrate, lift-and-shift, VM sizing, 6R, on-premises, datacenter, move to Azure.

## Procedure

### Step 1: 6R Strategy Selection (Mandatory Gate)

Present the 6R decision tree before generating any plan:

| Strategy | Description | When to Use |
|----------|-------------|-------------|
| **Rehost** (Lift & Shift) | Move as-is to Azure VMs | Urgent migration, minimal changes |
| **Replatform** (Lift & Reshape) | Minor optimizations (managed DB) | Quick wins without rearchitecture |
| **Refactor** | Rearchitect for cloud-native | Long-term, maximize cloud benefits |
| **Repurchase** | Replace with SaaS/PaaS | COTS apps with Azure alternatives |
| **Retain** | Keep on-premises | Compliance, dependency, or cost reasons |
| **Retire** | Decommission | Unused or redundant workloads |

### Step 2: Research Strategy-Specific Best Practices

1. Research with `microsoft_docs_search` for strategy-specific patterns
2. Query `mcp_azure_mcp_azuremigrate` for tooling recommendations
3. Use `mcp_azure_mcp_pricing` for strategy-appropriate SKU pricing
4. Generate phased migration plan with Gantt timeline
5. Produce cost calculator if requested (see `cost-calculator.md`)

### Step 3: Generate Migration Plan

Output template:
1. **Strategy Confirmation** -- Echo selected 6R strategy with rationale
2. **Migration Phases** -- Phased approach with milestones (Mermaid Gantt)
3. **Workload Inventory** -- Mapped to target Azure services
4. **Cost Calculator** -- Excel workbook via Excel MCP (see `cost-calculator.md`)
5. **Risk Assessment** -- Blockers, dependencies, rollback plan
6. **Timeline** -- Mermaid Gantt chart

### Step 4: Hybrid Scenario Handling

When hybrid_split > 0 or Azure Arc / ExpressRoute / VPN is mentioned:
- Include on-premises retained workloads in the plan
- Show ExpressRoute/VPN connectivity requirements
- Calculate split-percentage cost formulas (e.g., 60% cloud / 40% on-prem)
- Reference Azure Arc management for retained workloads

### Step 5: Effort Estimates

| Effort | Description | Typical Duration |
|--------|-------------|------------------|
| Low | Rehost, no app changes | 1-2 weeks |
| Medium | Replatform, managed services | 2-6 weeks |
| High | Refactor, architecture changes | 2-6 months |
