# CDAZZDEV-MLE-AasirWaseer

**Candidate:** Aasir Waseer  
**Role:** Senior Machine Learning Engineer  
**Assessment:** Ceylon Dazzling Dev Holding (Pvt.) Ltd. — Technical Assessment  
**Tasks completed:** Task 1 (Financial AI) · Task 2 (Generative AI) · Task 3 (Agentic Workflows)

---

## Repository Structure

```
CDAZZDEV-MLE-AasirWaseer/
├── task1_financial/
│   ├── task1_financial_ai.ipynb        # Main notebook (cell outputs visible)
│   └── README.md
├── task2_genai/
│   ├── task2_genai.ipynb               # Main notebook (cell outputs visible)
│   └── README.md
├── task3_agentic/
│   ├── task3_agentic.ipynb             # Main notebook (cell outputs visible)
│   ├── logs/
│   │   └── agent_trace.jsonl           # Every tool call logged per spec
│   └── README.md
├── CITATIONS.md
├── REFLECTION.md
└── README.md  ← you are here
```

---

## Task 1 — Financial AI: LLM-Powered Equity Research Assistant

**Ticker:** AAPL (Apple Inc.)  
**LLM:** Groq `llama-3.3-70b-versatile`  
**Colab Notebook:** [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/drive/1rkw6iT32Ernk17DFYmkrBFsA06xw18WI?usp=sharing)

### What was built

**Task 1A — Financial Data Pipeline**
- 2+ years of daily OHLCV data fetched via `yfinance` with dynamic date computation (no hardcoded strings)
- All five technical indicators implemented from first principles without TA-Lib:
  - SMA-50, SMA-200 (rolling arithmetic mean)
  - RSI-14 with Wilder smoothing (EWM alpha = 1/14, as per Wilder 1978)
  - MACD (12, 26, 9) using `adjust=False` EMA to match charting convention
  - Bollinger Bands (window 20, 2σ, population std ddof=0)
- News headlines retrieved via yfinance news endpoint with Google News RSS fallback (≥10 headlines guaranteed)
- Summary dictionary: current price, 52-week high/low, P/E ratio, YTD return, and a rule-based momentum signal
- Robust null handling throughout; no unhandled exceptions on missing data

**Task 1B — LLM Sentiment and Signal Reasoning**
- Per-headline JSON output: `headline`, `sentiment`, `confidence`, `brief_reason` — validated with Pydantic
- Aggregated sentiment score across all headlines
- Reasoned Buy/Hold/Sell signal: LLM reasons over indicator combinations (SMA crossover + RSI zone + MACD direction), not individual values
- Full Pydantic validation with logged failure handling; validation errors caught and gracefully degraded
- Prompt constants defined as module-level strings (`SENTIMENT_SYSTEM_PROMPT`, `SIGNAL_SYSTEM_PROMPT`), cleanly separated from business logic

**Bonus — HTML Research Brief**
- One-page styled HTML equity brief: company snapshot, technical outlook, news sentiment summary (top 3 headlines), LLM recommendation, embedded matplotlib chart (price + RSI + MACD subplots), and mandatory risk disclaimer
- Dark-themed GitHub-style design using IBM Plex fonts

### Deliverables checklist

| Criterion | Status |
|-----------|--------|
| OHLCV ≥2 years, dynamic dates | ✅ |
| All 5 indicators from first principles | ✅ |
| ≥10 headlines with fallback | ✅ |
| Summary dictionary — all required fields | ✅ |
| Per-headline JSON (4 fields) + aggregation | ✅ |
| Reasoned Buy/Hold/Sell (3-5 sentences) | ✅ |
| Pydantic validation + failure logging | ✅ |
| Prompt separation from business logic | ✅ |
| Bonus HTML brief with embedded chart | ✅ |

---

## Task 2 — Generative AI: Domain-Specific Fine-Tuning Pipeline

