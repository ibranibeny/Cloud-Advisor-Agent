# MCP Tool Usage Reference

## Tool Families and When to Use

| Tool Family | When to Use | Example Call |
|-------------|-------------|-------------|
| `mcp_azure_mcp_pricing` | Cost estimation, price comparison, TCO | Get Firewall Standard monthly price in East US 2 |
| `mcp_azure_mcp_compute` | VM sizing, SKU availability | List available VM sizes in a region |
| `mcp_azure_mcp_get_azure_bestpractices` | Architecture validation, WAF alignment | Validate hub-spoke design choices |
| `mcp_azure_mcp_wellarchitectedframework` | Deep-dive WAF reviews, pillar-specific advice | Check security pillar for networking |
| `mcp_azure_mcp_documentation` | Official architecture patterns, how-to guides | Hub-spoke reference architecture |
| `mcp_azure_mcp_cloudarchitect` | Architecture design, pattern recommendation | Design a multi-region strategy |
| `mcp_azure_mcp_advisor` | Optimization recommendations from Azure Advisor | Review cost recommendations for a subscription |
| `mcp_azure_mcp_policy` | Governance, compliance, policy definitions | Find built-in policies for networking |
| `mcp_microsoft-lea_microsoft_docs_search` | Broad documentation search, conceptual research | Search for "Azure Firewall SKU comparison" |
| `mcp_microsoft-lea_microsoft_docs_fetch` | Full page fetch when search snippets are insufficient | Fetch a specific docs URL |
| `mcp_microsoft-lea_microsoft_code_sample_search` | Code examples, Bicep/Terraform samples | Find Bicep for hub-spoke deployment |
| `mcp_excel-mcp_*` | Generate Excel cost calculators, dashboards | Create workbook with pricing data |
| `mcp_azure_mcp_arm` | Resource Graph queries, existing deployments | Query deployed resources |
| `mcp_azure_mcp_keyvault` | Secrets, certificates management | Check Key Vault recommendations |
| `mcp_azure_mcp_monitor` | Alerting, diagnostics, log queries | Design monitoring strategy |

## Usage Rules

1. **Always attempt MCP first** -- fall back to training data only when tools unavailable
2. **Cite source** -- state "Source: Azure MCP (live)" or "Source: Microsoft Learn" after tool-sourced info
3. **Graceful degradation** -- if MCP fails, use approximate values with "[estimated -- verify at azure.microsoft.com/pricing]"
4. **Rate sensitivity** -- batch pricing calls when comparing multiple SKUs; single call per SKU family
5. **Region matters** -- always pass region parameter; pricing varies by region
6. **Freshness note** -- MCP data is live; add date stamp when cost figures are presented

## Tool Priority Order (for common scenarios)

### Cost Questions
1. `mcp_azure_mcp_pricing` (primary)
2. `mcp_microsoft-lea_microsoft_docs_search` (supplemental context)
3. `mcp_excel-mcp_*` (output generation)

### Architecture Advisory
1. `mcp_azure_mcp_cloudarchitect` (design)
2. `mcp_azure_mcp_get_azure_bestpractices` (validation)
3. `mcp_azure_mcp_documentation` (reference)

### Migration Planning
1. `mcp_microsoft-lea_microsoft_docs_search` (patterns)
2. `mcp_azure_mcp_compute` (target sizing)
3. `mcp_azure_mcp_pricing` (cost projection)

### Multi-Cloud Comparison
1. `mcp_microsoft-lea_microsoft_docs_search` (Azure side)
2. Training knowledge (AWS/GCP equivalents)
3. `references/service-mapping-tables.md` (canonical mapping)
