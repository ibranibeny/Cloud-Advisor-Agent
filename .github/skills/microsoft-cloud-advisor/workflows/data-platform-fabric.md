# Data Platform & Fabric Workflow

## Trigger
Request contains: lakehouse, Fabric, data warehouse, KQL, OneLake, eventstream, unified analytics, Synapse, medallion, data mesh.

## Procedure

### Step 1: Classify data workload

```
What is the primary workload?
+-- Unified analytics + governance -> Microsoft Fabric
+-- Big data analytics (>100TB, complex ETL) -> Synapse Dedicated Pools
+-- Real-time streaming + analytics -> Fabric Eventstreams + KQL
+-- Transactional OLTP -> Azure SQL / Cosmos DB
+-- Document/graph queries -> Cosmos DB
+-- AI/ML training data -> Azure ML + Fabric Lakehouse
+-- Existing on-prem SQL Server -> Azure SQL MI (replatform) or Fabric (modernize)
```

### Step 2: Query Fabric MCP for live context

| Tool | Use Case |
|------|----------|
| `mcp_microsoft_fab_kusto_list_databases` | Discover existing KQL databases |
| `mcp_microsoft_fab_kusto_list_tables` | Explore table schemas |
| `mcp_microsoft_fab_kusto_sample_table_data` | Show sample data |
| `mcp_microsoft_fab_onelake_list_workspaces` | List Fabric workspaces |
| `mcp_microsoft_fab_onelake_list_items` | Browse lakehouse items |
| `mcp_microsoft_fab_eventstream_list` | List eventstreams |
| `mcp_microsoft_fab_eventstream_get` | Get eventstream details |
| `mcp_microsoft_fab_kusto_query` | Run KQL queries for analytics examples |

### Step 3: Research with Microsoft Learn

- `microsoft_docs_search` for "Microsoft Fabric lakehouse architecture"
- `microsoft_docs_search` for "Fabric medallion architecture"
- `microsoft_docs_search` for "Fabric eventstream real-time analytics"

### Step 4: Design architecture

Recommend pattern based on classification:
- **Lakehouse**: OneLake + Delta tables + SQL analytics endpoint + Power BI
- **Medallion Architecture**: Bronze (raw ingestion) -> Silver (cleaned/validated) -> Gold (curated/aggregated)
- **Real-Time Analytics**: Eventstreams -> KQL Database -> Real-Time Dashboard
- **Hybrid with Synapse**: Fabric for unified analytics + Synapse Dedicated for heavy ETL workloads

### Step 5: Include KQL examples where applicable

Use `mcp_microsoft_fab_kusto_query` to demonstrate patterns.

## Fabric Cost

Source Fabric capacity pricing from Azure Retail Prices API via `azure/pricing` (`mcp_azure_mcp_pricing`):
- Filter: `serviceName eq 'Microsoft Fabric'` (serviceFamily `Data`)
- Returns both Consumption (per-CU-hour) and Reservation (1-Year / 3-Year) pricing
- No separate Fabric pricing source needed for cost calculators

## Hands-on Fabric Operations

For end-to-end Fabric authoring/consumption/operations/migration via Copilot CLI, route to the `skills-for-fabric` collection. For live in-agent tool calls, use the `fabric/*` MCP tools above.
