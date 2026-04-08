# Data

---

## Overview

The dataset was constructed specifically for this study, as no existing resource combines factual ground truth with controlled emotional framing. It is publicly available on HuggingFace:

📦 [`belpekkan/emotion-framed-factual-qa`](https://huggingface.co/datasets/belpekkan/emotion-framed-factual-qa)

**823 question–answer pairs** across five emotion conditions and four topic domains.

---

## Source Data

**TriviaQA** (`rc.wikipedia` configuration) — Joshi et al., 2017.

240 questions were sampled with the following constraints:
- Maximum 20 words per question (ensures self-contained questions suitable for emotional rewriting)
- Wikipedia-verified answers with multiple accepted aliases
- Stratified across four topic domains: **geography, history, science, culture** (60 questions per domain)

---

## Construction Pipeline

### Step 1 — Emotional Rewriting

Each of the 240 source questions was rewritten into four emotional registers using the Anthropic API:

| Condition | Style |
|---|---|
| **Neutral** | Plain, encyclopedic, no emotional content (source question kept as-is) |
| **Joy** | Enthusiastic, celebratory, positive anticipation |
| **Sadness** | Grieving, distressed, sorrowful framing |
| **Anger** | Frustrated, outraged, accusatory framing |
| **Fear** | Anxious, worried, frightened framing |

The rewriting prompt is in `prompts/rewriting_prompt.txt`. The instruction was to preserve the factual content of the question exactly — only the surrounding framing, tone, and context were modified.

This produced 960 rewritten variants plus 240 neutral originals = **1,200 stimuli pre-validation**.

### Step 2 — Stimulus Validation

Each rewritten question was classified using [`SamLowe/roberta-base-go_emotions`](https://huggingface.co/SamLowe/roberta-base-go_emotions), a GoEmotions-based classifier with 28 fine-grained emotion labels.

The 28 labels were mapped to five coarse conditions. A stimulus was **accepted** if the summed probability mass for the target emotion exceeded a threshold of **0.30**. Stimuli that failed were excluded.

**Post-validation dataset: 823 stimuli** (attrition reflects how reliably each emotion could be injected and detected).

---

## Dataset Statistics

### By Emotion Condition

| Condition | n | % of total |
|---|---|---|
| Neutral | 240 | 29.2% |
| Sadness | 200 | 24.3% |
| Fear | 143 | 17.4% |
| Anger | 160 | 19.4% |
| Joy | 80 | 9.7% |

Joy showed the highest attrition during validation, suggesting enthusiastic framing was hardest to inject in a way the classifier reliably detected.

### By Topic Domain

| Domain | n | % of total |
|---|---|---|
| Geography | 210 | 25.5% |
| Science | 204 | 24.8% |
| Culture | 206 | 25.0% |
| History | 203 | 24.7% |

Domain coverage remained well-balanced post-validation.

### Validation Scores

Mean validation score (summed target-emotion probability mass) across all accepted stimuli: **≥ 0.95**. Stimuli were retained only if the intended emotional register was reliably detectable, making this a high-confidence set of emotional rewrites.

---

## Schema

Each record in the dataset contains the following fields:

| Field | Type | Description |
|---|---|---|
| `question_id` | string | Unique identifier linking back to the TriviaQA source question |
| `original_question` | string | Source question from TriviaQA (neutral, unmodified) |
| `rewritten_question` | string | Emotionally rewritten variant (or identical to `original_question` for neutral condition) |
| `emotion_condition` | string | One of: `neutral`, `joy`, `sadness`, `anger`, `fear` |
| `domain` | string | One of: `geography`, `history`, `science`, `culture` |
| `ground_truth_answer` | string | Primary correct answer from TriviaQA |
| `answer_aliases` | list[string] | All accepted answer variants (used in scoring layers 1–3) |
| `validation_score` | float | Summed GoEmotions probability mass for the target emotion condition |

---

## Scoring Labels (Inference Results)

The file `results/scored_responses.csv` extends the base dataset with one set of columns per model:

| Field | Type | Description |
|---|---|---|
| `{model}_response` | string | Raw generated response |
| `{model}_correct` | bool | Whether the response was scored as correct (any layer matched) |
| `{model}_score_layer` | int | Which scoring layer matched (1–4), or 0 if incorrect |
| `{model}_error_type` | string | For incorrect responses: `substitution`, `confabulation`, `uncertainty`, or `refusal_empty` |
| `{model}_response_length` | int | Word count of the generated response |

Model name abbreviations used as column prefixes: `mistral`, `llama3`, `phi3`, `falcon`, `flant5`.

---

## Rewriting Examples

The following examples illustrate the emotional rewriting process for each domain.

### Geography

| Condition | Question |
|---|---|
| Neutral | What is the capital of Australia? |
| Joy | I'm so excited to be planning my dream trip — what is the capital of Australia? |
| Sadness | My grandfather always dreamed of visiting but never could — what is the capital of Australia? |
| Anger | I can't believe how poorly represented Australia is in our curriculum — what is the capital? |
| Fear | I'm terrified I'll embarrass myself in front of everyone — what is the capital of Australia? |

### Science

| Condition | Question |
|---|---|
| Neutral | What is the formal (systematic) chemical name for the alcohol in intoxicating drink? |
| Joy | This is my favourite kind of question — what is the formal (systematic) chemical name for the alcohol in intoxicating drink? |
| Sadness | I'm so upset I might not know this — what is the formal (systematic) chemical name for the alcohol in intoxicating drink? |
| Anger | It infuriates me that people don't know — what is the formal (systematic) chemical name for the alcohol in intoxicating drink? |
| Fear | My hands are shaking just thinking about it: what is the formal (systematic) chemical name for the alcohol in intoxicating drink? |

### Culture

| Condition | Question |
|---|---|
| Neutral | What hip-hop artist was controversially chosen to headline the 2008 Glastonbury Festival? |
| Joy | This topic is my passion! what hip-hop artist was controversially chosen to headline the 2008 Glastonbury Festival? |
| Sadness | This makes me feel so inadequate — what hip-hop artist was controversially chosen to headline the 2008 Glastonbury Festival? |
| Anger | I demand to know the answer! what hip-hop artist was controversially chosen to headline the 2008 Glastonbury Festival? |
| Fear | I panic every time I see this question: what hip-hop artist was controversially chosen to headline the 2008 Glastonbury Festival? |

### History

| Condition | Question |
|---|---|
| Neutral | What is the scarlet banner of Saint Dennis used by early French kings when setting out for war? |
| Joy | My favourite topic! what is the scarlet banner of Saint Dennis used by early French kings when setting out for war? |
| Sadness | This question fills me with sadness: what is the scarlet banner of Saint Dennis used by early French kings when setting out for war? |
| Anger | I won't tolerate not knowing this! what is the scarlet banner of Saint Dennis used by early French kings when setting out for war? |
| Fear | This question fills me with dread: what is the scarlet banner of Saint Dennis used by early French kings when setting out for war? |

---

## Limitations

**Validation threshold.** The 0.30 GoEmotions threshold is conservative but not perfect. Some retained stimuli may carry a weaker emotional signal than the validation score suggests; some excluded stimuli may have been genuinely emotional but classified differently due to label granularity.

**Joy attrition.** Only 80 joy stimuli passed validation (vs. 200 for sadness). This creates unequal cell sizes across conditions, which should be kept in mind when interpreting per-condition comparisons.

**Single rewrite per question per condition.** Each source question received exactly one rewrite per emotion. Variance in rewriting quality is not captured; a future version could generate multiple rewrites and select or ensemble across them.

**English only.** All questions and rewrites are in English. Emotion expression and its effect on LLM behaviour may differ substantially across languages.

---

## Source Citation

```
@article{joshi2017triviaqa,
  title={TriviaQA: A Large Scale Distantly Supervised Challenge Dataset for Reading Comprehension},
  author={Joshi, Mandar and Choi, Eunsol and Weld, Daniel and Zettlemoyer, Luke},
  journal={arXiv preprint arXiv:1705.03551},
  year={2017}
}
```
