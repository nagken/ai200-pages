# AI-200 Common Pitfalls

Test-day traps and "looks right but isn't" patterns.

| Wrong instinct | Why it's wrong | Right answer |
|---|---|---|
| Use ACR admin user for image pull | Violates security baseline; rotates poorly | Managed identity + <code>AcrPull</code> role |
| Set ACA <code>minReplicas: 0</code> with TCP scale rule | HTTP scale-to-zero only fires for HTTP scaler | Use HTTP scaler, or KEDA event scaler that supports zero |
| Run an LLM HTTP function for 5 min | Default 230s function timeout | Stream (SSE) or move to Durable Functions / ACA job |
| Add vector index to an existing Cosmos NoSQL container | <code>vectorEmbeddingPolicy</code> is immutable after creation | Create new container; backfill via change feed |
| Use Cosmos cross-region writes for cost savings | Multi-region writes cost more, not less | Single-region writes for cost; multi-region for availability |
| Trust <code>kubectl logs</code> for AKS retention | Pods are ephemeral, logs vanish on restart | Container Insights -> Log Analytics |
| Disable AOAI public access without a PE | Outage; client can't reach the endpoint | Create private endpoint + private DNS zone first |
| Use one APIM token-limit bucket per IP | Shared NAT collapses tenants into one bucket | Key on subscription / managed identity / API key |
| Treat AOAI 429 as transient with exponential backoff | Throttling - <code>Retry-After</code> is authoritative | Honor <code>Retry-After</code>; consider multi-region LB |
| Use Service Bus peek-lock without lock renewal | Message redelivered while still being processed -> dupes | Renew lock or use sessions; ensure idempotency |
| Cache embeddings without versioning the model | Old vectors silently mis-match new query embeddings | Include <code>embedding_model_id</code> in cache keys; rebuild on bump |
| Bake API keys into container image | Leaks in registry; rotation = rebuild | Managed identity or Key Vault reference |
| App Insights with default sampling at high QPS | 5x bill; ingestion throttled | Configure adaptive / fixed sampling appropriate to traffic |
| Use IVFFlat without enough <code>lists</code> | Long index scans, bad recall | Set <code>lists</code> approximately sqrt(rows); benchmark |
| Embed JSON-stringified arrays in Cosmos | Indexer stores as string, vector ops fail | Store as <code>number[]</code>; declare <code>vectorEmbeddingPolicy</code> at create |
| Reuse Event Hubs consumer group across services | Cursors collide; events seem missing | One consumer group per independent consumer |
| Skip <code>azure-monitor-opentelemetry</code> for LLM tracing | Custom OTel exporters miss GenAI conventions | Use the Azure Monitor distro >= 1.6 |
