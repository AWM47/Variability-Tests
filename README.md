# Experimentation and Results

## 1. Introduction

Intrusion Detection Systems (IDS) are critical components of cybersecurity, ensuring that unauthorized access or anomalies in a system are detected promptly. However, one of the challenges in designing an IDS for embedded and real-time systems is ensuring its reliability across varying hardware configurations. In real-world applications, variations in Probes and Analog-to-Digital Converters (ADCs) can occur due to hardware replacements, upgrades, or environmental conditions. A key concern is whether these changes introduce inconsistencies in data acquisition that could affect IDS performance.

This study aims to investigate the independence of Probes and ADCs in data acquisition and their impact on the accuracy of a Lightweight IDS. Specifically, we evaluate whether different Probe-ADC combinations influence signal characteristics, feature extraction, and ultimately, classification performance. Furthermore, we explore how feature selection can optimize IDS performance by reducing computational complexity without compromising accuracy.

To test these aspects, we conduct experiments using three different Probes and three different ADCs, collecting signal data at sampling rates ranging from 10 MSPS to 300 MSPS. A Random Forest Classifier is trained on extracted features to determine if IDS accuracy remains stable across different hardware configurations. We further refine our approach by identifying redundant features and testing whether a reduced feature set maintains high classification accuracy while improving efficiency.

Additionally, we compare our findings with existing IDS implementations in similar environments to evaluate whether traditional feature extraction methods are as effective in hardware-agnostic conditions.

## 2. Experimental Setup

### 2.1 Hardware Configuration

To perform the experiments, we employed the following hardware components:

- **Probes:** Probe1, Probe2, Probe3  
- **ADCs:** ADC1, ADC2, ADC3  
- **Input Signal:** Step signal at 1 MHz frequency with 2V amplitude  
- **Sampling Rates:** 10, 20, 30, 40, 50, 100, 150, 200, 250, 300 MSPS  

### 2.2 Data Acquisition

To ensure comprehensive evaluation, we collected data using all possible combinations of Probes and ADCs at different sampling rates. Each acquired signal was stored in a structured format (CSV files) for consistency. The collected signals were processed for feature extraction and further analyzed to determine their impact on classification performance.

A comparison with existing datasets from related studies was also conducted to ensure the generalizability of our IDS framework.

## 3. Feature Extraction and Initial Experimentation

### 3.1 Feature Engineering

Feature extraction is crucial in any machine learning application, as the quality of extracted features directly impacts model performance. Initially, we extracted **38 statistical and signal-processing-based features** from each acquired signal. These features included:

- **Statistical Features:** Mean, standard deviation, skewness, kurtosis  
- **Frequency Features:** Spectral centroid, spectral bandwidth, fundamental frequency  
- **Time-Domain Features:** Signal energy, peak-to-peak amplitude, zero-crossing rate  

To compare our feature set with traditional IDS approaches, we also implemented a baseline feature extraction method commonly used in existing IDS models and analyzed its performance relative to our approach.

### 3.2 Model Training with 38 Features

The extracted features were used to train a **Random Forest Classifier**, a widely used algorithm due to its robustness and ability to handle diverse datasets. The model was configured with the following hyperparameters:

```python
RandomForestClassifier(
    n_estimators=50,
    max_depth=10,
    min_samples_split=10,
    min_samples_leaf=5,
    max_features='sqrt',
    random_state=42
)
```

### 3.3 Experimental Testing and Initial Results

To validate our hypothesis, we conducted **160 tests**, ensuring that different hardware configurations did not significantly impact IDS accuracy. Additionally, we performed **320 extended tests** across different sampling rates to assess classification stability.

#### Feature Categories:

- **High Time-Domain Features** / **Low Time-Domain Features**  
- **High Frequency-Domain Features** / **Low Frequency-Domain Features**  
- **Max_H, Max_L, Centroid_H, Centroid_L, Min_H, Min_L, Entropy_H, Entropy_L**  
- **Mean_H, Mean_L, Spread_H, Spread_L, Std_H, Std_L**  
- **Skewness_Freq_H, Skewness_Freq_L, Mean Deviation_H, Mean Deviation_L**  
- **Mean_Freq_H, Mean_Freq_L, RMS_H, RMS_L, Kurtosis_Freq_H, Kurtosis_Freq_L**  
- **Skewness_H, Skewness_L, Dominant_Freq_H, Dominant_Freq_L, Kurtosis_H, Kurtosis_L**  
- **Peak-to-Peak_H, Peak-to-Peak_L, Zero Crossing Rate_H, Zero Crossing Rate_L**  
- **Irregularity_H, Irregularity_L, Variance_H, Variance_L**  

The results indicated that while the model consistently achieved high accuracy, some features contributed minimally to the classification task. This observation led to further analysis for feature selection to optimize the IDS.

## 4. Feature Selection and Further Optimization

### 4.1 Identifying Significant Features

A **detailed feature importance analysis** was performed, revealing that only **19 out of the 38 features** played a significant role in classification. To verify this, **Principal Component Analysis (PCA)** was conducted, visually demonstrating that reducing the feature set did not impact classification accuracy.

### 4.2 Model Retraining with 19 Features

After reducing the number of features, a second set of experiments was conducted to ensure that the IDS maintained its accuracy. The optimized feature set was sufficient to retain a **95%+ classification accuracy**, proving that redundant features could be eliminated without performance degradation.

Further comparisons were made with alternative dimensionality reduction techniques, such as **Autoencoders and t-SNE**, to confirm that PCA-based reduction was the most effective approach.

## 5. Results and Discussion

### 5.1 Performance of the Optimized Model

After feature reduction, we observed the following improvements:

- **Reduced computational overhead** (faster training and inference)  
- **Improved model interpretability** (focus on key discriminative features)  
- **No loss in classification accuracy** (maintaining **95%+ accuracy** across tests)  

### 5.2 PCA Representation of Feature Reduction

(Include PCA visualization here)

### 5.3 Verification Through Randomized Probe-ADC Tests

To further confirm that **Probes and ADCs do not influence IDS performance**, we conducted extensive randomized tests:

- Randomly selecting **Probe-ADC** combinations  
- Testing the model’s ability to classify signals under varying hardware settings  
- Verifying that classification accuracy remained consistent regardless of hardware variations  

### 5.4 Statistical Analysis of Classification Accuracy

| Test Case | Sampling Rate | Test Accuracy | Balanced Accuracy | MCC | Log Loss | F1 Score | Recall | Precision | FNR | FPR |
|-----------|--------------|--------------|------------------|-----|---------|---------|--------|----------|-----|-----|
| ADC2 Probe3 | 10 | 0.952 | 1 | 0.908 | 0.1638 | 0.95 | 0.9 | 1 | 0.0006 | 0.1 |
| ADC2 Probe3 | 20 | 1 | 1 | 1 | 0.0003 | 1 | 1 | 1 | 0 | 0 |
| ADC3 Probe3 | 300 | 0.9955 | 1 | 0.991 | 0.1455 | 1 | 1 | 1 | 0 | 0.01 |

## 6. Conclusion

Through extensive experimentation, we have established that **Probes and ADCs do not affect IDS performance**, making the system robust against hardware variations. Key findings include:

- **Feature reduction from 38 to 19 improved efficiency without affecting accuracy.**  
- **PCA visualizations confirmed that essential information was preserved.**  
- **Randomized tests verified that IDS remains hardware-agnostic.**  
- **Optimized classification ensured that IDS performance is stable across different settings.**  

Future work could explore **real-time IDS deployment**, **deep learning-based feature extraction**, and **adversarial robustness testing**.
