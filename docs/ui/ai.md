# AI services and providers

Part of the [OSO website and v1 UI design](../ui-v1.md).

AI is used in six places, each a separate task that a community configures on its own ([OIP-15](https://github.com/open-science-org/OIPs/blob/master/OIPS/oip-15.md)):

| Task | Where it appears | Phase | Needs |
| --- | --- | --- | --- |
| Chat about an idea | Idea page | A | A capable chat model; answers grounded in the idea and its neighbours, with citations |
| Edge assessment | Graph evidence panel, Submit step 3 | A (curated slice, offline), B (submissions) | Two models from different families for anything that can move value |
| Similarity and duplicate search | Search, pre-screen | A | Hybrid search: keyword ranking (BM25) and embedding similarity run together and their results are merged by reciprocal rank fusion, as [Paperclip](https://paperclip.gxl.ai/) (GXL) does ([description](https://gxl.ai/blog/biomedical-literature-as-a-filesystem/)); the fusion method is from Cormack, Clarke and Büttcher, "Reciprocal Rank Fusion outperforms Condorcet and individual Rank Learning Methods", SIGIR 2009. No generative model needed |
| Pre-screen report | Validator queue, My work | B | Two models; structured output |
| Review draft notes | Review form | B | A chat model; always labelled as AI |
| Domain tagging | Ingestion, idea pages | A | A small, cheap model |

**Swappable by design.** All model calls go through one adapter in the API, never from the browser. The adapter speaks the widely supported OpenAI-style chat-completions interface, which OpenRouter, the major hosted providers and local servers (Ollama, llama.cpp, vLLM, LM Studio) offer; embeddings use the matching embeddings interface where the provider has one (check before relying on it; local servers are the safe default). Switching provider or model is a configuration change, not a code change:

```yaml
ai:
  providers:
    openrouter: { base_url: "https://openrouter.ai/api/v1", api_key_env: OPENROUTER_API_KEY }
    local:      { base_url: "http://localhost:11434/v1" }   # Ollama on a laptop or server
  tasks:
    chat:        { provider: openrouter, model: "<pinned model id>", daily_budget_usd: "<cap>" }
    edge_assess: [ { provider: openrouter, model: "<model A>" }, { provider: local, model: "<open-weight model B>" } ]
    embeddings:  { provider: local, model: "<embedding model>" }
    domain_tags: { provider: local, model: "<small model>" }
```

In Phase B these task settings move from this file into each community's setup ([Modular and customizable design](modularity.md)), so a community can pin its own models. Every output that feeds a decision is recorded with its model, version and prompt ID (OIP-13, OIP-15), so swapping a model never changes past results.

**Options compared**

| Option | Pros | Cons | Use it for |
| --- | --- | --- | --- |
| **OpenRouter (one API key, many models)** | One key and one bill for models from many companies; easy to switch and to get two model families; some free, rate-limited models | Pay per use; free models are rate-limited and may change or disappear; check each model's data policy before sending unpublished work | **Recommended default for Phase A:** chat and the two-model edge assessments |
| Specialized literature tools as AI services, for example [Paperclip](https://paperclip.gxl.ai/) (GXL) | Agent-ready full-text search and reading over millions of papers (strongest for biomedicine) | Hosted, closed index; terms and pricing to check; outputs must be recorded as ledger inputs to keep replay exact (OIP-13) | Optional, for reading and novelty checks, especially a biomedical community; long term, an open equivalent becomes an OSO module ([Agent access: the graph as a filesystem](integrations.md#agent-access-the-graph-as-a-filesystem)) |
| A single provider's API directly | Often the best model quality and reliability; clear data terms | One vendor; a second key is needed for a second model family | Later, if one model is clearly best for a task |
| **Local, open-weight (Ollama, llama.cpp, LM Studio; vLLM for bulk)** | No per-call cost; unpublished work never leaves our machine; exact weights can be recorded for audits | Needs hardware (a GPU for larger models); lower quality than top hosted models for hard tasks; someone runs it | **Recommended for embeddings, domain tagging and development**, and for authors who ask for local-only processing |
| Free tiers only | No cost | Rate limits, unpredictable availability, and terms that may allow logging or training on inputs | Experiments only, never for unpublished submissions |

**Protecting unpublished work.** Unpublished submissions go only to local models or to providers whose terms exclude training and retention, and every AI processing of a submission is logged for its owners to see (OIP-15 section 7a). This choice limits which hosted providers and models a community can pin for pre-screening; check each provider's current terms before pinning it.

**Recommendation for Phase A.** Use OpenRouter for chat and edge assessments, with a small daily budget that is published, and a local Ollama server for embeddings and tagging. Developers can run everything locally at no cost by pointing every task at the local provider. API keys live only on the API server, in environment variables, never in the browser or the repository. Later, users may bring their own key or local model for heavy chat use (OIP-15 section 5).

**Costs to expect.** The curated slice is small (50 to 100 works, a few hundred edges), so assessing it once with two models is a one-off cost that is easy to bound; chat is the open-ended cost, capped by the daily budget and a per-user allowance.
