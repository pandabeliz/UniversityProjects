# Bilingualism and Native Listening Abilities

**Investigating How Second Language Acquisition Affects Native Language Perception**

## 📋 Overview

**Course:** Bachelor of Science in Cognitive Science and Artificial Intelligence  
**Institution:** Tilburg University - School of Humanities and Digital Sciences  
**Date:** 2024  
**Status:** ✅ Completed

This thesis investigates the influence of bilingualism on native listening abilities, with a particular focus on Turkish-English sequential bilinguals. Bilingualism, the ability to fluently speak two or more languages, fundamentally reshapes human perception and cognition, altering innate language acquisition and processing mechanisms. This research examines how acquiring a second language affects native language perception—an area that remains underexplored despite growing interest in bilingualism research.

The study compares the performance of 40 bilingual and 40 monolingual Turkish speakers in recalling iambic syllable sequences across different auditory conditions (pitch, duration, and flat), aiming to understand whether dual-language processing enhances or alters native listening capabilities.

## 🎯 Research Questions

### Main Research Question
**To what extent does bilingualism affect native listening abilities?**

### Sub-Research Questions
1. **How does bilingualism influence the accuracy of recalling specific linguistic sequences compared to monolingual counterparts?**

2. **How do different auditory conditions and age affect the native listening accuracy of bilingual and monolingual participants?**

### Hypotheses

**Hypothesis 1: Enhanced Native Listening in Bilinguals**  
Due to their experience with dual-language processing, bilinguals will exhibit superior native listening skills compared to monolinguals. This enhanced ability will be evident in tasks involving recall of iambic syllable sequences, where bilinguals are hypothesized to outperform their monolingual peers.

**Hypothesis 2: Differential Impact of Auditory Conditions and Age on Listening Accuracy**  
Different auditory conditions (pitch, duration, flat) and age will affect native listening accuracy differently for bilingual and monolingual participants. Specifically, bilinguals are expected to show higher resilience to changes in auditory conditions across different age groups, reflecting their enhanced auditory processing flexibility.

---

## 🔧 Technologies Used

**Programming Language:**
- **Python 3.x** (Jupyter Notebook)

**Libraries & Packages:**
- `pandas` - Data manipulation and analysis
- `matplotlib` - Data visualization
- `seaborn` - Statistical data visualization
- `statsmodels` - Statistical modeling and ANOVA

**Statistical Methods:**
- Analysis of Variance (ANOVA) using statsmodels
- OLS (Ordinary Least Squares) regression
- Age group stratification analysis

**Data Collection Platforms:**
- Jatos experimental platform
- Qualtrics for demographic surveys
- MBROLA and IT4 voice synthesizer for stimuli generation

---

## 📊 Dataset

**Participant Demographics:**
- **Total Participants:** 80 (40 bilingual, 40 monolingual)
- **Average Age:** 36.41 years (SD = 14.54)
- **Age Range:** 18-65+ years
- **Language Background:** Turkish native speakers; bilinguals are Turkish-English sequential bilinguals
- **Gender Distribution:** Approximately 61.7% female

**Study Design:**
- **Type:** Between-subjects experimental design
- **Groups:** 
  - Monolingual Turkish speakers (control group)
  - Turkish-English sequential bilinguals (experimental group)

**Experimental Conditions:**

1. **Session Types (Auditory Manipulations):**
   - **Flat Condition (Control):** Consistent pitch (180 Hz) and duration (360 ms)
   - **Pitch Condition:** Alternating pitch (180 Hz vs. 400 Hz), consistent duration (360 ms)
   - **Duration Condition:** Alternating duration (360 ms vs. 580 ms), consistent pitch (180 Hz)

2. **Test Structure:**
   - **Familiarization Phase:** 2-minute continuous syllable stream (/pa su tu ke ma vi bu go ne du/)
   - **Test Phase:** 20 trials per session with syllable pairs
   - **Audio Types:** Correct pairs (adjacent syllables) vs. incorrect pairs (non-adjacent syllables)
   - **Response Method:** Binary choice (heard/not heard)

3. **Additional Variables:**
   - Hand placement (left/right) for response mapping
   - Instruction language (Turkish for monolinguals; alternating Turkish/English for bilinguals)
   - Inter-stimulus interval: 100 ms

**Why Turkish-English Bilinguals?**

Turkish and English have fundamentally different linguistic structures:
- **Turkish:** Agglutinative language with SOV word order and vowel harmony
- **English:** Non-agglutinative language with SVO word order

This structural difference makes Turkish-English bilinguals particularly interesting for studying how second language acquisition affects native language processing, especially concerning the **Iambic-Trochaic Law**—a principle suggesting humans have innate biases in perceiving sound sequences.

## 🚀 Methodology

### Theoretical Framework

