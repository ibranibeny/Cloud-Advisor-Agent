# Hub-Spoke Patterns Reference

## Mermaid Diagram Templates

### L100 -- Simple Hub-Spoke
```mermaid
graph TB
    HUB[Hub VNet] --> S1[Spoke 1]
    HUB --> S2[Spoke 2]
    HUB --> S3[Spoke 3]
    ON_PREM[On-Premises] --> HUB
```

### L200 -- Hub Services Labeled
```mermaid
graph TB
    subgraph Hub VNet
        FW[Azure Firewall]
        GW[VPN/ER Gateway]
        BAS[Azure Bastion]
        DNS[DNS Private Resolver]
    end
    subgraph Spoke - Production
        APP1[App Tier]
        DB1[Data Tier]
    end
    subgraph Spoke - Dev/Test
        APP2[App Tier]
        DB2[Data Tier]
    end
    ON_PREM[On-Premises] -->|VPN/ER| GW
    GW --> FW
    FW --> APP1
    FW --> APP2
```

### L300 -- With Subnets and IP Ranges
```mermaid
graph TB
    subgraph Hub VNet - 10.0.0.0/16
        subgraph GatewaySubnet - 10.0.1.0/24
            GW[VPN Gateway]
        end
        subgraph AzureFirewallSubnet - 10.0.2.0/24
            FW[Firewall Standard]
        end
        subgraph AzureBastionSubnet - 10.0.3.0/24
            BAS[Bastion]
        end
        subgraph DNSSubnet - 10.0.4.0/24
            DNS[DNS Resolver]
        end
    end
    subgraph Spoke Prod - 10.1.0.0/16
        subgraph AppSubnet - 10.1.1.0/24
            APP[App Servers]
        end
        subgraph DataSubnet - 10.1.2.0/24
            DB[SQL MI / PaaS]
        end
    end
    ON_PREM[On-Premises - 192.168.0.0/16] -->|S2S VPN| GW
    GW --> FW
    FW -->|UDR 10.1.0.0/16| APP
```

### L400 -- With Route Tables and Firewall Rules
```mermaid
graph TB
    subgraph Hub VNet - 10.0.0.0/16
        subgraph GatewaySubnet
            GW[VPN GW VpnGw1]
        end
        subgraph AzureFirewallSubnet
            FW["Firewall Premium<br/>IDPS + TLS Inspection"]
        end
        subgraph AzureBastionSubnet
            BAS[Bastion Standard]
        end
    end
    subgraph Spoke Prod - 10.1.0.0/16
        subgraph AppSubnet
            APP[VM Scale Set]
        end
        subgraph PESubnet
            PE[Private Endpoints]
        end
    end
    subgraph Route Table - Spoke
        RT["0.0.0.0/0 → Firewall IP<br/>10.0.0.0/16 → Firewall IP"]
    end
    ON_PREM -->|ER Circuit| GW
    GW --> FW
    FW -->|Network Rule: Allow 10.1.1.0/24:443| APP
    APP --> PE
    RT -.->|Applied to AppSubnet| APP
```

## Firewall SKU Decision Table

| Factor | Basic | Standard | Premium |
|--------|-------|----------|---------|
| Throughput | ~250 Mbps | ~30 Gbps | ~100 Gbps |
| IDPS | No | No | Yes |
| TLS Inspection | No | No | Yes |
| URL Filtering | Limited | FQDN-based | Full category-based |
| Threat Intel | Basic deny | Full deny/alert | Full + custom feeds |
| IP Groups | Limited | Yes | Yes |
| Monthly Approx | ~$285 | ~$730 | ~$1,752 |
| Best For | SMB, dev/test, PoC | Enterprise general purpose | Regulated, PCI, HIPAA |

## Cost Comparison Template

| Resource | Shared Hub (1x) | Per-Spoke (Nx) | Savings (N-1)x |
|----------|-----------------|----------------|----------------|
| Azure Firewall ({SKU}) | ${fw_monthly} | ${fw_monthly} x N | ${fw_monthly} x (N-1) |
| VPN Gateway ({sku}) | ${gw_monthly} | ${gw_monthly} x N | ${gw_monthly} x (N-1) |
| Azure Bastion Standard | ${bas_monthly} | ${bas_monthly} x N | ${bas_monthly} x (N-1) |
| DNS Private Resolver | ${dns_monthly} | ${dns_monthly} x N | ${dns_monthly} x (N-1) |
| **VNet Peering (overhead)** | $0.01/GB x GB x 2 x N | $0 | -${peering_total} |
| **Net Monthly Savings** | -- | -- | ${total_savings} |

## VNet Peering Cost Formula

```
peering_cost_monthly = spoke_count × data_gb_per_spoke × $0.01/GB × 2 (bidirectional)
```

Notes:
- Same-region peering: $0.01/GB each direction
- Cross-region peering: $0.02-$0.08/GB (varies by region pair)
- Always multiply by 2 for bidirectional traffic
- Compare peering cost against savings from shared hub resources
