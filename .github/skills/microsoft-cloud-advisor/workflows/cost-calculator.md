# Cost Calculator Workflow (Excel MCP)

## Trigger
User requests cost estimation, TCO analysis, or Azure pricing breakdown in Excel format.

## Mandatory Input Gate

Before generating any calculator, confirm **three required inputs** (ask if missing):

1. **Region** -- Pricing varies by Azure region. Pull region-specific pricing from `mcp_azure_mcp_pricing`.
2. **Migration Strategy (6R)** -- Workbook must reflect cost differences by strategy:
   | Strategy | Cost Model | Primary Azure Targets |
   |----------|-----------|------------------------|
   | Rehost | IaaS VM + managed disks | Virtual Machines, Managed Disks |
   | Replatform | PaaS, minimal changes | App Service, ACI, SQL MI |
   | Refactor | Serverless / containers | Functions, Container Apps |
   | Rearchitect | Cloud-native rebuild | AKS, microservices, Cosmos DB |
   | Rebuild | Greenfield Azure-native | Azure-native PaaS stack |
   | Replace | SaaS substitution | M365, Dynamics, Power Platform |
3. **Hybrid split** -- % on-prem vs % Azure. Include Azure Arc, ExpressRoute/VPN, AVS/HCI line items.

## Azure Hybrid Benefit (AHB) Gate

For Windows Server or SQL Server workloads, evaluate AHB:
- Only apply when customer owns eligible licenses with Software Assurance
- Cap at owned-license capacity (Win Server: 8-core pack -> up to 8 vCPU; SQL Enterprise: 4 vCore/license; SQL Standard: 1:1)
- AHB removes license portion only -- never base compute, storage, or networking
- Show AHB savings as an explicit memo line, never silently bake in

## Procedure

1. Confirm the three mandatory inputs -- ask if missing
2. Research pricing with `mcp_azure_mcp_pricing` for relevant SKUs in the chosen region
3. Select cost model and target services based on 6R strategy
4. Use Excel MCP to create workbook:
   - `mcp_excel-mcp_file` -> Create/open workbook
   - `mcp_excel-mcp_worksheet` -> Create sheets
   - `mcp_excel-mcp_range_edit` -> Populate pricing data
   - `mcp_excel-mcp_range_format` -> Format as currency, headers, borders
   - `mcp_excel-mcp_table` -> Create structured tables
   - `mcp_excel-mcp_chart` -> Add cost breakdown charts

## Calculator Sheet Structure

| Sheet | Contents |
|-------|----------|
| **Summary** | Executive cost summary, monthly/annual totals, charts |
| **Compute** | VMs, AKS, App Service, Functions -- SKU, qty, unit cost, total |
| **Storage** | Disks, Blob, Files, Data Lake -- tier, capacity, cost |
| **Networking** | VPN, ExpressRoute, Load Balancer, Bandwidth -- usage, cost |
| **PaaS & Data** | SQL, Cosmos, Fabric, AI services -- tier, DTU/RU, cost |
| **Strategy (6R)** | Selected strategy -> cost model, target services, multipliers |
| **Hybrid** | Azure Arc, ExpressRoute/VPN, AVS/HCI, on-prem vs Azure split % |
| **AHB** | AHB -- license uplift avoided, eligibility & coverage, net savings |
| **Assumptions** | Editable inputs: region, 6R strategy, hybrid split %, AHB toggles, reserved vs PAYG, growth % |

## Key Formulas

```
=SUMPRODUCT(qty, unit_price)                           -- Line item totals
=SUM(monthly_costs)                                     -- Category subtotals
=monthly_total * 12                                     -- Annual projection
=monthly_total * (1 + growth_rate)^months               -- Growth forecast
=IF(reserved, price*0.6, price)                         -- Reserved instance discount
=VLOOKUP(region, region_pricing, col)                   -- Region-driven unit price
=base_cost * strategy_multiplier                        -- 6R strategy cost adjustment
=azure_cost*azure_split + onprem_cost*(1-azure_split)   -- Hybrid split
=IF(ahb_applied, 0, license_uplift)                     -- AHB removes license uplift
```
