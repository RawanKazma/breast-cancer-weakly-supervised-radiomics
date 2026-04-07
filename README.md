# Breast Cancer Detection from 3D μ-Calcifications: Weakly Supervised Radiomics

## Project Overview

This project focuses on the detection of breast cancer using imaging data, specifically through the analysis of μ-calcifications extracted from high-resolution 3D breast tissue scans.

Breast cancer diagnosis is commonly performed using mammography (two X-ray images per breast). Tumor detection can be challenging, particularly for small tumors. μ-Calcifications are tiny calcium deposits (~1 mm) that appear as bright spots in imaging. While often benign, certain patterns or structural features can indicate malignancy. This project investigates whether the **shape and texture properties of individual μ-calcifications** can be used to predict cancer.

- **Clinical Relevance:** Radiologists typically label clusters of μ-calcifications rather than individual instances. This introduces **label noise** at the micro-level, which can lead to misdiagnosis.  
- **Radiomic Features:** Quantitative measures of shape, texture, and wavelet-transformed features capture patterns beyond what is visually apparent, potentially improving early detection and reducing unnecessary invasive procedures.  
- **Research Questoin:** _Can radiometric features of individual MCs be used to distinguish between benign and malignant regions in the presence of label noise on the patient-level diagnosis, and will it  be able to improve patient-level breast cancer detection?_

## Research Hypothesis

- **Malignant μ-calcification:** located near a tumor  
- **Benign μ-calcification:** not near a tumor  

The hypothesis is that radiomic features of individual μ-calcifications, including shape, texture, and wavelet-transformed descriptors, can help predict malignancy, even under weak supervision from patient-level labels.

## Dataset

- ~100 biopsy samples from female patients  
- ~3500 μ-calcifications extracted  
- 150 radiomic features per μ-calcification (shape, texture, wavelet-transformed)  
- Labels assigned at **patient level**, introducing **label noise**  

## Problem Statement

Two main tasks:

1. **Micro-level classification:** classify individual μ-calcifications as benign or malignant  
2. **Patient-level prediction:** determine whether a patient has breast cancer based on multiple μ-calcification predictions  

Key challenges:

- Label noise due to patient-level labeling  
- Multiple μ-calcifications per patient → data dependency  
- High dimensionality and limited patient number  
- Class imbalance and ambiguity  

## Methodology

**Data Preparation**  
- Standardization and grouped splits by patient ID to avoid leakage  
- Feature reduction: correlation filtering (≥ 0.9) and mutual information (< 0.015) → 59 features  

**Modeling**  
- Logistic Regression (linear baseline)  
- Random Forest (tree-based ensemble)  
- Multi-Layer Perceptron (MLP, baseline and tuned)  
- Hyperparameter tuning: GridSearchCV and Optuna  
- Stratified 5-fold cross-validation at patient level  

**Patient-Level Aggregation**  
- Max probability per patient for μ-calcifications  
- Threshold optimized on **validation set** for balanced precision and recall  

## Results

### Micro-Level Classification

| Model               | Accuracy | Precision | Recall | ROC-AUC | Notes                     |
|---------------------|---------|-----------|--------|---------|----------------------------|
| Logistic Regression | 0.735   | 0.729     | 0.618  | 0.781   |   
| Random Forest       | 0.775   | 0.814     | 0.624  | 0.825   | Best overall performance   |
| MLP                 | 0.761   | 0.807     | 0.591  | 0.741   | 
### Patient-Level Prediction

- F1-score ≈ 0.70  
- Balanced precision and recall  
- Aggregating micro-level predictions improves clinical relevance  

## Observations

- Random Forest is the best-performing model overall  
- MLP tuning increased precision but reduced recall → **shows precision-recall trade-off **
- Logistic Regression provides an interpretable baseline  
- Weak supervision and label noise remain significant challenges  

## Future Work

- Explore **Multiple Instance Learning (MIL)** for explicit weak supervision  
- Increase dataset size to improve generalization  
- Investigate confidence calibration and uncertainty quantification  

---

## References

1. Zhai, X. *Cross-Validation with Code Examples*. Medium, 2021  
2. Markham, K. *18 Hyperparameter Search Results*. GitHub  
3. Optuna Contributors. *Optuna: A Next-generation Hyperparameter Optimization Framework*  
4. Huo, T., et al. *Stratified Split Sampling of Electronic Health Records*, BMC Med. Res. Methodol., 2023  
5. Glaser, J. *Attention-Based Deep Multiple Instance Learning*, Towards Data Science, 2021  
6. Sharma, K. *Mastering Random Forest Hyperparameter Tuning*, Medium, 2022
