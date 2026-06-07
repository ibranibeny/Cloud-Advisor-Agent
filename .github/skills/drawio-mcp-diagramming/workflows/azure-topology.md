# Azure Network Topology Patterns

When creating **Azure infrastructure network diagrams** with VNets, subnets, and network isolation.

## Canvas Sizing
- Use larger canvas for complex infrastructure: `pageWidth="1900" pageHeight="1500"`
- Standard canvas may be too small for multi-VNet topologies

## VNet and Subnet Visualization
- **VNets**: Use thick borders (`strokeWidth=4`) and large containers
  - DMZ VNet: Yellow (`fillColor=#fff2cc`, `strokeColor=#d6b656`)
  - Internal VNet: Green (`fillColor=#d5e8d4`, `strokeColor=#82b366`)
  - Management Zone: Blue (`fillColor=#dae8fc`, `strokeColor=#6c8ebf`)
- **Subnets**: Use dashed borders (`strokeWidth=2`, `dashed=1`, `dashPattern=8 8`)
  - Position subnet containers **inside** VNet containers
  - Use lighter shades of parent VNet color
  - Label with subnet name and CIDR (e.g., "Application Subnet - 10.x.2.0/24")
- **Delegated Subnets**: Add delegation info to label (e.g., "PostgreSQL Subnet - 10.x.4.0/24 (Delegated to Microsoft.DBforPostgreSQL/flexibleServers)")

## Resource Positioning
- Position all resources **inside their respective subnet containers**
- VMs, databases, load balancers must be visually contained within their subnets
- This clearly shows network isolation boundaries

## Traffic Flow Visualization
- **Label all traffic arrows** with protocols and ports, using this colour palette:
  - HTTPS:443 — **Azure blue** (`#0078D4`, thick solid) for internet ingress; prominent but professional
  - HTTP:80/8080/8090/8095 — **Teal** (`#00897B`, solid) for backend pool traffic; signals allowed/healthy east-west flow
  - PostgreSQL:5432 — **Indigo** (`#5C6BC0`, dashed) for database connections; purple/indigo conventionally marks the data tier
  - NFS/Gluster — **Green** (`#43A047`, solid) for shared storage flows
  - RBAC/Identity/SMTP — **Amber** (`#F57C00`, dashed) for management/control-plane traffic
  - Denied/Blocked (WAF, NSG deny rules) — **Red** (`#C62828`) — reserve red exclusively for blocked or denied traffic
- Use `edgeStyle=orthogonalEdgeStyle` for clean routing
- Include `<Array>` waypoints for complex routing
- **Direction animation on key edges**: `flowAnimation=1;` adds a moving dot along a connector arrow, making ingress paths, egress routes, and replication flows readable at a glance — the effect renders in SVG export and draw.io desktop and works on any edge style. Before generating the diagram, ask the user: *"Would you like any of the traffic arrows animated to show flow direction? If so, which ones?"* Apply `flowAnimation=1;` only to the edges they identify. Example style for an animated internet ingress arrow: `style="edgeStyle=orthogonalEdgeStyle;flowAnimation=1;strokeWidth=3;strokeColor=#0078D4;"`

## Essential Components

Include two annotation boxes in every Azure topology diagram:
1. **Network Isolation Explanation** (top-left, `fillColor=#fff9cc`) — visual conventions: VNet thick borders, subnet dashed borders, NSG/DNS notes
2. **Zone Separation** — VNet Peering zone (grey `fillColor=#f5f5f5`) and External Services zone (orange `fillColor=#ffe6cc`)

For a complete example, see [../references/topology-patterns.md](../references/topology-patterns.md).

## Professional Topology Checklist (Azure)
- [ ] VNets have thick borders (strokeWidth=4)
- [ ] Subnets have dashed borders (strokeWidth=2, dashPattern=8 8)
- [ ] All resources positioned inside their subnets
- [ ] Traffic arrows labelled with protocols and ports using the standard colour palette
- [ ] Network isolation explanation box included
- [ ] Color-coded zones for different purposes
- [ ] Canvas sized appropriately (1900x1500 for complex infra)
- [ ] VNet peering connections shown in separate zone
- [ ] External services grouped in separate zone
- [ ] Animation preference confirmed with user before generating (*"Would you like any flow arrows animated? If so, which ones?"*)
