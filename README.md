# AI Consulting Q&A — Custom Dataset Evaluation with LangSmith

**Ironhack AI Consulting & Integration Bootcamp — Week 7 Lab**  
**Student:** Carolina | **Date:** March 17, 2026

A custom LLM evaluation project that creates a domain-specific dataset of realistic AI consulting questions, runs evaluation experiments across two models using LangSmith, and produces a comparative analysis report.

---

## Project Overview

**Domain:** AI consulting Q&A for business clients — SMEs, e-commerce brands, and marketing teams exploring AI adoption.

**Objective:** Build a custom evaluation pipeline from scratch using LangSmith to compare how two OpenAI models perform when answering realistic client questions in a defined consulting persona.

**Models evaluated:**
- `gpt-4.1-mini`
- `gpt-4.1-nano`

---

## Lab Deliverables Checklist

| Requirement | Status |
|---|---|
| Custom LangSmith dataset with at least 10 examples | ✅ 14 examples |
| Domain documented (2-3 sentences) | ✅ See Dataset section |
| Working target function with tracing enabled | ✅ Two functions (mini + nano) |
| LLM-as-judge evaluator configured | ✅ CORRECTNESS_PROMPT via openevals |
| Evaluation experiment run successfully | ✅ Two experiments completed |
| Results reviewed in LangSmith UI | ✅ Screenshots below |
| Analysis report with key metrics and insights | ✅ In notebook final section |

---

## Learning Objectives

- Create a custom LangSmith dataset from a real-world domain
- Implement traceable target functions for LLM evaluation
- Set up an LLM-as-judge evaluator using `openevals`
- Run and compare evaluation experiments in LangSmith
- Analyse results and produce an evaluation report

---

## Dataset

14 examples created from scratch, each representing a realistic question a business owner or marketing team would bring to an AI consulting session.

**Topics covered:**
- Agentic AI vs premium LLM subscriptions
- Marketing automation (Meta Ads, Google Ads, reporting)
- Content creation and creative automation
- SEO and product description generation
- Competitive intelligence dashboards
- Email newsletter automation
- Product catalogue and feed management
- Campaign naming convention automation
- Review generation and website integrations
- Industry trend monitoring

Each example has:
- `input`: a specific, realistic client question
- `output`: a practical consulting answer covering tool recommendations, limitations, prerequisites, and cost/time estimates

---

## Setup

### Prerequisites

- Python 3.10+
- LangSmith account (EU endpoint)
- OpenAI API key

### Installation

```bash
pip install langsmith openai openevals python-dotenv
```

### Environment variables

Create a `.env` file in the project root:

```
LANGSMITH_API_KEY=your_key_here
OPENAI_API_KEY=your_key_here
```

### LangSmith configuration

The notebook sets the following automatically:

```python
os.environ["LANGSMITH_TRACING"]  = "true"
os.environ["LANGSMITH_ENDPOINT"] = "https://eu.api.smith.langchain.com"
os.environ["LANGSMITH_PROJECT"]  = "ai-consulting-qa-eval"
```

---

## Project Structure

```
├── lab_langsmith.ipynb     # Main notebook — dataset, experiments, analysis
├── .env                    # API keys (not committed)
├── README.md               # This file
```

---

## Evaluation Pipeline

```
Custom dataset (14 examples)
        ↓
Target function (system prompt + model)
        ↓
LLM-as-judge evaluator (CORRECTNESS_PROMPT via openevals)
        ↓
LangSmith experiment results
        ↓
Comparative analysis + report
```

**System prompt design:** The consultant persona was explicitly designed to be direct and jargon-free, recommend whether a premium LLM is sufficient before suggesting complex tools, be upfront about limitations and implementation costs, and avoid generic or overly technical answers.

**Evaluator:** `create_llm_as_judge` with `CORRECTNESS_PROMPT` from `openevals`, using `gpt-4.1-mini` as the judge model.

---

## Results

| Model | Correct | Total | Accuracy | Latency P50 | Total Tokens | Est. Cost |
|---|---|---|---|---|---|---|
| gpt-4.1-mini | 14 | 14 | 100% | 8.76s | 10,622 | ~$0.008 |
| gpt-4.1-nano | 14 | 14 | 100% | 4.89s | 8,860 | ~$0.001 |

**Key findings:**
- Both models scored 14/14 on correctness
- `gpt-4.1-nano` is **1.8x faster** than mini
- `gpt-4.1-nano` uses **16.6% fewer tokens**
- `gpt-4.1-nano` is approximately **8x cheaper**
- No measurable quality trade-off at this dataset size

**Recommendation:** `gpt-4.1-nano` is the more practical choice for deployment in a live consulting tool — it matches mini on correctness while being significantly faster and cheaper.

---

## Key Observations

- Prompt engineering matters even with a clear brief — an early test showed both models opening with "Good question!" despite the persona instructions. A single added instruction fixed this.
- The binary correctness evaluator rewards topical coverage but does not capture tone, specificity, or consulting quality. A custom evaluator would add meaningful signal for this domain.
- 14 examples is sufficient for a proof-of-concept but too small for statistically reliable conclusions. A dataset of 50–100 examples is recommended for production evaluation.

---

## Screenshots

### gpt-4.1-mini experiment results
![gpt-4.1-mini results](screenshots/mini_results.png)

### gpt-4.1-nano experiment results
![gpt-4.1-nano results](screenshots/nano_results.png)

### Side-by-side model comparison
![Model comparison](screenshots/comparison.png)

---

## Built With

- [LangSmith](https://smith.langchain.com/) — experiment tracking, dataset management, tracing
- [OpenAI API](https://platform.openai.com/) — gpt-4.1-mini and gpt-4.1-nano
- [openevals](https://github.com/langchain-ai/openevals) — LLM-as-judge evaluator
- [python-dotenv](https://pypi.org/project/python-dotenv/) — environment management

---

## Author

Carolina (Bootcamp Student)
Ironhack — AI Consulting & Integration Bootcamp, 2026