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

**Day 4: Initial logistic-regression baseline completed; further model development remains.**

- [x] Define an 11-feature baseline and exclude customer IDs and demographic fields
- [x] Create stratified 60% training, 20% validation and 20% held-out test splits
- [x] Fit median imputation, standardisation and logistic regression using training data only
- [x] Evaluate validation ROC-AUC, average precision, Brier score, accuracy, precision, recall and F1
- [x] Produce a validation confusion matrix, ROC/precision–recall curves and a calibration plot
- [x] Interpret standardised coefficients and odds ratios, noting non-causality and correlated predictors
- [x] Explore validation classification-threshold trade-offs without selecting a final threshold
- [x] Save the fitted pipeline and validation outputs locally (excluded from Git)
- [ ] Compare against additional baseline/classifier approaches (e.g., Random Forest, gradient boosting)
- [ ] Tune and compare candidates with appropriate cross-validation while preventing leakage
- [ ] Investigate stability, subgroup performance and possible fairness concerns
- [ ] Select and freeze the modelling approach, then evaluate once on the held-out test set
- [ ] Document final model limitations and the suitability of any risk-decision use case

Day 4 reports validation **ROC-AUC 0.751**, **average precision 0.486**, and **Brier score 0.146**. These are notebook-reported validation results, not final test results.

## Stage 5 — Hypothetical decisions

- [ ] Define illustrative risk thresholds and explicit assumptions
- [ ] Evaluate cost-sensitive decisions and sensitivity to calibration
- [ ] Document fairness, selection bias, generalisability and other limitations
- [ ] Do not present simulation outputs as real lending recommendations
