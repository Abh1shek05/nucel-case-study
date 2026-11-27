# AI workflows, embeddings, and retrieval

## AI enrichment of saved content

After content extraction, the documented AI workflow enriches a saved item with structured information. The workflow includes:

- **Summarization** — a concise representation of the source.
- **Tagging and topic classification** — labels and topic signals to organize the library.
- **Category and sentiment analysis** — additional structured metadata where applicable.
- **Embedding generation** — a vector representation used for semantic retrieval.

The AI logic is centralized in a shared package rather than having each application call model providers independently. Prompts and provider routing are also centralized, which gives the platform a consistent place to manage model selection and fallback behavior.

## Semantic and hybrid retrieval

The database documentation describes PostgreSQL full-text search alongside pgvector embeddings. This supports complementary retrieval strategies:

| Retrieval mode | Strength | Trade-off |
|---|---|---|
| Keyword / full-text | Matches explicit words and phrases | Can miss a result when the user remembers the idea but not the wording |
| Vector / semantic | Finds conceptually related content | Similarity alone may return broad or less precise matches |
| Hybrid | Combines lexical and semantic signals | Requires ranking and weighting choices to be evaluated |

The product website emphasizes this distinction as “find it by meaning, not just the title.”

## Retrieval-augmented generation

The documented chat path follows a retrieval-augmented pattern:

1. Accept a natural-language question.
2. Retrieve relevant saved items using the search infrastructure.
3. Assemble a bounded context from those items.
4. Generate an answer grounded in that context.
5. Retain source item references so the UI can show citations that lead back to the saved material.

The design goal is not simply to generate a fluent response. It is to make the response traceable to the user's own saved content. If retrieval does not find supporting material, the system should not imply that an answer came from a source it did not retrieve.

## Provider routing and fallbacks

The engineering documentation describes a shared AI package with role/plan-based model routing and provider fallback behavior across Google Gemini and Anthropic. This centralization supports:

- Selecting a model based on the task or product tier.
- Keeping prompts and provider clients in one shared integration layer.
- Falling back when a configured provider is unavailable.
- Monitoring usage and managing model-related costs.

Exact model IDs, provider keys, quotas, and environment settings are deliberately excluded from this public case study because they are operational details and can change.

## Evaluation questions

A useful retrieval evaluation should separate:

- Whether relevant saves appear in the top K results (Recall@K / Precision@K).
- Whether the best result is ranked near the top (MRR / nDCG).
- Whether generated claims are supported by retrieved source passages.
- Whether citations resolve to the correct original saves.
- Latency and cost for retrieval and generation as separate components.

See [Evaluation and metrics](evaluation-and-metrics.md).
