---
name: skills-for-fabric
description: >
  Reference to the Microsoft "skills-for-fabric" collection — reusable AI-assistant skills and
  MCP setup for operating Microsoft Fabric (authoring, consumption, operations, migration) from
  GitHub Copilot CLI / VS Code and compatible AI coding tools. Use when the user wants to author,
  query, operate, or migrate Fabric workloads via CLI/REST/notebooks, or wants to install the
  Fabric skills plugin collection. Covers SQL Data Warehouse, Spark/Lakehouse, Power BI semantic
  models, Eventhouse/KQL, Eventstreams, Dataflows Gen2, OneLake, catalog search, and end-to-end
  medallion architecture. Triggers: "Fabric skill", "skills for fabric", "Fabric CLI", "author
  Fabric warehouse", "query lakehouse", "Fabric notebook", "Dataflow Gen2", "Eventhouse KQL",
  "semantic model", "Fabric migration", "databricks migration", "synapse migration", "medallion
  architecture", "install Fabric plugin".
metadata:
  author: Microsoft
  license: MIT
  upstream: https://github.com/microsoft/skills-for-fabric
  version: "0.3.2"
---

# Microsoft Fabric Skills (Reference)

> This is a **pointer skill**. The full collection lives upstream at
> [microsoft/skills-for-fabric](https://github.com/microsoft/skills-for-fabric) (MIT License,
> © Microsoft Corporation). It is an installable Copilot CLI / Claude / Cursor / Windsurf plugin
> collection of **24 interdependent skills** that share a `common/` core, so it is referenced
> here rather than vendored file-by-file. Install it as a plugin (below) instead of copying skills.

Use this reference when the user wants to **operate Microsoft Fabric** — author, query, run,
diagnose, or migrate Fabric workloads — from GitHub Copilot CLI, VS Code, or a compatible AI
coding tool, and route them to install/use the upstream collection.

## When to Use

Route here when the request is about **doing work inside Fabric via skills/CLI**, e.g.:
- Authoring: create/update Warehouses, Lakehouses, Spark notebooks, T-SQL/KQL, Dataflows Gen2,
  Eventstreams, Eventhouses, semantic models, activators.
- Consumption: read-only exploration/query across Warehouses, Lakehouses, Power BI semantic
  models, Eventhouse/KQL, Eventstreams, Dataflows Gen2, and catalog search.
- Operations: performance/health diagnostics, warehouse query insights, slow-query investigation.
- Migration: Databricks → Fabric, Synapse → Fabric, HDInsight → Fabric, end-to-end medallion
  (Bronze → Silver → Gold) architecture.

For **architecture/advisory, costing, or comparison** about Fabric (which platform to choose,
TCO, lakehouse vs warehouse vs Synapse), stay in the `microsoft-cloud-advisor` skill and the
Data Platform Advisory section of the agent — this reference is for hands-on Fabric operations.

## Bundles

Install via the GitHub Copilot CLI plugin marketplace:

```bash
# Add the marketplace
/plugin marketplace add microsoft/skills-for-fabric

# Full collection (authoring + consumption + operations + migration + e2e architecture)
/plugin install fabric-skills@fabric-collection

# Or install a focused bundle:
/plugin install fabric-authoring@fabric-collection    # REST APIs, CLI, notebooks, T-SQL, KQL, Dataflows Gen2, Eventstreams, semantic models
/plugin install fabric-consumption@fabric-collection   # read-only query/exploration across all workloads + catalog search
/plugin install fabric-operations@fabric-collection    # performance/health diagnostics, query insights, slow-query investigation
```

Workload filters (install a subset): `--filter "sqldw-*"`, `"spark-*"`, `"eventhouse-*"`, etc.

## Upstream Skills (24)

`sqldw-authoring-cli`, `sqldw-consumption-cli`, `sqldw-operations-cli`,
`spark-authoring-cli`, `spark-consumption-cli`, `spark-operations-cli`,
`eventhouse-authoring-cli`, `eventhouse-consumption-cli`,
`eventstream-authoring-cli`, `eventstream-consumption-cli`,
`dataflows-authoring-cli`, `dataflows-consumption-cli`, `dataflows-save-as-authoring-cli`,
`semantic-model-authoring`, `semantic-model-consumption`,
`activator-authoring-cli`, `activator-consumption-cli`,
`search-consumption-cli`, `e2e-medallion-architecture`, `fabriciq`, `check-updates`,
`databricks-migration`, `synapse-migration`, `hdinsight-migration`.

## Authentication

```bash
az login
az account get-access-token --resource https://api.fabric.microsoft.com
```

The skills authenticate to the Fabric REST API (`https://api.fabric.microsoft.com`) using the
Azure CLI token. MCP setup is documented upstream in `mcp-setup/README.md` and
`docs/mcp-servers-guide.md`.

## Relationship to this repo

- **`fabric` MCP server** (already configured in this agent, `@microsoft/fabric-mcp`): live Fabric
  tool calls (KQL queries, OneLake items, eventstreams) used by the agent's Data Platform Advisory.
- **This `skills-for-fabric` reference**: the broader installable skill collection for end-to-end
  Fabric authoring/consumption/operations/migration via Copilot CLI.
- **Fabric cost**: source Fabric capacity pricing from the **Azure Retail Prices API** via the
  `azure/pricing` MCP tool — `serviceName eq 'Microsoft Fabric'` returns both Consumption
  (per-CU-hour) and Reservation (1-Year / 3-Year capacity) meters. No separate pricing source is
  needed for Fabric in cost calculators.
