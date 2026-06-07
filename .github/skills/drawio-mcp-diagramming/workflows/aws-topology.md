# AWS Network Topology Patterns

When creating **AWS infrastructure network diagrams** with VPCs, subnets, and network isolation.

## Canvas Sizing
- Use larger canvas for complex infrastructure: `pageWidth="1900" pageHeight="1500"`
- Standard canvas may be too small for multi-VPC/multi-account topologies

## VPC and Subnet Visualization
- **VPCs**: Use thick borders (`strokeWidth=4`) and large containers
  - Production VPC: Green (`fillColor=#d5e8d4`, `strokeColor=#82b366`)
  - Development VPC: Blue (`fillColor=#dae8fc`, `strokeColor=#6c8ebf`)
  - Shared Services VPC: Yellow (`fillColor=#fff2cc`, `strokeColor=#d6b656`)
- **Subnets**: Use dashed borders (`strokeWidth=2`, `dashed=1`, `dashPattern=8 8`)
  - Public Subnets: Light green (`fillColor=#e6f4ea`, `strokeColor=#82b366`)
  - Private Subnets: Light blue (`fillColor=#EFF7FF`, `strokeColor=#6c8ebf`)
  - Isolated Subnets (databases): Light orange (`fillColor=#fff3e0`, `strokeColor=#e6821e`)
  - Position subnet containers **inside** VPC containers
  - Label with subnet name, AZ, and CIDR (e.g., "Public Subnet A - us-east-1a - 10.x.1.0/24")
- **Availability Zones**: Use light grey container inside VPC to group subnets per AZ

## Resource Positioning
- Position all resources **inside their respective subnet containers**
- EC2 instances, RDS, Lambda, etc. must be visually contained within their subnets
- Internet-facing resources (ALB, NAT Gateway, Bastion) go in **public subnets**
- Application servers / ECS tasks go in **private subnets**
- Databases (RDS, ElastiCache) go in **isolated subnets** with no outbound internet

## Traffic Flow Visualization
- **Label all traffic arrows** with protocols and ports, using the same colour palette as Azure:
  - HTTPS:443 *(internet ingress)* — **Azure blue** (`#0078D4`, thick solid) for external traffic entering via ALB/CloudFront
  - HTTP:80→HTTPS redirect — **Teal** (`#00897B`, solid) for healthy/redirected traffic
  - Port 5432/3306 — **Indigo** (`#5C6BC0`, dashed) for database connections
  - HTTPS:443 *(internal AWS service calls)* — **Green** (`#43A047`, solid) for traffic to VPC Endpoints and AWS-managed services (S3, SSM, Secrets Manager, etc.)
  - SSH:22 / SSM — **Amber** (`#F57C00`, dashed) for management / Bastion access
  - Denied/Blocked (WAF, Security Group deny rules) — **Red** (`#C62828`) — reserve red exclusively for blocked traffic
- Use `edgeStyle=orthogonalEdgeStyle` for clean routing
- Show NAT Gateway path for private subnet → internet egress
- **Direction animation on key edges**: `flowAnimation=1;` adds a moving dot along a connector arrow, making ingress paths, egress routes, and data-transfer flows readable at a glance — the effect renders in SVG export and draw.io desktop and can be applied to any edge style. Before generating the diagram, ask the user: *"Would you like any of the traffic arrows animated to show flow direction? If so, which ones?"* Apply `flowAnimation=1;` only to the edges they identify. Example style for an animated ingress path: `style="edgeStyle=orthogonalEdgeStyle;flowAnimation=1;strokeWidth=3;strokeColor=#0078D4;"`

## Essential Components

Include two annotation boxes in every AWS topology diagram:
1. **Network Isolation Explanation** (top-left) — visual conventions: VPC thick borders, subnet tiers (public/private/isolated), SG/NACL notes, VPC Endpoints
2. **Zone Separation** — Internet/Edge zone (orange), VPC Peering/Transit Gateway zone (grey), AWS Managed Services zone (purple)

For a complete example, see [../references/topology-patterns.md](../references/topology-patterns.md).

## Professional Topology Checklist (AWS)
- [ ] VPCs have thick borders (strokeWidth=4) and are colour-coded by environment
- [ ] Subnets have dashed borders (strokeWidth=2, dashPattern=8 8) and are colour-coded by tier (public/private/isolated)
- [ ] Availability Zone containers group subnets per AZ
- [ ] All resources positioned inside their respective subnets
- [ ] Internet Gateway and NAT Gateway shown for public/private egress
- [ ] Traffic arrows labelled with protocols and ports using the standard colour palette
- [ ] Security Group boundaries annotated where important
- [ ] Network isolation explanation box included
- [ ] Canvas sized appropriately (1900x1500 for complex infra)
- [ ] VPC Peering / Transit Gateway shown in separate zone
- [ ] Edge/internet services (CloudFront, Route53, WAF) in separate zone
- [ ] Animation preference confirmed with user before generating (*"Would you like any flow arrows animated? If so, which ones?"*)
