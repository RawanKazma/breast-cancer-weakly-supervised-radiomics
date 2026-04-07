# Breast Cancer Detection from 3D μ-Calcification - weakly supervised radiomics

## Project Overview

This project focuses on the detection of breast cancer using imaging data, specifically through the analysis of micro-calcifications extracted from high-resolution 3D breast tissue scans.

Breast cancer diagnosis is commonly performed using mammography which relies on X-ray imaging. However, due to the complex structure of breast tissue, tumors can be difficult to detect, especially when they are small or not clearly visible.

Micro-calcifications are small calcium deposits that are easily visible in imaging. While they can occur as part of the natural aging process, certain patterns and characteristics have been associated with malignancy. This project investigates whether the shape and texture properties of individual micro-calcifications can be used to predict cancer.

## Background

Mammography is the current state-of-the-art technique for breast cancer screening. It typically involves two X-ray images per breast. Despite its widespread use, tumor detection can be challenging unless the tumor is large or clearly distinguishable.
Micro-calcifications are small deposits, typically around 1 mm in size, that appear as bright spots in imaging. Although they are often benign, clusters or specific structural patterns may indicate the presence of cancer. Radiologists often rely on these patterns during diagnosis.

Recent research suggests that not only clusters, but also the properties of individual micro-calcifications, such as shape and texture, may provide predictive information about malignancy.

## Research Hypothesis

The project is based on the hypothesis that shape and texture features of individual micro-calcifications can be used to predict malignancy.

For this purpose, two types of micro-calcifications are defined:

1. Malignant micro-calcification: a micro-calcification located in the neighborhood of a tumor
2. Benign micro-calcification: a micro-calcification not located near a tumor

## Dataset Description

The dataset consists of approximately **100 biopsy samples** from female patients. Each sample is labeled as malignant if cancer cells are detected. High-resolution 3D scans are used to segment individual micro-calcifications, **resulting in approximately 3500 instances**.

For each micro-calcification, around **150 radiomic features** are extracted. These include shape and texture descriptors, as well as features obtained through wavelet transformations, which are known to improve machine learning performance.

_It is important to note that labels are assigned at the patient level. This introduces ambiguity, as benign micro-calcifications may be present in malignant samples and are therefore labeled as malignant_.

## Problem Statement

The project consists of two related machine learning tasks
- Task 1 focuses on classifying individual micro-calcifications as benign or malignant. This task assumes that all micro-calcifications within a patient share the same label.
- Task 2 focuses on predicting whether a patient has breast cancer based on the classification results of multiple micro-calcifications belonging to that patient.

## Key Challenges

- One of the main challenges is **label noise**, since micro-calcification labels are derived from patient-level diagnoses rather than direct annotations
- Another challenge is the data structure, where **each patient contributes multiple instances**, making standard independent assumptions less valid
- There is also inherent **class ambiguity**, as not all micro-calcifications in a malignant case are necessarily malignant.
- The dataset has relatively **high dimensionality**, with approximately 150 features per instance, combined **with a limited number of patients**
  
## Objectives

The objective of this project is to develop machine learning models that can accurately classify micro-calcifications and infer patient-level diagnoses from these predictions.

Additionally, the project aims to evaluate the usefulness of radiomic features and transformations such as wavelets in improving classification performance.

## Next Steps

The following sections will describe the methodology, model design, implementation details, and evaluation results.
