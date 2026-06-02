# Feature Specification: Hub-and-Spoke Network Architecture Skill

**Feature Branch**: `002-hub-spoke-networking`

**Created**: 2026-06-02

**Status**: Draft

**Input**: User description: "Implement the feature specification based on the updated constitution — add hub-and-spoke networking as a core skill so that best practice and price comparison questions always use hub shared resources including VNet peering, multiple spoke or single spoke patterns."

## User Scenarios & Testing *(mandatory)*

### User Story 1 - Best Practice Network Architecture Advisory (Priority: P1)

A cloud architect asks the agent for best practices on Azure networking. The agent responds with a hub-and-spoke topology as the recommended pattern, explaining centralized hub resources (Azure Firewall, VPN/ExpressRoute Gateway, Azure Bastion, DNS) and spoke isolation.

**Why this priority**: Hub-spoke is the foundational Azure Landing Zone pattern — every networking question should default to this model per Microsoft best practices.

**Independent Test**: Ask the agent "What's the best practice for Azure networking?" and verify it recommends hub-spoke with shared hub resources, VNet peering, and spoke isolation.

**Acceptance Scenarios**:

1. **Given** a user asks about Azure networking best practices, **When** the agent classifies the request, **Then** it routes to the Architecture/Hub-Spoke domain and responds with a hub-spoke topology diagram and explanation.
2. **Given** a user asks about connecting multiple environments (dev/test/prod), **When** the agent generates architecture guidance, **Then** it recommends separate spoke VNets per environment peered to a shared hub.
3. **Given** a user does not specify a network topology, **When** they ask a general Azure architecture question involving networking, **Then** the agent defaults to hub-spoke as the recommended pattern.

---

### User Story 2 - Price Comparison with Hub Shared Resources (Priority: P1)

A cost-conscious IT manager asks for a price comparison between deploying shared hub resources vs. duplicating resources per spoke. The agent produces a cost breakdown showing savings from centralized Firewall, VPN Gateway, and Bastion in the hub.

**Why this priority**: Cost optimization through shared infrastructure is a primary value driver for hub-spoke adoption. Users must see quantified savings.

**Independent Test**: Ask the agent "Compare cost of hub-spoke vs per-VNet firewall" and verify it produces a comparison table with monthly cost savings.

**Acceptance Scenarios**:

1. **Given** a user asks for price comparison of networking options, **When** the agent generates cost analysis, **Then** it includes a table showing per-resource cost in hub (1x) vs. duplicated per-spoke (Nx) with calculated savings.
2. **Given** a user specifies 4 spokes, **When** the agent calculates costs, **Then** it shows savings = cost × (N-1) for each shared hub resource (Firewall, Gateway, Bastion).
3. **Given** a user asks about VNet peering costs, **When** the agent responds, **Then** it includes per-GB peering cost ($0.01/GB same-region) as the inherent cost of the hub-spoke model alongside the savings from shared resources.

---

### User Story 3 - Single Spoke vs Multiple Spokes Decision (Priority: P2)

An architect asks whether to use a single spoke or multiple spokes for their workloads. The agent provides a decision matrix covering isolation, cost, compliance, and management overhead factors.

**Why this priority**: Spoke topology is a critical early design decision that impacts security boundaries, cost, and operational complexity.

**Independent Test**: Ask "Should I use one spoke or multiple spokes?" and verify the agent produces a decision table with factors and recommendations per scenario.

**Acceptance Scenarios**:

1. **Given** a user asks about single vs. multiple spokes, **When** the agent responds, **Then** it provides a comparison table with at least 5 factors (isolation, cost, compliance, management, scalability).
2. **Given** a user describes a small dev/test environment, **When** asked about spoke topology, **Then** the agent recommends a single spoke with rationale (simpler, lower cost, sufficient for non-regulated workloads).
3. **Given** a user describes a production environment with compliance requirements, **When** asked about spoke topology, **Then** the agent recommends multiple spokes with rationale (fault isolation, compliance boundaries, team separation).

---

### User Story 4 - Hub-Spoke Architecture Diagram Generation (Priority: P2)

A user requests a network architecture diagram. The agent produces a Mermaid diagram showing hub VNet with shared services, spoke VNets with workloads, VNet peering connections, and on-premises connectivity.

**Why this priority**: Visual architecture diagrams are essential for stakeholder communication and implementation planning.

