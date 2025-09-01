# FRAUD DETECTION SYSTEM 

# OVERVIEW 
```bash
This project implements a fraud detection system using a highly imbalanced financial transactions dataset. 
The goal is to build a machine learning pipeline that can identify fraudulent activities (mainly in TRANSFER and CASH_OUT transactions)
while balancing detection performance and false alarms.
Fraud detection is critical in financial systems because even small amounts of fraud can lead to large financial and reputational losses. 
This project demonstrates how data exploration, feature engineering, and machine learning can be applied to detect fraudulent transactions.
```
# KEY FEATURES 
```bash
 - Exploratory Data Analysis (EDA): Fraud ratio, transaction types, missing values, system rule (isFlaggedFraud) effectiveness.
 - Visualization: Fraud distribution, fraud vs non-fraud across transaction types.
 - Feature Engineering: Encoded transaction type, cleaned dataset, selected meaningful features.
 - Machine Learning Models: RandomForest and LightGBM classifiers.
 - Evaluation Metrics: Precision, Recall, F1-score, ROC-AUC, Confusion Matrix, Precision-Recall Curve.
 - Class Imbalance Handling: Subsampling + support for weighted models.
 - Business Insights: Fraud is highly concentrated in specific transaction types, existing rules are ineffective, ML improves coverage.
```

# SYSTEM ARCHITECTURE 

<img width="954" height="480" alt="diagram-export-9-2-2025-1_32_21-AM" src="https://github.com/user-attachments/assets/e42296b7-26be-4ff2-bba1-081415d4b6a3" />

# USAGE 
```bash
git clone <repo-url>
cd repo
```
# DATA INSIGHTS 

1. FRAUD VS NON-FRAUD TRANSACTION PER TYPE

   Fraud is concentrated in TRANSFER and CASH_OUT transactions.
   PAYMENT and CASH_IN transactions rarely exhibit fraud.
   This aligns with real-world fraud patterns: fraudsters often transfer or cash out stolen money rather than paying bills.

   <img width="867" height="638" alt="Screenshot 2025-09-02 013735" src="https://github.com/user-attachments/assets/04d6eeb2-d286-4bc7-a8bd-04efc2a55efc" />


2. FRAUD RATIO CHART

    Fraud accounts for only ~0.13% of all transactions — a highly imbalanced dataset.
   This imbalance is why naive models (e.g., predicting all as non-fraud) would show very high accuracy but fail in real detection.
   Precision, recall, and AUC are much more important metrics than accuracy for this use case.

   <img width="507" height="532" alt="Screenshot 2025-09-02 013645" src="https://github.com/user-attachments/assets/090f3fca-b4ff-497d-a42a-86da513c3149" />

3. TRANSACTION AMOUNT DISTRIBUTION

   Fraudulent transactions tend to occur at higher transaction amounts compared to non-fraud.
   After log transformation, fraud shows a distinct distribution peak at higher ranges.
   This suggests transaction amount is a strong predictor of fraud.

   <img width="868" height="585" alt="Screenshot 2025-09-02 013826" src="https://github.com/user-attachments/assets/5bc9c65b-4490-4db7-a169-6ea57eb1bc69" />

# MODEL APPLIED

1. LOGISTIC REGRESSION

    Strengths:
     - Very high recall (0.9830), meaning it detects almost all fraud cases.
     - Reliable baseline model with strong ROC-AUC (0.9777) and PR-AUC (0.9540).
    Weaknesses:
     - Low precision (0.6338) due to a large number of false positives (933).
     - In production, this would create significant customer friction (legitimate transactions frequently flagged as fraud).
       
2. RANDOM FOREST

    Strengths:
    - Excellent balance of precision (0.9994) and recall (0.9957).
    - Very few false positives (1) and false negatives (7).
    - Near-perfect F1 (0.9976) and AUC scores (ROC-AUC: 0.9998, PR-AUC: 0.9996).
   Weaknesses:
    - Computationally heavier than Logistic Regression.
    - While highly accurate, it still produces a small number of false alarms compared to LightGBM. 

