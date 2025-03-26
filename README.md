# fraudulent-detection
model for predicting fraudulent transactions for a financial company

Dataset ="https://drive.google.com/file/d/1mn5Co145391f57G12DwRmPf3igioMOsF/view?usp=drive_link"

# Fraud Detection Project Report

## **1. Data Cleaning: Handling Missing Values, Outliers & Multi-Collinearity**
### **Data Cleaning Steps Taken:**
- **Missing Values:** Missing values were replaced with appropriate statistics (e.g., median for numerical features) or set to zero if data was unavailable.
- **Outliers:** Box plots and z-scores were used to detect and cap extreme outliers, especially in the `amount` and balance-related fields.
- **Multi-Collinearity:** A correlation heatmap was used to identify redundant features. Highly correlated variables like `oldbalanceOrg`, `newbalanceOrig`, `oldbalanceDest`, and `newbalanceDest` were removed to avoid redundancy and improve model performance.

---

## **2. Fraud Detection Model Description**
The fraud detection model was built using **XGBoost**, a powerful tree-based machine learning algorithm. Key steps included:
- **SMOTE (Synthetic Minority Over-sampling Technique)** was applied to handle class imbalance by generating synthetic fraud samples.
- **Feature Engineering** was performed to create new variables, such as `orig_balance_change_rate` and `dest_balance_change_rate`, which capture transaction patterns.
- **Threshold Optimization** was applied to select the best fraud detection threshold, balancing precision and recall.
- **Evaluation Metrics:** The model was evaluated using precision, recall, F1-score, AUC-ROC, and confusion matrix analysis.

---

## **3. Variable Selection for the Model**
The final set of selected variables was determined based on:
- **Feature Importance from XGBoost:** The most predictive features included `type`, `orig_balance_change_rate`, and `amount`.
- **Domain Knowledge:** Features that logically impact fraud, such as transaction amount and balance fluctuations, were prioritized.
- **Correlation Analysis:** Features with high correlation were removed to avoid redundancy.

---

## **4. Model Performance Demonstration**
**Final Model Metrics:**
- **Accuracy:** 100%
- **Precision (Fraud Cases):** 95%
- **Recall (Fraud Cases):** 87% (Improved after applying SMOTE and threshold tuning)
- **AUC-ROC Score:** 0.94 (indicating excellent fraud detection capability)
- **Confusion Matrix:** False negatives reduced from 251 to 207, improving recall.

---

## **5. Key Factors That Predict Fraudulent Customers**
```python
import pandas as pd
feature_importance = pd.Series({
    'type': 0.447582,
    'orig_balance_change_rate': 0.196948,
    'amount': 0.155388,
    'dest_balance_change_rate': 0.065695,
    'step': 0.059606,
    'deltaDest': 0.038661,
    'deltaOrig': 0.036120
})
feature_importance.sort_values(ascending=False)
```

| **Feature**                      | **Importance** | **Interpretation** |
|---------------------------------|---------------|----------------|
| `type`                           | **44.8%**  | Certain transaction types (e.g., CASH-OUT, TRANSFER) are more fraud-prone. |
| `orig_balance_change_rate`       | **19.7%**  | Fraudsters tend to empty accounts quickly. |
| `amount`                         | **15.5%**  | Higher transaction amounts are more likely fraudulent. |
| `dest_balance_change_rate`       | **6.6%**   | Fraudsters may transfer money to an empty account and quickly withdraw. |
| `step`                           | **5.9%**   | Fraud often occurs in bursts. |

---

## **6. Do These Factors Make Sense?**
Yes, the key fraud indicators align with real-world fraud patterns:
- **Fraudulent transactions are often high-value and involve rapid balance depletion.**
- **Certain transaction types (e.g., CASH-OUT, TRANSFER) are frequently used in fraud schemes.**
- **Fraudsters often create mule accounts to receive illicit funds and withdraw immediately.**

---

## **7. Fraud Prevention Strategies for the Company**
- **Real-time Monitoring:** Use machine learning models for real-time fraud detection.
- **Transaction Limits:** Place stricter limits on high-risk transaction types (e.g., high-value CASH-OUT transactions).
- **Behavioral Analytics:** Monitor sudden spikes in transaction frequency or amount.
- **Multi-Factor Authentication (MFA):** Require OTPs for high-risk transactions.
- **Automated Fraud Alerts:** Flag unusual activity and request manual verification for suspicious transactions.

---

## **8. Evaluating the Effectiveness of Implemented Actions**
To measure success:
- **Compare Fraud Rates Before & After Implementation**: Analyze whether fraud rates decline post-deployment.
- **Monitor False Positives & Negatives:** Ensure the model maintains a low false positive rate while catching more fraud cases.
- **Customer Feedback & Complaints:** Track reports of unauthorized transactions to assess model effectiveness.
- **Periodic Model Retraining:** Update the fraud detection model as new fraud patterns emerge.

---

## **Final Conclusion**
The fraud detection model successfully identified fraudulent transactions with high precision and recall. The use of **feature engineering, SMOTE balancing, and threshold optimization** significantly improved detection performance. Future improvements can include **deep learning approaches, real-time analytics, and adaptive fraud prevention techniques**. 🚀


