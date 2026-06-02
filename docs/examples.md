---
layout: default
title: Examples
nav_order: 4
permalink: /examples/
---

# Examples
{: .no_toc }

1. TOC
{:toc}

---

## L400 Migration Advisory

A full implementation-ready (L400) advisory — costed comparison, architecture spec, and
migration steps — for a 3-tier lift-and-shift to a hub-spoke landing zone.

**Prompt**

> "We have an on-prem 3-tier app — 2 web servers (8 vCPU / 32 GB), 2 app servers
> (16 vCPU / 64 GB), and a SQL Server 2019 cluster (32 vCPU / 256 GB). Give me an L400
> lift-and-shift plan to Azure with a hub-spoke network, a full cost comparison including
> Azure Hybrid Benefit and 3-year Reserved Instances, the architecture spec, and the
> migration steps. Target region East US 2."

### Solution Summary

| Tier | Source (each) | Count | Azure target | SKU |
|------|---------------|-------|--------------|-----|
| Web | 8 vCPU / 32 GB | 2 | Azure VM | `D8s_v5` |
| App | 16 vCPU / 64 GB | 2 | Azure VM | `D16s_v5` |
| Data | 32 vCPU / 256 GB | 1 cluster | Azure SQL MI | Business Critical, 32 vCore Gen5 |

### Cost Comparison (whole solution, East US 2, monthly)

| Strategy | Monthly | vs PAYG |
|----------|---------|---------|
| PAYG, no AHB | $11,128 | baseline |
| PAYG + AHB | $8,448 | −24% |
| **3Y RI + AHB (recommended)** | **$6,678** | **−40%** |

{: .tip }
> Combining Azure Hybrid Benefit with 3-year Reserved Instances saves roughly **$4,450/month
> (~$53K/year)** versus on-demand list pricing.

### Architecture Spec

Hub `10.0.0.0/16` (AzureFirewallSubnet `/26`, AzureBastionSubnet `/26`, GatewaySubnet `/27`);
spoke `10.1.0.0/16` (snet-web `10.1.0.0/24`, snet-app `10.1.1.0/24`, snet-data `10.1.2.0/24`
delegated to SQL MI). Deny-all-inbound NSGs, `0.0.0.0/0` UDR → Azure Firewall, Private DNS for
the SQL MI private endpoint, plus Defender for Cloud and DDoS Standard.

### Migration Steps

1. **Assess** with Azure Migrate + DMA
2. **Deploy** the hub-spoke landing zone via Bicep
3. **Establish** ExpressRoute / VPN connectivity
4. **Migrate data** online to SQL MI via DMS
5. **Replicate** web/app VMs across availability zones
6. **Test & validate** routing, DNS, security posture
7. **Cutover** and decommission source

[Read the full L400 example on GitHub](https://github.com/ibranibeny/Cloud-Advisor-Agent/blob/main/examples/l400-migration-advisory.md){: .btn .btn-primary }

---

## More Examples

- [Migration advisory (abbreviated)](https://github.com/ibranibeny/Cloud-Advisor-Agent/blob/main/examples/migration-advisory.md)
- [L400 deep-dive (full)](https://github.com/ibranibeny/Cloud-Advisor-Agent/blob/main/examples/l400-migration-advisory.md)
