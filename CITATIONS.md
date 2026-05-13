# CITATIONS.md

All AI tool usage and adapted open-source code is documented below per the CDAZZDEV citation policy (Section 2.2).

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

---

## Task 2 — Generative AI Fine-Tuning

### AI-Assisted Code

```python
# AI-ASSISTED: Claude (claude-sonnet-4-20250514), Prompt: 'Identify all pip packages needed for QLoRA fine-tuning with PEFT, TRL, BitsAndBytes on Colab', Date: 2026-05-13
# AI-ASSISTED: Claude (claude-sonnet-4-20250514), Prompt: 'Write a robust dataset generator with retry logic, JSON validation, and progress tracking for Groq API', Date: 2026-05-13
# AI-ASSISTED: Claude (claude-sonnet-4-20250514), Prompt: 'Compute dataset diversity metrics: prompt length distribution, clause type balance, keyword frequency', Date: 2026-05-13
# AI-ASSISTED: Claude (claude-sonnet-4-20250514), Prompt: 'Format legal extraction examples as chat JSONL for Phi-2 with system/user/assistant turns, stratified split', Date: 2026-05-13
# AI-ASSISTED: Claude (claude-sonnet-4-20250514), Prompt: 'Document QLoRA hyperparameter choices for legal NLP fine-tuning on Colab T4', Date: 2026-05-13
# AI-ASSISTED: Claude (claude-sonnet-4-20250514), Prompt: 'Configure LoRA for Phi-2 with PEFT, show trainable parameter count', Date: 2026-05-13
# AI-ASSISTED: Claude (claude-sonnet-4-20250514), Prompt: 'Configure SFTTrainer with per-epoch val loss logging and manual loss tracking callback', Date: 2026-05-13
# AI-ASSISTED: Claude (claude-sonnet-4-20250514), Prompt: 'Merge LoRA adapters with merge_and_unload, save merged model, push to Hugging Face Hub', Date: 2026-05-13
# AI-ASSISTED: Claude (claude-sonnet-4-20250514), Prompt: 'Run ROUGE-L and BERTScore evaluation on base vs fine-tuned model outputs, return comparison table', Date: 2026-05-13
# AI-ASSISTED: Claude (claude-sonnet-4-20250514), Prompt: 'Build hallucination reviewer that checks fine-tuned outputs against ground truth', Date: 2026-05-13
# AI-ASSISTED: Claude (claude-sonnet-4-20250514), Prompt: 'Build ChromaDB RAG fallback that triggers on low confidence, retrieves domain context, re-queries model', Date: 2026-05-13
```

### Open-Source References (Task 2)

- Dettmers, T. et al. (2023). *QLoRA: Efficient Finetuning of Quantized LLMs*. arXiv:2305.14314
- Hu, E. et al. (2021). *LoRA: Low-Rank Adaptation of Large Language Models*. arXiv:2106.09685
- TRL SFTTrainer documentation: https://huggingface.co/docs/trl/sft_trainer
- PEFT documentation: https://huggingface.co/docs/peft
- BERTScore: https://github.com/Tiiiger/bert_score — used as pip library only

---

*All architectural decisions and hyperparameter justifications were made by the candidate.*
