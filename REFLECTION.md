# REFLECTION.md

**Candidate:** Aasir Waseer  
**Tasks completed:** Task 1 · Task 2 · Task 3  
**Word count:** ~570

---

## Architectural Decisions

**Task 1** centered on a clean separation of concerns. The data pipeline (Task 1A) is fully decoupled from the LLM layer (Task 1B) — each function takes explicit inputs and returns typed outputs, so the pipeline can be tested in isolation without any API calls. For the LLM integration, I separated prompt constants from business logic (prompts defined as module-level strings, never inline) and routed all LLM responses through Pydantic validators before any downstream use. This mirrors how I would build this in production: no raw string parsing, no silent failures. The Bonus HTML brief embeds the matplotlib chart as base64 to produce a fully self-contained single-file report, which is practical for distribution.

**Task 2** required the biggest architectural choice: selecting `microsoft/phi-2` (2.7B) as the student model over the more common Mistral-7B. Phi-2 was chosen deliberately — it fits comfortably in Colab T4 memory after 4-bit NF4 quantization (~1.8 GB vs ~1.4 GB available headroom) while still being capable enough to learn structured JSON output. The teacher/student separation (Groq Llama-3-70B generating data, Phi-2 being fine-tuned) is architecturally important and explicitly enforced. Hyperparameter choices — `lora_r=16`, cosine scheduler, 3 epochs — were each justified against the dataset size (96 training examples) to avoid overfitting while achieving measurable improvement. The RAG fallback uses perplexity as a confidence proxy, routing low-confidence outputs to ChromaDB retrieval before re-inference, which is a realistic production pattern for high-stakes domains like legal clause extraction.

**Task 3** used LangGraph's `StateGraph` rather than LangChain's legacy `AgentExecutor` because the multi-agent critique loop requires explicit state routing — a graph-native concern, not an executor concern. Agent tool access is enforced at the `ToolNode` level by passing distinct tool lists per agent, not just by instruction. The Pydantic `DataBrief` schema as the Agent A → B handoff prevents the most common multi-agent failure mode: hallucinated data summaries that never touched a real API. The `SESSION_MEMORY` dict provides short-term memory without adding a vector store dependency to the session loop.

## What I Would Improve With More Time

For Task 2, I would invest in a larger and more adversarially diverse dataset — 500+ examples spanning edge cases like clauses that span clause types (e.g. a termination clause with embedded liability language) and deliberately ambiguous risk levels. I would also add LLM-as-judge evaluation using a rubric-based Groq call to supplement ROUGE-L, which measures lexical overlap rather than legal accuracy. Additionally, I would fine-tune on a larger base model (Mistral-7B) with gradient checkpointing to get a better accuracy ceiling.

For Task 3, I would integrate LangSmith for production-grade trace observability and add a confidence-based routing layer: when the research agent's LLM confidence on a risk assessment is below a threshold, it automatically triggers an additional `web_search` call before finalising the report. The Streamlit dashboard is functional but basic — a production version would add filtering by tool, time-range sliders, and token cost tracking per agent run.

## Limitations Encountered

The primary limitation was Groq free-tier rate limiting during dataset generation (Task 2) and agent runs (Task 3). This was mitigated with exponential backoff in the generator loop and a 0.3-second inter-call delay. All retry logic is visible in the notebooks. No tasks were blocked; the rate limiting added approximately 10–15 minutes to dataset generation and 2–3 minutes to multi-agent runs.