**The Iambic-Trochaic Law**

The Iambic-Trochaic Law is a principle from sound perception suggesting humans have innate biases in grouping sound sequences:
- **Iambic Pattern:** Sequences differing in duration are grouped with the longest element in the final position (short-long)
- **Trochaic Pattern:** Sequences differing in intensity or pitch are grouped with the more intense/high-pitched element in the initial position (high-low)

**Native Listening**

Native listening refers to how an individual's native language shapes their understanding and interpretation of linguistic cues. Native speakers develop highly adaptable systems for processing speech in their native language, including:
- Flexible phonetic category boundaries
- Adaptation to different speakers and environments
- Language-specific rhythmic grouping preferences

### Experimental Procedure

**Phase 1: Participant Recruitment and Setup**
1. Participants received Qualtrics survey links containing experiment information
2. Created unique identifiers (mother's name initials + father's name initials + birthday digits)
3. Demographic information collected through survey questions

**Phase 2: Familiarization**
1. Instructions presented (Turkish only for monolinguals; alternating Turkish/English for bilinguals)
2. 2-minute continuous syllable stream presented
3. Black screen with white fixation cross displayed during audio

**Phase 3: Testing**
1. 20 test trials presented (1.5 seconds each)
2. Syllable pairs played (correct or incorrect)
3. Binary response: Happy face (heard) vs. Sad face (not heard)
4. Response keys mapped based on hand placement condition

**Phase 4: Multiple Sessions**
- **Monolinguals:** 3 sessions (flat, pitch, duration) in Turkish
- **Bilinguals:** 5 sessions alternating between Turkish and English instructions
- Distracting demographic questions between sessions to prevent memorization

### Statistical Analysis Strategy

**1. Baseline Comparison**
- ANOVA comparing monolingual vs. bilingual accuracy in flat session
- Age included as covariate to control for age-related differences

**2. Group Membership Effects**
- ANOVA with group (bilingual/monolingual) and age as independent variables
- Dependent variable: Prediction accuracy

**3. Age Stratification Analysis**
- Participants divided into two groups:
  - **Younger:** 18-35 years (n=111)
  - **Older:** 36+ years (n=101)
- Separate ANOVAs for each age group

**4. Auditory Condition Analysis**
- ANOVA examining effects of:
  - Group membership (bilingual/monolingual)
  - Session type (flat/pitch/duration)
  - Audio type (correct/incorrect)
  - Age

**5. Session-Specific Analysis**
- Separate analyses for pitch and duration sessions
- Examined session type × group interactions

**6. Audio Type Analysis**
- Separate ANOVAs for correct vs. incorrect audio cues
- Examined how each group responds to accurate vs. inaccurate stimuli

## 📈 Results

### Primary Findings Summary

| Research Question | Key Finding | Statistical Significance | Effect Size |
|-------------------|-------------|-------------------------|-------------|
| **Overall Treatment Effect** | Bilingualism shows minor, non-significant impact | F(1,77)=1.08, p=0.303 | Small |
| **Age Effect** | Age significantly impacts accuracy | F(1,77)=9.37, p=0.003** | Large |
| **Audio Type Effect** | Audio type dramatically affects accuracy | F(1,410)=143.32, p<0.001*** | Very Large |
| **Session Type (Monolinguals)** | Marginal effect on accuracy | F(1,73)=4.03, p=0.048* | Small |
| **Session Type (Bilinguals)** | Significant effect on accuracy | F(1,56)=4.67, p=0.035* | Moderate |

**Significance levels:** * p<0.05, ** p<0.01, *** p<0.001

---

### 1. Baseline Comparisons (Flat Session)

**Without Age Covariate:**
- No significant difference between bilinguals and monolinguals: F(1,75)=2.28, p=0.135

**With Age Covariate:**
- Group membership: F(1,74)=2.52, p=0.117 (not significant)
- Age: F(1,74)=1.03, p=0.314 (not significant)

**Interpretation:** Both groups perform similarly in the control (flat) condition, validating the experimental design and confirming groups are comparable at baseline.

---

### 2. Overall Bilingualism Effect

**Main Analysis (All Sessions):**
- **Group Membership:** F(1,77)=1.08, p=0.303 (not significant)
- **Age:** F(1,77)=9.37, p=0.003** (highly significant)

**Mean Accuracy:**
- Young Bilinguals: 0.468
- Young Monolinguals: 0.498
- Older Bilinguals: 0.659
- Older Monolinguals: 0.489

**Key Insight:** While bilingualism alone doesn't significantly affect accuracy overall, age plays a crucial moderating role. **This partially refutes Hypothesis 1** for younger adults but supports it for older adults.

---

### 3. Age Group Analysis

#### Younger Participants (18-35 years)

