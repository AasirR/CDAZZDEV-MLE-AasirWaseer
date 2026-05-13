# REFLECTION.md

**Candidate:** Aasir Waseer  
**Tasks Submitted:** Task 1 (Financial AI), Task 2 (Generative AI)  
**Word count:** ~590

---

## Task 1 — Architectural Decisions

**Ticker choice (AAPL):** Apple was selected for its consistent data availability, rich news ecosystem, and broad analyst coverage — stress-testing the sentiment and signal pipeline against real-world complexity rather than a thinly traded ticker.

**Indicator implementation:** All five indicators were built from raw pandas with no TA-Lib dependency. RSI uses Wilder's exponential smoothing (`alpha=1/period, adjust=False`) rather than a simple rolling mean, which is the correct specification. MACD uses `adjust=False` to match charting library conventions. Bollinger Bands use `ddof=0` (population std), the standard convention. These choices matter — incorrect implementations would produce silently wrong signals.

**News retrieval layering:** A two-source fallback was implemented — yfinance as primary, Google News RSS as secondary. The yfinance news schema changed in recent versions (content nested under a `content` dict), so the fetch function handles both shapes. The fallback guards against zero headlines without a paid API key.

**Prompt separation:** All LLM prompts live as module-level string constants rather than inline f-strings. In production these would be versioned YAML templates — separating them is a prerequisite for prompt testing and A/B evaluation.

---

## Task 2 — Architectural Decisions

**Domain choice (Legal Clause Extraction):** Legal contract analysis is a commercially valuable, non-trivial NLP task where general models demonstrably fail — hallucinating obligations, misclassifying clause types. This maximises the measurable delta between base and fine-tuned model, the core evaluation criterion.

**Teacher vs Student separation:** Groq's `llama3-70b-8192` (teacher) generates training data for `microsoft/phi-2` (student). These are entirely different model families, satisfying the requirement. Phi-2 (2.7B) was chosen over Llama-2-7B because its smaller size trains faster on Colab T4 (15 GB VRAM), enabling more epochs within the free-tier time limit.

**Hyperparameter rationale summary:** LoRA rank 16 provides enough intrinsic dimension for 10 clause types plus JSON formatting. Alpha=2×r is the standard scaling convention. Batch size 2 + gradient accumulation 8 gives effective batch 16 while staying under T4 memory limits. Cosine LR scheduling prevents oscillation near the loss minimum. `paged_adamw_8bit` saves ~75% of optimiser VRAM versus fp32 AdamW.

**Checkpoint saving during generation:** The generator writes a checkpoint every 10 examples so a Groq rate-limit interruption doesn't lose prior work. Standard practice for API-dependent pipelines.

**RAG confidence proxy:** Using JSON field-completeness as confidence is a practical heuristic requiring no extra inference call. A more principled approach would use token-level log-probabilities as a perplexity proxy.

---

## What I Would Improve With More Time

1. **Larger training set:** 96 examples is the minimum viable floor. With 300+ examples the hallucination rate on complex nested clauses would drop significantly.

2. **DPO fine-tuning stage:** After SFT, a Direct Preference Optimisation pass using human-labelled preference pairs would further reduce hallucination without additional labelled data.

3. **Streaming generation:** The current inference loop generates serially. vLLM continuous batching would reduce evaluation time from ~20 minutes to ~3 minutes on the same hardware.

4. **LLM-as-judge pipeline:** BERTScore is string-similarity based and misses semantic errors. An LLM judge scoring on a rubric gives a more meaningful improvement signal.

---

## Limitations Encountered

- **Groq rate limits:** The free tier allows ~30 requests/minute. Dataset generation for 120 examples took ~10 minutes with `time.sleep(0.3)` between calls. The checkpoint system mitigated restart risk.
- **Phi-2 tokenizer:** Phi-2 uses a CodeGen-based tokenizer that doesn't natively support chat templates. The `<|system|>/<|user|>/<|assistant|>` delimiters are custom additions the model learns during fine-tuning.
- **T4 VRAM ceiling:** Batch size cannot exceed 2 without OOM. Gradient accumulation compensates but slows training. An A100 (Colab Pro) would allow batch size 8 and complete training in ~5 minutes vs ~25 minutes.
