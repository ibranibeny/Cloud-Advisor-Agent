# Hub-Spoke Networking Workflow

## Triggers
- "best practice" (networking context), "price comparison" (networking), "VNet peering"
- "multiple environments", "landing zone", "network topology"
- "hub-spoke" / "hub and spoke", "spoke", "Virtual WAN" / "VWAN"
- "network architecture", "shared services network"

## Sub-Flow Routing

| Sub-Flow | Additional Triggers | Section |
|----------|---------------------|---------|
| **Best Practice** | "best practice", "recommend", "how should I design" | Step A |
| **Cost Comparison** | "cost", "price", "savings", "compare cost" | Step B |
| **Decision Matrix** | "single spoke vs multiple", "how many spokes", "topology" | Step C |
| **Diagram** | "diagram", "show me", "architecture diagram", "visualize" | Step D |
| **Landing Zone** | "landing zone", "enterprise-scale", "connectivity subscription" | Step E |
| **Virtual WAN** | "Virtual WAN", "VWAN", "managed hub" | Step F |

## L-Level Depth Scaling

| Level | Diagram Detail | Cost Detail | Advisory Detail |
|-------|---------------|-------------|-----------------|
| L100 | Hub + spoke boxes only | Monthly range estimate | 1-paragraph summary |
| L200 | Hub services labeled, spokes named | Comparison table with savings | + Hub services table + alternatives |
| L300 | Subnet names, IP ranges, NSGs | + SKU breakdown + peering math | + Security controls + UDRs |
| L400 | Route tables, FW rules, DNS zones | + TCO + Reserved Instance | + Threat model + compliance mapping |

## Firewall SKU Clarification Gate (Mandatory for Cost)

When cost/pricing detected AND Firewall SKU not provided:

1. STOP before generating cost figures
2. Present SKU comparison:
   | SKU | Throughput | Monthly Cost (approx) | Best For |
   |-----|-----------|----------------------|----------|
   | Basic | ~250 Mbps | ~$285/mo | SMB, dev/test |
   | Standard | ~30 Gbps | ~$730/mo | Enterprise general purpose |
   | Premium | ~100 Gbps | ~$1,752/mo | Regulated (TLS inspection, IDPS) |
3. Wait for response before proceeding
4. Also ask for spoke count and region if not provided

## MCP Pricing with Graceful Degradation

1. **Attempt**: Call `mcp_azure_mcp_pricing` for Firewall, VPN Gateway, Bastion in user's region
2. **If successful**: Use live prices, state "Pricing source: Live Azure MCP (as of {date})"
3. **If unavailable**: Use approximate estimates with disclaimer:
   - Firewall Basic: ~$285/mo | Standard: ~$730/mo | Premium: ~$1,752/mo
   - VPN Gateway VpnGw1: ~$140/mo | Azure Bastion Standard: ~$278/mo
   - DNS Private Resolver: ~$130/mo
   - Add: "Verify current pricing at https://azure.microsoft.com/pricing/"

---

## Step A: Best Practice Advisory

1. **Summary** -- 2-3 sentences: hub-spoke as recommended Azure networking pattern. Cite Azure Architecture Center.
2. **Hub Services Table**:
   | Service | Purpose | Shared Benefit |
   |---------|---------|----------------|
   | Azure Firewall | Centralized security, FQDN filtering, threat intel | Single policy for all spokes |
   | VPN/ExpressRoute Gateway | Cross-premises connectivity | One gateway serves all spokes |
   | Azure Bastion | Secure RDP/SSH without public IPs | Access VMs in any spoke from hub |
   | DNS Private Resolver | Hybrid DNS resolution | Centralized DNS for all spokes |
3. **Mermaid Diagram** -- See `references/hub-spoke-patterns.md` for L-level templates
4. **WAF Pillar Alignment** (L200+):
   - Security: Centralized firewall inspection
   - Cost Optimization: Shared hub resources
   - Reliability: Spoke isolation
   - Operational Excellence: Single management point
   - Performance: Low-latency VNet peering
