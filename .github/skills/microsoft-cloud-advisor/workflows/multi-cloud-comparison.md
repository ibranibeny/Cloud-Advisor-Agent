# Multi-Cloud Comparison Workflow

## Trigger
Request contains: vs AWS, vs GCP, compare, multi-cloud, EKS, GKE, BigQuery, S3, service mapping, equivalent, map service, translate service, application stack, infrastructure stack.

## Procedure

### Step 1: Identify services to compare
Map the user's use case to equivalent services across providers.

### Step 2: Query each provider's MCP
- Azure: Use `mcp_azure_mcp_pricing`, `mcp_azure_mcp_cloudarchitect`
- AWS: Use `aws/*` tools for service catalog and pricing
- GCP: Use `gcp/*` tools for service catalog and pricing

### Step 3: Normalize results
Create comparable metrics: monthly cost, features, SLA, regions.

### Step 4: Generate comparison table
Minimum 3 criteria:
| Criteria | Azure | AWS | GCP |
|----------|-------|-----|-----|
| Service | {azure_service} | {aws_service} | {gcp_service} |
| Monthly Cost | ${X} | ${Y} | ${Z} |
| Key Feature | {feature} | {feature} | {feature} |
| Maturity | {rating} | {rating} | {rating} |
| Integration | {notes} | {notes} | {notes} |

### Step 5: Provide recommendation
May recommend non-Azure where appropriate. Always note tradeoffs of switching providers (migration effort, team skills, existing investments).

## Handling Partial Data
If a provider's MCP is unavailable:
- Mark that column with "Data unavailable"
- Note which MCP server needs to be installed
- Provide comparison with available data
- DO NOT block the entire comparison

## Cross-Cloud Service Mapping by Stack Layer

When asked to map/translate/find equivalent services, classify into stack layers:

| Stack | Definition | Answers |
|-------|-----------|---------|
| **Application Stack** | Services the workload code consumes directly | "What do I build on top of?" |
| **Infrastructure Stack** | Foundational services the app runs within | "What environment does my app run inside?" |

Always state which stack a mapping belongs to. When spanning both, split output into Application Stack and Infrastructure Stack tables.

See `references/service-mapping-tables.md` for the full cross-cloud reference mapping.

## Mapping Quality Rules
1. Live `aws/*` or `gcp/*` MCP data overrides reference tables -- cite the source
2. Flag non-1:1 mappings explicitly (e.g., Pub/Sub spans both Event Hubs and Service Bus)
3. When no equivalent exists, mark "No direct equivalent" and describe closest pattern
4. At L300+, add SKU/tier column and call out feature-parity gaps
