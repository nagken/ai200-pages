# AI-200 Exam Decision Reference (Cheatsheet)

> Single-page "when X then Y" lookup. Bullets / tables only.

## Identity and access

| Need | Pick | Notes |
|---|---|---|
| App -> AOAI / AI Search / Cosmos | **System-assigned managed identity** + scoped data role | No keys. |
| Multiple apps share principal | **User-assigned MI** | Same RBAC reused. |
| AKS pod -> Azure resource | **Workload Identity (OIDC)** | Federated SA token. |
| Pull from ACR | `AcrPull` on the registry | Required for ACA, AKS, App Service. |
| AOAI data role | `Cognitive Services User` (call) / `Cognitive Services OpenAI User` | RBAC at the AOAI resource. |
| Key Vault data role | `Key Vault Secrets User` (read), `... Officer` (write) | RBAC mode preferred. |

## Storage / data

| Workload | Store | Index |
|---|---|---|
| Multi-tenant low-ms vector | Cosmos DB NoSQL | DiskANN (>10k), quantizedFlat (small) |
| Mongo wire protocol + vector | Cosmos DB MongoDB vCore | HNSW (recall) / IVF (speed) |
| Relational + vector + SQL joins | Postgres + `pgvector` | HNSW / IVFFlat |
| Sub-ms cache, semantic cache | Azure Managed Redis | RediSearch VECTOR |
| Hybrid enterprise search | Azure AI Search | Integrated vectorization + semantic ranker |

## Compute / hosting

| Need | Pick |
|---|---|
| Scale-to-zero HTTP / KEDA / Dapr | **Azure Container Apps** |
| GPU inference, KAITO, custom CRDs | **AKS** (with GPU node pool) |
| One-shot / quick burst | **Container Instances** |
| Web app + slots from a Docker image | **App Service for Containers** |
| Long-running orchestration | **Durable Functions** |
| Bulk async LLM scoring | **AOAI Batch API** |

## Messaging / events

| Need | Pick |
|---|---|
| Ordered per session, DLQ | **Service Bus** queue with sessions |
| Million events/sec, replay | **Event Hubs** + checkpoint store |
| React to Azure resource event | **Event Grid** |
| Pub/sub abstraction in containers | **Dapr pub/sub** (ACA) |

## Networking

- **Private endpoint** + `publicNetworkAccess: Disabled` on AOAI / Cosmos / Search / KV / ACR for production.
- ACA: **VNet integration** + internal ingress for backend services.
- AKS: **Azure CNI Overlay** (default for new), **private API server** for prod.

## Monitoring / governance

- **App Insights + OpenTelemetry GenAI conventions** for prompt, tokens, model.
- **Diagnostic settings** to Log Analytics for AOAI, AI Search, Cosmos, Key Vault.
- **Container Insights** for AKS / ACA.
- **APIM AI Gateway** for token limits, semantic cache, safety policies.

## Resilience / DR

- AOAI: deploy in 2+ regions, **APIM load-balanced backends**, retry on 429 with `Retry-After`.
- Cosmos: multi-region writes for active-active; single-region writes for cost.
- Service Bus / Event Hubs: **Premium / Dedicated** tier + **geo-DR pairing**.
- ACA / AKS: deploy across availability zones; AKS uptime SLA tier for prod.
