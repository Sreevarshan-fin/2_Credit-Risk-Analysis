
# Credit Risk Modelling



---

## Problem Statement

Lauki Finance is a Non-Banking Financial Company (NBFC) that provides loans to customers often underserved by traditional banks. While this increases financial inclusion, it also exposes the company to higher credit risk.

At the time of this project, credit evaluation relied heavily on manual assessment by risk analysts. As loan applications increased, this process became slow, inconsistent, and difficult to scale. This resulted in delayed loan decisions and increased exposure to potential defaults.

The objective of this project was to build an **interpretable credit risk model** capable of estimating the **Probability of Default (PD)** for each borrower. The model would support the risk team in making faster, more consistent, and data-driven credit decisions.

---

## How the Problem Was Approached

The project followed a structured credit risk modeling workflow similar to industry practices used by banks and NBFCs.

The approach focused on:

* Preparing reliable borrower and loan data
* Engineering risk-relevant financial indicators
* Handling class imbalance in default prediction
* Training multiple machine learning models
* Evaluating models using credit risk performance metrics

The goal was not just predictive accuracy, but **strong risk ranking capability and model interpretability**.

---

## Data Resources

The dataset contained historical loan information including:

**Customer Attributes**

* Demographics
* Income details
* Employment information

**Loan Characteristics**

* Loan amount
* Loan purpose
* Loan tenure

**Credit Bureau Data**

* Past delinquencies
* Days past due (DPD)
* Credit behavior indicators

### Time-based Validation

To ensure realistic model evaluation:

* **Training Data:** Feb 2022 – Feb 2024
* **Out-of-Time Test Data:** Mar 2024 – May 2024

Out-of-time validation ensures the model performs well on **future unseen data**, which is critical in credit risk modeling.

---

## Solution Approach

### 1. Data Cleaning

Several preprocessing steps were performed to improve data quality:

* Corrected **improper text formats**
* Standardized **data types**
* Handled **missing values**
* Removed **invalid loan purpose values**

These steps ensured reliable model inputs.

---

### 2. Feature Engineering

Domain-driven features were created to improve predictive power.

Key engineered variables include:

**Loan to Income Ratio (LTI)**
Measures borrower debt burden relative to income.

**Delinquency Ratio**
Captures frequency of missed payments.

**Average Days Past Due (Avg DPD per Delinquency)**
Measures severity of late payments.

Feature selection was performed using:

* **Weight of Evidence (WOE)**
* **Information Value (IV)**
* **Variance Inflation Factor (VIF)** to detect multicollinearity.

---

### 3. Model Training

Multiple modeling strategies were explored to address class imbalance and improve predictive performance.

| Attempt   | Method               | Model               | Precision (Default) | Recall (Default) | F1 Score | Accuracy |
| --------- | -------------------- | ------------------- | ------------------- | ---------------- | -------- | -------- |
| Attempt 1 | RandomizedSearchCV   | Logistic Regression | 0.83                | 0.74             | 0.78     | 0.96     |
| Attempt 1 | RandomizedSearchCV   | XGBoost             | 0.77                | 0.84             | 0.80     | 0.96     |
| Attempt 2 | Under Sampling       | Logistic Regression | 0.51                | 0.96             | 0.67     | 0.92     |
| Attempt 2 | Under Sampling       | XGBoost             | 0.51                | 0.98             | 0.67     | 0.92     |
| Attempt 3 | SMOTE Tomek          | Logistic Regression | 0.55                | 0.94             | 0.70     | 0.93     |
**| **Attempt 4** | **SMOTE Tomek + Optuna** | **Logistic Regression** | **0.56**            | **0.94**         | **0.70** | **0.93** |**
| Attempt 4 | SMOTE Tomek + Optuna | XGBoost             | 0.56                | 0.94             | 0.70     | 0.93     |

### Best Model

The best performing model was:

**Logistic Regression with SMOTE-Tomek balancing and Optuna hyperparameter tuning**

This model was selected because it provided:

* Strong ranking power
* High recall for defaulters
* Interpretability suitable for risk teams

---

### 4. Model Evaluation

The model was evaluated using standard credit risk metrics.

**AUC-ROC**

* **AUC Score:** 0.9837
* **Gini Coefficient:** 0.9673

Higher AUC indicates strong ability to rank risky borrowers.

<img width="578" height="455" alt="image" src="https://github.com/user-attachments/assets/ee320ca0-dbee-4154-ae2d-7dd69b400626" />


---

### KS Statistic

The Kolmogorov–Smirnov statistic measures the separation between defaulters and non-defaulters.

| Decile | Min Probability | Max Probability | Events | Non-events | Event Rate (%) | Non-event Rate (%) | Cum Events | Cum Non-events | Cum Event Rate (%) | Cum Non-event Rate (%) | KS         |
| ------ | --------------- | --------------- | ------ | ---------- | -------------- | ------------------ | ---------- | -------------- | ------------------ | ---------------------- | ---------- |
| 9      | 0.821           | 1.000           | 899    | 351        | 71.92          | 28.08              | 899        | 351            | 83.706             | 3.073                  | **80.633** |
| 8      | 0.212           | 0.819           | 160    | 1090       | 12.80          | 87.20              | 1059       | 1441           | 98.603             | 12.615                 | **85.988** |
| 7      | 0.029           | 0.212           | 10     | 1239       | 0.801          | 99.199             | 1069       | 2680           | 99.534             | 23.461                 | **76.073** |
| 6      | 0.004           | 0.029           | 5      | 1245       | 0.400          | 99.600             | 1074       | 3925           | 100.000            | 34.361                 | **65.639** |
| 5      | 0.001           | 0.004           | 0      | 1249       | 0.000          | 100.000            | 1074       | 5174           | 100.000            | 45.295                 | **54.705** |
| 4      | 0.000           | 0.001           | 0      | 1250       | 0.000          | 100.000            | 1074       | 6424           | 100.000            | 56.237                 | **43.763** |
| 3      | 0.000           | 0.000           | 0      | 1250       | 0.000          | 100.000            | 1074       | 7674           | 100.000            | 67.180                 | **32.820** |
| 2      | 0.000           | 0.000           | 0      | 1249       | 0.000          | 100.000            | 1074       | 8923           | 100.000            | 78.114                 | **21.886** |
| 1      | 0.000           | 0.000           | 0      | 1250       | 0.000          | 100.000            | 1074       | 10173          | 100.000            | 89.057                 | **10.943** |
| 0      | 0.000           | 0.000           | 0      | 1250       | 0.000          | 100.000            | 1074       | 11423          | 100.000            | 100.000                | **0.000**  |


