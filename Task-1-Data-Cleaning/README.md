# FinGuard — Task 1: Data Cleaning & Visualization

## Project overview

Task 1 of FinGuard prepares the IEEE-CIS Fraud Detection dataset for
modelling: it loads the training transaction and identity files, inspects
them, assesses data quality, handles missing values with a documented rule,
removes confirmed duplicates, validates data types, merges the two files with
a LEFT JOIN on `TransactionID`, analyses outliers, runs exploratory analysis
and exports one clean merged dataset — with all findings visualised.

## Objective

Produce a single, validated, analysis-ready dataset
(`cleaned_data/merged_cleaned_transactions.csv`) and 8 publication-quality
figures, without doing any modelling, feature selection, class balancing,
train-test splitting or model-specific encoding (those are later tasks).

## Dataset information

| File | Rows | Columns | Key |
| --- | ---: | ---: | --- |
| `train_transaction.csv` | 590,540 | 394 | `TransactionID`, target `isFraud` |
| `train_identity.csv` | 144,233 | 41 | `TransactionID`, device/browser fields |

- Target: `isFraud` — 0 legitimate, 1 fraud. Measured fraud rate **3.50%**
  (20,663 fraud / 569,877 legitimate; ratio 27.6 : 1).
- Only the **training** split is used. Test files are untouched.
- Verified counts match the official Kaggle competition figures.
- Note: the mirror used to obtain the files names identity columns
  `id_01..id_38`; the notebook renames them to the canonical Kaggle names
  `id-01..id-38` (Section 7).

## Key findings (measured in the notebook)

- **Duplicates:** 0 full-row duplicates and 0 duplicate `TransactionID`s in
  either file.
- **Missingness:** 11 columns > 90% missing, 175 columns 50–90%, 118 columns
  10–50%, 108 columns < 10%, 23 columns with no missing values.
  Median/mode imputation for columns with < 5% missing filled 46,987 values
  in the transaction file (90 numeric columns median-imputed, 2 categorical
  mode-imputed) and 51,485 values in the identity file (5 numeric, 9
  categorical). Columns with >= 5% missing (282 transaction + 24 identity) were
  kept as NaN by design — their missingness is informative for fraud.
- **Merge:** LEFT JOIN preserves all 590,540 transactions → **590,540 x 434**;
  144,233 transactions (24.42%) matched identity data.
- **Outliers (IQR):** amounts above $247.52 (66,482 rows) show a 5.07% fraud
  rate vs 3.30% for the rest — outliers are **kept**, not removed.
- **Amounts:** median fraud $75.00 vs median legitimate $68.50.

## Technology stack

Python 3.12 · pandas · NumPy · Matplotlib · Seaborn · scikit-learn
(`SimpleImputer` only) · Jupyter (`nbformat`/`ipykernel` to run).

## Repository structure

```text
Task-1-Data-Cleaning/
├── data/
│   └── README.md            # dataset source + placement instructions (CSVs gitignored)
├── notebooks/
│   └── Task1_Data_Cleaning_Visualization.ipynb   # the whole implementation (13 sections)
├── visualizations/          # 01–08 PNG figures, 300 dpi
├── cleaned_data/
│   ├── README.md
│   └── merged_cleaned_transactions.csv   # 590,540 x 434, ~679 MB (gitignored)
├── requirements.txt
└── README.md
```

## Installation

```bash
python -m venv .venv
source .venv/bin/activate        # Windows: .venv\Scripts\activate
pip install -r Task-1-Data-Cleaning/requirements.txt
python -m ipykernel install --user --name finguard --display-name "FinGuard (Python 3.12)"
```

## Dataset placement

Download from Kaggle (https://www.kaggle.com/competitions/ieee-fraud-detection/data
— sign in and accept the competition rules first) or use the mirror noted in
`data/README.md`, then place the two files here:

```text
Task-1-Data-Cleaning/data/train_transaction.csv
Task-1-Data-Cleaning/data/train_identity.csv
```

Do not commit the CSVs — they are gitignored.

## Notebook execution

```bash
cd Task-1-Data-Cleaning/notebooks
jupyter nbconvert --to notebook --execute --inplace Task1_Data_Cleaning_Visualization.ipynb
```

Takes ~3 minutes and ~1 GB RAM for the merged frame. All paths are relative
to the `notebooks/` folder.

## Generated visualizations

| # | File | Content |
| --- | --- | --- |
| 1 | `01_missing_values.png` | Top 30 columns by missing % |
| 2 | `02_fraud_vs_genuine.png` | Fraud vs legitimate counts |
| 3 | `03_transaction_amount_distribution.png` | Transaction amount distribution (log scale) |
| 4 | `04_amount_by_fraud.png` | Amount comparison, fraud vs legitimate |
| 5 | `05_fraud_pct_by_product.png` | Fraud % by ProductCD |
| 6 | `06_correlation_heatmap.png` | Correlation heatmap (Amt, card, C1–C14, D1–D15) |
| 7 | `07_outlier_analysis.png` | Amount outliers by fraud status (log1p) |
| 8 | `08_fraud_by_categorical.png` | Fraud rate by card4, card6, DeviceType, M4 |

## Output dataset information

`cleaned_data/merged_cleaned_transactions.csv` — 590,540 rows x 434 columns,
one row per transaction, unique `TransactionID`, validated `isFraud` (0/1, no
missing values), no duplicates, outliers retained. Regenerate it by re-running
the notebook; it is gitignored because of its size (~679 MB).
