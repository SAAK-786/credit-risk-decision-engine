# Project roadmap

This is a living plan, not a list of finished deliverables.

## Stage 1 — Data audit

- [x] Retrieve the UCI credit default dataset (ID 350)
- [x] Inspect schema, missingness, duplicates, category codes and target balance
- [x] Create local data and output folders
- [x] Add descriptive Markdown to the audit notebook
- [x] Fix invalid-target diagnostic (valid labels are 0 and 1)
- [x] Fix the dimensional print label

## Stage 2 — Cleaning and EDA

- [x] Group undocumented education and marital-status codes into Unknown while retaining originals
- [x] Count recorded repayment delays and derive maximum recorded delay
- [x] Summarise default rates by repayment-status and credit-limit groups
- [x] Inspect demographic differences for data-quality and fairness considerations
- [ ] Refine plots, interpretation and reproducibility checks

## Stage 3 — Feature engineering

- [x] Create credit utilisation and balance-to-limit features
- [x] Create payment consistency and payment-to-bill ratios
- [x] Explore changes in balances and delays across months
- [x] Summarise engineered feature distributions, missingness and observed default associations
- [x] Export the feature-engineered dataset and summary outputs (within the Day 3 notebook)
- [x] Save `data/processed/credit_default_cleaned.csv` from Day 2 for Day 3
- [ ] Move reusable feature preparation into leak-free training-only pipelines (Stage 4)

## Stage 4 — Model training and evaluation

- [ ] Design stratified training, validation and held-out testing
- [ ] Train a simple baseline and suitable classifiers (e.g., logistic regression, Random Forest, XGBoost)
- [ ] Tune hyperparameters with cross-validation without leakage
- [ ] Evaluate ROC-AUC, PR-AUC, precision/recall and threshold trade-offs
- [ ] Evaluate calibration, stability and subgroup error patterns

## Stage 5 — Hypothetical decisions

- [ ] Define illustrative risk thresholds and explicit assumptions
- [ ] Evaluate cost-sensitive decisions and sensitivity to calibration
- [ ] Document fairness, selection bias, generalisability and other limitations
- [ ] Do not present simulation outputs as real lending recommendations
