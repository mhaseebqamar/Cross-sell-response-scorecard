# Credit Risk Scorecard & Automated Validation Framework
### End-to-End Financial Risk Modeling & Regulatory Reporting

## 📌 Project Overview
This project implements a professional-grade **Credit Scoring Engine** designed for institutional risk assessment. It automates the entire model lifecycle—from Weight of Evidence (WoE) transformation to the generation of a **13-sheet Automated Validation Report**. 

The framework is built to satisfy both predictive requirements and regulatory transparency standards (Basel III/IFRS 9), providing a clear path from complex statistical "Logits" to a simplified, points-based Scorecard for real-time decisioning.

---

## 🛠️ Technical Stack & Statistical Methodology
### Modeling Pipeline:
* **Feature Engineering:** Implemented **Weight of Evidence (WoE)** and **Information Value (IV)** to handle non-linear relationships and rank variable predictive power.
* **Algorithm:** Logistic Regression (The industry standard for scorecard interpretability and regulatory compliance).
* **Score Transformation:** Developed the "Logit-to-Score" logic with calibrated scaling (PDO, Points to Double the Odds).

### Validation Metrics (The 13-Sheet Report):
* **Discrimination:** Gini Coefficient, KS Statistic, and Wald Test.
* **Calibration:** Alignment of Expected PD (Probability of Default) vs. Actual Bad Rates across deciles.
* **Stability:** **PSI (Population Stability Index)** and CSI (Characteristic Stability Index) to monitor demographic shifts.

---

## 🚀 Key Performance Results
The model demonstrates exceptional stability and zero overfitting across training and testing samples.

* **High Predictive Power:** * **Gini Coefficient:** 0.811 (Train) / **0.815 (Test)**
    * **KS Statistic:** Consistent performance indicating strong separation between "Good" and "Bad" borrowers.
* **Variable Drivers:**
    * `act_ccss_n_loans_hist` (45% IV): Historical loan count identified as the primary risk driver.
    * `act_ccss_maxdue` (18% IV): Maximum delinquency duration as a secondary critical factor.

---

## 📈 Financial & Business Impact
1.  **Risk-Adjusted Decisioning:** Provides a standardized "Cut-off" framework, allowing stakeholders to balance loan approval volume against target default rates.
2.  **Regulatory Readiness:** The automated 13-sheet validation report ensures the model is "audit-ready," covering all necessary documentation for internal and external risk committees.
3.  **Real-Time Scorecard:** Translates coefficients into an easy-to-use points system, enabling instant loan processing without manual re-calculation.

---

## 📂 Repository Structure
* `Credit_Risk_Scorecard.ipynb`: Core Python implementation (WoE, Logistic Regression, Scoring).
* `Validation_Report_Generator.py`: Script to automate the export of the 13-sheet performance diagnostic.
* `reports/`: Sample output of the Automated Validation Report (Excel format).

---

## 💻 Tech Stack
* **Language:** Python
* **Libraries:** Scikit-Learn, Pandas, NumPy, Matplotlib
* **Methods:** Logistic Regression, WoE/IV Transformation, PSI Analysis
