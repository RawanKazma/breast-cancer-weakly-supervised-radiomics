# Breast Cancer Detection from 3D μ-Calcifications: Weakly Supervised Radiomics

## Project Overview

This project aims to improve breast cancer detection by analyzing micro-calcifications (μ-calcifications) from high-resolution 3D breast tissue scans.

While mammography, the standard screening method,relies on X-ray images to identify tumors, detecting small or subtle tumors is often challenging due to the complex structure of breast tissue. μ-Calcifications, tiny calcium deposits (around 1 mm), are highly visible in imaging and can provide additional diagnostic information

Although many μ-calcifications are benign and occur naturally with aging, certain patterns, shapes, and textures are associated with malignancy. This project investigates whether radiomic features of individual μ-calcifications can be used to distinguish benign from malignant cases and ultimately support more accurate patient-level diagnosis. 

## Background
Mammography is the current state-of-the-art technique for breast cancer screening. It typically involves two X-ray images per breast. Despite its widespread use, tumor detection can be challenging unless the tumor is large or clearly distinguishable.

Micro-calcifications are small deposits, typically around 1 mm in size, that appear as bright spots in imaging. Although they are often benign, clusters or specific structural patterns may indicate the presence of cancer. Radiologists often rely on these patterns during diagnosis.

Recent research suggests that not only clusters, but also the properties of individual micro-calcifications, such as shape and texture, may provide predictive information about malignancy.

**Clinical and Technical Motivation:**  
- Radiologists typically label clusters of μ-calcifications, not individual instances, introducing label noise at the micro-level  
- **Radiomic features** (shape, texture and wavelet-transformed descriptors) can capture patterns beyond human visual assessment, supporting early detection and reducing unnecessary invasive procedures  

**Research Question:**  
> Can radiomic features of individual μ-calcifications distinguish between benign and malignant regions despite patient-level label noise, and can this improve patient-level breast cancer detection?

---

## Research Hypothesis

- **Malignant μ-calcification:** one that appears in the neighborhood of a tumor. These are considered potentially cancer-related because their presence is associated with malignant tissue
- **Benign μ-calcification:** a μ-calcification that doesnt appear near a tumor. These may occur naturally due to aging or other non-cancerous processes and are generally considered harmless 

We hypothesize that the radiomic features of individual μ-calcificationscan help distinguish malignant from benign μ-calcifications, even when labels are noisy because only patient-level diagnoses are available. This, in turn, may improve the accuracy of patient-level breast cancer detection.

## Dataset

- 100 biopsy samples from female patients  
- 3500 μ-calcifications extracted  
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
- Threshold optimized on validation set for balanced precision and recall  

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

- Inconclusion: radiomic features of individual μ-calcifications do contain predictive information, allowing models to classify them as benign or malignant under weak supervision. Aggregating micro-level predictions improves patient-level breast cancer detection (with F1 0.70). However, the limited dataset (100 patients) restricts model generalization and confidence in performance, indicating that further studies with larger cohorts or advanced weakly supervised methods like Multiple Instance Learning are needed to validate and enhance these results.

## Future Work

- Explore **Multiple Instance Learning (MIL)** for explicit weak supervision  
- Increase dataset size to improve generalization  
- Investigate confidence calibration and uncertainty quantification  

> **Note:** The Colab notebook highlights key observations and insights throughout the project, offering reasoning and context for data analysis, feature selection and model evaluation.
---

## References

1. Zhai, X. *Cross-Validation with Code Examples*. Medium, 2021  
2. Markham, K. *18 Hyperparameter Search Results*. GitHub  
3. Optuna Contributors. *Optuna: A Next-generation Hyperparameter Optimization Framework*  
4. Huo, T., et al. *Stratified Split Sampling of Electronic Health Records*, BMC Med. Res. Methodol., 2023  
5. Glaser, J. *Attention-Based Deep Multiple Instance Learning*, Towards Data Science, 2021  
6. Sharma, K. *Mastering Random Forest Hyperparameter Tuning*, Medium, 2022
