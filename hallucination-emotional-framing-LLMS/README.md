# Emotional Framing & LLM Hallucination Study

**Does emotionally charged language make LLMs hallucinate more?**

This project investigates whether emotional framing of factual questions influences hallucination rates in transformer-based large language models. Five models spanning a range of architectures and parameter scales were evaluated on a purpose-built dataset of emotionally rewritten factual questions.

---

## Dataset

📦 [`belpekkan/emotion-framed-factual-qa`](https://huggingface.co/datasets/belpekkan/emotion-framed-factual-qa) on HuggingFace

**823 validated question–answer pairs** derived from [TriviaQA](https://nlp.cs.washington.edu/triviaqa/) (`rc.wikipedia` split). Each source question was rewritten into four emotional registers — joy, sadness, anger, fear — alongside the original neutral form.

| Split | n |
|---|---|
| Neutral | 240 |
| Sadness | 200 |
| Fear | 143 |
| Anger | 160 |
| Joy | 80 |

Topic domains (geography, history, science, culture) are balanced across all conditions (~205 questions each).

**Validation:** Each rewrite was passed through a [GoEmotions classifier](https://huggingface.co/SamLowe/roberta-base-go_emotions) and accepted only if the target emotion's summed probability mass exceeded 0.30. Mean validation score across accepted stimuli: ≥ 0.95.

---

## Models Evaluated

| Model | Parameters | Architecture |
|---|---|---|
| `google/flan-t5-large` | ~780M | Encoder-decoder |
| `mistralai/Mistral-7B-Instruct-v0.3` | 7B | Decoder-only |
| `tiiuae/falcon-7b-instruct` | 7B | Decoder-only |
| `meta-llama/Meta-Llama-3-8B-Instruct` | 8B | Decoder-only |
| `microsoft/Phi-3-mini-4k-instruct` | 3.8B | Decoder-only |

---

## Methodology

### Stimulus Construction

240 TriviaQA questions (≤ 20 words, Wikipedia-verified answers) were stratified across four domains (60 per domain). Each was rewritten into four emotional registers using the Anthropic API with a controlled prompt template (see `prompts/rewriting_prompt.txt`), then validated with a GoEmotions classifier.

### Inference Protocol

All models received every stimulus exactly once. Generation used greedy decoding (`do_sample=False`, `max_new_tokens=50`). Decoder-only models used their native chat templates; Flan-T5 received a direct instruction prefix. A checkpointing strategy (every 50 rows) allowed recovery from GPU interruptions.

### Scoring Pipeline

Responses were scored through a four-layer correctness pipeline, applied in sequence:

1. **Exact match** — case-insensitive comparison against answer and all accepted aliases
2. **Alias containment** — whole-word regex search for any alias as a substring
3. **Normalised match** — lowercase, strip articles and punctuation, collapse whitespace
4. **Semantic similarity** — encode response and all candidates with `all-MiniLM-L6-v2`; cosine similarity threshold 0.75

Incorrect responses were further categorised as **substitution** (≤ 15 words, confident but wrong), **confabulation** (> 15 words, elaborated incorrect), **uncertainty** (hedging language), or **refusal/empty**.

### Statistical Analysis

- Pearson chi-squared test of independence on 5 × 2 (emotion × correct/incorrect) contingency tables per model
- Effect size: Cramér's V (negligible < 0.10, small 0.10–0.20, medium 0.20–0.40, large ≥ 0.40)
- Domain analysis: 4 × 2 contingency tables with pairwise Bonferroni-corrected comparisons (α = 0.0083)

---

## Results

### Overall Hallucination Rates

| Model | Accuracy | Hallucination Rate | Neutral Baseline HR |
|---|---|---|---|
| Mistral-7B | 74.5% | 25.5% | 25.0% |
| Llama-3-8B | 74.2% | 25.8% | 25.0% |
| Phi-3 Mini | 60.8% | 39.2% | 37.5% |
| Falcon-7B | 40.7% | 59.3% | 57.1% |
| Flan-T5-Large | 21.4% | 78.6% | 78.8% |

### Effect of Emotional Framing

No model showed statistically significant differences across emotion conditions (all p > 0.05). Cramér's V effect sizes for emotion ranged from **0.020 to 0.058** (negligible).

A consistent directional pattern did emerge: **anger framing reduced hallucination rates below neutral in 4/5 models** (−3.6 to −3.8 pp for Mistral, Llama-3, and Phi-3). Sadness and fear tended to increase rates modestly. Flan-T5 showed near-uniform rates across all emotional conditions.

### Effect of Topic Domain

Domain effects were **highly significant across all models** (all p ≤ 0.0001), with Cramér's V ranging from **0.186 to 0.316** — 4 to 16× larger than emotion effects.

| Domain | Mistral-7B | Llama-3-8B | Phi-3 Mini | Falcon-7B | Flan-T5 |
|---|---|---|---|---|---|
| Geography | 13.8% | 13.8% | 28.6% | 40.5% | 63.3% |
| Science | 28.9% | 20.1% | 31.4% | 63.7% | 74.5% |
| Culture | 23.8% | 30.6% | 50.0% | 68.0% | 99.0% |
| History | 36.0% | 38.9% | 47.3% | 65.5% | 77.8% |

### Error Types

Substitution errors (plausible but wrong answer) dominated across all models. Confabulation (fully fabricated content) was rare — 32 instances for Falcon, 21 for Mistral, 0 for Llama-3. Incorrect responses were consistently longer than correct ones (most pronounced in Mistral: 2.7 vs 6.0 mean words).

---

## Key Findings

- **Emotional framing does not significantly influence hallucination rates** at the sample sizes tested. Effects, while directionally consistent, are negligible in magnitude.
- **Topic domain is the dominant predictor**, with cross-domain spreads of 22–36 pp versus cross-emotion spreads of only 2–8 pp.
- **Anger framing consistently reduces hallucination** across most models — the opposite of the intuitive prediction. This may reflect implicit self-checking behaviour triggered by assertive framing.
- **Emotional framing appears to bias retrieval, not generation**: substitution dominates, suggesting models surface the wrong answer from existing knowledge rather than fabricating novel content.
- **Model quality matters most.** The 50+ pp spread in hallucination rates across models dwarfs any framing effect.

---

## Collaborators

Christiana Kyritsi · Geanina Verestiuc · Mariyana Shishmanova · Beliz Pekkan

*Utrecht University — Transformers: Applications in Language and Communication (2026)*
