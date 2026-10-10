<div align="center">

# 🔍 IEEE-CIS Fraud Detection

**Building a fraud-detection model for real e-commerce transactions, step by step**

![Status](https://img.shields.io/badge/Status-In_progress-F59E0B?style=for-the-badge)
![Python](https://img.shields.io/badge/Python-3776AB?style=for-the-badge&logo=python&logoColor=white)
![Pandas](https://img.shields.io/badge/Pandas-150458?style=for-the-badge&logo=pandas&logoColor=white)
![NumPy](https://img.shields.io/badge/NumPy-013243?style=for-the-badge&logo=numpy&logoColor=white)
![Seaborn](https://img.shields.io/badge/Seaborn-4C72B0?style=for-the-badge)
![Kaggle](https://img.shields.io/badge/Kaggle-20BEFF?style=for-the-badge&logo=kaggle&logoColor=white)

<p>
  <a href="#-overview">Overview</a> •
  <a href="#-progress">Progress</a> •
  <a href="#-dataset">Dataset</a> •
  <a href="#-findings-so-far">Findings</a> •
  <a href="#-project-structure">Structure</a> •
  <a href="#-how-to-use">How to Use</a> •
  <a href="#-license">License</a>
</p>

</div>

---

## 🔍 Overview

Fraud is rare, costly and hidden among hundreds of thousands of legitimate transactions. This project works on the [IEEE-CIS Fraud Detection](https://www.kaggle.com/c/ieee-fraud-detection) Kaggle competition, built on real e-commerce transactions from Vesta Corporation, to predict the probability that a transaction is fraudulent.

> 🚧 **Work in progress.** Exploratory analysis and memory optimization are done; data integration, feature engineering and modeling are next. Every step is documented in [`docs/documentacao_projeto.md`](docs/documentacao_projeto.md) (Portuguese).

## 🧭 Progress

```mermaid
flowchart LR
    A[Setup] --> B[Load 4 CSV files] --> C[Target analysis] --> D[Column groups] --> E[Missing values] --> F[Memory optimization]
    F --> G[Merge on TransactionID] --> H[Feature engineering] --> I[Imputation] --> J[Baseline model] --> K[LightGBM / XGBoost] --> L[Evaluation and tuning] --> M[Kaggle submission]
    classDef done fill:#16a34a,color:#fff,stroke:#14532d
    classDef todo fill:#f3f4f6,color:#111827,stroke:#9ca3af
    class A,B,C,D,E,F done
    class G,H,I,J,K,L,M todo
```

| Phase | Steps | Status |
|---|---|---|
| 1. Understanding and preparing the data | Setup, loading, target, column groups, missing values, memory | ✅ Done |
| 2. Integration and feature engineering | Merge, new features, imputation | 🔲 Next |
| 3. Modeling | Baseline, LightGBM / XGBoost, AUC-ROC evaluation, tuning | 🔲 Planned |
| 4. Delivery | Submission file and leaderboard analysis | 🔲 Planned |

## 📊 Dataset

Four files from the competition (not stored in this repository):

| File | Rows | Columns |
|---|---:|---:|
| `train_transaction.csv` | 590,540 | 394 |
| `train_identity.csv` | 144,233 | 41 |
| `test_transaction.csv` | 506,691 | 393 |
| `test_identity.csv` | 141,907 | 41 |

<details>
<summary><b>📖 Column groups</b></summary>
<br>

| Group | Columns | Description |
|---|---:|---|
| `TransactionDT` | 1 | Relative timestamp |
| `TransactionAmt` | 1 | Transaction amount |
| `card1`–`card6` | 6 | Card data |
| `addr1`–`addr2` | 2 | Addresses |
| `dist1`–`dist2` | 2 | Distances between addresses |
| `C1`–`C14` | 14 | Counts (e.g. addresses linked to the card) |
| `D1`–`D15` | 15 | Time deltas between events |
| `M1`–`M9` | 9 | Match flags (e.g. name matches the card) |
| `V1`–`V339` | 339 | Vesta engineered features |
| `id_01`–`id_38` | 38 | Identity data |
| `DeviceType`, `DeviceInfo` | 2 | Device |

</details>

## 💡 Findings So Far

| Topic | Finding |
|---|---|
| ⚖️ Class imbalance | Only **3.50%** of the training transactions are fraud (20,663 of 590,540), so accuracy is misleading; the plan is to evaluate with **AUC-ROC, precision and recall** |
| 🕳️ Missing values | **41.1%** of all cells in `train_transaction` are empty; 374 of 394 columns have gaps |
| 📉 Worst columns | `dist2` 93.6%, `D7` 93.4%, `D13` and `D14` 89.5% missing |
| 🧩 Missing blocks | Several `V` columns share exactly **508,595** missing values, suggesting a common source |
| 💡 Informative gaps | A missing `dist2` may mean billing and shipping addresses match, so missingness itself can be a feature |
| 💾 Memory | `train_transaction` dropped from **2,062 MB to 1,203 MB** after type downcasting; 17 low-cardinality text columns of `train_identity` became `category` |

**Missing-value rule defined for the next phase:**

| Missing share | Plan |
|---|---|
| < 5% | Impute directly (mean, median or mode) |
| 5–30% | Check the relationship with the target first |
| 30–80% | Add an `is_null` flag and impute with −999 for tree models |
| > 80% | Candidate to drop, after checking the target relationship |

## 📁 Project Structure

```
📁 Detection_of_Fraud_in_Financial_Transactions/
├── 📁 docs/
│   └── 📄 documentacao_projeto.md          # Step-by-step log (Portuguese)
├── 📁 fraud-detection-ieee/
│   ├── 📁 data/raw/                         # Competition CSVs (git-ignored)
│   ├── 📁 notebooks/
│   │   └── 📓 01_eda_inicial.ipynb          # Exploratory analysis (steps 1–6)
│   ├── 📄 requirements.txt                  # Full environment freeze (Windows)
│   └── 📄 .gitignore
├── 📄 LICENSE.md                            # CC BY-NC-ND 4.0
└── 📄 README.md
```

## 🚀 How to Use

1. **Clone the repository**

   ```bash
   git clone https://github.com/Laurentius96/Detection_of_Fraud_in_Financial_Transactions.git
   cd Detection_of_Fraud_in_Financial_Transactions/fraud-detection-ieee
   ```

2. **Install the libraries used so far**

   ```bash
   pip install pandas numpy matplotlib seaborn jupyter
   ```

   `requirements.txt` is a full freeze of the author's Windows environment; on Linux or macOS, use the command above.

3. **Get the data:** accept the competition rules on [Kaggle](https://www.kaggle.com/c/ieee-fraud-detection/data), download the four CSV files and place them in `fraud-detection-ieee/data/raw/`.

4. **Run the notebook**

   ```bash
   jupyter notebook notebooks/01_eda_inicial.ipynb
   ```

> 💡 The training transactions alone take about 2 GB of RAM when loaded.

## 🤝 Contributing

Contributions, ideas and feedback are welcome!

- 🐛 **Found a bug or something unclear?** [Open an issue](https://github.com/Laurentius96/Detection_of_Fraud_in_Financial_Transactions/issues/new) describing what happened and how to reproduce it.
- 💡 **Have an idea or a question?** [Open an issue](https://github.com/Laurentius96/Detection_of_Fraud_in_Financial_Transactions/issues/new) and tell me about it.
- 🔧 **Want to propose a fix?** Fork the repository, create a branch (`git checkout -b fix/short-description`), commit your changes and open a pull request explaining what changed and why.

By contributing, you agree that your contribution is shared under the same license as this project.

## 📜 License

This project is licensed under the [Creative Commons Attribution-NonCommercial-NoDerivatives 4.0 International License (CC BY-NC-ND 4.0)](LICENSE.md).

**What this means:**
- ✅ You may **share** this work with **attribution**
- ❌ You may **not** use it for **commercial purposes**
- ❌ You may **not** distribute **modified versions**

---

<div align="center">

**Lorenzo C. Bianchi** · [GitHub](https://github.com/Laurentius96) · [LinkedIn](https://www.linkedin.com/in/cb-lorenzo/)

</div>
