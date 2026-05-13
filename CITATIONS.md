# CITATIONS.md

All AI tool usage and adapted open-source code is documented below per the CDAZZDEV citation policy.

---

## Task 1 — Financial AI

### AI-Assisted Code

```python
# AI-ASSISTED: Claude (claude-sonnet-4-20250514), Prompt: 'Identify required pip packages for yfinance, groq, pydantic pipeline', Date: 2026-05-13
# File: task1_financial_ai.ipynb, Cell: Environment Setup
```

```python
# AI-ASSISTED: Claude (claude-sonnet-4-20250514), Prompt: 'Implement RSI with Wilder smoothing and MACD from first principles in pandas', Date: 2026-05-13
# File: task1_financial_ai.ipynb, Cell: Technical Indicators
```

```python
# AI-ASSISTED: Claude (claude-sonnet-4-20250514), Prompt: 'Design Pydantic v2 models for headline sentiment and trading signal with validators', Date: 2026-05-13
# File: task1_financial_ai.ipynb, Cell: Pydantic Schemas
```

```python
# AI-ASSISTED: Claude (claude-sonnet-4-20250514), Prompt: 'Write system/user prompts for financial news sentiment and trading signal generation', Date: 2026-05-13
# File: task1_financial_ai.ipynb, Cell: Prompt Templates
```

```python
# AI-ASSISTED: Claude (claude-sonnet-4-20250514), Prompt: 'Implement Groq API wrapper with retry, JSON parsing, and Pydantic validation', Date: 2026-05-13
# File: task1_financial_ai.ipynb, Cell: LLM Client + Sentiment Analysis
```

```python
# AI-ASSISTED: Claude (claude-sonnet-4-20250514), Prompt: 'Generate styled HTML equity research brief template with embedded matplotlib base64 chart', Date: 2026-05-13
# File: task1_financial_ai.ipynb, Cell: Bonus HTML Report
```

### Open-Source References

- yfinance documentation: https://github.com/ranaroussi/yfinance — used for API method signatures (no code copied)
- Pydantic v2 documentation: https://docs.pydantic.dev/latest/ — used for field_validator syntax reference
- Wilder RSI specification: Wilder, J.W. (1978). *New Concepts in Technical Trading Systems* — indicator formula reference only

---

*All architectural decisions, prompt engineering, and integration logic were designed by the candidate.*
