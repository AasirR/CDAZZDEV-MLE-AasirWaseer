# REFLECTION.md

**Candidate:** Aasir Waseer  
**Tasks Submitted:** Task 1 (Financial AI), Task 2 (Generative AI — code complete, GPU execution blocked), Task 3 (Agentic Workflows)

---

## Task 1 — Architectural Decisions

**Ticker choice (AAPL):** Apple was selected for its consistent data availability, rich news ecosystem, and broad analyst coverage — stress-testing the sentiment and signal pipeline against real-world complexity rather than a thinly traded ticker.

**Indicator implementation:** All five indicators were built from raw pandas with no TA-Lib dependency. RSI uses Wilder's exponential smoothing (`alpha=1/period, adjust=False`) rather than a simple rolling mean, which is the correct specification. MACD uses `adjust=False` to match charting library conventions. Bollinger Bands use `ddof=0` (population std), the standard convention. These choices matter — incorrect implementations produce silently wrong signals that mislead the downstream LLM.

**News retrieval layering:** A two-source fallback was implemented — yfinance as primary, Google News RSS as secondary. The yfinance news schema changed in recent versions (content nested under a `content` dict), so the fetch function handles both shapes. The fallback guards against zero headlines without a paid API key.

**Prompt separation:** All LLM prompts live as module-level string constants rather than inline f-strings. In production these would be versioned YAML templates — separating them is a prerequisite for prompt testing and A/B evaluation.

---

## Task 2 — Architectural Decisions & Infrastructure Constraint

**Domain choice (Legal Clause Extraction):** Legal contract analysis is commercially valuable and non-trivial — general models demonstrably fail at it, misclassifying clause types and hallucinating obligations. This maximises the measurable delta between base and fine-tuned model, the core evaluation criterion.

**Teacher vs Student separation:** Groq's `llama-3.3-70b-versatile` (teacher) generates training data for `microsoft/phi-2` (student). These are entirely different model families. Phi-2 (2.7B) was chosen because its smaller size trains faster on a T4, enabling more epochs within the free-tier time limit.

**Hyperparameter rationale summary:** LoRA rank 16 provides enough intrinsic dimension for 10 clause types plus JSON formatting. Alpha=2×r is the standard scaling convention. Batch size 2 + gradient accumulation 8 gives effective batch 16 within T4 VRAM limits. Cosine LR scheduling prevents oscillation near the loss minimum. `paged_adamw_8bit` saves ~75% of optimiser VRAM versus fp32 AdamW.

**Infrastructure constraint — GPU unavailability:** The Colab free tier enforces dynamic GPU quotas shared across all users. During this submission window, T4 GPU access was denied at the platform level. Task 2A (dataset generation) was completed and single-example generation was confirmed working. Tasks 2B and 2C are fully implemented in code but could not be executed. This is documented transparently. To reproduce: run the notebook on Kaggle Notebooks (https://kaggle.com) (30 free GPU hours/week) or Colab Pro with no code changes required.

**Why not fabricate outputs:** Submitting mock loss curves or invented ROUGE scores would be straightforward but dishonest. Engineering integrity means documenting real constraints accurately — which is also more informative to a reviewer than unverifiable numbers.

---

## Task 3 — Architectural Decisions

**Why LangGraph instead of a linear chain:** A traditional sequential pipeline cannot naturally express conditional routing, iterative critique loops, or persistent state transitions between agents. LangGraph was selected because the assignment explicitly requires agentic behaviour — reasoning, tool usage, feedback, and replanning. The graph abstraction also makes execution traces observable, which becomes important for debugging autonomous systems.

**Multi-agent decomposition:** The workflow separates responsibilities into a quantitative **Data Analyst** agent and a narrative **Research Writer** agent. This mirrors real-world organisational boundaries and reduces prompt overload. A single monolithic agent tends to mix numerical analysis, retrieval, and prose generation into one unstable context window. Splitting the system improves controllability and allows agent-specific tool permissions.

**Structured handoffs via Pydantic:** Inter-agent communication uses typed Pydantic schemas rather than free-form text. This prevents schema drift and eliminates ambiguity about what downstream agents receive. In production systems, unstructured handoffs become a major reliability failure point because downstream prompts silently break when upstream output formatting changes.

**Observe/Replan loop design:** The autonomous research agent follows a lightweight ReAct-style cycle: reason → select tool → observe result → decide next action. This was intentionally implemented as an explicit loop rather than a single multi-tool prompt because iterative observation materially improves research quality. For example, volatility calculations may trigger additional news retrieval or sentiment checks before report synthesis.

**Tool isolation and permissions:** Each agent was intentionally restricted to a subset of tools. The writer agent cannot directly access raw market APIs; it only consumes validated analyst output. Restricting tool access reduces accidental prompt misuse and is closer to how production agent systems implement capability boundaries.

**Memory strategy:** Two memory layers were implemented:
1. Short-term conversational memory for preserving session context across follow-up queries
2. Persistent JSON caching for deterministic reuse of expensive API/tool outputs

This hybrid approach balances responsiveness with reproducibility. Cached financial data also reduces unnecessary API calls under free-tier rate limits.

**Observability-first architecture:** Autonomous systems are difficult to debug without trace visibility. Every tool invocation, duration, agent transition, and intermediate state is logged to JSONL. The Streamlit dashboard was added specifically to surface execution traces visually rather than treating the workflow as a black box.

**Why Streamlit instead of notebook-only logs:** Raw notebook output becomes unreadable once multiple agents and tools interact asynchronously. Streamlit provides a lightweight operational dashboard closer to what would exist in an internal ML platform or orchestration monitoring environment.

---

## What I Would Improve With More Time

**Task 1:**
- Rate-limiting and retries on the Groq sentiment loop for large headline sets
- Streaming HTML rendering via Streamlit for real-time feedback
- Backtesting the momentum signal rule against historical data before using it in prompts

**Task 2:**
- Larger training set (300+ examples) to reduce hallucination on complex nested clauses
- DPO fine-tuning stage after SFT using human-labelled preference pairs
- LLM-as-judge evaluation pipeline instead of string-similarity BERTScore
- Token log-probability as a principled confidence signal for the RAG fallback

**Task 3:**
- Add long-term vector memory for cross-session research continuity
- Introduce human-in-the-loop approval checkpoints before report publication
- Add asynchronous tool execution for lower latency
- Implement agent evaluation metrics (tool efficiency, hallucination rate, reasoning depth)
- Deploy LangGraph Studio or LangSmith tracing for richer observability

---

## Limitations Encountered

- **Groq rate limits (Task 1 & 2):** Free tier ~30 req/min. Checkpoint saving and sleep intervals mitigated interruption risk.
- **Phi-2 tokenizer (Task 2):** CodeGen-based tokenizer with no native chat template support — custom delimiters are learned during fine-tuning.
- **Colab GPU quota (Task 2):** T4 access was exhausted at the platform level. Documented above.

---

## Colab Notebook Links

| Task | Link |
|------|------|
| Task 1 — Financial AI | https://colab.research.google.com/drive/1rkw6iT32Ernk17DFYmkrBFsA06xw18WI?usp=sharing |
| Task 2 — Generative AI | https://colab.research.google.com/drive/1TN3op2--1Ue-zPM0FD3pC9yOfF5EEMF9?usp=sharing |
| Task 3 — Agentic Workflows | https://colab.research.google.com/drive/1OSEJWTvz-EewCQGvs1BpgqDdknQKn-XO?usp=sharing |
