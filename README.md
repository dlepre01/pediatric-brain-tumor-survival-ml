# Pediatric Brain Tumor Survival Analysis & Machine Learning

## Project Objective
This project analyzes electronic health records (EHRs) of pediatric patients diagnosed with primary brain tumors to:

- Identify key prognostic factors influencing overall survival (OS)
- Model survival using statistical survival techniques
- Predict mortality using supervised machine learning
- Explore latent patient profiles through unsupervised learning

Dataset: 174 pediatric patients treated in Serbia (2007–2016)  
Study type: Retrospective cohort  


## Clinical Context

Pediatric brain tumors are among the leading causes of cancer-related mortality in children.  
Survival outcomes depend on tumor biology, disease extent, surgical radicality, and treatment intensity.

This project integrates survival modeling and ML to support risk stratification.

##  Dataset Overview

- 174 patients (173 after preprocessing)
- 34 clinical, pathological, and treatment variables
- Outcome variables:
  - `Death` (binary)
  - `OS` (overall survival time in months)

Main feature groups:
- Demographics (Sex, Year_of_birth)
- Tumor histology (HGG, LGG, Embryonal, Unknown, etc.)
- Extent of disease (NED, LRD, Disseminated)
- Treatment (Radiotherapy, Chemotherapy, Surgery type)

Source publication:  
Stanić et al., *PLOS ONE*, 2021.


## Data Preprocessing

- Variable renaming and encoding
- Logical imputation of treatment variables
- Multiple imputation (MICE) for missing clinical fields
- Multicollinearity assessment (Correlation matrix + VIF)
- Feature engineering:
  - Operation_type_bin (GTR vs non-GTR)
  - CSF_cytology_combined
  - Rare histology aggregation

Final modeling dataset: 23 features (after VIF filtering)

## Exploratory Data Analysis

Key findings:

- Early mortality peak within first 20 months
- High-grade glioma (HGG) strongly associated with death
- Disseminated disease significantly worsens prognosis
- Gross total resection (GTR) improves survival

## Unsupervised Learning

Dimensionality reduction:
- PCA
- UMAP
- t-SNE

Clustering:
- K-means (raw space → weak structure)
- K-means on UMAP (K=3 → best solution)
  - Silhouette ≈ 0.55
  - Davies-Bouldin < 0.7

Three clinically interpretable clusters:
- Cluster 1: High-risk, poor prognosis
- Cluster 2: Intermediate risk
- Cluster 3: Aggressive tumors but intensive treatment

Mortality distribution significantly different across clusters (p < 0.05).

## Survival Analysis

### Kaplan–Meier Estimation
Significant survival differences by:
- Extent of disease
- HGG
- Operation type
- Unknown histology

### Cox Proportional Hazards Model

Initial model:
- C-index = 0.81

After PH violation diagnostics:
- Stratified Cox model
- Global PH test passed (p = 0.95)
- C-index ≈ 0.77

Strong independent predictors:
- HGG (HR ≈ 9.18)
- Disseminated disease (HR ≈ 3.37)
- No GTR
- Radiotherapy (protective)

## Supervised Learning — Mortality Prediction

Models evaluated:
- Random Forest
- XGBoost
- SVM
- Neural Network
- KNN

Training strategy:
- 10-fold cross-validation
- 20% hold-out test set
- Threshold optimization (Youden Index)

### Best Performance

Random Forest:
- AUC = 0.83
- Balanced accuracy ≈ 0.74
- Clinically coherent feature importance

Top predictive features:
- HGG
- Surgery type (GTR)
- Unknown histology
- Symptom duration


## 📈 Outputs

- Kaplan–Meier survival curves
- Forest plots (Cox HR)
- Clustering visualizations
- ROC curves
- Feature importance plots
- Model evaluation metrics


## 🛠 Tech Stack

- R
- survival
- glmnet
- mice
- randomForest
- xgboost
- e1071
- umap
- Rtsne
- cluster
- ggplot2


## Limitations

- Single-center dataset
- Moderate sample size (n=173)
- Retrospective design
- Treatment variables simplified


## Key Takeaway

Tumor biology (HGG), disease extent, and surgical radicality are the strongest determinants of survival.  
Machine learning models achieve moderate predictive power, with Random Forest providing the best balance between performance and interpretability.
