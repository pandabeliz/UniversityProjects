# Whale Migration Prediction Using Machine Learning

> Predicting blue whale migration shifts due to climate change using ensemble machine learning models

## 📋 Overview

**Course:** Artificial Intelligence for Nature and Environment  
**Institution:** Tilburg University  
**Date:** 2023  
**Status:** ✅ Completed  

This project investigates the impact of climate change on blue whale (*Balaenoptera musculus*) migration patterns using advanced machine learning algorithms. By analyzing historical whale sighting data and sea surface temperature changes, the study aims to predict future migration shifts to support marine conservation efforts.

---

## 🎯 Objectives

- Predict shifts in blue whale migration patterns caused by rising sea surface temperatures
- Analyze the relationship between environmental changes and cetacean distribution
- Compare the effectiveness of multiple machine learning approaches for ecological prediction
- Contribute to marine conservation strategies by identifying climate change impacts

---

## 🔧 Technologies Used

**Programming Languages:**
- Python

**Libraries & Frameworks:**
- **Data Processing:** pandas, NumPy
- **Machine Learning:** scikit-learn (Linear Regression, Random Forest, XGBoost)
- **Visualization:** matplotlib, seaborn
- **Geospatial Analysis:** basemap/cartopy for mapping

**Tools:**
- Jupyter Notebooks
- OBIS-SEAMAP database
- NOAA sea surface temperature data

---

## 📊 Dataset

**Primary Data Source:**
- **OBIS-SEAMAP Database** (Marine Geospatial Ecology Lab, Duke University)
- 12,073 blue whale sighting records (1990-2023)
- Subset focused on recent trends for accurate predictions

**Additional Data:**
- **NOAA Sea Surface Temperature (SST)** data
- Feature engineering to align temperature data with whale sighting records
- Temporal and geospatial coordinates

**Data Exploration:**
- Global sighting maps showing distribution patterns
- Kernel Density Estimation (KDE) plots revealing migration hotspots
- Temporal analysis comparing historical (1903-2023) vs recent (1990-2023) patterns

---

## 🚀 Methodology

### Multi-Model Approach

The study employed a progressive modeling strategy, transitioning from simple baseline models to more complex ensemble methods:

#### 1. Linear Regression Model
**Purpose:** Baseline understanding of whale sighting patterns  
**Approach:** Models relationship between time, sighting frequency, and sea surface temperature  
**Key Insight:** Establishes foundational trends in the data

#### 2. Random Forest Model
**Purpose:** Capture non-linear ecological relationships  
**Parameters:**
- `n_estimators=100` (balance between accuracy and efficiency)
- `max_depth=None` (allow full tree growth)

**Strengths:**
- Handles complex ecological datasets
- Captures subtle patterns and variable interactions
- Robust to overfitting through ensemble averaging

#### 3. XGBoost Model
**Purpose:** Enhanced prediction with gradient boosting  
**Parameters:**
- `objective='reg:squarederror'` (minimize MSE)
- Efficient handling of large, complex datasets

**Strengths:**
- Superior performance on ecological data
- Built-in regularization prevents overfitting
- Captures complex non-linear patterns

#### 4. Multi-Output Regression
**Approach:** Capture multiple dependent variables simultaneously  
**Benefits:**
- Holistic view of migration patterns
- Predicts location, timing, and density together
- More comprehensive than single-output models

### Feature Engineering

**Key Features:**
- Temporal variables (year, season)
- Geospatial coordinates (latitude, longitude)
- Sea surface temperature (aligned with sighting records)
- Sighting density and frequency metrics

---

## 📈 Results

### Model Performance Comparison

| Model | Avg R² Score | Test MSE | Test RMSE | Test MAE |
|-------|-------------|----------|-----------|----------|
| **Linear Regression** | -139.18 | 408,566.63 | 639.19 | 361.15 |
| **Random Forest** | -92.35 | 701,792.79 | 837.73 | 353.28 |
| **XGBoost** | -864.38 | 796,346.13 | 597.45 | **402.04** |

**Evaluation Metrics:**
- **R² Score:** Measures model's ability to explain variance in the data
- **MSE (Mean Squared Error):** Average squared difference between predictions and actual values
- **RMSE (Root Mean Squared Error):** Square root of MSE, interpretable in original units
- **MAE (Mean Absolute Error):** Average absolute error magnitude

### Key Findings

**1. Model Performance:**
- Negative R² scores indicate all models struggled to capture full complexity
- XGBoost showed relatively better efficiency with lower RMSE (597.45)
- Random Forest had the best MAE (353.28), suggesting good average prediction accuracy

**2. Pattern Analysis:**
- Linear Regression predicted higher densities in specific areas (conservative estimates)
- Random Forest showed broader prediction ranges, capturing more variability
- XGBoost predictions suggested potential migration shifts between 2024-2025

**3. Migration Insights:**
- Identified persistent whale sighting hotspots
- Detected slight pattern changes when comparing historical vs. recent data
- Sea surface temperature shows correlation with distribution shifts

### Visualizations

**Key Outputs:**
1. **Global Sighting Maps** - Historical distribution (1903-2023)
2. **Kernel Density Estimation (KDE) Maps** - Migration hotspot identification
3. **Temporal KDE Comparison** - Recent patterns (1990-2023) vs. historical
4. **Model Predictions for 2024-2025** - Projected migration patterns

---

## 📄 License

This project is for educational and research purposes as part of the Bachelor's curriculum at Tilburg University.

---

## 📧 Contact

**Beliz Pekkan**  
Email: belizpekkan@gmail.com  
Website: [belizpekkan.com](https://belizpekkan.com)  
LinkedIn: [linkedin.com/in/beliz-pekkan](https://www.linkedin.com/in/beliz-pekkan)

---

**Part of:** [University Projects Portfolio](https://github.com/pandabeliz/university-projects)

---

**Note:** This project demonstrates the challenges and potential of applying machine learning to ecological conservation. While current models show limitations, the framework provides a foundation for future improvements and highlights the critical need for continued research at the intersection of AI and environmental science.