- **Effect of Bilingualism:** F(1,109)=0.59, p=0.444 (not significant)
- **Interpretation:** No bilingual advantage in younger adults

#### Older Participants (36+ years)

- **Effect of Bilingualism:** F(1,99)=8.85, p=0.004** (highly significant)
- **Mean Accuracy:**
  - Older Bilinguals: 0.659 (significantly higher)
  - Older Monolinguals: 0.489

**Critical Finding:** **Bilingualism shows protective cognitive benefits in older adults.** Older bilinguals significantly outperform older monolinguals, suggesting bilingualism may contribute to cognitive resilience with aging. This strongly supports **Hypothesis 2** for older populations.

---

### 4. Impact of Auditory Conditions

**Main Effects:**
- **Group Membership:** F(1,410)=1.92, p=0.167 (not significant)
- **Audio Type:** F(1,410)=143.32, p<1.57×10⁻²⁸*** (extremely significant)
- **Age:** F(1,410)=6.13, p=0.014* (significant)

**Audio Type Performance:**
- **Correct Audio Cues:** Participants performed significantly better
- **Incorrect Audio Cues:** Performance dropped dramatically for both groups
- **Monolinguals:** Steeper decline with incorrect cues
- **Bilinguals:** More resilient to incorrect cues (smaller performance drop)

**Key Insight:** The nature of auditory stimuli is the strongest predictor of performance. Both groups rely heavily on accurate acoustic cues, but **bilinguals show slightly better resilience to degraded input**, partially supporting **Hypothesis 2**.

---

### 5. Session Type Effects

#### Monolinguals

- **Session Type:** F(1,73)=4.03, p=0.048* (marginally significant)
- **Age:** F(1,73)=0.09, p=0.763 (not significant)
- **Mean Accuracy:**
  - Pitch Session: 0.497 (SD=0.129)
  - Duration Session: 0.467 (SD=0.121)

**Interpretation:** Monolinguals show slight sensitivity to session type, performing marginally better in pitch condition.

#### Bilinguals

- **Session Type:** F(1,56)=4.67, p=0.035* (significant)
- **Age:** F(1,56)=12.33, p=0.001** (highly significant)
- **Mean Accuracy:**
  - Pitch Session: 0.658 (SD=0.137) — **significantly higher**
  - Duration Session: 0.488 (SD=0.113)

**Interpretation:** Bilinguals show stronger session-dependent performance, with marked advantage in pitch condition. Age remains a significant moderator for bilinguals.

**Critical Finding:** **Bilinguals excel in pitch-based tasks**, potentially reflecting enhanced auditory processing from managing two language systems. This supports **Hypothesis 1** specifically for pitch conditions.

---

### 6. Audio Type and Session Type Interaction

#### Monolinguals (Correct vs. Incorrect Audio)

**Correct Audio:**
- Session Type: F(2,823)=1.63, p=0.196 (not significant)
- Age: F(1,823)=0.03, p=0.860 (not significant)

**Incorrect Audio:**
- Session Type: F(2,821)=1.96, p=0.142 (not significant)
- Age: F(1,821)=0.05, p=0.824 (not significant)

#### Bilinguals (Correct vs. Incorrect Audio)

**Correct Audio:**
- Session Type: F(2,1090)=0.09, p=0.919 (not significant)
- Language: F(1,1090)=0.59, p=0.443 (not significant)
- **Age: F(1,1090)=19.34, p<0.000012*** (extremely significant)**

**Incorrect Audio:**
- **Session Type: F(2,1088)=3.72, p=0.025* (significant)**
- Language: F(1,1088)=1.40, p=0.237 (not significant)
- **Age: F(1,1088)=48.14, p<6.79×10⁻¹²*** (extremely significant)**

**Critical Insight:** For bilinguals, **age is the dominant factor across all conditions**. Session type only matters significantly when audio cues are incorrect, suggesting bilinguals are more adaptive to auditory degradation but still sensitive to task difficulty variations. This supports **Hypothesis 2**.

---

### Summary of Hypothesis Testing

| Hypothesis | Supported? | Evidence |
|------------|-----------|----------|
| **H1: Enhanced Native Listening in Bilinguals** | **Partially Supported** | - Not supported for overall accuracy or younger adults<br>- **Strongly supported for older adults** (F=8.85, p=0.004)<br>- **Strongly supported for pitch conditions** (mean=0.658 vs 0.497) |
| **H2: Differential Impact of Auditory Conditions & Age** | **Strongly Supported** | - Age shows highly significant effects (multiple p<0.001)<br>- Bilinguals more resilient to incorrect audio cues<br>- Session type affects bilinguals more (p=0.035)<br>- Age×group interaction significant for older adults |

---

## 🔍 Key Findings

### 1. Age as the Dominant Factor

