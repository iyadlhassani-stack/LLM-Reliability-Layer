# LLM System Reliability Lab

## Overview

This project explores the reliability of Large Language Models (LLMs) when used inside structured pipelines.

Instead of focusing on model training, the goal is to study **LLM system engineering** and understand how to make LLM outputs more reliable when they are used in real applications.

The project investigates several aspects of reliability:

- structured output generation
- JSON validation
- error analysis
- retry mechanisms
- reliability metrics

The repository is organized as a sequence of notebooks that progressively analyze and improve the behavior of a language model.

---

# Project Structure

```
llm-system-reliability-lab/

notebooks/
01_baseline_generation.ipynb
02_output_validation_and_error_taxonomy.ipynb
03_retry_layer_and_recovery.ipynb
04_reliability_metrics_analysis.ipynb

data/
qa_set.json

outputs/
baseline_results.json
validated_results.json
retry_results.json

README.md
```

---

# Dataset

The dataset contains a small set of questions used to evaluate the behavior of the model.

Each entry includes:

- a question
- a flag indicating whether the question is answerable

Example:

```json
{
  "question": "What is the capital of Peru?",
  "answerable": true
}
```

Some questions are intentionally **unanswerable** in order to test whether the model correctly refuses to answer.

---

# Notebook 01 — Baseline Generation

## Objective

Establish the **baseline behavior** of the model when asked to generate structured JSON outputs.

The model is required to return answers using the following schema:

```json
{
  "answer": string,
  "refused": boolean,
  "confidence": number
}
```

### Metrics measured

- JSON validity rate
- refusal accuracy
- hallucination rate

### Key observation

Even with explicit instructions, LLMs frequently:

- produce invalid JSON
- hallucinate answers
- fail to refuse questions they cannot answer.

This notebook establishes the **baseline reliability of the model**.

---

# Notebook 02 — Output Validation & Error Taxonomy

## Objective

Understand **why generation failures occur**.

A strict validation layer checks:

- JSON structure
- required fields
- correct data types
- confidence values within the expected range.

### Error categories

Structural errors:

- JSON parsing errors
- missing keys
- incorrect data types
- invalid confidence values.

Behavioral errors:

- hallucinations
- incorrect refusals
- correct refusals
- correct answers.

### Key insight

A model can produce **perfectly valid JSON while still giving incorrect answers**.

Reliability must therefore be evaluated at multiple levels.

---

# Notebook 03 — Retry Layer & Recovery

## Objective

Improve structural reliability by introducing a **retry mechanism**.

If the model generates invalid JSON:

1. the output is validated
2. if invalid, the model is prompted to correct the JSON
3. the process repeats up to a maximum number of retries.

Pipeline:

```
LLM generation → validation → retry if invalid → final output
```

### What is measured

- number of retries used
- improvement in JSON validity.

### Insight

LLMs are probabilistic and do not strictly follow formatting rules.

System-level techniques such as validation and retry loops can improve robustness.

---

# Notebook 04 — Reliability Metrics Analysis

## Objective

Evaluate the entire pipeline and compare the **baseline model behavior** with the **retry system**.

Metrics analyzed include:

- JSON validity rate
- refusal accuracy
- hallucination rate
- retry usage statistics.

Example observations from the experiment:

- baseline JSON validity ≈ 90%
- refusal accuracy ≈ 25%
- hallucination rate ≈ 75%

These results illustrate the challenges involved in building reliable LLM systems.

---

# Key Takeaways

This project highlights several important lessons in LLM engineering:

1. Language models are **probabilistic generators**, not deterministic systems.
2. Prompting alone is not enough to guarantee reliable structured outputs.
3. Robust LLM pipelines require additional components such as:

- validation layers
- retry mechanisms
- structured output constraints
- evaluation metrics.

Reliability comes not only from the model, but from the **system built around the model**.

---

# Technologies Used

- Python
- Hugging Face Transformers
- PyTorch
- JSON validation techniques

---

# Future Work

Possible extensions of this project include:

- schema-constrained decoding
- automatic correction models
- hallucination detection
- advanced reliability metrics.

---

# Author

This repository was created as an educational project exploring the engineering challenges of building reliable LLM systems.
