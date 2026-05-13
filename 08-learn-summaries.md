# Microsoft Learn Summaries

One-paragraph summary per official AI-200 learning path.

## Implement container application hosting on Azure

ACR concepts (registries, repositories, tags, manifests), authentication via managed identity (`AcrPull`), and ACR Tasks for build/patch. Deploy to App Service, ACI, ACA, AKS - and how to choose. Image security (Defender for Containers, signed images).

## Deploy and manage apps on Azure Container Apps

ACA environments, apps, revisions, replicas. Ingress (external/internal, traffic split between revisions), scale rules (HTTP, KEDA event-driven), Dapr integration, ACA jobs (manual/triggered/scheduled), secret management (`secretRef`, Key Vault references), and managed identity-based ACR pull.

## Deploy and monitor applications on Azure Kubernetes Service

AKS clusters (Automatic vs Standard), system + user node pools (including GPU pools), networking (Azure CNI Overlay, private API server), Workload Identity (OIDC federation), AGIC, Container Insights, and KAITO for serving open-source LLMs on GPU.

## Develop AI solutions with Azure Cosmos DB for NoSQL

Vector search via `vectorEmbeddingPolicy` and DiskANN/quantizedFlat indexes, querying with `VectorDistance(...)`, change feed for keeping vector indexes in sync, multi-region writes, partition strategy for embedding workloads, integrated cache for read-heavy patterns.

## Develop AI solutions with Azure Database for PostgreSQL

`pgvector` HNSW/IVFFlat indexes, the **`azure_ai`** extension for in-database calls to Azure OpenAI (embeddings, completions), the **`azure_local_ai`** extension for in-server inference, hybrid SQL + vector queries, and operational concerns (`maintenance_work_mem`, parallel index build).

## Enhance AI solutions with Azure Managed Redis

RediSearch vector indexes (HNSW), JSON-typed documents, semantic caching for LLM responses, key eviction strategies, Active-Active geo-replication, and the Azure-managed connection-string + entra-auth flow.

## Integrate backend services for AI solutions

Service Bus queues / topics / sessions / DLQ, Event Hubs partitioning + EventProcessorClient, Event Grid topics + filters, Durable Functions for fan-out / fan-in, and APIM AI Gateway policies for cost control and safety.
