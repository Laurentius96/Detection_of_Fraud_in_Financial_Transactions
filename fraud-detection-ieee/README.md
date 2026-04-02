<div align="center">
  <h1>🔍 IEEE Fraud Detection — Transaction Fraud Prediction</h1>
  <p><i>Complete machine learning pipeline for detecting fraudulent financial transactions</i></p>
</div>

<p align="center">
  <a href="#-overview">Overview</a> •
  <a href="#-objective">Objective</a> •
  <a href="#-technologies-used">Technologies</a> •
  <a href="#-project-structure">Project Structure</a> •
  <a href="#-data-pipeline">Data Pipeline</a> •
  <a href="#-implemented-features">Features</a> •
  <a href="#-results-and-insights">Results & Insights</a> •
  <a href="#-how-to-use">How to Use</a> •
  <a href="#-next-steps">Next Steps</a> •
  <a href="#-license">License</a>
</p>

---

## 🔍 Overview

This project implements a **complete machine learning pipeline** for detecting fraudulent financial transactions using the [IEEE-CIS Fraud Detection](https://www.kaggle.com/c/ieee-fraud-detection) Kaggle competition dataset.

The dataset combines **transaction data** and **identity data** from Vesta Corporation's real-world e-commerce transactions, making it one of the most realistic fraud detection challenges available publicly.

The project demonstrates essential **Data Science skills** including:
- Exploratory data analysis on highly imbalanced datasets
- Memory optimization for large-scale data processing
- Missing value analysis and strategic imputation
- Feature engineering for fraud detection
- Gradient boosting models (LightGBM / XGBoost)
- Evaluation with fraud-appropriate metrics (AUC-ROC, Precision, Recall, F1)

---

## 🎯 Objective

The main goal is to build a model capable of predicting whether a transaction is fraudulent, through a systematic pipeline that ensures:

1. **Data Understanding**: Analyze target distribution and class imbalance
2. **Memory Optimization**: Reduce RAM usage via type downcasting and category encoding
3. **Missing Value Strategy**: Map nulls, create informative flags, and apply context-aware imputation
4. **Data Integration**: Merge transaction and identity datasets via TransactionID
5. **Feature Engineering**: Create new features from existing ones to improve model signal
6. **Modeling**: Train and evaluate gradient boosting models
7. **Submission**: Generate Kaggle-ready prediction file

---

## 🛠️ Technologies Used

<div align="center">

| Technology | Purpose | Version |
|------------|---------|---------|
| ![Python](https://img.shields.io/badge/Python-3776AB?style=for-the-badge&logo=python&logoColor=white) | Core programming language | 3.x |
| ![Pandas](https://img.shields.io/badge/Pandas-150458?style=for-the-badge&logo=pandas&logoColor=white) | Data manipulation and analysis | 2.x |
| ![NumPy](https://img.shields.io/badge/NumPy-013243?style=for-the-badge&logo=numpy&logoColor=white) | Numerical computing | 1.x |
| ![Scikit-learn](https://img.shields.io/badge/Scikit--learn-F7931E?style=for-the-badge&logo=scikit-learn&logoColor=white) | Preprocessing and evaluation | 1.x |
| ![LightGBM](https://img.shields.io/badge/LightGBM-02569B?style=for-the-badge&logo=python&logoColor=white) | Gradient boosting model | latest |
| ![Matplotlib](https://img.shields.io/badge/Matplotlib-11557C?style=for-the-badge&logo=python&logoColor=white) | Data visualization | 3.x |
| ![Seaborn](https://img.shields.io/badge/Seaborn-4C72B0?style=for-the-badge&logo=python&logoColor=white) | Statistical visualization | latest |
| ![Jupyter](https://img.shields.io/badge/Jupyter-F37626?style=for-the-badge&logo=jupyter&logoColor=white) | Development environment | latest |

</div>

---

## 📁 Project Structure

```
📦 ieee-fraud-detection/
├── 📂 data/
│   ├── train_transaction.csv      # 590,540 rows × 394 columns
│   ├── train_identity.csv         # 144,233 rows × 41 columns
│   ├── test_transaction.csv       # 506,691 rows × 393 columns
│   └── test_identity.csv          # 119,930 rows × 41 columns
├── 📂 docs/
│   └── documentacao_projeto.md    # Step-by-step project documentation (PT-BR)
├── 📂 notebooks/
│   └── ieee_fraud_detection.ipynb # Main development notebook
├── 📂 models/
│   └── (trained models saved here)
├── 📂 outputs/
│   └── submission.csv             # Kaggle submission file
└── README.md
```

---

## 🔄 Data Pipeline

```
[Raw Data]
    │
    ▼
[Step 1] Setup & Environment Configuration
    │
    ▼
[Step 2] Data Loading — 4 CSV files
    │
    ▼
[Step 3] Target Analysis — Class Imbalance (96.5% / 3.5%)
    │
    ▼
[Step 4] Column Group Analysis — 12 feature groups identified
    │
    ▼
[Step 5] Missing Value Mapping — Null flags + decision rules
    │
    ▼
[Step 6] Memory Optimization — 2,062 MB → 1,203 MB (−41.7%)
    │
    ▼
[Step 7] DataFrame Merge — TransactionID join  ← current step
    │
    ▼
[Step 8] Feature Engineering
    │
    ▼
[Step 9] Null Imputation — Context-aware strategy
    │
    ▼
[Step 10] Baseline Model
    │
    ▼
[Step 11] LightGBM / XGBoost
    │
    ▼
[Step 12] Evaluation — AUC-ROC, Precision, Recall, F1
    │
    ▼
[Step 13] Hyperparameter Tuning
    │
    ▼
[Step 14–15] Submission & Score Analysis
```

---

## ⚙️ Implemented Features

### ✅ Completed

- [x] **Environment Setup** — Organized imports and display configuration
- [x] **Data Loading** — 4 files loaded with shape and type validation
- [x] **Target Analysis** — Class imbalance quantified and visualized
- [x] **Column Mapping** — 12 feature groups identified and documented
- [x] **Null Analysis** — Per-column null %, block detection, decision rules
- [x] **Memory Optimization** — Type downcast + category encoding (−41.7% RAM)

### 🔲 In Progress / Pending

- [ ] **DataFrame Merge** — Left join on TransactionID
- [ ] **Feature Engineering** — New features from existing columns
- [ ] **Null Imputation** — Flags + -999 strategy for tree models
- [ ] **Baseline Model** — Simple reference model
- [ ] **LightGBM Model** — Main gradient boosting model
- [ ] **Evaluation** — AUC-ROC focused metrics
- [ ] **Hyperparameter Tuning** — Optuna / GridSearch
- [ ] **Kaggle Submission** — submission.csv generation

---

## 📊 Results and Insights

### Dataset Overview

| Dataset | Rows | Columns | Memory (optimized) |
|---|---|---|---|
| train_transaction | 590,540 | 394 | ~1,203 MB |
| train_identity | 144,233 | 41 | ~120 MB |
| test_transaction | 506,691 | 393 | — |
| test_identity | 119,930 | 41 | — |

### Class Distribution

| Class | Count | Percentage |
|---|---|---|
| Legitimate (0) | 569,877 | 96.50% |
| Fraud (1) | 20,663 | 3.50% |

> ⚠️ **Highly imbalanced dataset** — accuracy is a misleading metric here. Focus on AUC-ROC, Precision, and Recall.

### Key Findings (so far)

- **Null blocks**: Multiple V-group columns share exactly 508,595 nulls → same collection source
- **Informative nulls**: `dist2` (93.6% null) — absence may indicate same billing/shipping address (lower fraud risk)
- **Memory reduction**: Type optimization reduced RAM by 41.7% on transaction data
- **Pandas 4 breaking change**: `str` dtype ≠ `object` dtype — requires explicit handling

---

## 🚀 How to Use

### 1. Clone the repository

```bash
git clone https://github.com/your-username/ieee-fraud-detection.git
cd ieee-fraud-detection
```

### 2. Install dependencies

```bash
pip install pandas numpy matplotlib seaborn scikit-learn lightgbm jupyter
```

### 3. Download the dataset

Download the data from [Kaggle Competition](https://www.kaggle.com/c/ieee-fraud-detection/data) and place the CSV files inside the `data/` folder.

### 4. Run the notebook

```bash
jupyter notebook notebooks/ieee_fraud_detection.ipynb
```

> Follow the steps sequentially — each step builds on the previous one.

---

## 🔭 Next Steps

- [ ] Complete DataFrame merge (Step 7)
- [ ] Implement feature engineering (Step 8)
- [ ] Apply null imputation strategy (Step 9)
- [ ] Train baseline and LightGBM models (Steps 10–11)
- [ ] Evaluate with AUC-ROC and fraud-specific metrics (Step 12)
- [ ] Submit to Kaggle and analyze public leaderboard score (Steps 14–15)

---

## 📄 License

This project is licensed under the [Creative Commons Attribution-NonCommercial-NoDerivatives 4.0 International License (CC BY-NC-ND 4.0)](LICENSE.md).

---

<div align="center">
  <p><i>Developed as part of a guided Data Science portfolio project</i></p>
  <p>⭐ If this project helped you, consider giving it a star!</p>
</div>