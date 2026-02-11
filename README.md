# 💰 Credit Risk Analysis & Prediction

[![en](https://img.shields.io/badge/lang-en-red.svg)](README.md)
[![pt-br](https://img.shields.io/badge/lang-pt--br-green.svg)](README_pt.md)

> *Para a versão em Português, clique no botão acima.*

---

![Python](https://img.shields.io/badge/Python-3.8+-blue?style=for-the-badge&logo=python)
![Pandas](https://img.shields.io/badge/Pandas-Data_Analysis-150458?style=for-the-badge&logo=pandas)
![Seaborn](https://img.shields.io/badge/Seaborn-Visualization-success?style=for-the-badge&logo=python)
![Status](https://img.shields.io/badge/Status-Modeling_Ready-orange?style=for-the-badge)

## 📌 Project Overview
Default risk is one of the biggest challenges for financial institutions. This project uses historical loan application data to identify behavioral patterns and predict the probability of a client defaulting on their payments (`TARGET`: 0 = Payer, 1 = Defaulter).

The goal is not just to predict risk, but to **understand the drivers** behind default, enabling more assertive credit granting strategies.

---

## 🛠️ Solution Strategy (Pipeline)

The project follows the **CRISP-DM** methodology, focusing on 3 strategic analysis steps before modeling:

### 1. Data Understanding & Sanity Check
Initial diagnosis of data health.
* **Class Imbalance:** Identified that only **8.07%** of the base consists of defaulters (minority class).
* **Data Quality:** Detection of columns with >50% nulls (`OWN_CAR_AGE`, `EXT_SOURCE_1`) and severe multicollinearity between `AMT_CREDIT` and `AMT_GOODS_PRICE`.

### 2. Data Preparation & Cleaning
Robust treatment to ensure modeling integrity.
* **Anomaly Handling:** Treatment of the `DAYS_EMPLOYED` variable, where 365,243 days (1000 years) represented errors or retirees.
* **Smart Imputation:** Filling null values in Income and Annuities using the median to avoid distortion by outliers.
* **Categorization:** Creation of the `Unknown` category for undisclosed professions, preserving the information of omission.

### 3. Feature Engineering & EDA
Creation of new variables and validation of business hypotheses.
* **Feature Creation:**
    * `EXT_SOURCE_MEAN`: Average of external bureau scores (Credit Bureau/FICO equivalent).
    * `DEBT_TO_INCOME_RATIO`: Income commitment to the loan.
    * `AGE`: Conversion from days to years.

---

## 📊 Key Business Insights

During Exploratory Data Analysis (EDA), we tested 5 major hypotheses. The results guided feature selection for the model:

### 1. History > Current Capacity
The strongest variable in the dataset is not income, but history. The **External Scores Mean (`EXT_SOURCE_MEAN`)** proved to be the "Holy Grail" of prediction, showing a sharp separation between good and bad payers.
> *Insight:* Clients with low external scores are imminent risks, regardless of declared income.

### 2. The Maturity Factor
We confirmed the hypothesis that **young people (< 30 years)** have significantly higher risk. The default curve decreases linearly as age advances.

### 3. Education as a Shield
Education level showed a strong negative correlation with risk. Clients with **Higher Education** have drastically lower default rates than those with only Secondary Education or lower.

---

## 📉 Top Risk Drivers (Correlation Ranking)

After feature engineering, we identified which variables most impact the credit decision:

| Variable | Impact | Description |
| :--- | :--- | :--- |
| **`EXT_SOURCE_MEAN`** | 📉 Protection | Higher score = Lower risk (Strong Negative Correlation). |
| **`AGE`** | 📉 Protection | Older clients are safer. |
| **`REGION_RATING`** | 📈 Risk | Residents of regions with low commercial ratings tend to default more. |
| **`DAYS_EMPLOYED`** | 📉 Protection | Job stability reduces risk. |

![Heatmap](img/correlation_ranking.png)

---

## 🚀 Conclusion & Next Steps

The analysis concluded that the **Low Risk** profile consists of older clients, with higher education, job stability, and good external history. Interestingly, income commitment (`DEBT_TO_INCOME`) had a lower impact than expected, suggesting that **Character** outweighs **Capacity** in this dataset.

**Next Steps:**
1.  **Modeling (Notebook 04):** Training tree-based algorithms (Random Forest / XGBoost) due to non-linearity identified in features.
2.  **Hyperparameter Optimization:** Focusing on the **ROC-AUC** metric due to class imbalance.

---

## 📁 Project Structure

```bash
├── data
│   ├── raw                 # Original data
│   ├── processed           # Cleaned data (df_cleaned.csv)
├── notebooks
│   ├── 01_Data_Understanding.ipynb
│   ├── 02_Data_Preparation.ipynb
│   ├── 03_EDA_Business_Insights.ipynb
├── README.md               # English Version
├── README_pt.md            # Portuguese Version