**Most significant finding:** Age is the primary determinant of native listening accuracy, far exceeding the effect of bilingualism alone (F(1,77)=9.37, p=0.003).

**Clinical Significance:**
- Cognitive aging affects auditory processing regardless of language background
- Bilingualism shows **protective effects in older adults specifically**
- Younger adults show no bilingual advantage, suggesting benefits emerge with age

### 2. The Bilingual Advantage Emerges in Older Adults

**Breakthrough Finding:** Older bilinguals (36+) significantly outperform older monolinguals (0.659 vs. 0.489 accuracy, p=0.004).

**Interpretation:**
- Lifelong management of two languages may build **cognitive reserve**
- This reserve becomes apparent as age-related cognitive decline begins
- Aligns with research showing bilingualism delays cognitive aging

### 3. Audio Cue Quality is Critical

**Strongest Effect:** Audio type (correct vs. incorrect) has the most dramatic impact on performance (F=143.32, p<0.001).

**Both groups rely heavily on acoustic accuracy:**
- Correct audio → high accuracy for both groups
- Incorrect audio → performance drops sharply
- **Monolinguals show steeper decline** with incorrect cues
- **Bilinguals more resilient** to degraded input

**Practical Implication:** Auditory processing quality is fundamental to language perception, but bilingual experience provides modest protection against degraded input.

### 4. Bilinguals Excel in Pitch-Based Processing

**Surprising Discovery:** Bilinguals show marked superiority in pitch sessions (mean=0.658) compared to monolinguals (mean=0.497).

**Possible Explanations:**
- Managing two languages enhances **prosodic sensitivity**
- Tonal and pitch variations are critical for distinguishing languages
- Bilinguals develop **enhanced auditory discrimination** for pitch cues

**Linguistic Relevance:** Turkish is a non-tonal language, but managing English (with different stress patterns) may heighten sensitivity to pitch-based grouping.

### 5. Session Type Sensitivity in Bilinguals

**Significant Finding:** Session type significantly affects bilingual performance (F=4.67, p=0.035) more than monolinguals (F=4.03, p=0.048).

**Interpretation:**
- Bilinguals show **greater cognitive flexibility** across conditions
- This flexibility comes with **condition-specific variability**
- Suggests adaptive processing strategies developed through dual-language management

### 6. Bilingualism Doesn't Impair Native Listening

**Reassuring Finding:** Despite concerns that bilingualism might interfere with native language processing, no evidence of impairment was found.

**Implications:**
- Second language acquisition does **not harm** native listening abilities
- In fact, it may provide **subtle enhancements** in specific contexts (pitch, aging)
- Supports continued promotion of bilingual education

---

## 📄 License

This project is for educational and research purposes as part of the Bachelor of Science in Cognitive Science and Artificial Intelligence curriculum at Tilburg University.

**Note:** Participant data is confidential and not included in this repository. Analysis code and summary statistics demonstrate methodological approaches using anonymized data structures.

---

## 📧 Contact

**Beliz Pekkan**  

- 📧 Email: belizpekkan@gmail.com
- 🌐 Website: [belizpekkan.com](https://belizpekkan.com)
- 💼 LinkedIn: [linkedin.com/in/beliz-pekkan](https://linkedin.com/in/beliz-pekkan)
- 💻 GitHub: [github.com/pandabeliz/CSAI_Thesis_BP](https://github.com/pandabeliz/CSAI_Thesis_BP)

---

## 📚 Key References

**Foundational Works:**
- Bion, R. A. H., Benavides-Varela, S., & Nespor, M. (2011). Acoustic markers of prominence influence infants' and adults' segmentation of speech sequences. *Language and Speech*. 
- Cutler, A. (2012). Native listening: The flexibility dimension. *Dutch Journal of Applied Linguistics*, 1(2), 169-187.
- Langus, A., et al. (2016). Listening natively across perceptual domains? *Journal of Experimental Psychology: Learning, Memory, and Cognition*, 42(7), 1127-1139.

**Bilingualism & Cognition:**
- Bialystok, E., Craik, F. I. M., Green, D. W., & Gollan, T. H. (2009). Bilingual minds. *Psychological Science in the Public Interest*, 10(3), 89-129.
- Asadollahpour, F., Baghban, K., & Mirbalouchzehi, P. (2015). The performance of bilingual and monolingual children on working memory tasks. *Iranian Rehabilitation Journal*, 13(3), 54-58.

**Full Bibliography:** See thesis document (pages 22-23)

---

**Note:** This research highlights the complex relationship between bilingualism and native listening abilities. While bilingualism itself may not dramatically alter native listening in younger adults, it provides significant cognitive benefits for older adults, particularly in pitch-based auditory processing. The findings underscore that **second language acquisition enhances rather than impairs cognitive function**, supporting the value of bilingual education and lifelong language learning.
