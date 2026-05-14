# CITATIONS.md

All AI tool usage and adapted open-source code for this assessment is documented below, per Section 2.2 of the assessment brief.

---

## AI-Assisted Code — Task 1 (Financial AI)

```
# AI-ASSISTED: Claude (claude-sonnet-4-20250514), Prompt: 'Identify required pip packages for yfinance, groq, pydantic pipeline', Date: 2026-05-13
# Location: task1_financial_ai.ipynb — Cell: Environment Setup
```

```
# AI-ASSISTED: Claude (claude-sonnet-4-20250514), Prompt: 'Implement RSI with Wilder smoothing and MACD from first principles in pandas', Date: 2026-05-13
# Location: task1_financial_ai.ipynb — Cell: 1A-2 Technical Indicators
```

```
# AI-ASSISTED: Claude (claude-sonnet-4-20250514), Prompt: 'Write a robust news fetcher using yfinance with RSS fallback, deduplication, and null-safe parsing', Date: 2026-05-13
# Location: task1_financial_ai.ipynb — Cell: 1A-3 News Headlines Retrieval
```

```
# AI-ASSISTED: Claude (claude-sonnet-4-20250514), Prompt: 'Design Pydantic schemas for per-headline sentiment JSON and aggregated sentiment score', Date: 2026-05-13
# Location: task1_financial_ai.ipynb — Cell: 1B-1 Pydantic Schemas
```

```
# AI-ASSISTED: Claude (claude-sonnet-4-20250514), Prompt: 'Separate prompt constants from business logic for a multi-step LLM sentiment and signal pipeline', Date: 2026-05-13
# Location: task1_financial_ai.ipynb — Cell: 1B-2 Prompt Constants
```

```
# AI-ASSISTED: Claude (claude-sonnet-4-20250514), Prompt: 'Render a one-page dark-themed HTML equity research brief with embedded base64 matplotlib chart', Date: 2026-05-13
# Location: task1_financial_ai.ipynb — Cell: Bonus HTML Brief
```

---

## AI-Assisted Code — Task 2 (Generative AI)

```
# AI-ASSISTED: Claude (claude-sonnet-4-20250514), Prompt: 'Identify all pip packages needed for QLoRA fine-tuning with PEFT, TRL, BitsAndBytes on Colab', Date: 2026-05-13
# Location: task2_genai.ipynb — Cell: Environment Setup
```

```
# AI-ASSISTED: Claude (claude-sonnet-4-20250514), Prompt: 'Write a robust dataset generator with retry logic, JSON validation, and progress tracking for Groq API', Date: 2026-05-13
# Location: task2_genai.ipynb — Cell: 2A-2 Dataset Generation
```

```
# AI-ASSISTED: Claude (claude-sonnet-4-20250514), Prompt: 'Compute dataset diversity metrics: prompt length distribution, clause type balance, keyword frequency', Date: 2026-05-13
# Location: task2_genai.ipynb — Cell: 2A-3 Diversity Analysis
```

```
# AI-ASSISTED: Claude (claude-sonnet-4-20250514), Prompt: 'Format legal extraction examples as chat JSONL for Phi-2 with system/user/assistant turns, stratified split', Date: 2026-05-13
# Location: task2_genai.ipynb — Cell: 2A-4 JSONL Format + Split
```

```
# AI-ASSISTED: Claude (claude-sonnet-4-20250514), Prompt: 'Configure LoRA for Phi-2 with PEFT, show trainable parameter count', Date: 2026-05-13
# Location: task2_genai.ipynb — Cell: 2B-3 Apply LoRA Adapters
```

```
# AI-ASSISTED: Claude (claude-sonnet-4-20250514), Prompt: 'Configure SFTTrainer with per-epoch val loss logging and manual loss tracking callback', Date: 2026-05-13
# Location: task2_genai.ipynb — Cell: 2B-5 Training with Loss Logging
```

```
# AI-ASSISTED: Claude (claude-sonnet-4-20250514), Prompt: 'Merge LoRA adapters with merge_and_unload, save merged model, push to Hugging Face Hub', Date: 2026-05-13
# Location: task2_genai.ipynb — Cell: 2B-7 Merge + Save
```

