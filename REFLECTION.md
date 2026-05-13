# REFLECTION.md

**Candidate:** Aasir Waseer  
**Word count:** ~520

---

## Task 1 — Architectural Decisions

**Ticker choice (AAPL):** Apple was selected for its consistent data availability across multiple years, rich news ecosystem, and strong analyst coverage — all of which stress-test the sentiment and signal pipeline against real-world complexity rather than a thinly traded ticker.

**Indicator implementation:** All five indicators were built from raw pandas operations with no TA-Lib dependency. RSI uses Wilder's original exponential smoothing (`alpha = 1/period, adjust=False`) rather than a simple rolling mean, which is the correct specification and produces different values for the first ~28 bars. MACD EMAs also use `adjust=False` to match charting library conventions. Bollinger Bands use `ddof=0` (population std), which is the standard — not sample std. These choices matter: incorrect implementations would produce silently wrong signals.

**News retrieval layering:** A two-source fallback was implemented — yfinance news as primary, Google News RSS as secondary. The yfinance news schema changed in recent library versions (content is nested under a `content` dict key), so the fetch function handles both the legacy and current response shapes. The fallback guards against zero headlines without requiring a paid API key.

**Prompt separation:** All LLM prompts live as module-level string constants (`SENTIMENT_SYSTEM_PROMPT`, `SIGNAL_USER_TEMPLATE`, etc.) rather than inline f-strings. This is a production practice — prompts are the contract between the application and the model; embedding them inline makes them invisible, untestable, and hard to iterate. In a real system these would be versioned YAML templates.

**Structured output enforcement:** Pydantic v2 models define the schema for every LLM response. The `parse_and_validate_json` helper strips markdown fences (models frequently disobey formatting instructions under load), parses JSON, and runs validation. Validation failures are logged at ERROR level with the raw output truncated to 200 chars — enough to diagnose the failure without flooding logs. The pipeline continues rather than raising, because a single failed headline should not abort a 10-headline batch.

**Signal reasoning quality:** The signal prompt explicitly instructs the LLM to reason over *combinations* of indicators, not echo individual values. For example: "SMA-50 is above SMA-200 (golden cross), which combined with RSI at 58 (momentum room before overbought) and a positive MACD crossover suggests…" — this is qualitatively different from "RSI is 58 which is moderate."

---

## What I Would Improve With More Time

1. **Rate-limiting and retries:** The Groq free tier throttles at ~30 requests/minute. A proper exponential backoff with jitter would make the sentiment loop production-safe for large headline sets.

2. **Streaming HTML rendering:** The current brief renders after all LLM calls complete. A streaming approach (Server-Sent Events or Streamlit) would give real-time feedback as each headline is processed.

3. **Indicator backtesting:** The momentum signal rule is heuristic. With more time I'd backtest the SMA crossover + RSI + MACD combination against historical AAPL data to validate its predictive value before using it in the prompt context.

4. **Structured LLM fallback:** If Groq is unavailable, the pipeline currently fails for Task 1B. A secondary provider (OpenRouter Llama-3) would make the pipeline resilient to API outages.

---

## Limitations Encountered

- **yfinance P/E data:** The `trailingPE` field is occasionally `None` even for large-caps when yfinance fails to populate info. The code falls back gracefully but the brief will show `N/A` in those cases.
- **News recency:** yfinance news may return stale headlines (>7 days old). The RSS fallback helps but is also query-dependent. For production, a paid news API (Benzinga, NewsAPI Pro) would be the right solution.
