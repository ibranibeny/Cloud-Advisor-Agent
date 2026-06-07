# Cross-Cloud Service Mapping Tables

## Application Stack

| Category | Azure | AWS | GCP |
|----------|-------|-----|-----|
| VM Compute | Virtual Machines | EC2 | Compute Engine |
| Containers (orchestration) | AKS | EKS | GKE |
| Serverless Containers | Container Apps | Fargate / App Runner | Cloud Run |
| Functions (FaaS) | Azure Functions | Lambda | Cloud Functions |
| PaaS Web Hosting | App Service | Elastic Beanstalk | App Engine |
| Relational DB (managed) | Azure SQL / DB for PostgreSQL/MySQL | RDS / Aurora | Cloud SQL / AlloyDB |
| NoSQL Document | Cosmos DB | DynamoDB | Firestore / Datastore |
| In-Memory Cache | Azure Cache for Redis | ElastiCache | Memorystore |
| Data Warehouse | Fabric / Synapse | Redshift | BigQuery |
| Event Streaming | Event Hubs | Kinesis / MSK | Pub/Sub |
| Message Queue | Service Bus | SQS | Pub/Sub / Cloud Tasks |
| Event Routing | Event Grid | EventBridge | Eventarc |
| API Management | API Management | API Gateway | Apigee / API Gateway |
| AI/ML Platform | Azure AI Foundry / Azure ML | SageMaker / Bedrock | Vertex AI |
| Workflow Orchestration | Logic Apps / Durable Functions | Step Functions | Workflows |

## Infrastructure Stack

| Category | Azure | AWS | GCP |
|----------|-------|-----|-----|
| Virtual Network | VNet | VPC | VPC |
| Network Peering | VNet Peering | VPC Peering / Transit Gateway | VPC Network Peering |
| Load Balancer (L4) | Azure Load Balancer | NLB | Cloud Load Balancing (TCP/UDP) |
| App Gateway / WAF (L7) | Application Gateway + WAF | ALB + AWS WAF | Cloud LB (HTTP) + Cloud Armor |
| CDN | Azure Front Door / CDN | CloudFront | Cloud CDN |
| DNS | Azure DNS | Route 53 | Cloud DNS |
| Firewall | Azure Firewall | Network Firewall | Cloud Firewall / Cloud NGFW |
| Private Connectivity | ExpressRoute | Direct Connect | Cloud Interconnect |
| VPN | VPN Gateway | Site-to-Site VPN | Cloud VPN |
| Object Storage | Blob Storage | S3 | Cloud Storage |
| Block Storage | Managed Disks | EBS | Persistent Disk |
| File Storage | Azure Files | EFS / FSx | Filestore |
| Secrets Management | Key Vault | Secrets Manager / KMS | Secret Manager / Cloud KMS |
| IAM / Identity | Microsoft Entra ID | IAM / IAM Identity Center | Cloud IAM |
| Governance / Policy | Azure Policy | Service Control Policies / Config | Organization Policy |
| Monitoring / Logs | Azure Monitor | CloudWatch | Cloud Monitoring / Logging |
| Infrastructure as Code | Bicep / ARM | CloudFormation | Deployment Manager / Config Controller |

## Mapping Quality Rules

1. Live `aws/*` or `gcp/*` MCP data overrides this reference table -- cite the source
2. Flag non-1:1 mappings explicitly (e.g., GCP Pub/Sub spans both Event Hubs and Service Bus)
3. When no equivalent exists, mark "No direct equivalent" and describe closest pattern
4. At L300+, add SKU/tier column and call out feature-parity gaps
