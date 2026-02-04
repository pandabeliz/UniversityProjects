# Statistical Language Models for Parallel Corpora

> Character-level language modeling and cross-linguistic analysis using the EuroParl corpus

## 📋 Overview

**Course:** Computational Linguistics  
**Institution:** Tilburg University  
**Date:** 2023  
**Status:** ✅ Completed  

This project develops and analyzes character-level Statistical Language Models (SLMs) trained on parallel corpora from the European Parliament (EuroParl). The assignment explores language modeling across English, Dutch, and Italian, examining how different training approaches and n-gram sizes affect model performance and cross-linguistic generalization.

---

## 🎯 Objectives

- Build character-level Statistical Language Models using n-gram approaches
- Compare bigram vs. tetragram models for language modeling
- Analyze the impact of training corpus type (full sentences vs. word types)
- Evaluate model performance using perplexity metrics
- Explore cross-linguistic patterns through language model analysis
- Identify characteristic linguistic patterns in Dutch and Italian

---

## 🔧 Technologies Used

**Programming Language:**
- Python 3.x

**Libraries:**
- **Data Processing:** NumPy, pandas
- **Collections:** defaultdict, Counter
- **File I/O:** json, pickle
- **Text Processing:** Regular expressions (re)

**Tools:**
- Jupyter Notebooks

---

## 📊 Dataset

**Source:** EuroParl Corpus
- **Description:** Parallel corpus from European Parliament proceedings
- **Languages:** English, Dutch, Italian
- **Format:** JSON files with pre-tokenized sentences
- **Structure:** Dictionary mapping IDs to translations in three languages

**Data Files:**
- `training_parallel_sentences_en-nl-it.json` - Training set
- `test_parallel_sentences_en-nl-it.json` - Test set

**Preprocessing Requirements:**
1. **Lowercasing:** All text converted to lowercase
2. **Digit Replacement:** All digits replaced with 'D'
3. **Character Filtering:** Only letters and whitespace retained
4. **Rare Character Handling:** Characters occurring < 20 times replaced with '?'

---

## 🏗️ Methodology

### Task 1: Data Preprocessing

**Preprocessing Pipeline:**
```python
def preprocess(text):
    # 1. Lowercase
    text = text.lower()
    
    # 2. Replace digits
    text = re.sub(r'\d', 'D', text)
    
    # 3. Remove non-letter, non-space characters
    text = re.sub(r'[^a-z\s]', '', text)
    
    return text
```

**Output:**
- Cleaned sentences in all three languages (English, Dutch, Italian)
- Separate preparation for full sentences vs. word types
- Vocabulary filtered for rare characters (threshold: 20 occurrences)

---

### Task 2: Language Model Training

**Four Language Models Built:**

1. **Bigram LM on Sentences**
   - **N-gram size:** 2 (current character given 1 previous)
   - **Training data:** Full English sentences (whitespace removed)
   - **Purpose:** Baseline sentence-level character prediction

2. **Bigram LM on Word Types**
   - **N-gram size:** 2
   - **Training data:** Unique English words (word types)
   - **Purpose:** Vocabulary-focused character patterns

3. **Tetragram LM on Sentences**
   - **N-gram size:** 4 (current character given 3 previous)
   - **Training data:** Full English sentences (whitespace removed)
   - **Purpose:** Enhanced context for character prediction

4. **Tetragram LM on Word Types**
   - **N-gram size:** 4
   - **Training data:** Unique English words
   - **Purpose:** Long-range dependencies in vocabulary

**Key Implementation Details:**

**Smoothing:**
- **Method:** Add-k smoothing
- **Parameter:** k = 0.01
- **Purpose:** Handle unseen character combinations

**Vocabulary Consistency:**
- All four models share the same vocabulary
- Vocabulary derived from English sentence training data
- Ensures fair model comparison

**Special Tokens:**
- **BoS (Beginning of Sentence):** Marks sentence start
- **EoS (End of Sentence):** Marks sentence end
- Ensures proper probability distribution at boundaries

**Class Structure:**
```python
class LM:
    def __init__(self, corpus, n, k=0.01):
        self.n = n              # N-gram size
        self.k = k              # Smoothing parameter
        self._counts_ = {}      # N-gram counts
        self._vocab_ = set()    # Character vocabulary
        self._vocab_size_ = 0   # Vocabulary size
        
    def perplexity(self, test_sentences):
        # Compute perplexity on test data
        pass
```