5. **Alternatives Note** -- Virtual WAN as Microsoft-managed alternative
6. **Next Steps** -- 3-5 actionable items

**Validate**: Call `mcp_azure_mcp_get_azure_bestpractices` to confirm alignment.

Source: https://learn.microsoft.com/azure/architecture/networking/architecture/hub-spoke

---

## Step B: Cost Comparison

1. **Firewall SKU Gate** -- Ask for SKU if not provided
2. **Gather inputs**: Firewall SKU, Spoke count (N), Region (default East US 2), Data transfer (default 1,000 GB/mo/spoke)
3. **Call MCP pricing** with graceful degradation
4. **Calculate savings**: Shared (1x) vs. duplicated (Nx spokes) -- savings = monthly_cost x (N-1)
5. **Add VNet peering cost**: $0.01/GB x GB/mo/spoke x 2 directions x N spokes
6. **Present net savings**: Total savings minus peering overhead

Spoke rendering: <=10 individual rows; >10 use "N spokes x unit cost" summary.

---

## Step C: Decision Matrix

1. **Decision Summary** -- 1-2 sentences framing the choice
2. **Comparison Table** (6 factors): Isolation, Cost, Compliance, Management, Scalability, Team Autonomy
3. **Scenario Recommendations**:
   - Dev/test or small org -> single spoke (or 2: prod + non-prod)
   - Production with compliance -> multiple spokes per compliance boundary
   - Multi-team / enterprise -> multiple spokes per team or BU
4. **Next Steps** -- 2-3 actionable items

---

## Step D: Diagram Generation

1. Detect L-level (default L200)
2. Get spoke count (default 3 for examples)
3. Select template from `references/hub-spoke-patterns.md` at appropriate L-level
4. Render with user context (spoke names, count, on-prem connectivity)
5. Include diagram key for L300+

---

## Step E: Landing Zone Foundation

1. Frame hub-spoke as Enterprise-Scale connectivity:
   - Hub VNet in Connectivity subscription under Platform management group
   - Spokes map to Landing Zone subscriptions
   - Hierarchy: Root -> Platform (Connectivity, Identity, Management) -> Landing Zones (Corp, Online)
2. Reference Azure Landing Zone accelerator (hub-spoke = CAF networking component)
3. L300+ detail: Firewall policies, route tables, DNS Private Resolver, reference https://aka.ms/alz
4. Next Steps

---

## Step F: Virtual WAN Comparison

Provide qualitative tradeoff by default. Quantified cost only on explicit request.

| Aspect | Customer-Managed Hub-Spoke | Azure Virtual WAN |
|--------|---------------------------|-------------------|
| Management | Full control | Microsoft-managed |
| Cost | Pay per resource | Hub fee + per-connection |
| Routing | Manual UDRs | Automatic any-to-any |
| Multi-region | Manual multi-hub | Built-in global transit |
| Flexibility | Full NVA choice | Limited configurations |
| Branch connectivity | VPN/ER to hub GW | Integrated SD-WAN |
| Best for | <50 branches, cost control | >50 branches, simplified routing |

---

## Security Controls (L300+)

- NSG Rules: Deny-all-inbound by default, explicit allow rules
- Private Endpoints: In spokes for PaaS access, DNS resolution via hub
- Azure Firewall Network Rules: Control spoke-to-spoke and spoke-to-internet
- Azure Firewall Application Rules: FQDN filtering for outbound
- DDoS Protection Plan: Attach to hub VNet, extends to peered spokes

## Edge Cases

- **Mesh networking**: Acknowledge, explain hub-spoke preference (centralized security, cost)
- **Flat network**: Recommend phased migration to hub-spoke
- **Multi-region**: Note single-region scope, mention VWAN or multi-hub, offer to elaborate
