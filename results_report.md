# Evaluation Report: AI Consulting Q&A Dataset
**Author:** Carolina  
**Date:** March 17, 2026  
**Project:** ai-consulting-qa-eval (LangSmith)

---

## Executive Summary

This evaluation tested two OpenAI models — gpt-4.1-mini and gpt-4.1-nano — on a 
custom dataset of 14 realistic AI consulting questions from SME and marketing clients. 
Both models achieved perfect correctness scores (14/14) when evaluated against 
reference answers using an LLM-as-judge approach. However, gpt-4.1-nano was 1.8x 
faster and used 16.6% fewer tokens, making it a more cost-efficient option for 
this use case with no measurable quality trade-off at this dataset size.

---

## Methodology

**Dataset:** 14 examples created from scratch, representing realistic questions 
a business owner or marketing team would bring to an AI consulting session. 
Each example has a question (input) and a reference consulting answer (output). 
Questions cover automation, reporting, content creation, SEO, competitive 
intelligence, and tool selection.

**Target functions:** Two functions were implemented using the same system prompt — 
a carefully designed consultant persona that prioritises directness, practical tool 
recommendations, honest limitations, and cost/time transparency. The only variable 
between experiments was the model: gpt-4.1-mini vs gpt-4.1-nano, both at 600 
max_tokens.

**Evaluator:** LLM-as-judge using the CORRECTNESS_PROMPT from the openevals library, 
with gpt-4.1-mini as the judge model. The evaluator compared each model output 
against the reference answer and returned a binary score (True/False) with reasoning.

**Infrastructure:** LangSmith EU endpoint, tracing enabled via @traceable decorator, 
max_concurrency=2.

---

## Results

| Model | Correct | Total | Accuracy | Latency P50 | Total Tokens | Est. Cost |
|---|---|---|---|---|---|---|
| gpt-4.1-mini | 14 | 14 | 100% | 8.76s | 10,622 | ~$0.008 |
| gpt-4.1-nano | 14 | 14 | 100% | 4.89s | 8,860 | ~$0.001 |

- Speed advantage of nano over mini: **1.8x faster**
- Token reduction: **16.6% fewer tokens** with nano
- Cost reduction: **~8x cheaper** with nano
- Correctness difference: **none**

---

## Analysis

**On correctness scores:** Both models scored 14/14, which at first looks like a 
tie. However, this result should be interpreted carefully. The LLM-as-judge 
evaluator rewards answers that cover the topic correctly — it does not penalise 
for verbosity, over-explanation, or lack of specificity. Manual review of the 
outputs shows that gpt-4.1-mini tends to produce longer, more structured answers 
with numbered lists and headers, while gpt-4.1-nano produces more concise answers 
that are arguably closer to the consulting voice defined in the system prompt.

**On latency and cost:** For a real-world AI consulting tool where answers are 
generated live in a client session or automated pipeline, the 1.8x speed advantage 
of nano is significant. Combined with the 8x cost reduction, nano is the more 
practical choice for deployment at scale.

**On prompt engineering:** An early test revealed that both models opened answers 
with "Good question!" and "Great question!" — the opposite of the direct tone 
specified. This was fixed by adding an explicit instruction to the system prompt. 
This highlights that even well-designed prompts require iteration, and that 
small wording changes can meaningfully affect output quality.

**On dataset size:** 14 examples is sufficient for a proof-of-concept evaluation 
but too small to draw statistically reliable conclusions. A larger dataset of 
50-100 examples covering more edge cases (ambiguous questions, questions outside 
AI scope, highly technical questions) would give more meaningful differentiation 
between models.

---

## Limitations

- **Small dataset:** 14 examples limits statistical reliability. Both models may 
  perform differently on edge cases not represented here.
- **Binary evaluator:** The correctness evaluator returns True/False only. It does 
  not capture nuance such as tone quality, answer length appropriateness, or 
  specificity of tool recommendations.
- **Self-referential judging:** The judge model (gpt-4.1-mini) is the same family 
  as one of the target models, which may introduce mild bias toward that model's 
  style of answers.
- **Reference answer quality:** The reference answers were written as part of 
  dataset creation, not validated by domain experts. A more rigorous evaluation 
  would use independently verified ground truth.

---

## Recommendations

1. **Deploy gpt-4.1-nano** for this use case — it matches mini on correctness 
   while being significantly faster and cheaper.
2. **Add a custom evaluator** that measures tone and consulting quality beyond 
   correctness — the binary score misses important dimensions for this domain.
3. **Expand the dataset** to at least 50 examples including edge cases and 
   questions the AI should decline or redirect.
4. **A/B test the system prompt** as a variable in future experiments — the 
   persona prompt likely has more impact on output quality than model choice 
   at this tier.