---

### Task 3: Perplexity Evaluation

**Evaluation Sets:**
1. **English Training Sentences** - Model fit to training data
2. **English Training Word Types** - Vocabulary coverage
3. **English Test Sentences** - Same-language generalization
4. **Dutch Test Sentences** - Cross-linguistic evaluation
5. **Italian Test Sentences** - Cross-linguistic evaluation

**Perplexity Formula:**
```
Perplexity = 2^(-1/N * Σ log₂ P(cᵢ | context))
```
Where:
- N = total number of characters
- P(cᵢ | context) = probability of character given n-1 previous characters

**Expected Patterns:**
- Lower perplexity = better model fit
- Training data should have lower perplexity than test data
- Same-language test data should have lower perplexity than cross-linguistic data

---

### Task 4: Extreme Perplexity Analysis

**Objective:** Identify characteristic linguistic patterns

**Selection Criteria:**
- **Minimum length:** 5 characters
- **Minimum frequency:** 5 occurrences in test data
- **Languages:** Dutch and Italian only

**Analysis:**
1. **Lowest Perplexity Words:**
   - Words that fit English character patterns well
   - Indicates cross-linguistic similarity
   - 8 words total (4 models × 2 languages)

2. **Highest Perplexity Words:**
   - Words that differ most from English patterns
   - Language-specific character combinations
   - 8 words total (4 models × 2 languages)

---

### Task 5: Analytical Questions

**Five Key Research Questions:**

**A. Training Set Comparison (English Sentences vs. Word Types)**
- How does training corpus type affect perplexity?
- Why do full sentences vs. word types yield different results?
- Impact of context availability and data distribution

**B. Bigram vs. Tetragram Generalization**
- Which model generalizes better to unseen English sentences?
- Trade-off between context and data sparsity
- Overfitting vs. underfitting considerations

**C. Cross-Linguistic Performance**
- Which factor matters more: n-gram size or training corpus type?
- English → Dutch vs. English → Italian transfer
- Linguistic distance and character-level patterns

**D. Low Perplexity Patterns**
- What makes some Dutch/Italian words easy for English LMs?
- Cognates and shared character sequences
- Romance language influences

**E. High Perplexity Patterns**
- What makes some Dutch/Italian words difficult for English LMs?
- Language-specific digraphs and trigraphs
- Character combinations absent in English

---

## 📈 Results

### Model Outputs

**Deliverables:**
1. **Four Language Model Files (.pkl):**
   - `BelizPekkan_sents_2gr_en.pkl` - Bigram on sentences
   - `BelizPekkan_words_2gr_en.pkl` - Bigram on word types
   - `BelizPekkan_sents_4gr_en.pkl` - Tetragram on sentences
   - `BelizPekkan_words_4gr_en.pkl` - Tetragram on word types

2. **Perplexity Results (.csv):**
   - `BelizPekkan_perplexities.csv` - All model evaluations
   - Structure:
     ```
     | ngram_size | training_data | test_data | perplexity |
     |------------|---------------|-----------|------------|
     | 2          | sents         | ENtrain   | X.XXXX     |
     | ...        | ...           | ...       | ...        |
     ```

3. **Extreme Words Analysis (.csv):**
   - `BelizPekkan_perplexities_min.csv` - Lowest perplexity words
   - `BelizPekkan_perplexities_max.csv` - Highest perplexity words
   - Structure:
     ```
     | lang | word | ngram_size | training_data | perplexity |
     |------|------|------------|---------------|------------|
     | nl   | ...  | 2          | sents         | X.XXXX     |
     | ...  | ...  | ...        | ...           | ...        |
     ```

### Key Findings

**1. Training Corpus Type Effect:**
- **Word types** show lower perplexity on vocabulary items
- **Full sentences** capture better contextual patterns
- Trade-off between vocabulary coverage and context modeling

**2. N-gram Size Impact:**
- **Bigrams (n=2):**
  - More robust to data sparsity
  - Better generalization on unseen data
  - Lower computational complexity

- **Tetragrams (n=4):**
  - Capture longer-range dependencies
  - Higher risk of overfitting
  - Better fit on training data but may struggle on test data

**3. Cross-Linguistic Patterns:**
- **Italian:** Higher similarity to English (Romance language, Latin roots)
- **Dutch:** More distinct patterns (Germanic language, compound words)
- Character-level models capture phonotactic similarities

