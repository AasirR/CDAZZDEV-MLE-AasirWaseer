# CITATIONS.md

All AI tool usage and adapted open-source code is documented below per the CDAZZDEV citation policy (Section 2.2).

---

## Task 1 — Financial AI

### AI-Assisted Code

```
# AI-ASSISTED: Claude (claude-sonnet-4-20250514), Prompt: 'Identify required pip packages for yfinance, groq, pydantic pipeline', Date: 2026-05-13
# AI-ASSISTED: Claude (claude-sonnet-4-20250514), Prompt: 'Implement RSI with Wilder smoothing and MACD from first principles in pandas', Date: 2026-05-13
# AI-ASSISTED: Claude (claude-sonnet-4-20250514), Prompt: 'Design Pydantic v2 models for headline sentiment and trading signal with validators', Date: 2026-05-13
# AI-ASSISTED: Claude (claude-sonnet-4-20250514), Prompt: 'Write system/user prompts for financial news sentiment and trading signal generation', Date: 2026-05-13
# AI-ASSISTED: Claude (claude-sonnet-4-20250514), Prompt: 'Implement Groq API wrapper with retry, JSON parsing, and Pydantic validation', Date: 2026-05-13
# AI-ASSISTED: Claude (claude-sonnet-4-20250514), Prompt: 'Generate styled HTML equity research brief template with embedded matplotlib base64 chart', Date: 2026-05-13
```

### Open-Source References (Task 1)

- yfinance documentation: https://github.com/ranaroussi/yfinance — API method signatures (no code copied)
- Pydantic v2 documentation: https://docs.pydantic.dev/latest/ — field_validator syntax reference
- Wilder, J.W. (1978). *New Concepts in Technical Trading Systems* — RSI formula specification

---

## Task 2 — Generative AI Fine-Tuning

### Execution Status

Task 2A (dataset generation pipeline) was fully implemented and single-example generation was confirmed working via Groq `llama-3.3-70b-versatile`. Tasks 2B and 2C could not be executed due to Colab free-tier GPU quota exhaustion during the submission window. All code is implemented and documented — see the Submission Note in `task2_genai/task2_genai.ipynb` for full details.

### AI-Assisted Code

```
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

## Task 3 — Agentic Workflows

*(To be added upon Task 3 completion)*

---

---

## Colab Notebook Links

| Task | Link |
|------|------|
| Task 1 — Financial AI | https://colab.research.google.com/drive/1rkw6iT32Ernk17DFYmkrBFsA06xw18WI?usp=sharing |
| Task 2 — Generative AI | https://colab.research.google.com/drive/1TN3op2--1Ue-zPM0FD3pC9yOfF5EEMF9?usp=sharing |
| Task 3 — Agentic Workflows | *(to be added)* |


*All architectural decisions and hyperparameter justifications were made by the candidate. AI assistance was used for code generation and syntax; all decisions can be defended in interview.*
