# Cross-Sell Response Scorecard: End-to-End Scorecard Development in Python

An individual MSc project (SGH Warsaw School of Economics, MSc Advanced Analytics & Big Data, 2025/26) that builds a points-based scorecard with the standard banking scorecard workflow: supervised binning, log-odds transformation, univariate screening (Gini, Information Value, PSI), multivariate selection, logistic regression, points scaling, calibration, stability checks and automated Excel / SAS outputs.

The model predicts **whether an existing consumer-credit customer responds to a cross-sell loan offer** (target `cross_response`). It is a response (propensity) model built with credit-scoring methodology, not a probability-of-default model.

## Project context

The dataset was provided by the course professor as a SAS file (`abt_app.sas7bdat`): an anonymised teaching dataset of consumer loan applications with application data, account behaviour and loan-history variables. The goal was to implement the full scorecard development and validation process step by step in Python, including the reporting outputs a model-validation team would expect.

## Data

* Filter: product `css`, accepted applications (`decision = A`), periods January 1975 – December 1987
* **11,437 applications**, overall response rate **37.3%**
* 57 candidate variables (50 numeric, 7 categorical) from application (`app_*`) and account-behaviour (`act_*`) data
* Random 60/40 split: 6,862 training and 4,575 test records

## Methodology

1. **Supervised binning**: numeric variables binned with a decision tree (max 4 bins, min 3% of records per bin, separate missing bin); categorical variables grouped by response rate with agglomerative clustering (max 4 groups, rare levels pooled into `<OTHERS>`)
2. **Log-odds transformation**: each bin replaced by its log-odds of response (WoE-style encoding)
3. **Univariate screening**: Gini (train and test), Information Value and PSI for every variable. Kept variables with Gini > 0.05, relative Gini drop train→test < 20% and PSI < 0.1, which left **46 variables**
4. **Variable clustering** (VarClusHi) to examine redundancy among candidates
5. **Multivariate selection**: RFE ranking with logistic regression; the top 12 variables were combined into every 6-variable model (924 models), each fitted with `statsmodels` and checked for coefficient signs, p-values, VIF, condition index, maximum pairwise correlation, and train/test Gini. **290 models** passed the strict rules (no negative coefficients, p ≤ 0.01, VIF ≤ 3), and the one with the highest test Gini was chosen
6. **Scorecard scaling**: coefficients converted to points (20 points to double the odds, base 300)
7. **Calibration**: score mapped to a response probability with a one-variable logistic regression, checked overall, by year and by score decile
8. **Stability**: Gini by year, score PSI between train and test, segment reports over time
9. **Explainability**: SHAP decomposition of each customer's points
10. **Automated outputs**: Excel reports (variable report, multi-sheet model report, segment report, variable clustering, Gini curve data), a scored-population CSV, charts, and generated **SAS scoring code** (`scoring_code.sas`) that applies the scorecard in SAS SQL

## Results

**Final model (6 variables)**: number of current `css` loans, maximum days past due on `css` loans, number of historical `css` loans, number of children, job code, and arrears days in the last 3 months.

| Measure | Train | Test |
|---|---|---|
| Gini | 0.811 | 0.816 |
| AUC | 0.906 | 0.908 |

* All coefficients positive and significant (p < 0.0001); maximum VIF 2.45
* **Stable discrimination over time**: Gini by year ranged from 0.772 (1981) to 0.837 (1975); Gini of the final points score on all data was 0.813
* **Stable score distribution**: PSI between train and test scores was 0.001
* **Calibration**: by construction the average predicted response rate equals the actual 37.3%. By year, the model under-predicted in the early years (by up to 5.3 percentage points in 1976) and over-predicted later (by up to 3.5 points in 1987), so calibration drifts over time. By score decile, the largest gap was 14.4 points in the middle of the score range
* **Biggest point contributors (SHAP)**: number of historical `css` loans and maximum days past due

## Tech stack

* Python, Jupyter Notebook
* pandas, NumPy
* scikit-learn: decision-tree binning, agglomerative clustering, RFE, ROC/AUC
* statsmodels: logistic regression, Wald tests, VIF
* varclushi: variable clustering
* SHAP
* Matplotlib, XlsxWriter, openpyxl
* SAS (generated scoring code; data read from `.sas7bdat`)

## Project structure

```
Credit-risk-scorecard-automation/
├── ASB_step_by_step.ipynb   # Full pipeline: data → binning → screening → selection → scorecard → calibration → reports
└── README.md
```

The dataset was provided for the course and is not included in this repository. The Gini-curve export also needs an Excel template (`gini_curves_template.xlsx`) from the course materials.

## Honest limitations

* **Response model, credit-risk template.** Parts of the reporting reuse a default-risk template: the target is renamed `target_bad`, the calibrated probability is labelled `PD`, and the cut-off section computes profit with an 18% APR and 55% LGD, treating responders as losses. For a cross-sell target, those profit and "PD" figures have no real business meaning.
* **The KS, gains and lift figures in the model report are not valid.** In that step the target column is not found, so the code falls back to the wrong column (KS shows 0.0002). The Gini and AUC values above are calculated correctly.
* **Random split, not out-of-time.** Train and test cover the same years, which is easier than validating on a later period.
* **Champion chosen on test Gini.** The test set was used to pick the final model, so the test Gini is slightly optimistic.
* **Very high Information Values** (around 1.5–1.9 for the strongest variables). In practice values this high would prompt a check that those variables are measured before the cross-sell offer, to rule out leakage.
* **SHAP on points**: the explanation decomposes the scorecard points around their average rather than adding independent insight into the model.
* Teaching dataset; the model has not been used on real customers.
