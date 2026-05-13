# Flashcards: Active Recall

> Click any card to reveal the answer. Use the **Domain pager bottom-right** to switch between exam areas.

<section class="fc-section" data-fc-title="Containerized Solutions">
<h2>1 - Containerized Solutions</h2>

<div class="flashcard-grid">

<div class="flashcard"><div class="fc-q">Default ACA scaler that allows scale-to-zero?</div><div class="fc-a">The <strong>HTTP scaler</strong>. Non-HTTP traffic needs a KEDA event source (queue length, etc.) and at least 1 replica unless the event scaler supports zero.</div></div>

<div class="flashcard"><div class="fc-q">Role needed for managed identity to pull from ACR?</div><div class="fc-a"><code>AcrPull</code> on the registry resource. Avoid the registry admin user.</div></div>

<div class="flashcard"><div class="fc-q">How do AKS pods request a GPU?</div><div class="fc-a">Set <code>resources.limits</code> with <code>nvidia.com/gpu: 1</code> (or more) and ensure the pod is scheduled on a GPU node pool.</div></div>

<div class="flashcard"><div class="fc-q">Difference between ACA jobs and ACA apps?</div><div class="fc-a"><strong>Jobs</strong> run to completion (manual, triggered, scheduled). <strong>Apps</strong> are long-running services with revisions.</div></div>

<div class="flashcard"><div class="fc-q">When to choose ACI over ACA?</div><div class="fc-a">One-off workloads, build agents, or burst from AKS via virtual nodes. ACI has faster cold start but no orchestration.</div></div>

<div class="flashcard"><div class="fc-q">What does KAITO solve on AKS?</div><div class="fc-a">Auto-provisions GPU nodes and deploys open-source LLMs (vLLM, transformers) via custom resource - no manual node-pool sizing.</div></div>

</div>
</section>

<section class="fc-section" data-fc-title="AI Data Management">
<h2>2 - AI Data Management Services</h2>

<div class="flashcard-grid">

<div class="flashcard"><div class="fc-q">Cosmos DB NoSQL vector index for >10k docs?</div><div class="fc-a"><strong>DiskANN</strong>. Use <code>quantizedFlat</code> only for small collections.</div></div>

<div class="flashcard"><div class="fc-q">Can you add a vector embedding policy to an existing Cosmos NoSQL container?</div><div class="fc-a">No - it must be defined at <strong>container creation</strong>. Recreate the container or migrate.</div></div>

<div class="flashcard"><div class="fc-q">What does the <code>azure_ai</code> Postgres extension do?</div><div class="fc-a">Calls Azure OpenAI from SQL: <code>azure_openai.create_embeddings()</code>, <code>azure_openai.create_chat_completion()</code>.</div></div>

<div class="flashcard"><div class="fc-q">pgvector recall vs speed: HNSW or IVFFlat?</div><div class="fc-a"><strong>HNSW</strong> = highest recall, slower build, more RAM. <strong>IVFFlat</strong> = faster build, tune <code>lists</code> approximately sqrt(rows).</div></div>

<div class="flashcard"><div class="fc-q">Redis vector index command?</div><div class="fc-a"><code>FT.CREATE idx ON HASH SCHEMA embedding VECTOR HNSW 6 TYPE FLOAT32 DIM 1536 DISTANCE_METRIC COSINE</code>.</div></div>

<div class="flashcard"><div class="fc-q">AI Search hybrid query =</div><div class="fc-a">BM25 (text) + vector similarity in a single request, optionally reranked by the <strong>semantic ranker</strong> (cross-encoder, top 50).</div></div>

<div class="flashcard"><div class="fc-q">How to keep a Cosmos-backed vector index fresh?</div><div class="fc-a"><strong>Change feed</strong> -> Function/ACA job -> embed -> upsert. Lease container provides exactly-once semantics.</div></div>

</div>
</section>

<section class="fc-section" data-fc-title="Connect and Consume">
<h2>3 - Connect and Consume Azure Services</h2>

<div class="flashcard-grid">

<div class="flashcard"><div class="fc-q">Best auth for app -> AOAI?</div><div class="fc-a"><strong>Managed identity</strong> + role <code>Cognitive Services User</code> (or <code>Cognitive Services OpenAI User</code>). No keys.</div></div>

<div class="flashcard"><div class="fc-q">Service Bus feature for FIFO per customer?</div><div class="fc-a"><strong>Sessions</strong> - the receiver locks the session; messages with same <code>SessionId</code> processed in order.</div></div>

<div class="flashcard"><div class="fc-q">Event Hubs at-least-once requires what?</div><div class="fc-a">A <strong>checkpoint store</strong> (typically Blob) used by EventProcessorClient.</div></div>

<div class="flashcard"><div class="fc-q">When to choose AOAI Batch API?</div><div class="fc-a">Bulk async scoring with no realtime SLA. ~50% cheaper, 24h completion window, JSONL in/out via blob.</div></div>

<div class="flashcard"><div class="fc-q">APIM policy that caps tokens per consumer?</div><div class="fc-a"><code>azure-openai-token-limit</code>. Pair with <code>azure-openai-emit-token-metric</code> for App Insights metrics.</div></div>

<div class="flashcard"><div class="fc-q">AOAI long-running call best practice?</div><div class="fc-a">Enable streaming (SSE) or move to Durable Functions; default HTTP timeouts kill non-streamed long completions.</div></div>

</div>
</section>

<section class="fc-section" data-fc-title="Secure Monitor Troubleshoot">
<h2>4 - Secure, Monitor, Troubleshoot</h2>

<div class="flashcard-grid">

<div class="flashcard"><div class="fc-q">AKS pod auth to Azure resources without secrets?</div><div class="fc-a"><strong>Workload Identity</strong> - OIDC federation between the cluster service account and an Entra app / managed identity.</div></div>

<div class="flashcard"><div class="fc-q">Mount Key Vault secrets in an AKS pod?</div><div class="fc-a"><strong>Secrets Store CSI driver</strong> + Workload Identity. Auto-rotates without app restart.</div></div>

<div class="flashcard"><div class="fc-q">AOAI returns 429. What's the right retry?</div><div class="fc-a">Honor <code>Retry-After</code> header. Don't blind-retry. Add a second deployment / region behind APIM load-balancing if persistent.</div></div>

<div class="flashcard"><div class="fc-q">Where do AOAI prompt token counts land for Application Insights?</div><div class="fc-a">Via <strong>OpenTelemetry GenAI semantic conventions</strong> (<code>gen_ai.usage.input_tokens</code>) emitted by the Azure Monitor OpenTelemetry distro.</div></div>

<div class="flashcard"><div class="fc-q">Why does <code>publicNetworkAccess: Disabled</code> on AOAI break the app?</div><div class="fc-a">No private endpoint configured first. Always create the PE + DNS zone before flipping the flag.</div></div>

<div class="flashcard"><div class="fc-q">Container Insights table for AKS pod CPU?</div><div class="fc-a"><code>Perf</code> table (CPU/memory counters); pod metadata in <code>KubePodInventory</code>; events in <code>KubeEvents</code>.</div></div>

</div>
</section>
