# Figures
---

## Figure 1 — Overall Hallucination Rate by Model

**File:** `results/figures/fig1_overall_hallucination_rate.png`

Bar chart showing the overall hallucination rate (%) for each of the five evaluated models. Bars are ordered left to right by descending hallucination rate: Flan-T5-Large (78.6%), Falcon-7B (59.3%), Phi-3 Mini (39.2%), Llama-3-8B (25.8%), Mistral-7B (25.5%).

Establishes the baseline performance hierarchy used as the reference throughout the results section. The large spread (>50 pp between best and worst) contextualises all subsequent framing effects.

---

## Figure 2 — Hallucination Rate by Emotion Condition Across All Models

**File:** `results/figures/fig2_hallucination_by_emotion.png`

Grouped bar chart with one group per emotion condition (Neutral, Joy, Sadness, Anger, Fear) and one bar per model within each group. Colour-coded by model.

Shows the absolute hallucination rate under each emotional condition for all five models simultaneously. Anger produces the most visually distinct pattern — bars for Mistral, Llama-3, and Phi-3 are noticeably shorter than their neutral counterparts. Flan-T5 bars remain uniformly tall across all conditions.

---

## Figure 3 — Emotion Effect: Δ Hallucination Rate vs. Neutral Baseline

**File:** `results/figures/fig3_delta_from_neutral.png`

Line plot with emotion condition on the x-axis (Neutral, Joy, Sadness, Anger, Fear) and deviation from the neutral baseline (percentage points) on the y-axis. One line per model.

The key visualisation for RQ1. The y-axis is anchored at 0 pp (neutral baseline), making increases and decreases immediately legible. Anger produces a consistent downward dip across Mistral, Llama-3, and Phi-3 (approximately −3.6 to −3.8 pp). Sadness and fear produce modest upward shifts. Flan-T5 traces a near-flat line close to 0 pp throughout.

---

## Figure 4 — Hallucination Rate by Topic Domain Across All Models

**File:** `results/figures/fig4_hallucination_by_domain.png`

Grouped bar chart with one group per domain (Geography, History, Science, Culture) and one bar per model within each group.

Shows the strong domain effect that emerges as the dominant predictor of hallucination in this study. Geography is consistently the lowest-error domain across all models; culture and history are the most error-prone. The most extreme value — Flan-T5 on culture (99.0%) — is clearly visible and represents a near-complete knowledge gap rather than a framing effect.

---

## Figure 5 — Cramér's V Effect Sizes: Emotion vs. Domain

**File:** `results/figures/fig5_cramers_v_effect_sizes.png`

Grouped bar chart with one group per model and two bars per group: one for the emotion effect (Cramér's V) and one for the domain effect (Cramér's V). Bars for emotion and domain are shown in contrasting colours.

The clearest single summary of the paper's central finding. Emotion effect sizes (0.020–0.058) are uniformly negligible; domain effect sizes (0.186–0.316) are consistently 4–16× larger and fall in the small-to-medium range. The visual gap between the two bars in each group is striking and directly supports the conclusion.

---

## Figure 6 — Emotion × Domain Interaction Heatmaps

**File:** `results/figures/fig6_emotion_domain_interaction.png`

Five heatmaps arranged in a single row, one per model. Rows represent emotion conditions (Neutral, Joy, Sadness, Anger, Fear); columns represent topic domains (Geography, History, Science, Culture). Cell values show hallucination rate (%).

Reveals that emotion effects are not uniform across domains. In domains where models already exhibit high uncertainty (history, culture), emotional framing produces more pronounced variation. In high-confidence domains (geography), emotional framing adds almost no additional noise. Flan-T5's heatmap is uniformly saturated, reflecting its near-ceiling hallucination rates across all cells.

---

## Figure 7 — Hallucination Type Breakdown (% of Errors)

**File:** `results/figures/fig7_hallucination_type_breakdown.png`

Stacked bar chart with one bar per model. Each bar shows the proportion of total incorrect responses attributable to substitution errors vs. confabulation. (Refusal/empty responses are negligible and not shown separately for Flan-T5.)

Substitution errors dominate across all models, typically accounting for >90% of all hallucinations. Confabulation is rare: highest in Falcon (32 instances) and Mistral (21), absent in Llama-3. This pattern suggests that emotional framing biases retrieval — surfacing wrong-but-plausible answers — rather than triggering generative fabrication.

---

## Figure 8 — Mean Response Length: Correct vs. Incorrect Answers

**File:** `results/figures/fig8_mean_response_length.png`

Paired bar chart with one pair per model. Each pair shows mean word count for correct responses (solid) and incorrect responses (hatched).

Incorrect responses are consistently longer than correct ones across all five models. The gap is most pronounced for Mistral (2.7 vs. 6.0 words) and Falcon (3.9 vs. 4.9 words); smallest for Llama-3 (1.6 vs. 1.8 words). Consistent with the substitution-dominated error profile: when models are wrong, they tend to elaborate rather than give a single wrong word.

---

## Figure 9 — Response Length Distributions: Correct vs. Incorrect (Violin Plots)

**File:** `results/figures/fig9_response_length_violin.png`

Two violin plots per model (correct vs. incorrect), arranged as a single multi-panel figure. The violin width encodes the density of responses at each word-count value.

Extends Figure 8 by showing the full distribution shape. Incorrect responses exhibit heavier right tails — particularly for Mistral and Phi-3 — indicating that a subset of hallucinated answers are substantially longer than typical. Correct responses are tightly concentrated at very short lengths, reflecting the greedy short-answer format used in inference.

---

## Figure 10 — Mean Response Length by Emotion Condition and Correctness

**File:** `results/figures/fig10_response_length_by_emotion.png`

Multi-panel bar chart: one panel per model (5 panels), with emotion condition on the x-axis and mean word count on the y-axis. Within each condition, correct and incorrect response lengths are shown as paired bars.

Tests whether any specific emotion condition systematically produces longer or shorter responses. The answer is no: the pattern of incorrect responses being longer than correct ones holds consistently across all emotions and models, with no single condition standing out. This rules out response length as a confound in the emotion effect analysis.