**Independent Test**: Ask "Show me a hub-spoke architecture diagram" and verify the output contains a valid Mermaid diagram with hub, spokes, peering, and on-prem connectivity.

**Acceptance Scenarios**:

1. **Given** a user requests a hub-spoke diagram, **When** the agent generates the response, **Then** it includes a Mermaid graph with labeled hub VNet (containing Firewall, Gateway, Bastion, DNS), at least 2 spoke VNets, and VNet peering connections.
2. **Given** a user specifies on-premises connectivity, **When** the diagram is generated, **Then** it includes ExpressRoute/VPN connection between the hub gateway and on-premises.
3. **Given** a user specifies the L300 depth level, **When** the diagram is generated, **Then** it includes subnet-level detail (GatewaySubnet, AzureFirewallSubnet, AzureBastionSubnet) and IP address ranges.

---

### User Story 5 - Landing Zone Hub-Spoke Foundation (Priority: P3)

A user asks about Azure Landing Zones. The agent presents hub-spoke as the foundational network topology for Enterprise-Scale Landing Zones, explaining how it maps to the management group hierarchy and connectivity subscription.

**Why this priority**: Landing Zones are the enterprise onboarding pattern for Azure — hub-spoke is always the networking foundation.

**Independent Test**: Ask "How do I set up an Azure Landing Zone?" and verify hub-spoke topology is presented as the networking foundation.

**Acceptance Scenarios**:

1. **Given** a user asks about Azure Landing Zones, **When** the agent responds, **Then** it includes hub-spoke as the networking foundation with reference to the connectivity subscription pattern.
2. **Given** a user asks about landing zone network design at L300+, **When** the agent responds, **Then** it includes Azure Firewall policies, route tables for forced tunneling, and DNS Private Resolver in the hub.

---

### Edge Cases

- What happens when a user asks about mesh networking instead of hub-spoke? → Agent acknowledges mesh as an alternative but explains when hub-spoke is preferred (compliance, cost control, centralized security).
- How does the agent handle Azure Virtual WAN vs. traditional hub-spoke? → Agent distinguishes between customer-managed hub-spoke and Azure Virtual WAN (Microsoft-managed hub) with a qualitative tradeoff comparison by default; a quantified cost table is produced only when the user explicitly asks for a cost comparison.
- What if the user has existing flat networking (no hub-spoke)? → Agent recommends migration path to hub-spoke with phased approach.

## Requirements *(mandatory)*

### Functional Requirements

- **FR-001**: Agent MUST recommend hub-and-spoke as the default network topology when users ask about Azure networking best practices.
- **FR-002**: Agent MUST include a price comparison showing hub shared-resource savings (Firewall, VPN Gateway, Bastion) whenever cost or pricing questions involve networking components. Agent MUST ask the user which Azure Firewall SKU (Basic/Standard/Premium) to use before generating cost figures. Agent MUST also ask the user for the target Azure region before generating cost figures, since networking prices are region-specific; if the region is unknown, the agent MUST default to East US 2, clearly labeled as an editable assumption.
- **FR-003**: Agent MUST provide a single-spoke vs. multiple-spoke decision matrix when users ask about spoke topology choices.
- **FR-004**: Agent MUST generate Mermaid hub-spoke diagrams with correct hub services (Firewall, Gateway, Bastion, DNS), spoke workloads, and VNet peering when architecture diagrams are requested. Diagrams MUST render up to 10 individual spokes; beyond 10, summarize as "N spokes × unit cost" in tables and use a collapsed representation in diagrams.
- **FR-005**: Agent MUST include VNet peering cost ($0.01/GB same-region ingress + egress) in all hub-spoke cost analyses. The agent MUST ask the user for expected monthly inter-VNet data transfer (GB/month per spoke) before quantifying peering cost; if the user cannot provide it, the agent MUST use a documented default of 1,000 GB/month/spoke, clearly labeled as an editable assumption.
- **FR-006**: Agent MUST present hub-spoke as the foundational networking pattern when users ask about Azure Landing Zones.
- **FR-007**: Agent MUST apply hub-spoke framing when any of these triggers are detected: "best practice", "price comparison", "VNet peering", "multiple environments", "landing zone", "network topology".
- **FR-008**: Agent MUST support L100–L400 depth levels for hub-spoke responses, scaling detail from high-level overview to subnet-level design with IP ranges and route tables.
- **FR-009**: Agent MUST distinguish between customer-managed hub-spoke and Azure Virtual WAN when users ask about managed networking options. By default the agent provides a qualitative tradeoff comparison; it MUST produce a quantified Virtual WAN vs. hub-spoke cost table (VWAN hub unit + connection units + data processing) only when the user explicitly requests a cost comparison.
- **FR-010**: Agent MUST include security controls (NSG, Azure Firewall rules, Private Endpoints) in hub-spoke recommendations at L300+ depth.

