# Hub-and-Spoke Architecture Diagram Templates

Use these Mermaid diagram templates at the appropriate L-level depth.

## L100 — Executive Overview

Simple hub + spoke topology. No internal detail.

```mermaid
graph TD
    Hub["Hub VNet"]
    Spoke1["Spoke 1"]
    Spoke2["Spoke 2"]
    Spoke3["Spoke 3"]

    Hub --- Spoke1
    Hub --- Spoke2
    Hub --- Spoke3
```

**Use when**: L100 depth, executive audience, first introduction to hub-spoke concept.

---

## L200 — Architecture Overview

Hub services labeled, spokes named by purpose.

```mermaid
graph TD
    subgraph Hub["Hub VNet — Shared Services"]
        FW["Azure Firewall"]
        GW["VPN/ExpressRoute Gateway"]
        BAS["Azure Bastion"]
        DNS["DNS Private Resolver"]
    end

    subgraph Spoke1["Spoke 1 — Production"]
        App1["Web App"]
    end

    subgraph Spoke2["Spoke 2 — Dev/Test"]
        App2["Dev Environment"]
    end

    subgraph Spoke3["Spoke 3 — Data"]
        App3["Analytics Platform"]
    end

    Hub ---|"VNet Peering"| Spoke1
    Hub ---|"VNet Peering"| Spoke2
    Hub ---|"VNet Peering"| Spoke3
```

**Use when**: L200 depth, IT architects, showing what services live in each VNet.

---

## L300 — Engineering Detail

Subnet-level detail with IP ranges and NSG references.

```mermaid
graph TD
    subgraph Hub["Hub VNet (10.0.0.0/16)"]
        subgraph HubSubnets["Hub Subnets"]
            GWSub["GatewaySubnet<br/>10.0.1.0/26"]
            FWSub["AzureFirewallSubnet<br/>10.0.2.0/26"]
            FWMgmt["AzureFirewallManagementSubnet<br/>10.0.3.0/26"]
            BASSub["AzureBastionSubnet<br/>10.0.4.0/26"]
            DNSSub["DNS Resolver<br/>10.0.5.0/28"]
        end
    end

    subgraph Spoke1["Spoke 1 — Production (10.1.0.0/16)"]
        S1App["App Subnet<br/>10.1.1.0/24<br/>NSG: nsg-spoke1-app"]
        S1Data["Data Subnet<br/>10.1.2.0/24<br/>NSG: nsg-spoke1-data"]
    end

    subgraph Spoke2["Spoke 2 — Dev/Test (10.2.0.0/16)"]
        S2App["App Subnet<br/>10.2.1.0/24<br/>NSG: nsg-spoke2-app"]
    end

    subgraph OnPrem["On-Premises Datacenter"]
        DC["Corporate Network"]
    end

    Hub ---|"Peering<br/>allowGatewayTransit"| Spoke1
    Hub ---|"Peering<br/>allowGatewayTransit"| Spoke2
    GWSub ---|"ExpressRoute/VPN"| OnPrem
```

**Use when**: L300 depth, engineers implementing the network, need subnet and IP planning.

---

## L400 — Deep Dive / Specialist

Route tables, Firewall policy rules, DNS Private Zones, and NSG flow.

```mermaid
graph TD
    subgraph Hub["Hub VNet (10.0.0.0/16)"]
        subgraph FWPolicy["Firewall Policy: fw-policy-hub"]
            FWRuleNet["Network Rule Collection:<br/>Allow-Spoke-to-Spoke<br/>Allow-Spoke-to-Internet"]
            FWRuleApp["Application Rule Collection:<br/>Allow-FQDN-*.microsoft.com<br/>Allow-FQDN-*.azure.com"]
        end
        GWSub["GatewaySubnet<br/>10.0.1.0/26"]
        FWSub["AzureFirewallSubnet<br/>10.0.2.0/26<br/>PIP: pip-fw-hub"]
        FWMgmt["AzureFirewallManagementSubnet<br/>10.0.3.0/26"]
        BASSub["AzureBastionSubnet<br/>10.0.4.0/26"]
        DNSSub["DNS Private Resolver<br/>10.0.5.0/28<br/>Inbound: 10.0.5.4"]
    end

    subgraph Spoke1["Spoke 1 — Production (10.1.0.0/16)"]
        S1App["App Subnet 10.1.1.0/24"]
        S1UDR["UDR: rt-spoke1<br/>0.0.0.0/0 → 10.0.2.4 (Firewall)"]
        S1DNS["DNS: 10.0.5.4 (Hub Resolver)"]
    end

    subgraph DNSZones["DNS Private Zones (linked to Hub)"]
        PZ1["privatelink.database.windows.net"]
        PZ2["privatelink.blob.core.windows.net"]
        PZ3["privatelink.vaultcore.azure.net"]
    end

    FWSub --> FWPolicy
    S1App --> S1UDR
    S1UDR -->|"Next hop: Firewall"| FWSub
    DNSSub --> DNSZones
    Hub ---|"Peering + GW Transit"| Spoke1
    GWSub ---|"ExpressRoute"| OnPrem["On-Premises"]
```

**Use when**: L400 depth, network security specialists, need UDR flow, firewall rules, DNS resolution chain.

---

## On-Premises Connectivity Element

Add this when user specifies on-prem or hybrid connectivity:

```mermaid
graph TD
    subgraph OnPrem["On-Premises Datacenter"]
        DC["Corporate Network<br/>192.168.0.0/16"]
        VPNDEV["VPN Device / Router"]
    end

    subgraph Hub["Hub VNet"]
        GW["VPN/ExpressRoute Gateway<br/>GatewaySubnet"]
    end

    VPNDEV ---|"Site-to-Site VPN<br/>or ExpressRoute"| GW

    %% Gateway transit annotation
    GW -.-|"allowGatewayTransit: true<br/>Spokes use useRemoteGateways: true"| SpokeNote["All spokes access on-prem<br/>via hub gateway"]
```

---

## Collapsed Spoke Group (>10 Spokes)

When rendering more than 10 spokes, use a collapsed group node:

```mermaid
graph TD
    subgraph Hub["Hub VNet — Shared Services"]
        FW["Azure Firewall"]
        GW["Gateway"]
        BAS["Bastion"]
    end

    Spoke1["Spoke 1 — Production"]
    Spoke2["Spoke 2 — Staging"]
    Spoke3["Spoke 3 — Dev"]
    SpokeGroup["Spoke Group<br/>(N spokes)"]

    Hub ---|"Peering"| Spoke1
    Hub ---|"Peering"| Spoke2
    Hub ---|"Peering"| Spoke3
    Hub ---|"Peering"| SpokeGroup
```

**Rule**: Render up to 10 spokes individually. Beyond 10, use the collapsed "Spoke Group (N spokes)" node.
