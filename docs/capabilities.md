---
layout: default
title: Capabilities
nav_order: 3
permalink: /capabilities/
---

# Capabilities
{: .no_toc }

1. TOC
{:toc}

---

The `@cloud-advisor` agent combines Azure domain expertise with live MCP context to deliver
advisory output at four depth levels (L100–L400). Each capability below can be invoked directly
in Copilot Chat.

---

## 1. Azure Architecture Advisory

Reference architectures, modernization paths, and topology recommendations rendered as
Mermaid diagrams with service-level rationale.

```
@cloud-advisor Design an Azure architecture for a multi-region e-commerce platform
```

---

## 2. Migration Planning (6R)

Applies the 6R framework (Rehost, Replatform, Refactor, Rearchitect, Rebuild, Replace) to
on-premises workloads, with phased migration steps and tooling guidance.

```
@cloud-advisor How do I migrate my on-prem SQL Server 2019 to Azure?
```

---

## 3. Hub-Spoke Network Design

Produces hub-and-spoke topologies with CIDR allocation, subnet sizing, NSG rules, and
forced-tunnel routing through Azure Firewall.

- Hub VNet (`/16`) with AzureFirewallSubnet (`/26`), GatewaySubnet (`/27`), AzureBastionSubnet (`/26`)
- Non-overlapping spoke VNets per workload with web / app / data subnets
- UDR `0.0.0.0/0` → Azure Firewall private IP; deny-all-inbound NSG defaults

```
@cloud-advisor Design a hub-spoke network for my Azure landing zone
```

---

## 4. Cost Calculators

Generates Excel workbooks (via the Excel MCP Server) comparing Pay-as-you-go, 1-year, and
3-year Reserved Instances, with Azure Hybrid Benefit savings.

```
@cloud-advisor Generate an Azure pricing calculator for a 3-tier web app with AKS
```

---

## 5. Well-Architected Reviews

Assesses a design against the five WAF pillars (Reliability, Security, Cost Optimization,
Operational Excellence, Performance Efficiency) using live Azure context.

```
@cloud-advisor Run a Well-Architected review on my AKS-based platform
```

---

## 6. Document Processing

Extracts content from uploaded Word, PDF, or PowerPoint files (via Markitdown MCP) and
returns advisory grounded in the document.

```
[Attach a .docx or .pptx file] @cloud-advisor Analyze this architecture document
```

---

## 7. Presentation Generation

Creates slide-ready markdown formatted for PowerPoint generation, tuned to the requested
L-level audience.

```
@cloud-advisor Create a 10-slide executive presentation on our Azure migration at L100
```

---

## 8. Multi-Cloud Comparison

Maps services across Azure, AWS, and GCP for both the application and infrastructure stacks.

```
@cloud-advisor Compare Azure Container Apps vs AWS Fargate vs GCP Cloud Run
```

---

## 9. Enterprise Infrastructure Planning + IaC

Architects enterprise Azure infrastructure end-to-end and generates deployable **Bicep or Terraform** using the bundled Microsoft `azure-enterprise-infra-planner` skill (MIT). It runs a 7-phase workflow — extract insights, research best practices, research resources, generate plan, verify, generate IaC, deploy — with WAF alignment and pairing/constraint checks.

```
@cloud-advisor Plan a hub-spoke landing zone with firewall and private endpoints, then generate Bicep
```

---

## 10. Cost Management (Live Billing)

Queries actual billed Azure spend, forecasts future costs, and finds optimization opportunities on **deployed** resources using the bundled Microsoft `azure-cost` skill (MIT). It calls the Cost Management Query and Forecast APIs and includes optimization workflows (orphaned-resource detection, VM rightsizing, Redis/AKS cost analysis, Azure Quick Review). Use `azure-cost` for live billing data; use `microsoft-cloud-advisor` with `azure/pricing` to estimate prices for new/proposed designs.

```
@cloud-advisor Show my Azure cost breakdown by service this month and forecast end-of-month spend
```

---

## Bundled Skills

| Skill | Source | Use For |
|-------|--------|---------|
| **microsoft-cloud-advisor** | This repo | Advisory, cost calculators, multi-cloud comparison, WAF reviews, presentations |
| **azure-enterprise-infra-planner** | [microsoft/azure-skills](https://github.com/microsoft/azure-skills/tree/main/skills/azure-enterprise-infra-planner) (MIT) | Architect/provision enterprise infrastructure and generate deployable Bicep or Terraform |
| **azure-cost** | [microsoft/azure-skills](https://github.com/microsoft/azure-skills/tree/main/skills/azure-cost) (MIT) | Query historical Azure spend, forecast future costs, and optimize/reduce waste on deployed resources |

---

## MCP Servers Used

| MCP Server | Tools | Purpose |
|------------|-------|---------|
| **Azure MCP** | `mcp_azure_mcp_*` | Live Azure context, pricing, architecture |
| **Microsoft Learn MCP** | `mcp_microsoft-lea_*` | Official documentation search & fetch |
| **Excel MCP** | `mcp_excel-mcp_*` | Cost-calculator workbook generation |
| **Markitdown MCP** | `mcp_microsoft_mar_convert_to_markdown` | Document conversion (Word/PDF/PPT → MD) |
