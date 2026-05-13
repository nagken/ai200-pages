# Reference Architectures - AI-200

## RAG on Azure (containerized)

```mermaid
flowchart LR
    User["Browser / mobile"] --> APIM["API Management<br/>(AI gateway)"]
    APIM --> ACA["Azure Container Apps<br/>(FastAPI / Python)"]
    ACA -->|MI| AOAI["Azure OpenAI<br/>(chat + embeddings)"]
    ACA -->|MI| Search["Azure AI Search<br/>(hybrid + semantic)"]
    ACA -->|MI| Redis["Managed Redis<br/>(semantic cache)"]
    Blob["Blob Storage<br/>(source docs)"] --> Idx["AI Search indexer<br/>(integrated vectorization)"]
    Idx --> Search
    ACA --> AI["App Insights + OTel"]
```

## Event-driven document processing

```mermaid
flowchart LR
    Up["Blob upload"] --> EG["Event Grid"]
    EG --> SB["Service Bus queue"]
    SB --> Job["ACA job<br/>(batch worker)"]
    Job -->|MI| DI["AI Document Intelligence"]
    Job -->|MI| AOAI["Azure OpenAI<br/>(structured extraction)"]
    Job --> Cosmos["Cosmos DB NoSQL<br/>(vector + metadata)"]
    Job --> AI["App Insights"]
```

## GPU model serving on AKS with KAITO

```mermaid
flowchart LR
    Client --> Ing["AGIC / NGINX ingress"]
    Ing --> Svc["KAITO workspace<br/>(vLLM / transformers)"]
    Svc --> GPU["GPU node pool<br/>(NCv3 / NCadsH100v5)"]
    Svc --> KV["Key Vault<br/>(model creds)"]
    Svc --> AI["App Insights + Container Insights"]
```

## Async chat with streaming + APIM cost guardrails

```mermaid
sequenceDiagram
    participant U as User
    participant A as APIM
    participant App as ACA app
    participant O as AOAI
    U->>A: POST /chat (token)
    A->>A: jwt, token-limit, content-safety
    A->>App: forward
    App->>O: chat.completions stream
    O-->>App: SSE chunks
    App-->>A: SSE chunks
    A-->>U: SSE chunks
    A->>A: emit-token-metric -> App Insights
```
