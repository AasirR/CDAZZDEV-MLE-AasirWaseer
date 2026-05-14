# CDAZZDEV-MLE-AasirWaseer

**Candidate:** Aasir Waseer  
**Role:** Senior Machine Learning Engineer  
**Assessment:** Ceylon Dazzling Dev Holding (Pvt.) Ltd.

---

## Repository Structure

```
CDAZZDEV-MLE-AasirWaseer/
├── task1_financial/
│   └── task1_financial_ai.ipynb   ← Task 1: Financial AI (complete, executed)
├── task2_genai/
│   └── task2_genai.ipynb          ← Task 2: Generative AI — 2A complete; 2B/2C GPU blocked
├── task3_agentic/
│   └── task3_agentic.ipynb        ← Task 3: Agentic Workflows (complete, executed)
├── CITATIONS.md                    ← AI tool usage documentation
├── REFLECTION.md                   ← Architectural decisions & retrospective
└── README.md                       ← This file
```

---

## Task 1 — Financial AI: LLM-Powered Equity Research Assistant

[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/drive/1rkw6iT32Ernk17DFYmkrBFsA06xw18WI?usp=sharing)

**Status: ✅ Complete and fully executed**

| Sub-task | Deliverable |
|----------|-------------|
| **1A** | ≥2 years OHLCV for `AAPL`; SMA-50, SMA-200, RSI-14 (Wilder), MACD (12,26,9), Bollinger Bands (20, 2σ) from first principles. ≥10 headlines via yfinance + Google News RSS fallback. Typed summary dictionary. |
| **1B** | Per-headline LLM sentiment (Groq `llama-3.3-70b-versatile`) → validated `HeadlineSentiment` Pydantic objects. Weighted aggregation score. Reasoned Buy/Hold/Sell `TradingSignal`. Prompts separated as constants. |
| **Bonus** | Dark-mode HTML equity research brief with embedded 3-panel matplotlib chart (price + BB + SMAs / RSI / MACD). |

**Setup:** Runtime → CPU · Secrets: `GROQ_API_KEY` · Runtime → Run all

| Tool | Purpose |
|------|---------|
| `yfinance` | OHLCV + headlines |
| `Groq API` free tier | `llama-3.3-70b-versatile` inference |
| `Pydantic v2` | Structured output validation |
| `matplotlib` | Embedded chart |
| Google News RSS | News fallback |

---

## Task 2 — Generative AI: Domain-Specific Fine-Tuning Pipeline

[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/drive/1TN3op2--1Ue-zPM0FD3pC9yOfF5EEMF9?usp=sharing)

**Status: ⚠️ Task 2A complete and executed — 2B/2C GPU blocked**

**Domain:** Legal Clause Extraction (10 clause types from commercial contracts → structured JSON)

| Sub-task | Status |
|----------|--------|
| **2A** Use case definition | ✅ Complete |
| **2A** Dataset generation via Groq teacher (`llama-3.3-70b-versatile`) | ✅ Complete — single-example generation confirmed working |
| **2A** Diversity metrics, JSONL format, 80/10/10 split | ✅ Complete |
| **2B** QLoRA 4-bit NF4 fine-tuning | ⚠️ Code removed — Colab free-tier GPU quota exhausted |
| **2C** ROUGE-L + BERTScore evaluation | ⚠️ Code removed — depends on 2B execution |
| **Bonus** ChromaDB RAG fallback | ⚠️ Code removed — depends on 2B execution |

> **Why:** Colab free-tier T4 GPU quota was exhausted at the platform level during the submission window. Rather than submitting fabricated outputs or non-executable dead code, 2B/2C cells were cleanly removed and the constraint documented transparently. All 2B/2C logic is described in full in `REFLECTION.md`. To reproduce: run on **Kaggle Notebooks** (30 free GPU hours/week) or Colab Pro with no code changes.

**Setup:** Runtime → **T4 GPU** · Secrets: `GROQ_API_KEY`, `HF_TOKEN` · Runtime → Run all

---

## Task 3 — Agentic Workflows: Multi-Agent Financial Research System

[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/drive/1OSEJWTvz-EewCQGvs1BpgqDdknQKn-XO?usp=sharing)

**Status: ✅ Complete and fully executed**

**Framework:** LangGraph  
**LLM:** Groq `llama-3.3-70b-versatile`  
**Domain:** Multi-Agent Financial Research & Analysis

### System Overview

A production-style agentic workflow implementing:

- Autonomous tool-calling research agent
- Multi-agent orchestration with LangGraph
- Structured Pydantic handoffs between agents
- Critique/replan loop
- Short-term memory
- Persistent caching
- Full observability tracing
- Streamlit monitoring dashboard

---

### Task 3A — Autonomous Research Agent

