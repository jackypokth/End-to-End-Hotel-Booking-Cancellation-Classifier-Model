# End-to-End-Hotel-Booking-Cancellation-Classifier-Model
End-to-end machine learning project predicting hotel booking cancellations on 119k+ records using feature engineering, cross-validation, and tuned ensemble models, achieving 0.9077 ROC-AUC on a held-out test set. ISOM3360_Hotel_Cancellation_Report_Final.docx
# Hotel Booking Cancellation Prediction

This project builds a machine learning pipeline to predict whether a hotel booking will be cancelled before arrival. The task is framed as a supervised binary classification problem using the Hotel Booking Demand dataset, with `is_canceled` as the target variable. The project covers the full workflow from data cleaning and feature engineering to baseline comparison, hyperparameter tuning, and final model evaluation. [file:1]

## Overview

Hotel cancellations reduce revenue predictability and make staffing, inventory, and pricing decisions harder. This project predicts cancellation risk in advance so a hotel could take actions such as adjusting overbooking policies, requesting deposits, or targeting reminders to at-risk customers. [file:1]

The dataset contains 119,390 booking records across 32 original columns, and the final workflow was implemented across eight Jupyter notebooks covering problem definition, data understanding, preprocessing, feature engineering, modeling, tuning, evaluation, and report packaging. [file:1]

## Problem Statement

The goal is to classify each booking as:

- `0`: not cancelled
- `1`: cancelled [file:1]

This is a business-oriented classification problem where identifying likely cancellations can support operational and revenue decisions. Because the dataset is imbalanced, model performance was evaluated using ROC-AUC as the primary metric, with F1, precision, recall, and accuracy used as supporting metrics. [file:1]

## Dataset

- Dataset: Hotel Booking Demand
- Source: Kaggle
- Raw size: 119,390 rows, 32 columns
- Target: `is_canceled` [file:1]

The features include booking lead time, stay duration, guest composition, market segment, distribution channel, pricing (`adr`), previous cancellations, and special requests. [file:1]

## Methodology

### 1. Data cleaning
- Removed leakage features: `reservation_status`, `reservation_status_date`
- Removed 32,252 exact duplicate rows
- Removed 166 invalid zero-guest records
- Clipped 1 negative `adr` value to 0
- Imputed missing values for `company`, `agent`, `country`, and `children` [file:1]

After cleaning, the dataset size became 87,138 rows, and an 80/20 stratified train-test split was used with `random_state=42`. [file:1]

### 2. Feature engineering
Created interpretable derived features such as:

- `total_nights`
- `total_guests`
- `is_family`
- `has_special_request`
- `has_previous_cancellation`
- `adr_per_person`
- `arrival_season`
- `is_summer` [file:1]

Log transformations were also applied to skewed variables including `lead_time`, `adr`, `adr_per_person`, and `total_nights`. High-cardinality categorical variables were handled by grouping rare countries and converting `agent` / `company` into binary presence flags. The final feature set contained 44 columns. [file:1]

### 3. Model development
The following models were compared under the same preprocessing and 5-fold stratified cross-validation framework:

- Dummy Classifier
- Logistic Regression
- Decision Tree
- Random Forest
- K-Nearest Neighbors [file:1]

Hyperparameter tuning was then conducted using `GridSearchCV` for Logistic Regression, Random Forest, and KNN, with ROC-AUC used as the selection metric. [file:1]

## Results

### Baseline model performance

| Model | ROC-AUC | F1 | Accuracy |
|---|---:|---:|---:|
| Dummy Most Frequent | 0.5000 | 0.0000 | 0.7269 |
| Logistic Regression | 0.8641 | 0.6106 | 0.8117 |
| Decision Tree | 0.7439 | 0.6260 | 0.7943 |
| Random Forest | 0.9019 | 0.6886 | 0.8469 |
| K-Nearest Neighbors | 0.8257 | 0.6196 | 0.7962 | [file:1]

### Best tuned model

The best model was a Random Forest with:

- `n_estimators=500`
- `max_depth=None`
- `min_samples_leaf=1`
- `class_weight=balanced` [file:1]

### Final test set performance

| Metric | Value |
|---|---:|
| ROC-AUC | 0.9077 |
| F1 | 0.6896 |
| Precision | 0.7678 |
| Recall | 0.6259 |
| Accuracy | 0.8462 | [file:1]

The tuned Random Forest generalized well from cross-validation to the held-out test set, with no meaningful performance drop between validation and final testing. [file:1]

## Key Insights

- Random Forest was the strongest model both before and after tuning. [file:1]
- The project maintained proper train/test discipline and removed target leakage before modeling. [file:1]
- Important predictive signals included `lead_time`, `adr`, `adr_per_person`, agent-related information, country, and special requests. [file:1]
- Error analysis showed both false positives and false negatives were concentrated in `No Deposit` bookings from the `Online TA` segment. [file:1]

## Repository Structure

```bash
.
├── data/
├── notebooks/
│   ├── 01_problem_definition.ipynb
│   ├── 02_data_understanding.ipynb
│   ├── 03_data_cleaning.ipynb
│   ├── 04_feature_engineering.ipynb
│   ├── 05_baseline_models.ipynb
│   ├── 06_hyperparameter_tuning.ipynb
│   ├── 07_final_evaluation.ipynb
│   └── 08_report_packaging.ipynb
├── figures/
├── report/
└── README.md
```

Update the notebook names above so they exactly match your repo. [file:1]

## Tech Stack

- Python
- pandas
- numpy
- scikit-learn
- matplotlib
- seaborn
- Jupyter Notebook [file:1]

## How to Run

1. Clone the repository.
2. Install dependencies from `requirements.txt`.
3. Add the dataset to the `data/` folder.
4. Run the notebooks in order from preprocessing to final evaluation. [file:1]

## Future Improvements

- Tune the classification threshold based on business cost trade-offs. [file:1]
- Try gradient boosting models such as XGBoost or LightGBM. [file:1]
- Add SHAP values for deeper model interpretability. [file:1]
- Convert the notebook workflow into a reusable training pipeline or deployed app. [file:1]