KS Statistic Analysis

The Kolmogorov–Smirnov (KS) statistic measures how well the model separates default (events) and non-default (non-events) borrowers.

The maximum KS value = 85.99, indicating strong discriminatory power.

Most defaults are concentrated in the top deciles, meaning the model effectively ranks high-risk borrowers.

---

## Final Outcome

The final credit risk model:

* Achieves **AUC ≈ 0.98**
* Achieves **KS ≈ 86**
* Successfully ranks borrowers by probability of default
* Maintains performance on out-of-time data
* Produces interpretable risk scores suitable for business use

---

5. ## **Data Drift and Monitoring**


**Note: Population Stability Index (PSI) and Characteristic Stability Index (CSI) were evaluated using an Out-of-Time (OOT) validation dataset rather than a simple random test split.**

1) OOT validation simulates real production scenarios by comparing the training data distribution with data from a later time period, enabling the detection of population drift and feature instability.

2) PSI measures shifts in the predicted probability distribution between training and OOT datasets.

3) CSI measures changes in the distribution of input features across time.

This approach ensures the model remains stable, reliable, and robust when deployed in real-world credit risk decision systems.

**Population Stability Index (PSI) table**

| Bin | Probability Range   | Train Count | Train % | Test Count | Test % | A-B    | ln(A/B) | PSI    |
| --- | ------------------- | ----------- | ------- | ---------- | ------ | ------ | ------- | ------ |
| 0   | 0.000000 – 0.000004 | 6839        | 10.0    | 2223       | 17.788 | -7.788 | -0.576  | 0.0449 |
| 1   | 0.000004 – 0.000087 | 6839        | 10.0    | 2337       | 18.700 | -8.700 | -0.626  | 0.0545 |
| 2   | 0.000087 – 0.001830 | 6839        | 10.0    | 2326       | 18.612 | -8.612 | -0.621  | 0.0535 |
| 3   | 0.001830 – 0.053255 | 6839        | 10.0    | 2243       | 17.948 | -7.948 | -0.585  | 0.0465 |
| 4   | 0.053255 – 0.603632 | 6839        | 10.0    | 1739       | 13.915 | -3.915 | -0.330  | 0.0129 |
| 5   | 0.603632 – 0.892263 | 6839        | 10.0    | 574        | 4.593  | 5.407  | 0.778   | 0.0421 |
| 6   | 0.892263 – 0.967171 | 6839        | 10.0    | 348        | 2.785  | 7.215  | 1.278   | 0.0922 |
| 7   | 0.967171 – 0.990929 | 6839        | 10.0    | 247        | 1.976  | 8.024  | 1.621   | 0.1301 |
| 8   | 0.990929 – 0.998719 | 6839        | 10.0    | 218        | 1.744  | 8.256  | 1.746   | 0.1442 |
| 9   | 0.998719 – 1.000000 | 6839        | 10.0    | 242        | 1.936  | 8.064  | 1.642   | 0.1324 |

Result

Total PSI ≈ 0.75

Indicates significant population shift

Model monitoring or recalibration may be required before production deployment.


**Characteristic Stability Index (CSI)**

| # | Feature                  | Train Count | Test Count | CSI      |
| - | ------------------------ | ----------- | ---------- | -------- |
| 0 | age                      | 37,488      | 12,497     | 0.000141 |
| 1 | avg_dpd_per_delinquency  | 37,488      | 12,497     | 0.000000 |
| 2 | credit_utilization_ratio | 37,488      | 12,497     | 0.000081 |
| 3 | delinquency_ratio        | 37,488      | 12,497     | 0.000000 |
| 4 | loan_purpose             | 37,488      | 12,497     | 0.000207 |
| 5 | loan_tenure_months       | 37,488      | 12,497     | 0.000037 |
| 6 | loan_to_income           | 37,488      | 12,497     | 0.000000 |
| 7 | loan_type                | 37,488      | 12,497     | 0.000008 |
| 8 | number_of_open_accounts  | 37,488      | 12,497     | 0.000047 |
| 9 | residence_type           | 37,488      | 12,497     | 0.000192 |


**Result:**

All features have CSI < 0.01, indicating very high stability.

No significant feature drift between training data and OOT/test dataset.

This suggests the model inputs remain consistent and reliable for production deployment.


## Project Structure

```
credit-risk-model/
│
├── credit_risk_model.ipynb   # Data exploration, feature engineering, and model experimentation
├── main.py                  # Entry point for executing predictions and application logic
├── prediction_helper.py     # Reusable preprocessing and inference utilities
├── model_data.joblib        # Trained model and associated preprocessing artifacts
│
├── README.md                # System overview and project documentation
├── requirements.txt         # Project dependencies
└── .gitignore               # Files excluded from version control
```

