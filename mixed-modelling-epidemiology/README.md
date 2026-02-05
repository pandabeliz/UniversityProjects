# Exercise Intervention Effects on Depression in Older Adults with Cancer

**A Comparative Analysis: Linear Mixed-Effects Models vs. Two-Stage Meta-Analysis**

## 📋 Overview

**Course:** Epidemiology and Medical Datascience  
**Institution:** Utrecht University - Applied Data Science Masters  
**Date:** 2026 
**Status:** ✅ Completed

This project analyzes data from a large-scale multi-center randomized controlled trial examining whether exercise interventions reduce depression severity in older adults with cancer. The unique aspect of this analysis is that **the same dataset is analyzed using two distinct statistical approaches**, allowing for comparison of methodological choices and their implications for clinical inference.

The study includes 1,912-1,929 participants across 27 research centers in 5 countries (Australia, Canada, Germany, United Kingdom, United States), randomized to either exercise intervention or control groups.

## 🎯 Research Questions

### Main Research Question
Does exercise intervention significantly reduce depression severity at follow-up compared to control conditions?

### Sub-Research Questions
1. Does patient age moderate the effect of exercise intervention on depression?
2. Does intervention duration moderate the effect of exercise intervention on depression?
3. Does the country where the study was conducted moderate the effect of exercise intervention on depression?

## 🔧 Technologies Used

**Programming Language:**
- R

**Libraries & Packages:**
- `lme4` - Linear mixed-effects models
- `lmerTest` - Statistical tests for mixed models
- `metafor` - Meta-analysis framework
- `dplyr` - Data manipulation
- `ggplot2` - Data visualization
- `tidyr` - Data tidying

**Statistical Methods:**
- Linear Mixed-Effects Models (LMM)
- Individual Participant Data Meta-Analysis (IPDMA)
- Restricted Maximum Likelihood (REML) estimation
- Likelihood Ratio Tests
- Random-effects meta-regression

## 📊 Dataset

**Data Source:**
- Multi-center randomized controlled trial
- 1,912-1,929 older adults with cancer
- 27 research centers across 5 countries

**Study Characteristics:**
- Countries: Australia (n=8 studies), Canada (n=5), Germany (n=4), United Kingdom (n=6), United States (n=4)
- Sample sizes: 40-100 participants per study
- Intervention duration: 6-24 weeks
- Treatment groups: Exercise intervention (n≈952-965) vs. Control (n≈960-964)

**Variables:**
- **Outcome:** Depression scores at baseline and follow-up (standardized scales)
- **Treatment:** Binary indicator (exercise intervention vs. control)
- **Individual-level:** Age (mean-centered), baseline depression scores
- **Study-level:** Study/center ID, intervention duration, country
- **Demographics:** Age (mean ≈ 69.8 years), gender (≈62% female)

## 🚀 Methodology

### Two Analytical Approaches

This project demonstrates two valid but fundamentally different ways to analyze hierarchically structured data from multiple sites.

---

### Approach 1: Linear Mixed-Effects Models (One-Stage Analysis)

**Conceptual Framework:** Treats data as a **single multi-center trial** with patients nested within research centers.

**Model Specification:**

1. **Random Effects Structure:**
   - Random intercepts for centers (baseline differences)
   - Random slopes for treatment by center (treatment effect variability)
   - Determined through likelihood ratio testing (p < .001)

2. **Fixed Effects:**
   - Baseline depression (covariate)
   - Treatment (main effect)
   - Moderators: age, duration, country (tested hierarchically)
   - Interactions: treatment × moderator

3. **Estimation:**
   - Maximum Likelihood (ML) for model comparisons
   - Restricted Maximum Likelihood (REML) for final estimates
   - Likelihood ratio tests for significance (α = .05)

**Model Building Strategy:**
```
Base Model → Add Main Effect → Add Interaction
```

**Diagnostics:**
- Q-Q plots for normality
- Residual vs. fitted plots for heteroscedasticity
- Visual inspection of center-level variation

**Key Advantages:**
- Statistically efficient when convergence achieved
- Directly models hierarchical structure
- Estimates center-specific effects
- Accounts for within-center correlation

---

### Approach 2: Two-Stage Individual Participant Data Meta-Analysis

**Conceptual Framework:** Treats data as **27 separate studies** combined through meta-analysis.

**Stage 1 - Study-Specific Analyses:**
- Separate linear regression models within each study
- Models: `depression_followup ~ treatment + baseline_depression`
- For age moderation: Added `treatment × age_centered` interaction
- Extract treatment effects and standard errors

**Stage 2 - Meta-Analysis:**
- Random-effects meta-analysis with REML estimation
- Pool study-specific effects with inverse-variance weighting
- Heterogeneity assessment: τ², I², Q-tests
- Meta-regression for study-level moderators (duration, country)

**Variable Handling:**
- **Individual-level moderators (age):** Centered within studies, tested in Stage 1
- **Study-level moderators (duration, country):** No within-study variation, tested in Stage 2

**Heterogeneity Metrics:**
- τ² (between-study variance)
- I² (% variation due to heterogeneity)
- R² (variance explained by moderators)

**Key Advantages:**
- Preserves study-level randomization
- More flexible for effect modification modeling
- Avoids convergence issues with limited clusters
- Explicit quantification of between-study heterogeneity
- Distinguishes individual-level vs. study-level moderators

---

## 📈 Results

### Treatment Effect (Primary Analysis)

