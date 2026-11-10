# Data — Task 1

Place the two **training** files from the IEEE-CIS Fraud Detection dataset
(Kaggle competition, Vesta Corporation) in this folder:

| File | Rows | Columns | Size |
| --- | ---: | ---: | ---: |
| `train_transaction.csv` | 590,540 | 394 | ~652 MB |
| `train_identity.csv` | 144,233 | 41 | ~26 MB |

## Where to get them

1. **Kaggle (official):** https://www.kaggle.com/competitions/ieee-fraud-detection/data
   — sign in, accept the competition rules, then download (or use the Kaggle API:
   `kaggle competitions download -c ieee-fraud-detection`).
2. **Mirror used for this Task 1 run:** https://www.antifraud.pp.ua/downloads/
   (`train_transaction` and `train_identity` endpoints) — same files, verified
   row/column counts. One naming difference: the mirror names the identity
   columns `id_01..id_38`; the notebook renames them to the canonical Kaggle
   names `id-01..id-38` in Section 7.

## Rules

- The CSVs are **gitignored** (see root `.gitignore`) — never commit them.
- **Test files (`test_transaction.csv`, `test_identity.csv`,
  `sample_submission.csv`) are not used in Task 1.** Keep them untouched for
  later tasks; do not use them for cleaning decisions or analysis.
- Do not edit the originals. All cleaning happens in the notebook and the
  output goes to `../cleaned_data/`.
