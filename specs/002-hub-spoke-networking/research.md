# Research: Hub-and-Spoke Network Architecture Skill

**Date**: 2026-06-02 | **Status**: Complete

## Decision 1: Hub-Spoke Reference Architecture Source

**Decision**: Use Microsoft's official hub-spoke reference architecture from Azure Architecture Center as the canonical source for all recommendations.

**Rationale**: The Azure Architecture Center page (`learn.microsoft.com/azure/architecture/networking/architecture/hub-spoke`) is the authoritative reference. It covers:
- Hub services: Azure Firewall, VPN/ExpressRoute Gateway, Azure Bastion, DNS
- Spoke isolation patterns (production vs. non-production)
- VNet peering as the connectivity mechanism (nontransitive, low-latency, Azure backbone)
- Gateway transit from hub to spokes
- Multi-hub for multi-region (out of scope for this feature)

**Alternatives considered**:
- Azure Virtual WAN (Microsoft-managed hub) — too opinionated, less flexible; document as alternative per FR-009
- Custom NVA-based hub — too complex for general advisory; mention for advanced scenarios only

**Source**: https://learn.microsoft.com/azure/architecture/networking/architecture/hub-spoke

## Decision 2: Azure Firewall SKU Selection Strategy

**Decision**: Agent asks user which SKU to use (no default). Present a 3-row comparison when asking.

**Rationale**: The three SKUs serve very different use cases and price points:

| SKU | Throughput | Key Differentiator | Use Case |
|-----|-----------|-------------------|----------|
| Basic | Up to 250 Mbps | Stateful firewall + FQDN filtering (SNI) | SMB, dev/test, low-traffic |
| Standard | Up to 30 Gbps | Threat intel + DNS proxy + web categories + FQDN all-port | Enterprise general purpose |
| Premium | Up to 100 Gbps | TLS inspection + IDPS + URL filtering + PCI DSS | Regulated/payment/healthcare |

Per clarification session: no default assumption — the agent must ask before generating cost figures.

**Alternatives considered**:
- Default to Standard (most common) — rejected because Basic saves ~50% for valid scenarios
- Present all three with pricing — good for comparison but user must choose one for cost calculator

**Source**: https://learn.microsoft.com/azure/firewall/choose-firewall-sku

## Decision 3: VNet Peering Cost Model

**Decision**: Use $0.01/GB for same-region peering (intra-region) as the baseline cost in hub-spoke analyses.

**Rationale**: Azure VNet peering pricing is:
- Same-region (intra-region): $0.01/GB ingress + $0.01/GB egress
- Cross-region (global peering): varies by zone pair (~$0.035–$0.075/GB)

Since this feature is single-region only, the fixed $0.01/GB model applies. This is the "inherent cost" of hub-spoke that offsets against savings from shared resources.

**Alternatives considered**:
- Live pricing lookup for peering — not available via MCP pricing tool (peering is a flat published rate)
- Include cross-region — out of scope per clarification (single-region only)

## Decision 4: Pricing Data Source Strategy

**Decision**: Use `mcp_azure_mcp_pricing` MCP tool for real-time Firewall, Gateway, and Bastion pricing. Graceful degradation to approximate estimates if unavailable.

**Rationale**: Live pricing ensures accuracy across regions and accounts for price changes. The MCP tool supports querying by service name and region. Fallback approach:
1. Attempt live MCP pricing call
2. If unavailable/error: use well-known approximate monthly costs with disclaimer
3. Always show "as of [date]" and "verify at azure.microsoft.com/pricing"

**Alternatives considered**:
- Hardcoded pricing tables — stale quickly, wrong for non-US regions
- Link to pricing calculator only — not actionable enough per Constitution Principle V

## Decision 5: Mermaid Diagram Approach

**Decision**: Use Mermaid `graph TD` (top-down) for hub-spoke diagrams with labeled subgraphs for hub and spokes.

**Rationale**: Mermaid renders natively in VS Code markdown preview. The top-down layout naturally shows:
- Hub at center/top with shared services as nodes
- Spokes below/around with workload labels
- Bidirectional peering arrows between hub and each spoke
- Optional on-premises connection via gateway

L-level depth scaling:
- L100: Simple box diagram (hub + spokes, no internal detail)
- L200: Hub services labeled, spokes named by purpose
- L300: Subnet-level detail (GatewaySubnet, AzureFirewallSubnet, AzureBastionSubnet) + IP ranges
- L400: Route tables, NSG rules, firewall policies included as annotations

**Constraints**: Max 10 individual spokes rendered; beyond 10, use collapsed `Spoke Group (N spokes)` node.

**Alternatives considered**:
- Mermaid `flowchart LR` (left-right) — less intuitive for network hierarchy
- ASCII art — not rich enough, hard to maintain

## Decision 6: Single vs. Multiple Spoke Decision Matrix

**Decision**: Present a 5+ factor comparison table with scenario-based recommendations.

**Rationale**: The decision depends on multiple orthogonal factors:

| Factor | Single Spoke | Multiple Spokes |
|--------|-------------|----------------|
| Isolation | Shared fault domain | Independent fault domains |
| Cost | Lower (fewer peerings, simpler routing) | Higher (more peerings, more UDRs) |
| Compliance | Harder to separate regulated data | Natural compliance boundaries |
| Management | Simpler (one VNet, fewer NSGs) | More complex (per-spoke policies) |
| Scalability | Limited by VNet address space | Each spoke has independent space |
| Team autonomy | Shared responsibility | Delegated per spoke |

**Alternatives considered**:
- Simple "always use multiple spokes" — too prescriptive for dev/test scenarios
- Flowchart decision tree — good for advanced but table is more accessible

## Decision 7: Hub-Spoke vs. Azure Virtual WAN Comparison (FR-009)

**Decision**: Present as a comparison table when explicitly asked. Hub-spoke is default recommendation; Virtual WAN is the alternative.

**Rationale**: Per the Azure Architecture Center, customer-managed hub-spoke offers:
- More control over NVA deployment
- Cost savings (no Virtual WAN hub hourly fee)
- Overcoming subscription limits
- Workload isolation and flexibility

Virtual WAN offers:
- Microsoft-managed routing (no UDR maintenance)
- Simplified multi-hub/multi-region
- Any-to-any transit without complex peering

The agent defaults to customer-managed hub-spoke unless the user has >50 branches, needs managed routing, or asks specifically about Virtual WAN.

**Source**: https://learn.microsoft.com/azure/architecture/networking/architecture/hub-spoke#architecture
