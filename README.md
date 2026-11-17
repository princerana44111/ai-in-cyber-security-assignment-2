# Lab 2 — Detecting Suspicious Login Activity Using Machine Learning

**Course:** ENSP355 — AI in Cyber Security Lab (2026–27)  
**Lab:** 2 — Detecting Suspicious Login Activity Using Machine Learning

This repository implements all six questions in the supplied Lab 2 manual:
1. Dataset inspection and cleaning
2. Stratified training/testing split
3. Decision Tree training, accuracy and confusion matrix
4. Standardized K-Means clustering and label comparison
5. Feature importance and three new-record predictions
6. False-positive / false-negative discussion and result export

## Important dataset note

The assignment refers to `simple_login_activity.csv` and a starter notebook, but those files were not included with the uploaded PDF. To make this repository runnable, it includes a **small synthetic teaching dataset** at `data/simple_login_activity.csv`. It contains 100 generated login records, two deliberately missing numeric values, and one duplicate row so the cleaning steps can be demonstrated. It contains no real account or network data.

If your instructor provides the official CSV, replace the included CSV while keeping these column names:
`failed_logins`, `login_hour`, `new_device`, `download_mb`, `label`.

The generated results in `outputs/` correspond to the included synthetic CSV and may change if you replace it.

## Project structure

```text
Lab2_Login_Activity_ML/
├── data/
│   └── simple_login_activity.csv
├── outputs/
│   ├── Lab2_Login_ML_Result.csv
│   ├── cluster_summary.csv
│   ├── cluster_vs_label.csv
│   ├── confusion_matrix.csv
│   ├── confusion_matrix.png
│   ├── feature_importance.csv
│   ├── feature_importance.png
│   ├── metrics.json
│   └── new_record_predictions.csv
├── notebooks/
│   └── Lab2_Suspicious_Login_Activity.ipynb
├── src/
│   └── solve_lab2.py
├── .gitignore
├── requirements.txt
└── README.md
```

## Run locally

Python 3.9+ is recommended.

```bash
python -m venv .venv
# macOS / Linux
source .venv/bin/activate
# Windows PowerShell: .venv\Scripts\Activate.ps1

pip install -r requirements.txt
python src/solve_lab2.py
```

The script reads the CSV from `data/` and writes tables, charts and the final result CSV to `outputs/`.

## Run in Google Colab

1. Upload this repository ZIP or clone the repository.
2. Open `notebooks/Lab2_Suspicious_Login_Activity.ipynb` in Colab.
3. Run all cells from top to bottom.

## Method and interpretation

- **Cleaning:** duplicate rows are removed; missing `failed_logins` and `download_mb` values are replaced by their respective medians.
- **Split:** 75% training and 25% testing, `random_state=42`, stratified by label.
- **Decision Tree:** `max_depth=4`, `random_state=42`.
- **K-Means:** four numeric features are standardized; two clusters are formed with `n_init=10`. The known label is used only for post-clustering comparison, not to train K-Means.
- **Feature importance:** reflects how the fitted tree used features; it does not establish causation.
- **False positive:** legitimate late-night work from a new device is flagged as suspicious.
- **False negative:** an attack with few obvious indicators is classified as normal.
- **Limitation:** the dataset is small and synthetic, so its patterns are not representative of all real users or attacks. A human analyst should review high-risk alerts before account restrictions or incident conclusions.

## Reproducibility

The data-generation seed and model random states are fixed. The included `outputs/` are reference outputs for the included CSV. Re-run the script or notebook to regenerate them.

## Academic use

Use this as a working implementation and verify the output on your instructor's official dataset if one is supplied. Add your own screenshots and explanations before submission if your lab instructor requires evidence captured from your own Colab run.
