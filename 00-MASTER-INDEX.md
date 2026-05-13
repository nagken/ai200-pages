# AI-200 - Microsoft Azure AI Cloud Developer Associate - Visual Study Guide

> Concept-only study aid. No exam questions reproduced. Source PDF (if any) stays local + gitignored.

**Skills outline:** https://aka.ms/AI200-StudyGuide

## The 4 Exam Domains - Mind Map

```mermaid
mindmap
  root((AI-200))
    Containerized Solutions
      Azure Container Registry
        ACR Tasks build scan
        Geo-replication
        AcrPull managed identity
      Azure Container Apps
        Revisions ingress
        KEDA HTTP scalers
        Scale-to-zero
        Dapr sidecar
        ACA jobs batch
      Azure Kubernetes Service
        GPU node pools
        KAITO operator
        Workload Identity OIDC
        AGIC private API server
      Container Instances
        Burst virtual nodes
        One-shot jobs
      Secrets and Networking
        Secrets Store CSI
        Key Vault references
        Private endpoints
    AI Data Management Services
      Cosmos DB NoSQL
        DiskANN quantizedFlat
        Vector embedding policy
        Change feed sync
      Cosmos DB MongoDB vCore
        HNSW IVF cosmosSearch
      PostgreSQL flexible
        pgvector HNSW IVFFlat
        azure_ai extension
      Azure Managed Redis
        RediSearch vector
        Semantic cache
      Azure AI Search
        Hybrid BM25 vector
        Integrated vectorization
        Semantic ranker
    Connect and Consume Services
      Azure OpenAI
        Managed identity auth
        Streaming SSE
        Batch API JSONL
      Service Bus
        Sessions FIFO
        Dead letter queue
      Event Hubs
        Partitioned log
        Checkpoint store
      Event Grid
        Push routing filters
      Async patterns
        Durable Functions
        ACA jobs
      APIM AI Gateway
        Token limit policy
        Semantic cache
        Content safety
        Load balanced backends
    Secure Monitor Troubleshoot
      Identity
        System assigned MI
        User assigned MI
        Workload Identity
      Secrets
        Key Vault RBAC
        CSI driver mounts
      Network
        Private endpoints
        Disable public access
      Observability
        App Insights OTel
        GenAI conventions
        Container Insights
        Diagnostic settings
      Troubleshoot
        AOAI 429 Retry-After
        Image pull backoff
        Quota and capacity
```

## Domain map

```mermaid
flowchart LR
    Master["AI-200 Master Index"]
    D01["Containerized Solutions"]
    Master --> D01
    D02["AI Data Management Services"]
    Master --> D02
    D03["Connect and Consume Azure Services"]
    Master --> D03
    D04["Secure Monitor and Troubleshoot"]
    Master --> D04
```

## Domain weights

```mermaid
pie showData
    title AI-200 domain weights
    "Containerized Solutions" : 25
    "AI Data Management Services" : 30
    "Connect and Consume Azure Services" : 25
    "Secure Monitor and Troubleshoot" : 20
```

> Click a slice / legend label to jump to that chapter.

## Recommended study order

```mermaid
gantt
    title Suggested study plan
    dateFormat X
    axisFormat Day %d
    section Plan
    Containerized Solutions :t1, 0, 2d
    AI Data Management Services :t2, after t1, 2d
    Connect and Consume Azure Services :t3, after t2, 2d
    Secure Monitor and Troubleshoot :t4, after t3, 2d
```

---

**Next:** open [01-containerized-solutions.md](01-containerized-solutions.md)
