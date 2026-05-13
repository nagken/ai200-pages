# AI-200 Glossary and Acronyms

| Term | Meaning |
|---|---|
| **ACA** | Azure Container Apps - serverless containers on managed Kubernetes (KEDA + Envoy). |
| **ACI** | Azure Container Instances - single container or container group, fastest cold start. |
| **ACR** | Azure Container Registry - private OCI registry. |
| **AGIC** | Application Gateway Ingress Controller for AKS. |
| **AKS** | Azure Kubernetes Service - managed Kubernetes. |
| **AOAI** | Azure OpenAI - tenanted hosting of OpenAI models on Azure. |
| **APIM** | Azure API Management - API gateway; supports AI gateway policies. |
| **AzureAD / Entra ID** | Microsoft Entra ID - identity provider; issues tokens for managed identities. |
| **Batch API (AOAI)** | Async bulk inference, ~50% cheaper, 24h SLA, JSONL in/out. |
| **Change feed** | Cosmos DB ordered log of mutations; consumed via lease container. |
| **Container Insights** | Azure Monitor add-on for AKS / ACA logs and metrics. |
| **CSI driver (Secrets Store)** | Mounts Key Vault secrets as files in pods. |
| **Dapr** | Distributed Application Runtime; sidecar for state/pubsub/secrets. |
| **DefaultAzureCredential** | Azure SDK helper that tries MI, CLI, env, VS Code in order. |
| **DiskANN** | Cosmos NoSQL vector index, optimized for >10k vectors. |
| **DLQ** | Dead-letter queue - poison messages parked for inspection. |
| **Durable Functions** | Workflow extension on Azure Functions (orchestrators + activities). |
| **Event Grid** | Push-based event routing service. |
| **Event Hubs** | High-throughput, partitioned event ingestion. |
| **HNSW** | Hierarchical Navigable Small World - approximate nearest neighbor index. |
| **IVFFlat** | Inverted-file flat ANN index; tunable via `lists` parameter. |
| **KAITO** | Kubernetes AI Toolchain Operator; auto-provisions GPU + serves LLMs on AKS. |
| **KEDA** | Kubernetes Event-Driven Autoscaler; powers ACA scale rules. |
| **Managed Identity** | Entra service principal whose lifecycle is bound to an Azure resource. |
| **OTel / OpenTelemetry** | Vendor-neutral observability standard; App Insights ingests OTel. |
| **pgvector** | PostgreSQL extension for vector similarity search. |
| **Private Endpoint** | NIC in your VNet that maps to a PaaS resource. |
| **RBAC (data plane)** | Roles applied at the Azure resource for data operations (e.g. `Cognitive Services User`). |
| **RediSearch** | Redis module for full-text + vector search. |
| **Semantic ranker** | AI Search reranker using a cross-encoder; reorders top 50. |
| **TPM** | Tokens per minute - AOAI deployment quota unit. |
| **Workload Identity** | OIDC federation between AKS service account and Entra app/MI. |
