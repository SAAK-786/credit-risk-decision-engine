# Credit Risk Decision Engine

> **Work in progress — Day 4 logistic-regression baseline completed.** The model has been evaluated on validation data; the held-out test set, model comparison, and hypothetical decision simulation remain outstanding.

## Overview

This project investigates credit-card default risk using the [Default of Credit Card Clients dataset](https://archive.ics.uci.edu/dataset/350/default+of+credit+card+clients) from the UCI Machine Learning Repository (dataset ID **350**).

It is an independent machine-learning learning project, progressing from data auditing and exploratory analysis to feature engineering, default-probability modelling, model validation and, eventually, a **hypothetical** risk-decision simulation.

The dataset includes **30,000 existing credit-card customers**, 23 predictor variables, a customer identifier and the binary target `default_next_month`. The initial audit found an overall subsequent-month default rate of **22.12%**.

## Current progress

| Stage | Status | Scope |
| --- | --- | --- |
| 1. Data acquisition and audit | Exploratory notebook completed | Import data, inspect metadata, data-quality checks, class distribution |
| 2. Cleaning and EDA | Exploratory notebook completed | Undocumented category codes, repayment behaviour, credit limits, demographic comparisons |
| 3. Further feature engineering | Completed (notebook) | Credit utilisation, payment consistency, repayment delays, balance trends, risk-group EDA and descriptive validation |
| 4. Baseline modelling and validation | Baseline completed (notebook) | Stratified split, train-only preprocessing, logistic regression, validation discrimination/calibration, coefficient and threshold analysis; further models and held-out testing pending |
| 5. Hypothetical decision simulation | Planned | Threshold trade-offs with fairness and selection-bias limitations |

### Exploratory findings

- The data audit identified no missing values or duplicate customer IDs.
- Education and marriage variables contain undocumented codes, grouped as `Unknown` for analysis while retaining the original variables.
- Repayment-status fields have negative codes that need careful interpretation; negative bill balances are not automatically treated as errors.
- September repayment-status groups had an observed **13.83%** default rate for customers without a recent positive delay, compared with **69.55%** for customers two or more months late.
- Day 3 found an observed default rate of **29.96%** in the highest average-utilisation quartile, versus **14.77%** in the lower-middle quartile; customers with no recorded positive payments had an observed default rate of **37.99%**, versus **13.92%** among those with positive payments in all six months.
- Day 3's payment-to-bill ratio is undefined for 966 customers with no positive bill history; these records are retained with an indicator.
- Associations by repayment behaviour, credit limit or demographics do **not** establish causal effects or justify automatic lending decisions.

These Day 1–3 findings are descriptive results from the exploratory notebooks, not causal effects or externally validated model-performance claims.

### Day 4 — Logistic-regression baseline (validation only)

The [Day 4 notebook](notebooks/04_logistic_regression_baseline.ipynb) selects **11 financially motivated features** from the engineered dataset and uses a stratified **60% / 20% / 20%** training, validation and held-out test split. A scikit-learn pipeline fits median imputation, standardisation and logistic regression using the training split only.

The notebook reports the following **validation** results (6,000 customers):

| Metric | Validation result |
| --- | ---: |
| ROC-AUC | 0.751 |
| Average precision | 0.486 |
| Brier score | 0.146 |
| Accuracy at threshold 0.50 | 80.13% |
| Precision at threshold 0.50 | 64.03% |
| Recall at threshold 0.50 | 23.21% |
| F1 score at threshold 0.50 | 0.341 |

At the default 0.50 classification threshold, the notebook reports **308 true positives**, **1,019 false negatives**, and **173 false positives**. It also examines calibration, standardised coefficients and threshold trade-offs. No final classification threshold is selected. These results are reported by the author in the notebook; the published notebook has cleared execution outputs and has not been independently rerun as part of this repository update. **The held-out test set is not evaluated in Day 4.**

## Repository structure

```text
credit-risk-decision-engine/
├── notebooks/
│   ├── 01_data_audit.ipynb
│   ├── 02_cleaning_eda.ipynb
│   ├── 03_feature_engineering.ipynb
│   └── 04_logistic_regression_baseline.ipynb
├── data/
│   ├── raw/
│   └── processed/
├── outputs/
│   ├── figures/
│   ├── tables/
│   └── models/ (generated locally; ignored by Git)
├── docs/
│   └── roadmap.md
├── README.md
├── requirements.txt
└── .gitignore
```

## Getting started

Clone this repository and create a virtual environment:

```bash
python3 -m venv .venv
source .venv/bin/activate
python -m pip install -r requirements.txt
jupyter lab
```

Run **`notebooks/01_data_audit.ipynb` first**. It downloads UCI dataset 350 via `ucimlrepo`, creates local directories, saves `data/raw/credit_default_uci.csv` and exports audit tables. An internet connection is needed for the initial data download.

Then run **`notebooks/02_cleaning_eda.ipynb`**, which reads the saved raw CSV, performs cleaning, feature summaries and descriptive analysis, and exports `data/processed/credit_default_cleaned.csv` for Day 3.

**Day 3:** `notebooks/03_feature_engineering.ipynb` builds customer-level features from April–September repayment, billing and payment histories; checks feature ranges and missingness; explores default rates by utilisation and payment consistency; and saves `data/processed/credit_default_features.csv`, summary tables and a figure.

**Day 4:** `notebooks/04_logistic_regression_baseline.ipynb` loads the processed Day 3 feature file, creates stratified training/validation/test splits, fits an interpretable logistic-regression baseline, and analyses validation performance, calibration, coefficients and threshold trade-offs. The notebook saves the fitted pipeline to `outputs/models/logistic_regression_baseline.joblib`, figures to `outputs/figures/`, and validation metrics, predictions and split assignments to `outputs/tables/`. All generated artefacts remain local and are excluded from Git.

Run the notebooks in order **01 → 02 → 03 → 04**, because each subsequent stage relies on data exported by the preceding stage. Saved cell outputs have been cleared from the committed notebooks; execute them locally to regenerate results and figures. This repository update has not independently rerun the entire pipeline.

## Dataset and responsible use

**Source:** [UCI Machine Learning Repository, Default of Credit Card Clients](https://archive.ics.uci.edu/dataset/350/default+of+credit+card+clients). This dataset concerns **existing cardholders**, not a representative sample of all applicants. Any future decision simulation must therefore be described as hypothetical and must acknowledge selection bias, fairness concerns and other limitations.

The original UCI dataset was created by **I-Cheng Yeh**: *Default of Credit Card Clients*, UCI Machine Learning Repository. Source: https://doi.org/10.24432/C55S3H. Licence: **CC BY 4.0** — https://creativecommons.org/licenses/by/4.0/.

`data/raw/credit_default_uci.csv` is included in this repository under that licence. It was retrieved with `ucimlrepo.fetch_ucirepo(id=350)`, assembled from customer IDs, features and target, converted to CSV, and given the descriptive column names defined in notebook 01. Customer rows and source values were retained; no cleaning or feature engineering was applied to this raw export. These conversion and column-name changes are modifications to the source representation. The UCI licence applies to the dataset; no endorsement by its creator is implied.

Other raw data, processed datasets (including `data/processed/credit_default_cleaned.csv` and `data/processed/credit_default_features.csv`) and generated tables remain excluded from Git version control.

## Known issues

- Day 3's full-dataset utilisation quartiles are **descriptive EDA only** and must not be reused as fitted transformations across validation/test folds.
- Day 4 performs an initial train-only preprocessing pipeline and validation evaluation, but **not** the final held-out test evaluation, model comparison, cross-validation tuning, subgroup stability/fairness analysis, or deployment.
- Day 4's validation results do not demonstrate generalisability to future borrowers or applicant populations. The model is an educational baseline, not a validated lending policy.
- The saved `.joblib` model and customer-level prediction/split files are generated only on the user's computer and are **not** tracked in Git.

See [the roadmap](docs/roadmap.md) for next steps.

## Updating the project

Keep editing the notebooks in Jupyter or VS Code, then commit and push your work:

```bash
git add notebooks/ README.md docs/ requirements.txt
git commit -m "Continue credit-risk analysis"
git push
```

You can also upload updated files through the GitHub web interface. Larger changes can use feature branches and pull requests.

**Status: In development.** An initial logistic-regression baseline is trained within Day 4, but no final test-set performance, production lending decision system or deployment is claimed.
