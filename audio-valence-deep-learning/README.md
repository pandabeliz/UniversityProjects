# Deep Learning Model for Audio Valence Prediction

> Predicting emotional valence from audio data using fully connected neural networks with Bayesian optimization

## 📋 Overview

**Course:** Introduction to Deep Learning  
**Institution:** Tilburg University  
**Date:** 2023  
**Status:** ✅ Completed  

This project develops a deep learning model to predict valence values (emotional positivity/negativity) from audio data using fully connected neural networks. The model employs Bayesian optimization for hyperparameter tuning and achieves strong performance on validation data.

---

## 🎯 Objectives

- Predict emotional valence from audio features using deep learning
- Design an effective neural network architecture for audio regression tasks
- Optimize hyperparameters using Bayesian optimization
- Achieve minimal prediction error on unseen validation data
- Balance model complexity with generalization capability

---

## 🔧 Technologies Used

**Programming Language:**
- Python 3.x

**Deep Learning Frameworks:**
- TensorFlow / Keras

**Libraries:**
- **Data Processing:** NumPy, pandas
- **Audio Processing:** librosa (for Mel spectrogram extraction)
- **Optimization:** scikit-optimize (Bayesian optimization)
- **Visualization:** matplotlib
- **Model Training:** scikit-learn (train-test split)

**Tools:**
- Jupyter Notebooks
- Google Colab (for GPU acceleration)

---

## 📊 Dataset

**Data Format:**
- Training data provided in `.pkl` (pickle) format
- Audio files converted to Mel spectrograms
- Continuous valence labels (target variable)

**Data Split:**
- **Training Set:** 95% of data
- **Validation Set:** 5% of data
- Split performed using `train_test_split` from scikit-learn

**Preprocessing Steps:**
1. **Mel Spectrogram Conversion:**
   - Audio signals transformed into Mel spectrograms
   - Captures frequency content over time
   - Standard representation for audio ML tasks

2. **Normalization:**
   - Features normalized to ensure stable training
   - Prevents gradient issues from varying scales

3. **Feature Extraction:**
   - Audio features extracted from spectrograms
   - Labels prepared for both training and validation sets

---

## 🏗️ Architecture Design

### Network Structure

**Fully Connected Neural Network with 4 Layers:**

```
Input Layer → Dense 256 → Dense 128 → Dense 64 → Output (1 unit)
             ↓            ↓            ↓
           ReLU +       ReLU +       ReLU +
           BatchNorm +  BatchNorm +  BatchNorm +
           Dropout      Dropout      Dropout
```

### Layer-by-Layer Breakdown

#### Layer 1: Dense (256 units)
- **Activation:** ReLU
- **Purpose:** Transform input features into higher-dimensional space
- **Function:** Capture complex relationships in audio data
- **Regularization:**
  - Batch Normalization (stable, faster training)
  - Dropout (prevent overfitting)

#### Layer 2: Dense (128 units)
- **Activation:** ReLU
- **Purpose:** Further refine extracted features
- **Function:** Intermediate representation learning
- **Regularization:**
  - Batch Normalization
  - Dropout

#### Layer 3: Dense (64 units)
- **Activation:** ReLU
- **Purpose:** Prepare features for final prediction
- **Function:** High-level feature compression
- **Regularization:**
  - Batch Normalization
  - Dropout

#### Output Layer: Dense (1 unit)
- **Activation:** Linear
- **Purpose:** Predict continuous valence value
- **Output:** Single continuous value (emotional valence)

### Architectural Components

**1. ReLU Activation:**
- Introduces non-linearity to the network
- Enables learning of complex patterns
- Computationally efficient
- Helps mitigate vanishing gradient problem

**2. Batch Normalization:**
- Normalizes layer outputs
- Ensures stable and faster training
- Reduces internal covariate shift
- Improves gradient flow

**3. Dropout:**
- Randomly drops units during training
- Prevents overfitting on training data
- Improves generalization to unseen data
- Acts as ensemble learning

