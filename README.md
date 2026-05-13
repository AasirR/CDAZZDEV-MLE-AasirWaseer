# CDAZZDEV-MLE-AasirWaseer

**Candidate:** Aasir Waseer  
**Role:** Senior Machine Learning Engineer  
**Assessment:** Ceylon Dazzling Dev Holding (Pvt.) Ltd.

---

## Repository Structure

```
CDAZZDEV-MLE-AasirWaseer/
├── task1_financial/
│   └── task1_financial_ai.ipynb   ← Task 1: Financial AI (complete)
├── CITATIONS.md                    ← AI tool usage documentation
├── REFLECTION.md                   ← Architectural decisions & retrospective
└── README.md                       ← This file
```

---

## Task 1 — Financial AI: LLM-Powered Equity Research Assistant

[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/drive/1rkw6iT32Ernk17DFYmkrBFsA06xw18WI?usp=sharing)

### What it does

| Sub-task | Deliverable |
|----------|-------------|
| **1A** | Fetches ≥2 years OHLCV for `AAPL`, computes SMA-50, SMA-200, RSI-14 (Wilder), MACD (12,26,9), Bollinger Bands (20, 2σ) — all from first principles. Retrieves ≥10 news headlines with fallback sources. Produces a typed summary dictionary. |
| **1B** | Per-headline LLM sentiment (Groq Llama-3-70B) → validated `HeadlineSentiment` objects. Aggregated sentiment score. Reasoned Buy/Hold/Sell signal via `TradingSignal` Pydantic model. All prompts separated as constants. |
| **Bonus** | Styled one-page HTML equity research brief with embedded 3-panel matplotlib chart (price + RSI + MACD). |

### Setup (Colab)

1. Open the notebook via the badge above.
2. In the Colab **Secrets** panel (🔑), add a secret named `GROQ_API_KEY` with your free [Groq API key](https://console.groq.com).
3. Run all cells top-to-bottom. Cell outputs are pre-populated for review.

### Free tools used

| Tool | Purpose |
|------|---------|
| `yfinance` | OHLCV data + news headlines |
| `Groq API` (free tier) | LLM inference — `llama3-70b-8192` |
| `Pydantic v2` | Structured output validation |
| `matplotlib` | Embedded chart in HTML brief |
| Google News RSS | News fallback (no API key required) |

---

## Scoring Summary (self-assessed)

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
| **Total** | **105** | **~104** |

---

## AI Tools Policy

All AI assistance is documented in [`CITATIONS.md`](./CITATIONS.md). Claude (claude-sonnet-4-20250514) was used for code generation assistance. All architectural decisions, prompt design, and integration logic were made by the candidate and can be defended in the follow-up interview.
