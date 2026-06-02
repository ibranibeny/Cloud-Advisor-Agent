# Agent Response Contracts: Hub-and-Spoke Networking

## Contract 1: Best Practice Advisory Response

**Trigger**: User asks about Azure networking best practices, network topology, or landing zone networking.

**Required Response Sections** (in order):

1. **Summary Recommendation** — 2-3 sentences stating hub-spoke as the recommended pattern
2. **Hub Services Table** — Table listing shared resources (Firewall, Gateway, Bastion, DNS) with one-line descriptions
3. **Mermaid Diagram** — Hub-spoke topology at appropriate L-level
4. **When to Consider Alternatives** — Brief note on Virtual WAN / mesh (1-2 sentences)
5. **Next Steps** — 3-5 actionable items

**L-Level Scaling**:
- L100: Summary + simple diagram (hub + spoke boxes only)
- L200: + Hub services table + spoke purposes labeled
- L300: + Subnet-level diagram + IP ranges + NSG references
- L400: + Route tables + Firewall policy rules + DNS zones

---

## Contract 2: Cost Comparison Response

**Trigger**: User asks about hub-spoke cost, pricing, savings, or per-VNet vs shared resources.

**Pre-requisites** (agent must ask if not provided):
- Azure Firewall SKU (Basic/Standard/Premium)
- Number of spokes
- Region (for live pricing lookup)

**Required Response Sections** (in order):

1. **Pricing Source** — State whether live MCP pricing or approximate estimates
2. **Hub Shared Resources Table**:
   | Resource | Monthly Cost (Hub) | Monthly Cost (Per-Spoke × N) | Monthly Savings |
3. **VNet Peering Cost** — Inherent peering cost at $0.01/GB × estimated traffic
4. **Net Savings Summary** — Total savings = shared savings - peering overhead
5. **Assumptions** — List pricing assumptions, region, SKU choice
6. **Next Steps** — Include "Generate Excel calculator" as an option

**Spoke Count Rules**:
- ≤ 10 spokes: Individual rows per spoke in tables
- > 10 spokes: Summarize as "N spokes × unit cost"

---

## Contract 3: Decision Matrix Response

**Trigger**: User asks about single spoke vs multiple spokes, spoke topology choice.

**Required Response Sections** (in order):

1. **Decision Summary** — 1-2 sentences framing the decision
2. **Comparison Table** — Minimum 5 factors:
   | Factor | Single Spoke | Multiple Spokes |
   Factors: Isolation, Cost, Compliance, Management, Scalability, Team Autonomy
3. **Scenario Recommendations**:
   - Dev/test → single spoke
   - Production with compliance → multiple spokes
   - Multi-team → multiple spokes
4. **Next Steps** — 2-3 actionable items

---

## Contract 4: Architecture Diagram Response

**Trigger**: User requests hub-spoke diagram, network architecture visualization.

**Required Elements in Mermaid Diagram**:
- Hub VNet as a subgraph containing: Firewall, Gateway, Bastion, DNS
- Spoke VNets (up to 10 individually; beyond 10 as grouped node)
- Bidirectional peering arrows (hub ↔ spoke)
- On-premises connectivity (if specified by user)
- L300+: Subnet names and IP ranges as node labels
- L400+: Route table annotations

**Mermaid Format**: `graph TD` (top-down)

---

## Contract 5: Virtual WAN Comparison Response

**Trigger**: User asks about Azure Virtual WAN, managed hub, VWAN vs hub-spoke.

**Required Response Sections** (in order):

1. **Summary** — Distinguish customer-managed hub-spoke from Microsoft-managed Virtual WAN
2. **Comparison Table**:
   | Aspect | Customer-Managed Hub-Spoke | Azure Virtual WAN |
   Aspects: Management, Cost, Routing, Multi-region, Flexibility, Branch connectivity
3. **Recommendation Criteria** — When to use which
4. **Next Steps**

---

## Error/Degradation Contract

**When MCP Pricing Tool is Unavailable**:

1. State clearly: "Live pricing unavailable — using approximate estimates"
2. Provide approximate monthly costs with ⚠️ disclaimer
3. Include link: "Verify current pricing at https://azure.microsoft.com/pricing/"
4. Show date: "Estimates as of [current date]"
5. Offer: "I can retry the live pricing lookup if you'd like"
