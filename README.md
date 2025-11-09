## **Part 1: Short Answer Questions (30 points)**

### **1. Problem Definition (6 points)**

**Hypothetical AI Problem:**
“Predicting equipment failure in manufacturing plants.”

**Objectives:**

1. Minimize unplanned equipment downtime through early failure prediction.
2. Reduce maintenance costs by shifting from reactive to predictive maintenance.
3. Improve operational efficiency and production throughput.

**Stakeholders:**

* **Plant Operations Manager** — wants fewer breakdowns and higher uptime.
* **Maintenance Engineers** — need actionable insights to schedule repairs efficiently.

**KPI:**

* **Mean Time Between Failures (MTBF)** — measures the average time between equipment breakdowns. Improvement indicates predictive system success.

---

### **2. Data Collection & Preprocessing (8 points)**

**Data Sources:**

1. IoT sensor data from machinery (temperature, vibration, pressure logs).
2. Maintenance logs and production system records.

**Potential Bias:**

* **Operational bias:** Sensors on newer machines may produce higher-quality data, causing the model to underperform on older or less-instrumented machines.

**Preprocessing Steps:**

1. **Handling Missing Data:** Impute missing sensor readings using interpolation or k-NN imputation.
2. **Normalization:** Scale sensor features (e.g., MinMaxScaler) for neural models or distance-based algorithms.
3. **Feature Engineering:** Create rolling averages, gradients, and thresholds from time-series sensor data.

---

### **3. Model Development (8 points)**

**Model Choice:**

* **Random Forest Classifier** — handles high-dimensional, mixed-type data and captures nonlinear relationships with minimal tuning. It also provides feature importance for interpretability.

**Data Splitting:**

* **70% training**, **15% validation**, **15% test** — stratified to preserve class balance (e.g., failure vs non-failure).

**Hyperparameters to Tune:**

1. **n_estimators** — controls the number of trees; affects bias-variance trade-off.
2. **max_depth** — prevents overfitting by limiting tree depth.

---

### **4. Evaluation & Deployment (8 points)**

**Evaluation Metrics:**

1. **F1-score:** Balances precision and recall — useful in imbalanced datasets (failures are rare).
2. **ROC-AUC:** Measures ranking ability across thresholds; provides an overall sense of separability.

**Concept Drift:**

* Concept drift occurs when data distributions change over time (e.g., new machines, updated sensors).
  **Monitoring method:** Track model accuracy and prediction probability distributions over time using a drift detector like DDM (Drift Detection Method).

**Deployment Challenge:**

* **Scalability:** Handling real-time inference from thousands of sensors per second; requires distributed data processing (e.g., Apache Kafka + Spark Streaming).

---

## **Part 2: Case Study Application (40 points)**

### **Scenario:**

A hospital wants an AI system to **predict patient readmission risk within 30 days of discharge.**

---

### **1. Problem Scope (5 points)**

**Problem Definition:**
Develop an AI model that predicts the likelihood of a patient being readmitted within 30 days post-discharge to improve care quality and reduce healthcare costs.

**Objectives:**

1. Identify high-risk patients early for post-discharge interventions.
2. Reduce 30-day readmission rates.
3. Support clinicians in care planning.

**Stakeholders:**

* **Hospital Administration:** Concerned with readmission penalties and patient satisfaction.
* **Clinicians/Nurses:** Need actionable insights to allocate follow-up resources.

---

### **2. Data Strategy (10 points)**

**Data Sources:**

1. **Electronic Health Records (EHR):** Admission history, diagnoses (ICD codes), lab results, prescriptions.
2. **Demographic Data:** Age, gender, socioeconomic status, and insurance type.

**Ethical Concerns:**

1. **Patient Privacy:** Data misuse or re-identification risks.
2. **Algorithmic Bias:** Model could underperform for underrepresented demographic groups.

**Preprocessing & Feature Engineering Pipeline:**

1. **Data Cleaning:** Remove duplicates, correct data entry errors.
2. **Handling Missing Values:** Median imputation for numerical labs; mode imputation for categorical variables.
3. **Encoding:** One-hot encoding for categorical variables like diagnosis type.
4. **Feature Engineering:**

   * Calculate “number of prior admissions.”
   * Compute “average length of stay.”
   * Create “comorbidity index” from ICD codes.
5. **Normalization:** Min-Max scaling for continuous variables.

---

### **3. Model Development (10 points)**

**Model Choice:**

* **Gradient Boosted Trees (XGBoost):** Handles tabular healthcare data efficiently, manages missing values internally, provides explainability (feature importance), and performs well on structured datasets.

**Hypothetical Confusion Matrix:**

|                       | Predicted Readmit | Predicted No Readmit |
| --------------------- | ----------------: | -------------------: |
| **Actual Readmit**    |           80 (TP) |              20 (FN) |
| **Actual No Readmit** |           30 (FP) |             870 (TN) |

**Precision & Recall:**

* Precision = 80 / (80 + 30) = **0.727**
* Recall = 80 / (80 + 20) = **0.8**

Interpretation: The model retrieves 80% of actual readmissions and is correct ~73% of the time when predicting readmission.

---

### **4. Deployment (10 points)**

**Integration Steps:**

1. **Containerize the Model:** Using Docker for environment consistency.
2. **Expose REST API Endpoint:** Allow EHR systems to query predictions in real-time.
3. **Integrate into Clinical Workflow:** Embed risk scores into the patient dashboard.
4. **Monitor Model Drift:** Re-evaluate model monthly using new patient data.

**Regulatory Compliance:**

* **HIPAA (Health Insurance Portability and Accountability Act):**

  * Encrypt patient data at rest and in transit (TLS, AES-256).
  * Limit access to authorized hospital personnel.
  * Conduct regular security audits and maintain audit logs.

---

### **5. Optimization (5 points)**

**Overfitting Mitigation Strategy:**

* **Regularization:** Use L2 regularization in XGBoost (parameter `lambda`) to penalize overly complex models, ensuring generalization on unseen patient data.

---

## **Part 3: Critical Thinking (20 points)**

### **1. Ethics & Bias (10 points)**

**Impact of Biased Data:**

* If the model is trained predominantly on data from urban, insured patients, it might misclassify rural or low-income patients as “low-risk.”
  Result: under-served populations could receive inadequate follow-up care, worsening health disparities.

**Bias Mitigation Strategy:**

* **Rebalancing & Fairness Auditing:**

  * Reweight training samples across demographics.
  * Use fairness metrics like equal opportunity difference to monitor model equity.

---

### **2. Trade-offs (10 points)**

**Interpretability vs. Accuracy:**

* **Complex models (e.g., deep neural nets)** may yield higher accuracy but low interpretability — dangerous in healthcare where explainability is vital for clinical trust.
* **Simpler models (e.g., logistic regression or decision trees)** are interpretable and can justify predictions to doctors, even if slightly less accurate.

**Computational Constraints:**

* With limited hardware, the hospital may prefer **lightweight models** (e.g., Logistic Regression or LightGBM) that run efficiently on-premise instead of GPU-heavy architectures.

---

## **Conclusion**

This structured framework covers the **complete AI lifecycle** — from problem definition and data strategy to ethical reflection and deployment. The hospital readmission case demonstrates how technical, ethical, and operational aspects intertwine in real-world AI for healthcare.

---