3. LIGHTGBM

   Strengths:
    - Achieves perfect precision (1.0) with zero false positives, ensuring no legitimate transaction is wrongly flagged.
    - High recall (0.9872), catching the vast majority of fraud cases (1622 out of 1643).
    - Excellent F1 (0.9936) and nearly perfect AUC scores (ROC-AUC: 0.9997, PR-AUC: 0.9995).
    - Faster training and prediction compared to Random Forest, making it more scalable for real-time fraud detection.
   Weaknesses:
   - Slightly lower recall than Random Forest.
   - In highly risk-sensitive environments, missing frauds could be more costly than a few false alarms.
   
    <img width="858" height="168" alt="Screenshot 2025-09-02 020055" src="https://github.com/user-attachments/assets/22b1d3c9-344c-49ed-aac0-ec04f5fc8ad5" />

CONCLUSION 

Random Forest emerges as the most balanced performer, offering exceptionally high precision and recall with minimal trade-offs. However, LightGBM stands out as the most practical choice for production deployment due to its zero false positives, scalability, and ability to deliver a seamless customer experience. While it misses slightly more fraud cases than Random Forest, this trade-off can be mitigated through threshold tuning or ensemble techniques. Overall, LightGBM is the recommended model for a production-ready fraud detection system, with Random Forest serving as a strong alternative or a complementary model in an ensemble setup.

# RESULT AND PERFORMANCE 
 - Precision: 1.0000 → Every transaction flagged as fraud was truly fraud (zero false positives).
 - Recall: 0.9872 → Detected 1622 out of 1643 fraud cases, missing only 21.
 - F1 Score: 0.9936 → Excellent balance between precision and recall.
 - ROC-AUC: 0.9997 | PR-AUC: 0.9995 → Near-perfect ability to distinguish fraud from genuine transactions.

   <img width="202" height="161" alt="Screenshot 2025-09-02 020255" src="https://github.com/user-attachments/assets/8ac218f4-0824-4858-af3c-6f61b09ac58f" />

 - Confusion Matrix:
   - True Positives (TP): 1622
   - False Negatives (FN): 21
   - False Positives (FP): 0
   - True Negatives (TN): 102,170
   
   <img width="632" height="490" alt="Screenshot 2025-09-02 020237" src="https://github.com/user-attachments/assets/039bae30-9967-43b9-af6f-6448c80ed0f6" />

PERFORMANCE INSIGHTS
 - LightGBM delivers flawless precision, ensuring that no legitimate customer is wrongly flagged as fraud.
 - Recall is extremely high, with only a tiny fraction of fraud cases missed (21 out of 1,643).
 - Its speed, scalability, and memory efficiency make it highly suitable for real-time fraud detection systems.
 - Compared to Random Forest, it sacrifices a little recall but completely eliminates false positives — a critical advantage in customer-facing applications.
   <img width="1143" height="603" alt="Screenshot 2025-09-02 020119" src="https://github.com/user-attachments/assets/78e8acef-0e8e-45ac-8c6b-fece2932dc9c" />
   <img width="1111" height="730" alt="Screenshot 2025-09-02 020210" src="https://github.com/user-attachments/assets/10dd47e1-71c0-4185-affa-1902cb0a40ad" />

# CONCLUSION
LightGBM achieves production-grade performance, combining perfect precision, very high recall, and near-perfect AUC scores, making it an excellent choice for a fraud detection system that prioritizes both accuracy and user experience.

# BUSINESS INSIGHTS 
 - Zero false positives ensure smooth customer experience, reducing complaints and manual review costs.
 - High fraud detection (98.7% recall) prevents financial losses with minimal missed cases.
 - Scalable and fast, making LightGBM suitable for real-time fraud detection.
 - Supports regulatory compliance by balancing fraud prevention with low customer friction.

# FUTURE SCOPE 
 - Threshold tuning to improve recall while maintaining precision.
 - Hybrid models/ensembles for stronger fraud defense.
 - Explainability tools (SHAP/feature importance) for compliance and trust.
 - Continuous learning to adapt to new fraud patterns.
 - Big data integration for large-scale, real-time monitoring across industries.
