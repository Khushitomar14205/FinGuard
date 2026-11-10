# Cleaned data — Task 1 output

## `merged_cleaned_transactions.csv`

The final cleaned dataset produced by
`../notebooks/Task1_Data_Cleaning_Visualization.ipynb` (Section 13).

- **Shape:** 590,540 rows x 434 columns
  (394 transaction columns + 41 identity columns − 1 shared `TransactionID`).
- **Join:** LEFT JOIN on `TransactionID` — every transaction is preserved;
  144,233 transactions matched identity data, 446,307 did not (identity
  columns are NaN for those rows, by design).
- **Target:** `isFraud` (0 = legitimate, 1 = fraud), validated to contain
  only 0/1 with no missing values.
- **Cleaning applied:** median imputation for numeric columns with < 5%
  missing, mode imputation for categorical columns with < 5% missing,
  removal of full-row duplicates (none found), float64 → float32 and
  int64 → int32 downcasting, `id_01..id_38` renamed to `id-01..id-38`.
  Columns with >= 5% missing were deliberately kept as NaN — their
  missingness is informative for fraud detection.
- **Not done here:** no model training, feature selection, class balancing,
  train-test splitting or model-specific encoding (later tasks).

## Rules

- This file is **gitignored** (large, ~679 MB) — regenerate it by running the
  notebook; do not commit it to GitHub.
- To regenerate: from `Task-1-Data-Cleaning/notebooks/`, run
  `jupyter nbconvert --to notebook --execute --inplace Task1_Data_Cleaning_Visualization.ipynb`.
