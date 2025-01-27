# Multi-Class Classification of Cardiovascular Conditions

This repository contains our work on classifying ECG signals into five clinically important superclasses—**Myocardial Infarction (MI)**, **ST-T changes (STTC)**, **Conduction Disturbances (CD)**, **Hypertrophy (HYP)**, and **Normal (NORM)**—using the [PTB-XL](https://physionet.org/content/ptb-xl/1.0.3/) dataset. Our project compares a range of models, from a baseline Logistic Regression to advanced deep learning architectures such as **ResNet1D**, **Inception1D**, **xResNet1D**, and various **Custom** models (e.g., Custom Inception1D, Custom ResNet1D, U-Net). 

> **Key Result**:  
> Our **Custom Inception1D** model achieved the highest performance, with an **F1-score of 0.77** and an **ROC-AUC of 0.92**.

---

## Table of Contents
1. [Overview](#overview)
2. [Dataset](#dataset)
3. [Methodology](#methodology)
4. [Models](#models)
5. [Results](#results)
6. [Installation & Requirements](#installation--requirements)
7. [Usage](#usage)
8. [Project Structure](#project-structure)
9. [Contributing](#contributing)
10. [License](#license)
11. [References](#references)

---

## Overview

Cardiovascular diseases are a leading cause of mortality worldwide, encompassing conditions like **Myocardial Infarction**, **Conduction Disturbances**, **Hypertrophy**, and **ST-T changes**. Electrocardiogram (ECG) signals play a crucial role in early diagnosis. Recent advancements in deep learning have shown promising results in automated ECG classification. 

This project:
- Explores multiple deep learning architectures tailored for 1D time-series data (ECG).
- Implements baseline machine learning (Logistic Regression) for comparison.
- Investigates the challenges of class imbalance (notably **Hypertrophy**).
- Proposes and evaluates **Custom Inception1D** that outperforms other models on the PTB-XL dataset.

---

## Dataset

We use the **PTB-XL** dataset, a large publicly available repository of 12-lead ECG recordings:
- **Source**: [PhysioNet](https://physionet.org/content/ptb-xl/1.0.3/)
- **Size**: 21,799 records from 18,869 patients
- **Duration**: Each record is 10 seconds
- **Leads**: 12 standard leads
- **Sampling Rate**: 500 Hz (and a downsampled 100 Hz version available)
- **Annotations**: Multiple cardiologist-based diagnoses mapped to a standardized SCP-ECG format

**Classes**:
- **NORM** (Normal)  
- **MI** (Myocardial Infarction)  
- **STTC** (ST-T changes)  
- **CD** (Conduction Disturbances)  
- **HYP** (Hypertrophy)  

Notably, **HYP** is underrepresented, contributing to class imbalance and making it harder to classify.

> *Important*: You must **download the dataset** separately from the official PhysioNet repository due to licensing constraints.

---

## Methodology

1. **Data Preparation**  
   - Minimal preprocessing for deep learning models (e.g., no explicit filtering).
   - For the baseline Logistic Regression, we used PCA dimensionality reduction.
   - Employed a sliding window technique for training to increase data variability.

2. **Cross-Validation & Training**  
   - Used **10-fold cross-validation** to ensure robust performance estimates.
   - Trained models for ~40 epochs each fold.
   - Balanced distribution of classes in splits wherever possible.

3. **Evaluation Metrics**  
   - **F1-Score**: Balances precision and recall, especially relevant for imbalanced datasets.  
   - **ROC-AUC**: Assesses overall discriminative power across all classes.  
   - **Accuracy**: Provides a simple measure but can be misleading with imbalance.  

4. **Hyperparameter Tuning**  
   - Optimized learning rate (0.001), batch size (32), and used **Adam** optimizer.
   - Explored SMOTE and cost-sensitive loss functions to address class imbalance.

---

## Models

We implemented and compared the following architectures:

1. **Logistic Regression** (Baseline)
2. **ResNet1D**  
3. **Inception1D**  
4. **xResNet1D**  
5. **Custom Inception1D** (Best Performer)  
6. **U-Net**  
7. **Custom ResNet1D**  
8. **Custom CNN (2 layers)**  

**Highlight**:  
- **Custom Inception1D** included modified inception modules, hybrid pooling strategies, and dropout layers, delivering the best performance.

---

## Results

| **Model**               | **Accuracy** | **ROC-AUC** | **F1-Score** |
|:------------------------|:------------:|:----------:|:------------:|
| Logistic Regression     | 44.76%       | 0.495       | 0.28         |
| ResNet1D               | 73.26%       | 0.88        | 0.71         |
| Inception1D            | 74.47%       | 0.91        | 0.73         |
| xResNet1D              | 72.50%       | 0.89        | 0.71         |
| **Custom Inception1D** | **77.12%**   | **0.92**    | **0.77**     |
| U-Net                   | 70.10%       | 0.87        | 0.68         |
| Custom ResNet1D        | 73.68%       | 0.88        | 0.72         |
| CNN (2 Layers)         | 65.84%       | 0.87        | 0.67         |

- **Custom Inception1D** yielded the highest performance (F1=0.77, ROC-AUC=0.92).  
- **Hypertrophy (HYP)** remains difficult to classify due to **class imbalance** and overlapping ECG patterns.  
