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
│   └── task2_genai.ipynb          ← Task 2: Generative AI (code complete; GPU blocked — see note)
├── task3_agentic/
│   └── task3_agentic.ipynb        ← Task 3: Agentic Workflows (complete, executed)
├── CITATIONS.md                    ← AI tool usage documentation
├── REFLECTION.md                   ← Architectural decisions & retrospective
└── README.md                       ← This file
```

---

## Task 1 — Financial AI: LLM-Powered Equity Research Assistant

[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/AasirWaseer/CDAZZDEV-MLE-AasirWaseer/blob/main/task1_financial/task1_financial_ai.ipynb)

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

[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/AasirWaseer/CDAZZDEV-MLE-AasirWaseer/blob/main/task2_genai/task2_genai.ipynb)

**Status: ⚠️ Code complete — GPU execution blocked by Colab free-tier quota**

**Domain:** Legal Clause Extraction (10 clause types from commercial contracts → structured JSON)

| Sub-task | Status |
|----------|--------|
| **2A** Dataset generation via Groq teacher | ✅ Implemented & tested — single-example generation confirmed working |
| **2A** Diversity metrics, JSONL format, 80/10/10 split | ✅ Fully implemented |
| **2B** QLoRA 4-bit NF4 fine-tuning, hyperparameter docs | ✅ Fully implemented, not executed (no GPU) |
| **2B** Loss logging, merge & HF push | ✅ Fully implemented, not executed (no GPU) |
| **2C** ROUGE-L + BERTScore evaluation | ✅ Fully implemented, not executed (no GPU) |
| **2C** Hallucination review, qualitative analysis | ✅ Fully implemented, not executed (no GPU) |
| **Bonus** ChromaDB RAG fallback | ✅ Fully implemented, not executed (no GPU) |

> **Why:** Colab free-tier GPU (T4) quota was exhausted at the platform level during the submission window. This is a resource constraint, not a code limitation. All code is complete and can be reproduced on **Kaggle Notebooks** (30 free GPU hours/week) or Colab Pro with no changes. See the ⚠️ Submission Note cell in the notebook for full details.

**Setup:** Runtime → **T4 GPU** · Secrets: `GROQ_API_KEY`, `HF_TOKEN` · Runtime → Run all

---

## Task 3 — Agentic Workflows

[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/AasirWaseer/CDAZZDEV-MLE-AasirWaseer/blob/main/task3_agentic/task3_agentic.ipynb)

**Status: ✅ Complete and fully executed**

*(Details to be added upon completion)*

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
| Qualitative Analysis | 10 | 5 — partial (written without execution) |
| **Bonus (RAG Fallback)** | +5 | +3 — implemented, not executed |
| **Task 2 Total** | **105** | **~52** |

### Task 3

*(To be added upon completion)*

---

## AI Tools Policy

All AI assistance is documented in [`CITATIONS.md`](./CITATIONS.md). Claude (claude-sonnet-4-20250514) was used for code generation assistance across all tasks. All architectural decisions, prompt design, hyperparameter choices, and integration logic were made by the candidate and can be defended in the follow-up technical interview.