**Use case:** Legal clause extraction from commercial contracts  
**Base (student) model:** `microsoft/phi-2` (2.7B parameters)  
**Teacher model:** `llama3-70b-8192` via Groq API (different from student — per assessment requirement)  
**Hugging Face repository:** [AasirWaseer/phi2-legal-clause-extractor](https://huggingface.co/AasirWaseer/phi2-legal-clause-extractor)  
**Colab Notebook:** [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/drive/1TN3op2--1Ue-zPM0FD3pC9yOfF5EEMF9?usp=sharing)

### What was built

**Task 2A — Use Case Definition & Dataset Engineering**
- Problem: extract and classify legal clauses (liability cap, indemnification, termination, governing law, confidentiality, IP, data protection, force majeure, payment terms, dispute resolution) from raw contract text, returning structured JSON with `clause_type`, `extracted_text`, `risk_level`, and `plain_english_summary`
- 120 training examples generated with Groq Llama-3-70B as teacher; full system prompt included in notebook cell 2A-1
- Diversity analysis reported: clause type distribution, risk level distribution, prompt word count statistics, and legal keyword frequency
- JSONL format with system/user/assistant chat turns compatible with Phi-2's chat template
- 80/10/10 split: 96 train / 12 val / 12 test

**Task 2B — Fine-Tuning Execution (QLoRA 4-bit NF4 on Colab T4)**
- Every hyperparameter explicitly justified in notebook cell 2B-1:
  - `lora_r=16`, `lora_alpha=32`, targets `q_proj`, `v_proj`, `k_proj`, `dense`
  - `learning_rate=2e-4`, cosine scheduler, 3 epochs, batch size 2, gradient accumulation 4 steps
  - `max_seq_length=512`, 4-bit NF4 quantization, bf16 precision
- Train and validation loss logged per epoch; validation loss decreases across epochs (plotted in notebook)
- LoRA adapters merged with `merge_and_unload()` and pushed to Hugging Face Hub

**Task 2C — Evaluation & Baseline Comparison**

| Model | ROUGE-L | BERTScore F1 |
|-------|---------|--------------|
| phi-2 (base, no fine-tuning) | — | — |
| phi-2-legal-finetuned | — | — |
| **Improvement** | **—** | **—** |

*(Exact values visible in executed notebook cells)*

- Both ROUGE-L and BERTScore F1 (distilbert) computed on identical held-out test set
- Hallucination rate computed over 10 manually reviewed responses (correct / partially correct / hallucinated)
- Two-paragraph qualitative analysis in notebook cell 2C-5

**Bonus — RAG Fallback Layer**
- ChromaDB vector store built from training domain documents
- When fine-tuned model confidence (perplexity-based) falls below threshold, relevant context retrieved and model re-queried
- Before-and-after comparison example included in notebook

### Deliverables checklist

| Criterion | Status |
|-----------|--------|
| Non-trivial domain use case with clear I/O spec | ✅ |
| ≥100 examples; teacher prompt included | ✅ |
| Diversity metrics reported | ✅ |
| JSONL chat format; 80/10/10 split stated | ✅ |
| QLoRA 4-bit NF4 correctly configured | ✅ |
| All hyperparameters documented with justification | ✅ |
| Train + val loss per epoch; val loss decreases | ✅ |
| Merged model saved and accessible (HF Hub) | ✅ |
| ROUGE-L comparison table (base vs fine-tuned) | ✅ |
| BERTScore F1 as additional metric | ✅ |
| Hallucination rate over ≥10 responses | ✅ |
| Two-paragraph qualitative analysis | ✅ |
| Bonus RAG fallback with before/after example | ✅ |

---

## Task 3 — Agentic Workflows: Multi-Agent Financial Research System

**Ticker:** NVDA (NVIDIA Corporation)  
**Framework:** LangGraph (StateGraph with ReAct)  
**LLM:** Groq `llama-3.3-70b-versatile`  
**Colab Notebook:** [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/drive/1OSEJWTvz-EewCQGvs1BpgqDdknQKn-XO?usp=sharing)

### System architecture

```
User Query
    │
    ▼
┌─────────────────────────────────────────────────────┐
│  Task 3A — Single Research Agent (LangGraph ReAct)  │
│  Tools: get_price_data, get_news,                   │
│  calculate_volatility, llm_sentiment, web_search    │
│  Autonomous tool selection + observe/replan cycle   │
└───────────────────┬─────────────────────────────────┘
                    │ Pydantic DataBrief handoff
                    ▼
┌─────────────────────────────────────────────────────┐
│  Task 3B — Two-Agent Pipeline (LangGraph)           │
│  Agent A (Data Analyst): price_data, volatility,    │
│    sentiment → DataBrief (Pydantic schema)          │
│  Agent B (Research Writer): web_search, news →      │
│    Final Report                                     │
│  Critique loop: B requests beta → A responds        │
└───────────────────┬─────────────────────────────────┘
                    │
                    ▼
┌─────────────────────────────────────────────────────┐
│  Task 3C — Memory & Observability                   │
│  Short-term: SESSION_MEMORY across tool calls       │
│  Persistent: JSON cache keyed by ticker+date        │
│  agent_trace.jsonl: every tool call logged          │
└─────────────────────────────────────────────────────┘
```

### What was built

**Task 3A — Tool-Using Research Agent**
- All five tools implemented as LangChain `@tool`-decorated functions with error handling and trace logging:
  - `get_price_data(ticker, period)` — yfinance OHLCV + computed indicators
  - `get_news(ticker, n)` — structured headline list
  - `calculate_volatility(ticker, window)` — annualised historical volatility
  - `llm_sentiment(headlines)` — Groq LLM structured sentiment score
  - `web_search(query)` — duckduckgo-search library
- LangGraph ReAct agent selects tools autonomously; no hardcoded sequence
- At least one full observe → decide → replan cycle visible in notebook output
- Final report: Financial Health Summary, Top Three Risks (with evidence), Hedge Strategy Recommendation
- All tool failures caught and handled; agent attempts alternative on error

**Task 3B — Two-Agent Coordination**
- Agent A (Data Analyst): restricted to `get_price_data`, `calculate_volatility`, `llm_sentiment`
- Agent B (Research Writer): restricted to `web_search`, `get_news`
- Structured Pydantic handoff schema (`DataBrief`, `ClarificationRequest`, `ClarificationResponse`) — no raw string passing
- Full agent message trace printed in notebook output
- Critique loop: Agent B requests stock beta → Agent A responds with computed beta → Agent B incorporates it into final report
- LangGraph `StateGraph` runs end-to-end without manual intervention

**Task 3C — Memory and Observability**
- Short-term memory: `SESSION_MEMORY` dict stores tool call results; follow-up question answered from cache without re-fetching
- Persistent cache: final brief saved as JSON keyed by `{ticker}_{date}`; second run detects and loads cached brief
- `agent_trace.jsonl`: every tool call logged with `tool`, `inputs`, `output` (≤200 chars), `duration_s`, `timestamp`

**Bonus — Streamlit Observability Dashboard**
- `streamlit_dashboard.py` reads `agent_trace.jsonl` and renders tool call timeline, duration bar chart, and full log table
- Run instructions included in notebook

### Deliverables checklist

| Criterion | Status |
|-----------|--------|
| All 5 tools implemented and callable | ✅ |
| Autonomous tool selection (ReAct, not hardcoded) | ✅ |
| Observe and replan cycle visible in output | ✅ |
| Final report: 3 sections with evidence | ✅ |
| Graceful error handling on tool failures | ✅ |
| Distinct agent roles with enforced tool restriction | ✅ |
| Pydantic structured handoff (not raw strings) | ✅ |
| Full agent message trace visible | ✅ |
| Critique loop executed ≥ once and visible | ✅ |
| End-to-end automation (no manual steps) | ✅ |
| Short-term memory demonstrated | ✅ |
| Persistent JSON cache with second-run detection | ✅ |
| agent_trace.jsonl present in repo | ✅ |
| Bonus Streamlit dashboard | ✅ |

---

## Credentials & Security

No API keys, Hugging Face tokens, or credentials are hardcoded anywhere in this repository. All secrets are loaded at runtime via `google.colab.userdata` (Colab Secrets panel) with `os.environ` fallback. This is enforced in every notebook's setup cell.

---

## Environment

All notebooks are designed for **Google Colab free tier (T4 GPU)**. All dependencies are installed via `!pip install` in the first cell of each notebook. No local environment required.

**Key libraries:** `yfinance`, `groq`, `pydantic`, `transformers==4.44.2`, `peft==0.12.0`, `trl==0.10.1`, `bitsandbytes==0.43.3`, `langgraph==0.2.28`, `langchain-groq==0.2.1`, `chromadb`, `duckduckgo-search`, `rouge-score`, `bert-score`

---

*CDAZZDEV Senior MLE Technical Assessment — Confidential — Candidate use only*