**4. Linear Output Activation:**
- Allows continuous value prediction
- Suitable for regression tasks
- No constraints on output range

### Design Philosophy

**Balance Complexity and Generalization:**
- Gradually decreasing layer sizes (256 → 128 → 64)
- Multiple regularization techniques prevent overfitting
- Sufficient capacity to learn audio patterns
- Not overly complex to avoid memorization

---

## 🧪 Experiments

### Bayesian Optimization for Hyperparameter Tuning

**Optimization Strategy:**
```python
def optimize(train_features, train_labels, val_features, val_labels):
    # Define objective function
    def objective(learning_rate, batch_size):
        # Train model with given hyperparameters
        # Return validation MSE
        pass
    
    # Define search space
    pbounds = {
        'learning_rate': (0.0001, 0.005),
        'batch_size': (16, 128)
    }
    
    # Run Bayesian Optimization
    # 2 initial random points + 5 optimization iterations
    return best_params
```

**Hyperparameters Optimized:**
1. **Learning Rate:** Range [0.0001, 0.005]
2. **Batch Size:** Range [16, 128] (integers)

**Optimization Process:**
- **Initial Exploration:** 2 random parameter combinations
- **Iterative Refinement:** 5 optimization iterations
- **Objective:** Minimize validation MSE
- **Method:** Gaussian Process-based Bayesian optimization

### Optimization Results

| Iteration | Target (MSE) | Batch Size | Learning Rate | Result MSE |
|-----------|--------------|------------|---------------|------------|
| 1 | 0.6205 | 62.71 | 0.00363 | 0.6205 |
| 2 | 0.5287 | 16.01 | 0.001581 | 0.5287 |
| 3 | 0.6118 | 61.39 | 0.001776 | 0.6118 |
| 4 | 0.5456 | 73.83 | 0.005 | 0.5456 |
| **5** | **0.7194** | **128.0** | **0.001546** | **0.7194** ↑ |
| 6 | 0.5729 | 121.2 | 0.003511 | 0.5729 |
| 7 | 0.5802 | 125.9 | 0.002324 | 0.5802 |

**Best Parameters Identified:**
- **Batch Size:** 128
- **Learning Rate:** 0.0015 (approximately)
- **Target achieved:** Lower MSE indicates better performance

---

## 📈 Results

### Final Model Performance

**Training Performance:**
- **Training MSE:** 0.8713
- Indicates model learned patterns from training data

**Validation Performance:**
- **Validation MSE:** 0.5068 ✨
- **Key Finding:** Lower validation MSE than training MSE
- **Interpretation:** Strong generalization to unseen data

### Performance Analysis

**Why Validation MSE < Training MSE?**
1. **Effective Regularization:**
   - Dropout and batch normalization prevented overfitting
   - Model didn't memorize training data

2. **Data Split Quality:**
   - Validation set may have been slightly easier
   - 95-5 split ensures sufficient training data

3. **Optimal Hyperparameters:**
   - Bayesian optimization found good parameter combination
   - Learning rate and batch size well-suited for the task

### Training Curves

**Observation from Loss Plot:**
- Initial rapid decrease in both training and validation loss
- Convergence around epoch 10-15
- Stable performance after epoch 20
- Minimal overfitting (validation loss tracks training loss)

**Visual Analysis:**
- Training loss: Smooth decrease from ~3.5 to ~0.87
- Validation loss: Follows similar trajectory to ~0.51
- No divergence between training and validation curves
- Indicates well-balanced model capacity


---

## 📄 License

This project is for educational purposes as part of the Introduction to Deep Learning course at Tilburg University.

---

## 📧 Contact

**Beliz Pekkan**  
Email: belizpekkan@gmail.com  
Website: [belizpekkan.com](https://belizpekkan.com)  
LinkedIn: [linkedin.com/in/beliz-pekkan](https://www.linkedin.com/in/beliz-pekkan)

---

**Note:** This project demonstrates effective application of deep learning to audio emotion recognition, showcasing skills in neural network design, hyperparameter optimization, and audio signal processing.
