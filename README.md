# Fraud Detection Model – Financial Transactions

## Project Overview

This project develops a machine learning model to detect fraudulent financial transactions.
The goal is to analyze transaction behavior, identify key fraud indicators, and build a predictive model that can proactively detect fraudulent activity.

The dataset contains **6.3 million financial transactions** simulated over **30 days**, with each row representing a transaction.

The project includes:

* Data cleaning and preprocessing
* Exploratory data analysis
* Feature engineering
* Fraud detection model development
* Model evaluation
* Business insights and fraud prevention strategy

---

# Dataset Description

The dataset contains the following variables:

| Feature        | Description                               |
| -------------- | ----------------------------------------- |
| step           | Time step (1 step = 1 hour)               |
| type           | Type of transaction                       |
| amount         | Transaction amount                        |
| nameOrig       | Sender account                            |
| oldbalanceOrg  | Sender balance before transaction         |
| newbalanceOrig | Sender balance after transaction          |
| nameDest       | Recipient account                         |
| oldbalanceDest | Recipient balance before transaction      |
| newbalanceDest | Recipient balance after transaction       |
| isFraud        | Indicates fraudulent transaction          |
| isFlaggedFraud | Transactions flagged by rule-based system |

The dataset contains **6,362,620 rows and 11 columns**.

---

# 1. Data Cleaning

### Missing Values

No missing values were found in the dataset.

### Duplicates

Duplicate transactions were checked and removed if present.

### Outliers

Large transaction values were observed. However, outliers were not removed because fraudulent transactions often involve unusually large amounts.

### Multicollinearity

A correlation heatmap was used to analyze multicollinearity between variables.

High correlations were observed between balance-related variables such as:

* oldbalanceOrg
* newbalanceOrig
* oldbalanceDest
* newbalanceDest

This occurs because balances before and after a transaction are mathematically related.

Since the model used is **Random Forest**, which is robust to multicollinearity, no variables were removed.

---

# 2. Fraud Detection Model

A **Random Forest Classifier** was used for fraud detection.

Random Forest is an ensemble machine learning algorithm that builds multiple decision trees and aggregates their predictions. It is well suited for large tabular datasets and can capture complex nonlinear relationships between features.

The model was trained using:

* 80% training data
* 20% testing data

To address severe class imbalance, **class weighting was applied** during model training.

---

# 3. Variable Selection

Variables were selected based on their relevance to financial transaction behavior.

Key variables include:

* Transaction amount
* Sender account balance
* Recipient account balance
* Transaction type
* Balance differences before and after transactions

Two engineered features were created:

* `balance_diff_orig = oldbalanceOrg - newbalanceOrig`
* `balance_diff_dest = newbalanceDest - oldbalanceDest`

These features help identify abnormal balance changes that often occur during fraud.

---

# 4. Model Performance

The model performance was evaluated using:

* Precision
* Recall
* F1-score
* Confusion matrix

### Classification Report

| Class  | Precision | Recall | F1 Score |
| ------ | --------- | ------ | -------- |
| Normal | 1.00      | 1.00   | 1.00     |
| Fraud  | 0.98      | 0.80   | 0.88     |

### Interpretation

* **Precision (0.98)** indicates that when the model predicts fraud, it is correct most of the time.
* **Recall (0.80)** indicates that the model detects approximately 80% of fraudulent transactions.

This trade-off is common in fraud detection because fraud is extremely rare in the dataset.

---

# 5. Key Factors Predicting Fraud

Feature importance analysis identified the following key predictors:

1. Balance difference of sender account
2. Sender account balance before transaction
3. Transaction amount
4. Sender balance after transaction
5. Balance difference of recipient account
6. Transfer transaction type

These factors indicate abnormal transaction patterns commonly associated with fraud.

---

# 6. Do These Factors Make Sense?

Yes.

Fraudulent behavior typically follows this pattern:

1. A fraudster gains access to a legitimate account
2. Funds are transferred to another account
3. The account is emptied quickly

The model correctly identifies:

* Large transfers
* Rapid balance changes
* Transfer-type transactions

These patterns align with real-world financial fraud scenarios.

---

# 7. Fraud Prevention Strategy

Based on the model insights, the following fraud prevention measures are recommended:

### Real-time transaction monitoring

Deploy the model in a real-time transaction scoring system.

### Transaction thresholds

Automatically flag transactions exceeding certain amounts.

### Behavioral anomaly detection

Detect unusual account activity such as rapid transfers or account draining.

### Multi-factor authentication

Require additional authentication for high-value transfers.

### Transaction velocity monitoring

Limit the number of transfers within short time intervals.

---

# 8. Measuring Success of Fraud Prevention

After implementing fraud prevention measures, success can be evaluated using the following metrics:

* Reduction in total fraud losses
* Fraud detection rate
* False positive rate
* Customer complaints related to blocked transactions
* Model performance over time

Monitoring these metrics helps ensure that the fraud detection system remains effective as fraud patterns evolve.

---

# Technologies Used

* Python
* Pandas
* NumPy
* Scikit-learn
* Matplotlib
* Seaborn

---

# Conclusion

This project demonstrates how machine learning can be used to detect fraudulent financial transactions. The Random Forest model successfully identifies key fraud patterns based on transaction amounts, account balances, and transaction types.

While the model performs well, real-world fraud detection systems must continuously adapt to evolving fraud strategies and incorporate additional behavioral and contextual data.

Machine learning models like this can significantly reduce financial fraud when integrated into real-time monitoring systems.
