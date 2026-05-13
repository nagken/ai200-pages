# Hands-On Labs - AI-200

Quick walkthroughs you can do in a free Azure subscription.

## Lab 1: Deploy a Python AOAI app to ACA with managed identity

1. `az containerapp env create -n aca-env -g rg --location eastus`
2. Build + push image: `az acr build -r myacr -t chat:1 .`
3. Create app: `az containerapp create -n chat -g rg --environment aca-env --image myacr.azurecr.io/chat:1 --system-assigned --registry-server myacr.azurecr.io --registry-identity system`
4. Grant `AcrPull` on the ACR to the app's identity.
5. Grant `Cognitive Services OpenAI User` on the AOAI resource to the same identity.
6. App uses `DefaultAzureCredential()` and the AOAI deployment name - no keys.

## Lab 2: RAG ingestion with AI Search integrated vectorization

1. Create AOAI deployment of `text-embedding-3-small` and `gpt-4o-mini`.
2. Create AI Search service (Standard) and a data source pointing at a blob container.
3. Run the `Import and vectorize data` wizard - it scaffolds skillset (chunk + embed) + indexer + index with vector field.
4. Query hybrid: top-10 BM25 + vector + semantic ranker rerank.

## Lab 3: Cosmos DB NoSQL vector container

1. Enable feature: `az cosmosdb update -n acct -g rg --capabilities EnableNoSQLVectorSearch`.
2. Create container with `vectorEmbeddingPolicy.path = /embedding`, `dataType = float32`, `dimensions = 1536`, `distanceFunction = cosine`.
3. Add an indexing policy with `vectorIndexes: [{path: "/embedding", type: "diskANN"}]`.
4. Query: `SELECT TOP 5 c.id, VectorDistance(c.embedding, @v) AS score FROM c ORDER BY VectorDistance(c.embedding, @v)`.

## Lab 4: Postgres + pgvector with `azure_ai`

1. Create flexible server; in `azure.extensions` enable `vector` and `azure_ai`.
2. `CREATE EXTENSION vector; CREATE EXTENSION azure_ai;`
3. `SELECT azure_ai.set_setting('azure_openai.endpoint', 'https://...');`
4. `INSERT INTO docs (text, embedding) VALUES ($1, azure_openai.create_embeddings('text-embedding-3-small', $1));`
5. `CREATE INDEX ON docs USING hnsw (embedding vector_cosine_ops);`

## Lab 5: APIM AI Gateway in front of AOAI

1. Import AOAI as backend in APIM.
2. Apply policies: `azure-openai-token-limit` (10000 TPM/sub), `azure-openai-emit-token-metric`, `llm-semantic-cache-lookup` + `-store`.
3. Create two AOAI deployments in different regions; `set-backend-service` round-robin with health probes.
4. Verify token metrics arrive in App Insights and 429s reflect the local cap, not the AOAI cap.

## Lab 6: Service Bus + ACA job for batch document processing

1. Create Service Bus queue + Premium namespace.
2. Wire Event Grid blob-uploaded -> Service Bus.
3. Create an ACA job (event-driven, KEDA SB scaler) that reads the queue, calls Document Intelligence + AOAI, writes to Cosmos.
4. Test poison handling: malformed message -> DLQ after 10 deliveries.
