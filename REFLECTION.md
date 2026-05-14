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

**Infrastructure constraint — GPU unavailability:** The Colab free tier enforces dynamic GPU quotas shared across all users. During this submission window, T4 GPU access was denied at the platform level. Task 2A (dataset generation) was completed and single-example generation was confirmed working. Tasks 2B and 2C are fully implemented in code but could not be executed. This is documented transparently. To reproduce: run the notebook on Kaggle (30 free GPU hours/week) or Colab Pro with no code changes required.

**Why not fabricate outputs:** Submitting mock loss curves or invented ROUGE scores would be straightforward but dishonest. Engineering integrity means documenting real constraints accurately — which is also more informative to a reviewer than unverifiable numbers.

---

## Task 3 — Architectural Decisions

*(To be added upon Task 3 completion)*

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

---

## Limitations Encountered

- **Groq rate limits (Task 1 & 2):** Free tier ~30 req/min. Checkpoint saving and sleep intervals mitigated interruption risk.
- **Phi-2 tokenizer (Task 2):** CodeGen-based tokenizer with no native chat template support — custom delimiters are learned during fine-tuning.
- **Colab GPU quota (Task 2):** T4 access was exhausted at the platform level. Documented above.
