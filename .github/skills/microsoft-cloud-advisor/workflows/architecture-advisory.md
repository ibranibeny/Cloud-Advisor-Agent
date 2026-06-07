# Architecture Advisory Workflow

## Trigger
Request classified as: architecture, design, hub-spoke, landing zone, workload, Well-Architected, reference architecture.

## Procedure

### Step 1: Classify the architecture request
- Compatibility: Can workload X run on Azure? What SKU/service fits?
- Migration: How to move from current state to Azure?
- Modernization: How to evolve an app to cloud-native?
- AI/ML: What Azure AI services fit this use case?
- Data: Which data platform (Fabric, Synapse, SQL, Cosmos) suits the scenario?
- Hybrid: How to span on-prem and cloud?
- Reference Architecture: What is the recommended topology?

### Step 2: Research with Microsoft Learn MCP

1. **`microsoft_docs_search`** -- Search for relevant documentation chunks (start broad, then narrow)
2. **`microsoft_code_sample_search`** -- Find implementation examples when code is needed
3. **`microsoft_docs_fetch`** -- Fetch full pages for detailed walkthroughs

Example queries:
- "migrate SQL Server to Azure SQL managed instance"
- "Microsoft Fabric lakehouse architecture"
- "Azure AI Foundry deployment patterns"
- "hybrid connectivity Azure Arc"
- "Well-Architected Framework reliability pillar"

### Step 3: Augment with Azure MCP Context

| Tool | Use Case |
|------|----------|
| `mcp_azure_mcp_cloudarchitect` | Architecture recommendations and patterns |
| `mcp_azure_mcp_wellarchitectedframework` | WAF pillar assessments |
| `mcp_azure_mcp_pricing` | Cost estimates and SKU pricing |
| `mcp_azure_mcp_documentation` | Azure-specific docs |
| `mcp_azure_mcp_get_azure_bestpractices` | Validate recommendations against authoritative best practices |
| `mcp_azure_mcp_compute` | VM sizing and availability |
| `mcp_azure_mcp_sql` | SQL service options |
| `mcp_azure_mcp_cosmos` | Cosmos DB guidance |
| `mcp_azure_mcp_aks` | Kubernetes/container options |
| `mcp_azure_mcp_containerapps` | Serverless container guidance |
| `mcp_azure_mcp_appservice` | Web app hosting |
| `mcp_azure_mcp_advisor` | Optimization recommendations |
| `mcp_azure_mcp_quota` | Region/SKU availability |
| `mcp_azure_mcp_get_available_region` | Region selection |

**Mandatory sequence**:
1. Query `mcp_azure_mcp_cloudarchitect` for reference architecture patterns
2. Use `microsoft_docs_search` for official architecture guidance
3. Check `mcp_azure_mcp_wellarchitectedframework` for relevant pillar considerations
4. Query `mcp_azure_mcp_pricing` for cost context on recommended services
5. Use compute/AKS/appservice tools for sizing
6. Call `mcp_azure_mcp_get_azure_bestpractices` to validate before finalizing

### Step 4: Synthesize and Present

Structure per L-level:

| Section | L100 | L200 | L300 | L400 |
|---------|------|------|------|------|
| Summary | 1-paragraph | + recommendation | full detail | full detail |
| Mermaid Diagram | high-level | component-level | + networking | + failure paths |
| Service Mapping | -- | table | + SKU/sizing | + justification |
| WAF Pillar Reference | -- | relevant pillars | all 5 pillars | + tradeoff analysis |
| Next Steps | bullets | phased | step-by-step | + CLI/Bicep snippets |

### Service Mapping Table Format

| Requirement | Azure Service | SKU | Justification |
|-------------|---------------|-----|---------------|
| {requirement} | {service} | {sku} | {why this fits} |

### WAF Pillar Requirement

Every architecture response at L200+ MUST reference at least one pillar tradeoff:
- Reliability
- Security
- Cost Optimization
- Operational Excellence
- Performance Efficiency