| Approach | Estimate | SE | p-value | Interpretation |
|----------|----------|-----|---------|----------------|
| Mixed Model | -0.17 | - | .023* | Significant reduction |
| Meta-Analysis | -0.172 | 0.074 | .020* | Significant reduction |

**Conclusion:** Exercise interventions significantly reduce depression scores by approximately 0.17 units compared to control conditions. **Both approaches converged on nearly identical effect sizes.**

---

### Moderation Analyses

#### 1. Age Moderation (Individual-Level)

**Mixed Model Results:**
- Treatment × Age interaction: **p < .001***
- Effect: Treatment becomes ~0.016 units less beneficial per year above mean age
- Interpretation: Younger participants benefit more from exercise

**Meta-Analysis Results:**
- Treatment × Age interaction coefficient: **β = 0.019, SE = 0.005, p < .001***
- Heterogeneity in interaction: τ² = 0.000, I² = 0.2% (minimal variation)
- Interpretation: Age consistently moderates treatment across studies

**Key Finding:** **Strong, consistent age moderation in both approaches** - younger older adults (60s) show greater depression reduction than oldest-old participants (80s+).

---

#### 2. Duration Moderation (Study-Level)

**Mixed Model Results:**
- Main effect of duration: **p = .048*** (significant)
- Treatment × Duration interaction: **p = .81** (not significant)
- Interpretation: Depression trajectories differ by duration, but treatment effectiveness remains consistent

**Meta-Analysis Results:**
- Meta-regression coefficient: **p = 0.86** (not significant)
- Variance explained: R² = 0.0%
- Residual heterogeneity: τ² = 0.116, I² = 77.4%
- Interpretation: Intervention duration (6-24 weeks) does not account for treatment effect variability

**Key Finding:** **Treatment effectiveness is consistent regardless of program duration** - both 6-week and 24-week interventions show similar benefits.

---

#### 3. Country Moderation (Study-Level)

**Mixed Model Results:**
- Main effect of country: **p = .59** (not significant)
- Treatment × Country interaction: **p = .06** (marginally non-significant)
- Interpretation: Intervention effect appears generalizable, but subtle differences may exist

**Meta-Analysis Results:**
- Meta-regression: **p = 0.453** (not significant)
- Variance explained: R² = 19.3%
- Residual heterogeneity: τ² = 0.089, I² = 72.3%
- Interpretation: Country explains 19% of between-study heterogeneity, but not statistically significant

**Key Finding:** **Differences in analytical framing** - Mixed model tests country as fixed effect; meta-analysis asks whether country predicts heterogeneity. Country shows modest non-significant trends in both approaches.

---

### Heterogeneity Assessment (Meta-Analysis Only)

**Overall Treatment Effect:**
- τ² = 0.111 (between-study variance)
- I² = 76.5% (proportion due to heterogeneity)
- Q-test: p < .001 (significant heterogeneity)

**Interpretation:** Approximately 77% of variation in treatment effects is due to true differences between studies rather than sampling error. This substantial heterogeneity justifies investigation of moderators.

---

### Model Comparison Summary

| Analysis Component | Mixed Model | Meta-Analysis | Convergence? |
|-------------------|-------------|---------------|--------------|
| **Treatment Effect** | -0.17* | -0.172* | ✅ Nearly identical |
| **Age Moderation** | Significant*** | Significant*** (β=0.019) | ✅ Strong agreement |
| **Duration Moderation** | Main effect only* | Not significant | ⚠️ Different framing |
| **Country Moderation** | p = .06 | R² = 19.3%, p = .45 | ⚠️ Different framing |

---

## 🔍 Key Findings

### 1. Robust Treatment Effect
Exercise interventions significantly reduce depression in older adults with cancer across both analytical approaches (≈0.17-unit reduction, p < .05).

### 2. Strong Age Moderation
**Most consistent finding across both methods:** Younger participants benefit more from exercise interventions. Treatment effectiveness decreases by approximately 0.016-0.019 units per year above mean age (p < .001 in both approaches).

### 3. Duration Consistency
Intervention effectiveness remains stable across 6-24 week programs, suggesting program duration flexibility for clinical implementation.

### 4. Geographic Considerations
While not statistically significant, country shows modest patterns (19% heterogeneity explained in meta-analysis, p = .06 in mixed model), warranting investigation in larger samples.

### 5. Methodological Insights

**When Both Approaches Agree:**
- Main treatment effect magnitude
- Age as strong, consistent moderator
- Direction and clinical significance of findings

**When Approaches Differ:**
- Conceptualization of study-level moderators (fixed effects vs. heterogeneity predictors)
- Statistical significance thresholds for country effects
- Framing of duration effects (trajectory differences vs. treatment moderation)

---

## 📄 License

This project is for educational and research purposes as part of the Applied Data Science Masters curriculum at Utrecht University.

**Note:** Patient data is confidential and not included in this repository. Analysis code and reports demonstrate methodological approaches using de-identified data structures.

---

## 📧 Contact

**Beliz Pekkan**  

- 📧 Email: belizpekkan@gmail.com
- 🌐 Website: [belizpekkan.com](https://belizpekkan.com)
- 💼 LinkedIn: [linkedin.com/in/beliz-pekkan](https://linkedin.com/in/beliz-pekkan)

---


**Note:** This project demonstrates the importance of methodological rigor and comparative analysis in medical research. The dual-approach framework highlights that robust findings emerge when multiple valid analytical perspectives converge on similar conclusions, strengthening confidence in clinical recommendations.
