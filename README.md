# FinGuard — Financial Fraud Detection System

FinGuard is a four-task project that builds a fraud detection system on the
**IEEE-CIS Fraud Detection** dataset (Kaggle competition, Vesta Corporation)
— real-world e-commerce card transactions labelled fraudulent (`isFraud = 1`)
or legitimate (`isFraud = 0`).

## Development roadmap

| Task | Scope | Status |
| --- | --- | --- |
| **Task 1 — Data Cleaning & Visualization** | Load, inspect, assess quality, handle missing values, de-duplicate, validate types, LEFT-merge transaction + identity on `TransactionID`, outlier analysis, EDA, 8 visualisations, export cleaned dataset | ✅ Complete — see `Task-1-Data-Cleaning/` |
| **Task 2 — Feature Engineering** | (Planned) Build modelling-ready features from the cleaned dataset | Planned |
| **Task 3 — Fraud Detection Model Development & Evaluation** | (Planned) Train and evaluate fraud classifiers; handle the ~3.5% fraud rate explicitly | Planned |
| **Task 4 — Explainable Fraud Detection Application & Dashboard** | (Planned) Explainable fraud detection application with a dashboard | Planned |

## Task 1 headline results (measured, not estimated)

- `train_transaction.csv`: **590,540 rows x 394 columns**; `train_identity.csv`:
  **144,233 rows x 41 columns**.
- Fraud rate: **3.50%** (20,663 fraud / 569,877 legitimate) — strongly imbalanced.
- Duplicates: **0** full-row duplicates, **0** duplicate `TransactionID`s.
- Merge (LEFT JOIN on `TransactionID`): **590,540 x 434** — all transactions
  preserved; 144,233 (24.42%) matched identity data.
- Missingness is heavy and informative: 11 columns > 90% missing, 175 columns
  50–90% missing. Columns with < 5% missing were median/mode-imputed;
  high-missingness columns were kept as NaN by design.
- Outliers kept as signal: amounts above the IQR upper bound ($247.52) have a
  **5.07%** fraud rate vs **3.30%** for the rest.
- Output: `Task-1-Data-Cleaning/cleaned_data/merged_cleaned_transactions.csv`
  (590,540 x 434, ~679 MB) + 8 PNG visualisations in
  `Task-1-Data-Cleaning/visualizations/`.

The cleaned dataset is generated locally by executing the notebook and is not
committed to the repository because of its size.

## Technology Stack

- **Python 3.12**
- **pandas, NumPy** — data loading, cleaning and analysis
- **Matplotlib, Seaborn** — visualisation
- **scikit-learn** — `SimpleImputer` only (median / mode imputation)
- **Jupyter** (`nbformat`, `ipykernel`) — notebook execution
- **Git & GitHub** — version control

## Quick Start

```bash
# 1. Clone the repository
git clone https://github.com/Khushitomar14205/FinGuard.git
cd FinGuard

# 2. Install dependencies
python -m venv .venv
source .venv/bin/activate        # Windows: .venv\Scripts\activate
pip install -r Task-1-Data-Cleaning/requirements.txt
python -m ipykernel install --user --name finguard --display-name "FinGuard (Python 3.12)"
```

**3. Download the dataset** — from Kaggle
(https://www.kaggle.com/competitions/ieee-fraud-detection/data; sign in and
accept the competition rules first) or the mirror noted in
`Task-1-Data-Cleaning/data/README.md`. Then place the files here:

```text
Task-1-Data-Cleaning/data/train_transaction.csv
Task-1-Data-Cleaning/data/train_identity.csv
```

**4. Run the notebook** (takes ~3 minutes):

```bash
cd Task-1-Data-Cleaning/notebooks
jupyter nbconvert --to notebook --execute --inplace Task1_Data_Cleaning_Visualization.ipynb
```

This regenerates the 8 visualisations and the cleaned dataset in
`Task-1-Data-Cleaning/cleaned_data/`.

## Repository structure

```text
FinGuard/
├── Task-1-Data-Cleaning/
│   ├── data/              # Original CSVs (gitignored — download separately, see data/README.md)
│   ├── notebooks/         # Task1_Data_Cleaning_Visualization.ipynb
│   ├── visualizations/    # 8 PNG figures (300 dpi)
│   ├── cleaned_data/      # merged_cleaned_transactions.csv (gitignored)
│   ├── requirements.txt
│   └── README.md
├── README.md
└── .gitignore
```

## Dataset

Source: https://www.kaggle.com/competitions/ieee-fraud-detection/data
(Kaggle account + accepted competition rules required). Test files
(`test_transaction.csv`, `test_identity.csv`, `sample_submission.csv`) are
not used in Task 1 and are kept untouched for later tasks. Large CSVs are
gitignored — never commit them.
