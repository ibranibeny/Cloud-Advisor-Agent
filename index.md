---
layout: home
title: Home
nav_order: 1
description: "GitHub Copilot custom agent for Microsoft Cloud & Azure advisory — architecture, migration, cost calculators, and presentation generation powered by live MCP context."
permalink: /
---

# Cloud Advisor Agent
{: .fs-9 }

A GitHub Copilot custom agent (`@cloud-advisor`) that turns Copilot into a **Microsoft Cloud & Azure expert advisor** — architecture design, migration planning, Well-Architected reviews, Excel cost calculators, and presentation-ready output, all grounded in live MCP context.
{: .fs-6 .fw-300 }

[Get Started]({{ site.baseurl }}{% link docs/installation.md %}){: .btn .btn-primary .fs-5 .mb-4 .mb-md-0 .mr-2 }
[View Capabilities]({{ site.baseurl }}{% link docs/capabilities.md %}){: .btn .fs-5 .mb-4 .mb-md-0 }

---

## What It Does

| # | Capability | Output |
|---|------------|--------|
| 1 | **Azure architecture advisory** | Reference architectures with Mermaid diagrams |
| 2 | **Migration planning (6R)** | Rehost/Replatform/Refactor decisions + steps |
| 3 | **Hub-spoke network design** | CIDR allocation, subnets, NSGs, firewall routing |
| 4 | **Cost calculators** | Excel workbooks with PAYG / RI / AHB breakdowns |
| 5 | **Well-Architected reviews** | WAF pillar assessments with live Azure context |
| 6 | **Document processing** | Advisory from uploaded Word / PDF / PPT files |
| 7 | **Presentation generation** | Slide-ready markdown formatted for PowerPoint |
| 8 | **Multi-cloud comparison** | Azure ↔ AWS ↔ GCP service mapping |
| 9 | **Enterprise infra planning + IaC** | Deployable Bicep or Terraform via the Microsoft `azure-enterprise-infra-planner` skill |

---

## Bundled Skills

| Skill | Source | Use For |
|-------|--------|---------|
| **microsoft-cloud-advisor** | This repo | Advisory, cost calculators, multi-cloud comparison, WAF reviews, presentations |
| **azure-enterprise-infra-planner** | [microsoft/azure-skills](https://github.com/microsoft/azure-skills/tree/main/skills/azure-enterprise-infra-planner) (MIT) | Architect/provision enterprise infrastructure and generate deployable Bicep or Terraform via a 7-phase workflow |

The agent uses `microsoft-cloud-advisor` for advisory, costing, and comparison, and escalates to `azure-enterprise-infra-planner` when you ask it to generate deployable IaC or a provisioning plan.

---

## Architecture at a Glance

| Component | Technology | Purpose |
|-----------|-----------|---------|
| 🤖 Agent Runtime | GitHub Copilot Chat | Orchestrates advisory workflow & L-levels |
| ☁️ Azure Context | Azure MCP Server | Live pricing, SKU availability, region checks |
| 📚 Documentation | Microsoft Learn MCP | Architecture best-practice validation |
| 📊 Cost Calculator | Excel MCP Server | Pricing workbook generation |
| 📄 Document Processing | Markitdown MCP | Word / PDF / PPT → markdown ingestion |

---

## L-Level Depth Control

Append an L-level to any prompt to control response depth:

| Level | Audience | Depth |
|-------|----------|-------|
| **L100** | Executives | Summary, business value, high-level diagrams |
| **L200** | Architects (default) | Service mapping, topology, trade-offs |
| **L300** | Engineers | Configuration detail, SKUs, sizing rationale |
| **L400** | Implementation | Subnet CIDRs, NSG rules, costed comparisons, migration steps |

---

## Key Design Decisions

- **Live MCP grounding** — pricing, SKUs, and docs come from Azure & Microsoft Learn MCP servers, not stale training data
- **Hub-and-spoke topology** — single hub with Azure Firewall; isolated spokes per workload
- **Cost transparency** — every estimate compares PAYG vs Reserved Instances with Azure Hybrid Benefit
- **Right-size, never downsize** — map on-prem servers to equal-or-larger Azure SKUs
- **Artifact-ready output** — Excel calculators, Mermaid diagrams, and PPT-ready markdown

---

{: .note }
> The Excel cost-calculator capability requires **Windows** for the Excel MCP Server (COM automation). All other capabilities work cross-platform.

---

## Quick Start

In VS Code Copilot Chat (agent mode):

```
@cloud-advisor Design a hub-spoke network for my Azure landing zone
```

Migration planning with a full L400 cost comparison:

```
@cloud-advisor Migrate my on-prem 3-tier app to Azure with hub-spoke,
Azure Hybrid Benefit, and 3-year Reserved Instances. Target East US 2. L400
```

See the [Installation guide]({{ site.baseurl }}{% link docs/installation.md %}) to set up the agent and MCP servers.
