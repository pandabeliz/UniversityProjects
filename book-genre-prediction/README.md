# 📚 Multi-Label Book Genre Classification
> Multi-label book genre classification from descriptions using TF-IDF, Word2Vec, FastText, and fine-tuned DistilBERT &amp; Sentence Transformer models — with full statistical comparison across 54 genres.

**Course:** Text and Media Analytics (INFOTMA) — 2025–2026 from Utrecht University
**Dataset:** [Best Books (10k) Multi-Genre](https://www.kaggle.com/datasets/ishikajohari/best-books-10k-multi-genre-data) — Kaggle  
**Language:** Python 3 | **Environment:** Google Colab

---

## Project Overview

This project tackles the challenge of **multi-label classification** — assigning one or more genre labels to a book based solely on its description text. Unlike single-label classification, each book can belong to multiple genres simultaneously (the average book in this dataset carries **3.34 genre labels**), making this a significantly harder problem.

The project follows a systematic comparative methodology: five distinct text representation pipelines are trained, tuned via grid search, evaluated with cross-validation, and compared using bootstrap statistical significance tests. An in-depth error analysis then investigates which genres are hardest to classify and why.

---

## Dataset

| Property | Value |
|---|---|
| Source | Best Books (10k) Multi-Genre — Kaggle |
| Total Books (after cleaning) | ~8,861 |
| Number of Genres | 54 |
| Input Feature | Book description (text) |
| Target | Multi-label genre vector |
| Avg. Genres per Book | 3.34 |
| Train / Test Split | 80 / 20 (stratified) |

### Preprocessing Steps

The raw dataset underwent several cleaning stages before modelling. Books with missing or very short descriptions (fewer than 20 words) were removed. Overly broad or non-content genres were dropped, and semantically similar genres were merged based on co-occurrence analysis. Text was lowercased, tokenized, lemmatized, and stripped of stopwords for the traditional NLP pipelines.

---

## Research Questions

The project is organised around three core research questions:

**RQ1 — How do different text representation methods compare for multi-label genre classification?**  
This is the central comparative question. Five pipelines spanning classical bag-of-words, static word embeddings, and contextual transformer embeddings are benchmarked head-to-head on identical train/test splits with statistical significance testing.

**RQ2 — Which genres are most difficult to classify, and what confusion patterns emerge?**  
A detailed per-genre breakdown of precision, recall, and F1-score is performed. Co-confusion matrices reveal which genre pairs are systematically confused, and hierarchical clustering groups genres by their confusion similarity.

**RQ3 — What textual and stylistic features are most predictive of each genre?**  
TF-IDF feature importance, distinctive word analysis, bigram extraction, and stylistic metrics (vocabulary richness, average sentence length, word count) are used to understand what makes each genre linguistically identifiable.

---

## Methodology

### Train/Test Protocol

A single stratified 80/20 split is used throughout, ensuring all five models are evaluated on the exact same test set for fair comparison. A fixed random seed (42) is used everywhere for reproducibility.

### Hyperparameter Tuning

Each model is tuned using grid search over a predefined parameter space. Cross-validation (5-fold) is used during the search to select the best configuration, and final performance is reported on the held-out test set.

### Statistical Validation

Model comparisons are not based on point estimates alone. **Bootstrap resampling (1,000 iterations)** is used to compute confidence intervals and two-sided p-values for all pairwise model comparisons, ensuring differences are statistically significant at α = 0.05.

### Evaluation Metrics

Given the multi-label nature of the task, four complementary metrics are reported:

| Metric | What It Measures |
|---|---|
| **Micro F1** | Overall token-level precision/recall balance across all labels |
| **Macro F1** | Average F1 per genre (treats rare genres equally) |
| **Weighted F1** | Average F1 weighted by genre support |
| **Hamming Loss** | Average fraction of incorrect label predictions per sample |
| **Subset Accuracy** | Fraction of samples where the entire predicted label set is exactly correct |

---

## Models Implemented

### Model 1 — TF-IDF + Random Forest (OneVsRest)

The baseline model. Text is vectorised using TF-IDF with up to bigrams (max 10,000 features). A Random Forest classifier is wrapped in a OneVsRest strategy to handle the multi-label output. Class weights are set to `balanced` to address genre imbalance.

### Model 2 — Word2Vec + Logistic Regression

Book descriptions are converted into fixed-length vectors by averaging the Word2Vec embeddings of all tokens. A Logistic Regression classifier (OneVsRest) is trained on these dense representations, tuned over regularisation strength C.

### Model 3 — FastText + Logistic Regression

Functionally similar to the Word2Vec pipeline, but uses FastText embeddings, which capture subword information. This makes them more robust to typos and rare words. The same Logistic Regression setup is used for direct comparison.

### Model 4 — DistilBERT (Fine-tuned Transformer)

A `distilbert-base-uncased` model is fine-tuned end-to-end for multi-label classification. The architecture adds a two-layer classification head (with dropout) on top of the transformer's `[CLS]` token output. The embedding layer is frozen to reduce memory usage. Training uses AdamW with a linear learning rate schedule, and the best checkpoint is selected based on validation Micro F1.

### Model 5 — Sentence Transformer + Logistic Regression

Sentence-level embeddings are generated using the `all-MiniLM-L6-v2` model (384-dimensional vectors). These pre-trained embeddings capture semantic meaning without task-specific fine-tuning. A Logistic Regression head is trained on top, following the same tuning protocol as Models 2 and 3.

---

## Results

### Aggregated Model Performance

| Model | Micro F1 | Macro F1 | Weighted F1 | Hamming Loss | Subset Accuracy |
|---|---|---|---|---|---|
| TF-IDF + Random Forest | 0.453 | 0.235 | 0.411 | 0.051 | 0.066 |
| Word2Vec + Logistic Regression | 0.382 | 0.203 | 0.347 | 0.054 | 0.052 |
| FastText + Logistic Regression | 0.349 | 0.171 | 0.314 | 0.056 | 0.048 |
| **DistilBERT Transformer** | **0.569** | **0.278** | 0.508 | **0.043** | **0.097** |
| Sentence Transformer + LR | 0.490 | 0.433 | **0.531** | 0.101 | 0.008 |

### Statistical Significance (Bootstrap, p < 0.05)

All pairwise comparisons between the top models yielded statistically significant differences. DistilBERT outperformed the Sentence Transformer on Micro F1 (p < 0.001), and both transformer-based approaches significantly outperformed the static embedding and TF-IDF baselines.

### Top and Bottom Genres (Best Model)

The genres with the highest F1 scores — Self Help (0.706), Classics (0.692), and Fantasy (0.686) — tend to have large support and distinctive vocabulary. The hardest genres — Magical Realism (0.129), Drama (0.141), and New Adult (0.142) — suffer from low support and heavy overlap with neighbouring genres like Contemporary Fiction and Literary Fiction.

---

## Key Findings

**Fine-tuned transformers dominate, but not uniformly.** DistilBERT achieves the best Micro F1 and Subset Accuracy, reflecting stronger local classification. However, the Sentence Transformer achieves the best Macro F1 (0.433 vs. 0.278), indicating it handles rare genres more equitably — a meaningful distinction in an imbalanced setting.

**TF-IDF is a surprisingly strong baseline.** Despite its simplicity, TF-IDF + Random Forest outperforms both Word2Vec and FastText. This suggests that for genre classification from short descriptions, the sparse bag-of-words signal (especially with bigrams) is more informative than averaged static embeddings, which lose word-order information.

**Genre confusion follows semantic logic.** The most confused pairs — Contemporary Fiction / Fantasy, Drama / Historical Fiction, Contemporary Fiction / Mystery — share thematic overlap rather than being random errors. Hierarchical clustering of confusion patterns reveals clear genre "families" that the models struggle to separate.

**Rare genres are systematically underclassified.** Genres with fewer than 50 training samples (e.g., Magical Realism, LGBTQ+ Fiction, New Adult) consistently fall below 0.15 F1. This is a structural limitation of the dataset size rather than a modelling failure.

---
