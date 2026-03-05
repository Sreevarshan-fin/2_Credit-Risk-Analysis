
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

## Preparing the Data for Modeling

Before training the models, several preprocessing steps were applied to ensure data quality and stability. Invalid values in loan purpose were handled, predictive features were selected using Information Value and domain knowledge, multicollinearity was checked using VIF, and numeric variables were scaled using Min–Max scaling. These steps ensured that the model inputs were both statistically sound and business-relevant.

---

## Model Development and Tuning

Multiple algorithms were trained and compared, including Logistic Regression, Random Forest, and XGBoost. This allowed a balance between interpretability and predictive power. To improve performance and avoid overfitting, hyperparameter tuning was carried out using RandomizedSearchCV and Optuna.

---

## How Model Performance Was Evaluated

Model evaluation focused on metrics that matter in credit risk:

* AUC and Gini to measure ranking power
* KS statistic to assess separation between defaulters and non-defaulters
* Classification reports to understand prediction behavior

Special attention was given to performance consistency across training, test, and out-of-time datasets.

---

## Business Perspective on the Model Output

Instead of directly approving or rejecting loans, the final model produces **interpretable risk scores**. These scores can be translated into business rules and integrated with a Business Rule Engine. This allows Lauki Finance to automate parts of the decision process while still retaining analyst oversight and regulatory transparency.

---

## Final Outcome 
The result of Phase 1 is a validated, industry-aligned credit risk model that:

* Accurately ranks customers by default risk
* Meets defined performance benchmarks
* Performs consistently on future data
* Produces explainable outputs suitable for risk teams
* Can be extended into downstream decision or rule-based systems

---

## Overall Goal

The broader goal of this project is to provide Lauki Finance with a **practical and scalable credit risk modeling foundation**. By combining strong statistical performance with interpretability, the model helps reduce bad loans, improve decision turnaround time, and support data-driven lending operations as the business grows.

---





