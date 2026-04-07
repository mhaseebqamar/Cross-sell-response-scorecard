# Credit Risk Scorecard & Automated Validation Report

📌 Project Overview

This project implements a professional-grade Credit Scoring model designed for financial institutions. It automates the entire lifecycle of model development—from feature selection to a comprehensive 13-sheet validation report—ensuring both high predictive power and regulatory transparency.

🛠️ Technical Stack

Language: Python (Pandas, NumPy, Scikit-Learn)

Statistical Methods: Logistic Regression, Weight of Evidence (WoE), Information Value (IV).

Validation Metrics: Gini Coefficient, PSI (Population Stability Index), KS Statistic, Wald Test.

🚀 Key Features & Performance

High Predictive Power: Achieved a Gini Coefficient of 0.811 (Train) / 0.815 (Test), demonstrating strong model stability and zero overfitting.

Variable Importance: Identified act_ccss_n_loans_hist (45% importance) and act_ccss_maxdue (18% importance) as the primary drivers of credit risk.

Automated Reporting: Generates a structured multi-sheet report including:

Calibration: Aligning Expected PD (Probability of Default) with Actual Bad Rates.

Stability: PSI checks to ensure demographic shifts don't degrade model performance.

Scorecard: A points-based system for real-time loan approval decisions.

📈 Financial Impact

The model provides a clear "Logit" to "Score" transformation, allowing stakeholders to set specific "Cut-off" scores to balance loan volume versus default risk.