```
# AI-ASSISTED: Claude (claude-sonnet-4-20250514), Prompt: 'Run ROUGE-L and BERTScore evaluation on base vs fine-tuned model outputs, return comparison table', Date: 2026-05-13
# Location: task2_genai.ipynb — Cell: 2C-3 ROUGE-L + BERTScore Evaluation
```

```
# AI-ASSISTED: Claude (claude-sonnet-4-20250514), Prompt: 'Build hallucination reviewer that checks fine-tuned outputs against ground truth for clause_type accuracy and extracted_text containment', Date: 2026-05-13
# Location: task2_genai.ipynb — Cell: 2C-4 Hallucination Rate
```

---

## AI-Assisted Code — Task 3 (Agentic Workflows)

```
# AI-ASSISTED: Claude (claude-sonnet-4-20250514), Prompt: 'Identify pip packages for LangGraph multi-agent system with Groq, yfinance, ChromaDB, duckduckgo-search', Date: 2026-05-14
# Location: task3_agentic.ipynb — Cell: Environment Setup
```

```
# AI-ASSISTED: Claude (claude-sonnet-4-20250514), Prompt: 'Build a tool call tracer that logs name, inputs, truncated output, and duration to JSONL', Date: 2026-05-14
# Location: task3_agentic.ipynb — Cell: Observability — Trace Logger
```

```
# AI-ASSISTED: Claude (claude-sonnet-4-20250514), Prompt: 'Implement five LangChain tools: get_price_data, get_news, calculate_volatility, llm_sentiment, web_search with error handling and trace logging', Date: 2026-05-14
# Location: task3_agentic.ipynb — Cell: 3A Five Tools Implementation
```

```
# AI-ASSISTED: Claude (claude-sonnet-4-20250514), Prompt: 'Define Pydantic structured handoff schema between Agent A and Agent B with volatility, sentiment, indicators', Date: 2026-05-14
# Location: task3_agentic.ipynb — Cell: 3B Pydantic Structured Handoff Schema
```

```
# AI-ASSISTED: Claude (claude-sonnet-4-20250514), Prompt: 'Assemble LangGraph StateGraph with Agent A, Agent B, critique loop routing, and END node', Date: 2026-05-14
# Location: task3_agentic.ipynb — Cell: 3B LangGraph Graph Assembly
```

```
# AI-ASSISTED: Claude (claude-sonnet-4-20250514), Prompt: 'Demonstrate short-term session memory: answer follow-up question from cached tool result without re-calling tool', Date: 2026-05-14
# Location: task3_agentic.ipynb — Cell: 3C-1 Short-Term Memory Demonstration
```

```
# AI-ASSISTED: Claude (claude-sonnet-4-20250514), Prompt: 'Write a Streamlit dashboard that reads agent_trace.jsonl and visualises tool call timeline, durations, and outputs', Date: 2026-05-14
# Location: task3_agentic.ipynb — Cell: Bonus Streamlit Observability Dashboard
```

---

## Teacher Model Usage (Task 2)

The Groq `llama3-70b-8192` model was used as a **teacher model** to generate the 120-example legal clause extraction dataset. This model is distinct from the student model (`microsoft/phi-2`) being fine-tuned, satisfying the assessment requirement.

**Full teacher system prompt** is included verbatim in Task 2 notebook cell 2A-1 (`TEACHER_SYSTEM_PROMPT`).

---

## Open-Source Libraries Referenced

All libraries used are standard open-source packages installed via `pip`. No private or proprietary repositories were adapted. Key references:

- Dettmers et al. (2023) — QLoRA paper (NF4 quantization justification, cited inline in hyperparameter docs)
- Hu et al. (2021) — LoRA paper (target module selection justification, cited inline)
- Wilder (1978) — RSI original specification (Wilder smoothing implementation justification, cited inline)

---

*Generated for CDAZZDEV-MLE-AasirWaseer — Assessment submission 2026*