| Capability | Deliverable |
|------------|-------------|
| Five tools implemented | `get_price_data`, `get_news`, `calculate_volatility`, `llm_sentiment`, `web_search` |
| Autonomous tool selection | LangGraph ReAct-style reasoning with dynamic tool choice |
| Observe/Replan loop | Agent adapts subsequent actions based on previous tool outputs |
| Final report generation | Evidence-backed financial research summary |
| Robustness | Graceful error handling and fallback logic |

---

### Task 3B — Two-Agent Pipeline with Critique Loop

| Component | Deliverable |
|-----------|-------------|
| Agent A — Data Analyst | Generates structured quantitative analysis |
| Agent B — Research Writer | Produces narrative investment report |
| Structured handoff | `DataBrief` Pydantic schema between agents |
| Tool restrictions | Each agent limited to domain-specific tools |
| Critique loop | Agent B requests missing information from Agent A |
| LangGraph orchestration | Fully automated multi-step execution pipeline |

---

### Task 3C — Memory & Observability

| Capability | Deliverable |
|------------|-------------|
| Short-term memory | Session context reused across follow-up queries |
| Persistent cache | JSON-based cache keyed by ticker/date |
| Trace logging | `agent_trace.jsonl` records every tool call |
| Monitoring | Tool inputs, outputs, and execution duration captured |

---

### Bonus — Streamlit Observability Dashboard

| Feature | Deliverable |
|---------|-------------|
| Dashboard UI | Streamlit-based trace visualisation |
| Timeline view | Displays chronological tool execution |
| Log inspection | Full tool-call history review |
| Colab support | Runs via `pyngrok` tunnel |

**Setup:** Runtime → CPU · Secrets: `GROQ_API_KEY` · Runtime → Run all

| Tool / Framework | Purpose |
|------------------|---------|
| `LangGraph` | Agent orchestration |
| `Groq API` | LLM inference |
| `Pydantic v2` | Structured agent handoffs |
| `yfinance` | Market data |
| `Google News RSS` | Financial news retrieval |
| `Streamlit` | Observability dashboard |

---

## Scoring Summary (self-assessed)

### Task 1

| Criterion | Max | Expected |
|-----------|-----|---------|
| OHLCV Data Fetch | 10 | 10 |
| Indicator Accuracy (no TA-Lib) | 25 | 25 |
| News Retrieval ≥10 | 10 | 10 |
| Summary Dictionary | 10 | 10 |
| Robustness | 5 | 5 |
| Per-Headline JSON | 10 | 10 |
| Signal Reasoning Quality | 15 | 14 |
| Structured Output Validation | 10 | 10 |
| Prompt Engineering | 5 | 5 |
| **Bonus (HTML Brief + Chart)** | +5 | +5 |
| **Task 1 Total** | **105** | **~104** |

### Task 2

| Criterion | Max | Expected |
|-----------|-----|---------|
| Use Case Definition | 10 | 10 |
| Dataset Quality & Diversity | 10 | 10 |
| Teacher Prompt Quality | 10 | 9 |
| Hyperparameter Justification | 15 | 15 |
| Training Execution (loss curves) | 10 | 0 — GPU unavailable |
| Model Save / HF Push | 5 | 0 — GPU unavailable |
| ROUGE-L Comparison | 15 | 0 — GPU unavailable |
| Hallucination Review | 15 | 0 — GPU unavailable |
| Qualitative Analysis | 10 | 5 — written without execution |
| **Bonus (RAG Fallback)** | +5 | 0 — GPU unavailable |
| **Task 2 Total** | **105** | **~49** |

### Task 3

### Task 3

| Criterion | Max | Expected |
|-----------|-----|---------|
| Five tools implemented | 15 | 15 |
| Autonomous tool selection | 10 | 10 |
| Observe/Replan cycle | 8 | 8 |
| Final report quality | 10 | 9 |
| Error handling & robustness | 7 | 7 |
| Distinct agent roles | 8 | 8 |
| Structured Pydantic handoff | 8 | 8 |
| Message trace visibility | 6 | 6 |
| Critique loop | 8 | 8 |
| End-to-end automation | 5 | 5 |
| Short-term memory | 5 | 5 |
| Persistent cache | 5 | 5 |
| Observability logging | 5 | 5 |
| **Bonus (Streamlit Dashboard)** | +5 | +5 |
| **Task 3 Total** | **105** | **~104** |

---

## AI Tools Policy

All AI assistance is documented in [`CITATIONS.md`](./CITATIONS.md). Claude (claude-sonnet-4-20250514) was used for code generation assistance across all tasks. All architectural decisions, prompt design, hyperparameter choices, and integration logic were made by the candidate and can be defended in the follow-up technical interview.