**4. Low Perplexity Words (Easy for English LM):**
- **Characteristics:**
  - Cognates (similar words across languages)
  - Common Romance/Germanic roots
  - Standard European character sequences
- **Examples:** Words with patterns like -tion, -al, -er

**5. High Perplexity Words (Hard for English LM):**
- **Characteristics:**
  - Language-specific digraphs (e.g., Dutch "ij", Italian "gli")
  - Uncommon character combinations
  - Compound word boundaries in Dutch
- **Examples:** Words with unique character sequences

---

## 💡 Key Learnings

### Technical Insights

1. **Character-Level Modeling:**
   - N-gram models capture phonotactic patterns
   - Character context crucial for morphology
   - Vocabulary filtering affects model robustness

2. **Smoothing Importance:**
   - Add-k smoothing prevents zero probabilities
   - Small k (0.01) balances seen/unseen events
   - Critical for cross-linguistic evaluation

3. **Perplexity as Metric:**
   - Measures model uncertainty
   - Lower = better fit
   - Sensitive to vocabulary size

4. **Model Comparison:**
   - Shared vocabulary essential for fair comparison
   - Training corpus type significantly affects results
   - Context length vs. data sparsity trade-off

### Linguistic Insights

1. **Cross-Linguistic Transfer:**
   - Character patterns transfer across related languages
   - Linguistic distance affects perplexity
   - English → Italian easier than English → Dutch

2. **Cognates & Borrowings:**
   - Shared Latin/Greek roots create low-perplexity words
   - European Parliament domain increases overlap
   - Formal register reduces language-specific patterns

3. **Language-Specific Patterns:**
   - Dutch compounds create unique character sequences
   - Italian double consonants distinct from English
   - Digraphs mark language boundaries

4. **Phonotactic Constraints:**
   - Character-level LMs implicitly learn phonotactics
   - Some character combinations universal, others language-specific
   - Perplexity reflects phonotactic distance

---

## 📚 Concepts Covered

### Statistical Language Modeling
- N-gram models (bigrams, tetragrams)
- Probability estimation from counts
- Add-k (Laplace) smoothing
- Perplexity as evaluation metric

### Corpus Linguistics
- Parallel corpora analysis
- Word types vs. tokens
- Cross-linguistic comparison
- EuroParl corpus characteristics

### Natural Language Processing
- Character-level modeling
- Text preprocessing and normalization
- Vocabulary management
- Rare event handling

### Cross-Linguistic Analysis
- Language transfer patterns
- Cognate identification
- Phonotactic similarity
- Typological distances

---

## 🔮 Future Improvements

**1. Advanced Smoothing:**
- Kneser-Ney smoothing
- Interpolation methods
- Backoff strategies

**2. Neural Language Models:**
- Character-level RNNs
- LSTMs for longer context
- Transformer-based models

**3. Multilingual Extensions:**
- Add more languages (Spanish, French, German)
- Multilingual vocabulary
- Cross-lingual transfer learning

**4. Linguistic Analysis:**
- Morphological decomposition
- Phoneme-level analysis
- Stress pattern modeling

**5. Applications:**
- Language identification
- Spell checking
- Text generation
- Machine translation support

---

## 📖 References

**Theoretical Background:**
1. Jurafsky, D., & Martin, J. H. (2023). *Speech and Language Processing* (3rd ed.). Chapter 3: N-gram Language Models.
2. Manning, C. D., & Schütze, H. (1999). *Foundations of Statistical Natural Language Processing*. MIT Press.

**EuroParl Corpus:**
- Koehn, P. (2005). Europarl: A parallel corpus for statistical machine translation. *MT Summit*.

---


## 📄 License

This project is for educational purposes as part of the Computational Linguistics course at Tilburg University.

---

## 📧 Contact

**Beliz Pekkan**  
Email: belizpekkan@gmail.com  
Website: [belizpekkan.com](https://belizpekkan.com)  
LinkedIn: [linkedin.com/in/beliz-pekkan](https://www.linkedin.com/in/beliz-pekkan)

---


**Note:** This project demonstrates proficiency in statistical NLP, language modeling, and cross-linguistic analysis. The implementation balances theoretical understanding with practical coding skills, showcasing both computational linguistics knowledge and Python programming expertise.
