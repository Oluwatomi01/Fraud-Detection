# Fraud Detection

##  Project Overview

Fraudulent transactions are rare but **highly costly**. The goal of this project was to **build a machine learning model to detect fraud** in financial transactions using the **PaySim dataset**, which simulates real-world mobile money operations.

This project walks through **everything from data exploration to model selection**, emphasizing **handling class imbalance** and **business-driven threshold tuning**.

---

## Dataset

* **Transactions:** 1.0 million+
* **Fraudulent cases:** ~0.13%
* **Features:**

  * `type` → transaction type (TRANSFER, CASH_OUT, etc.)
  * `amount` → transaction amount
  * `balance_diff_orig` / `balance_diff_dest` → difference in account balances before and after transaction
  * Other balance and step features

**Key challenge:** Fraud is extremely rare, making **class imbalance** the core problem.

---

##  Exploratory Data Analysis

* Fraud mainly happens in **TRANSFER** and **CASH_OUT** transactions.
* Fraud transactions tend to have **larger amounts** than normal transactions.
* The dataset is **heavily imbalanced**: ~0.13% of transactions are fraudulent.

**Visualization examples:**

* Fraud count by transaction type
* Transaction amount distribution (fraud vs non-fraud)

---

##  Feature Engineering

* Dropped ID columns (`nameOrig`, `nameDest`)
* Created `balance_diff_orig` and `balance_diff_dest` to highlight unusual money movement
* One-hot encoded `type`
* Scaled numerical features

**Top predictive features:**

1. `balance_diff_orig`
2. `balance_diff_dest`
3. `newbalanceDest`
4. `amount`
5. `type_TRANSFER` & `type_CASH_OUT`

---

##  Modeling Approach

### Baseline

* **Logistic Regression**
* ROC-AUC = 0.986, Fraud Recall = 0.47
* Simple, interpretable but missed many fraud cases

### Handling Class Imbalance

* Added **class weights** to penalize misclassifying fraud
* Fraud Recall improved to 0.67

### Advanced Models

* **Random Forest** (best-performing)

  * ROC-AUC = 0.997
  * Recall = 0.81
  * Precision = 0.97
* **Isolation Forest** (anomaly detection)

  * Good at spotting unusual transactions, but less precise

---

##  Threshold Tuning

* Adjusted the classification threshold to optimize fraud detection:

| Threshold | Recall | Precision | F1-Score |
| --------- | ------ | --------- | -------- |
| 0.2       | 0.91   | 0.86      | 0.88     |
| 0.3       | 0.87   | 0.91      | 0.89     |
| 0.4       | 0.84   | 0.95      | 0.89     |
| 0.5       | 0.81   | 0.97      | 0.89     |

**Recommendation:** Threshold 0.2–0.3 balances **catching fraud** while controlling false alarms.

**Graph:** Threshold vs Recall/Precision/F1 visually shows trade-offs.

---

##  Final Model Selection

* **Model:** Random Forest with class weighting
* **Threshold:** 0.2–0.3 for business-aligned fraud detection
* **Why:**

  * High recall (catch most fraud)
  * High precision (few false positives)
  * Captures non-linear relationships

**Feature importance plot** clearly shows which features the model relied on.


##  Key Takeaways

* Fraud detection requires **handling severe class imbalance**.
* **Feature engineering** (balance differences, transaction type) is crucial.
* **Threshold tuning** aligns model decisions with business needs.
* Random Forest is **highly effective**, but anomaly detection can complement it.


## Next Steps / Extensions

* Deploy model for **real-time fraud monitoring**
* Incorporate **temporal features** for early fraud detection
* Experiment with **ensemble models** for even better performance
* Regularly **retrain model** as fraud patterns evolve