### Key Entities

- **Hub VNet**: Centralized virtual network containing shared services (Firewall, Gateway, Bastion, DNS Private Resolver). Single instance per region.
- **Spoke VNet**: Workload-isolated virtual network peered to the hub. One per environment/team/compliance boundary.
- **VNet Peering**: Bidirectional connection between hub and spoke enabling traffic flow. Supports gateway transit from hub to spokes.
- **Shared Hub Resources**: Azure Firewall, VPN/ExpressRoute Gateway, Azure Bastion, DNS Private Resolver — deployed once in the hub, shared by all spokes.

## Success Criteria *(mandatory)*

### Measurable Outcomes

- **SC-001**: 100% of networking best-practice questions receive hub-spoke as the recommended topology.
- **SC-002**: All price comparisons involving networking include a hub shared-resource savings table with quantified monthly savings.
- **SC-003**: Users can determine single vs. multiple spoke choice within one interaction using the provided decision matrix.
- **SC-004**: All hub-spoke Mermaid diagrams render correctly and include minimum hub services (Firewall, Gateway, Bastion) plus at least 2 spokes.
- **SC-005**: Landing Zone questions always reference hub-spoke as the networking foundation within the first response section.
- **SC-006**: Hub-spoke responses adapt correctly to all 4 L-levels (L100 high-level → L400 subnet/route detail).

## Assumptions

- Users are deploying workloads in Azure public cloud (not Azure Stack or sovereign clouds unless specified).
- Single-region topology assumed: one hub VNet and all spokes in the same Azure region. Multi-region (hub-per-region) is out of scope for this feature.
- Azure Firewall, VPN Gateway, and Azure Bastion are generally available in the user's target region.
- Pricing data is sourced from the Live Azure Pricing API via MCP tool (`mcp_azure_mcp_pricing`) for real-time cost figures; the agent must gracefully degrade to approximate estimates if the MCP tool is unavailable.
- The agent already has the cloud-advisor.agent.md file with the hub-spoke section (US7) implemented — this spec covers validation and any remaining SKILL.md/template updates needed.
- Hub-spoke is the default recommendation; alternative topologies (mesh, Virtual WAN) are presented only when explicitly asked or when the scenario clearly warrants them.
- VNet peering data-transfer volume is user-supplied (GB/month per spoke); when unknown, a documented default of 1,000 GB/month/spoke is used as an editable assumption rather than inventing a figure.
- Cost tables are priced against a user-supplied target Azure region (region-specific pricing per the constitution's Domain 6); when the region is unknown, East US 2 is used as a documented, editable default assumption.

## Clarifications

### Session 2026-06-02

- Q: How should the agent source pricing data for cost comparisons? → A: Live Azure Pricing API via MCP tool (real-time pricing)
- Q: What is the maximum number of spokes the agent should support in cost calculations and diagrams? → A: Up to 10 spokes rendered individually; beyond 10, summarize as "N spokes × unit cost"
- Q: Should the agent support multi-region hub-spoke or single-region only by default? → A: Single-region only (one hub, all spokes same region)
- Q: What default Azure Firewall SKU should the agent recommend in hub-spoke cost calculations? → A: No default; ask user which SKU (Basic/Standard/Premium) before calculating costs
- Q: What monthly data-transfer volume should the agent assume for VNet peering cost? → A: Ask the user for GB/month per spoke; if unknown, use a documented default of 1,000 GB/month/spoke as an editable assumption
- Q: Which Azure region should hub-spoke cost tables be priced against? → A: Ask the user for the target region; if unknown, default to East US 2 as an editable assumption
- Q: Is a quantified Virtual WAN vs. hub-spoke cost comparison in scope? → A: Qualitative tradeoffs by default; quantified VWAN cost comparison only on explicit user request
