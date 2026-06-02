# Hub-and-Spoke Cost Comparison Template

Use this template when presenting shared vs. duplicated resource cost analysis.

## Cost Comparison Table

| Resource | SKU | Monthly Cost (Hub: 1×) | Monthly Cost (Per-Spoke: N×) | Monthly Savings |
|----------|-----|------------------------|------------------------------|-----------------|
| Azure Firewall | Standard | $730 | $730 × {N} = ${total} | ${savings} |
| VPN Gateway | VpnGw1 | $140 | $140 × {N} = ${total} | ${savings} |
| Azure Bastion | Standard | $278 | $278 × {N} = ${total} | ${savings} |
| DNS Private Resolver | — | $130 | $130 × {N} = ${total} | ${savings} |
| **Subtotal (Shared Savings)** | | | | **${total_savings}** |

## VNet Peering Cost (Inherent)

| Direction | Cost/GB | Estimated Monthly Traffic | Monthly Cost |
|-----------|---------|---------------------------|--------------|
| Hub → Spoke (egress) | $0.01/GB | {X} GB | ${cost} |
| Spoke → Hub (egress) | $0.01/GB | {X} GB | ${cost} |
| **Total Peering Cost** | | | **${peering_total}** |

## Net Savings Summary

| Line Item | Amount |
|-----------|--------|
| Shared resource savings | +${shared_savings}/mo |
| VNet peering overhead | -${peering_cost}/mo |
| **Net monthly savings** | **${net_savings}/mo** |
| **Net annual savings** | **${net_annual}/yr** |

## Assumptions

- Region: {region}
- Firewall SKU: {Basic | Standard | Premium}
- Spoke count: {N}
- Peering traffic estimate: {X} GB/mo per spoke (bidirectional)
- Pricing source: {Live MCP / Approximate estimates}
- Pricing date: {date}

## Firewall SKU Options (ask user)

| SKU | Throughput | Monthly Cost (approx) | Best For |
|-----|-----------|----------------------|----------|
| Basic | ~250 Mbps | ~$285/mo | SMB, dev/test, low-traffic |
| Standard | ~30 Gbps | ~$730/mo | Enterprise general purpose |
| Premium | ~100 Gbps | ~$1,752/mo | Regulated/payment/healthcare (TLS inspection, IDPS) |

## Spoke Count Rendering Rules

- **≤ 10 spokes**: Show individual rows per spoke in tables
- **> 10 spokes**: Summarize as "N spokes × unit cost = total"

## Next Steps

1. Choose Firewall SKU based on throughput and security requirements
2. Estimate monthly spoke-to-hub traffic for accurate peering cost
3. Consider Reserved Instance pricing for Firewall (1yr: ~35% discount)
4. **Generate Excel calculator** for detailed scenario modeling (uses cost-calculator-template.md)
5. Verify current pricing at https://azure.microsoft.com/pricing/
