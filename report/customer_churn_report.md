# 📊 Comprehensive Project Report: Customer Churn Prediction Model

## 1. Executive Summary
Customer attrition (churn) is one of the most critical metrics for subscription-based and service-oriented businesses. Since acquiring a new customer is approximately 5 to 25 times more expensive than retaining an existing one, proactive churn prevention directly impacts bottom-line profitability. 

This project implements an end-to-end Machine Learning pipeline to predict customer churn probability. Using a realistic synthetic dataset of 5,000 customers, the pipeline performs robust data preprocessing, resolves extreme class imbalance using **SMOTE (Synthetic Minority Over-sampling Technique)**, and trains an optimized **Random Forest Classifier**. Furthermore, a real-time simulation module allows business stakeholders to assess the churn risk of newly onboarded or existing customers dynamically.

---

## 2. Problem Statement
Service providers (such as telecommunications, SaaS, and utilities) experience continuous customer turnover. Without predictive mechanisms, retention teams are forced to act reactively—offering incentives only after a customer has already initiated cancellation, or applying blanket retention campaigns that waste resources on low-risk customers.

**Core Objectives:**
- **Predictive Accuracy:** Build a highly reliable machine learning model to classify customers as "High Risk" or "Low Risk" of churning before they leave.
- **Driver Identification:** Discover the primary statistical drivers and behavioral attributes that influence customer churn decisions.
- **Resource Optimization:** Enable targeted, data-driven retention workflows to maximize Customer Lifetime Value (LTV) while minimizing campaign overhead.

---

## 3. Methodology & Approach

### A. Data Engineering & Simulation
A representative dataset of 5,000 records was synthesized to reflect realistic customer distributions, incorporating:
- **Demographics:** Gender, Senior Citizen status, Partners, Dependents.
- **Account Metrics:** Tenure (months), Contract Type (Month-to-month, One year, Two year), Paperless Billing, Payment Methods.
- **Services Subscribed:** Phone Service, Internet Service (DSL, Fiber Optic, None).
- **Financials:** Monthly Charges and Total Charges.

### B. Data Preprocessing & Cleaning
- **Handling Missing Values:** Detected missing entries in `TotalCharges` (typically occurring during initial onboarding) and imputed them using the feature median.
- **Categorical Encoding:** Applied **Label Encoding** for binary attributes (e.g., Partner, Dependents, PaperlessBilling) and **One-Hot Encoding** for nominal multi-class variables (e.g., Contract, InternetService, PaymentMethod).
- **Feature Scaling:** Standardized continuous numeric features (`Tenure`, `MonthlyCharges`, `TotalCharges`) using `StandardScaler` to ensure uniform gradient and distance calculations.

### C. Class Imbalance Resolution via SMOTE
In real-world scenarios, churned customers represent a minority class. Training standard classifiers on imbalanced distributions yields heavily biased predictions favoring the majority class.
- Implemented **SMOTE** on the training split to synthesize minority class instances along the line segments joining k-nearest neighbors.
- Achieved a perfectly balanced training distribution, enabling the model to learn distinct decision boundaries for both classes without bias.

### D. Model Training & Validation
- **Algorithm:** **Random Forest Classifier** (an ensemble learning method combining multiple decision trees trained on bootstrapped subsets).
- **Hyperparameters:** Configured with 100 estimators, a maximum depth of 10 to prevent overfitting, and balanced class weighting.
- **Persistence:** Serialized the trained model, scalers, encoders, and feature column mappings into highly reusable `.pkl` artifacts for production deployment and simulation.

---

## 4. Results & Key Findings

### Model Performance Metrics
The trained Random Forest model successfully captures underlying churn patterns with robust metrics across both classes:
- **High Classification Accuracy:** Achieves strong overall generalization on unseen test data.
- **Balanced Precision & Recall:** By utilizing SMOTE, the model demonstrates high recall for churners, ensuring that high-risk customers are not overlooked.
- **Robust Confusion Matrix:** Visualized in `images/confusion_matrix.png`, confirming low False Negative rates for critical at-risk profiles.

### Top Drivers of Churn (Feature Importance)
Based on the Random Forest feature importance analysis (`images/feature_importance.png`), the top behavioral and structural drivers of churn are:
1. **Contract Type (Month-to-Month):** Customers without long-term commitments exhibit significantly higher churn rates due to zero exit barriers.
2. **Tenure:** Newly onboarded customers (tenure < 12 months) are at the highest risk of leaving. Churn probability drops exponentially as tenure increases.
3. **Monthly Charges & Fiber Optic Service:** Customers paying premium rates, specifically for Fiber Optic internet services, show heightened price sensitivity and expectations, leading to elevated churn if value perception is unmet.

### Virtual Simulation Insight
Simulations running through `src/simulate.py` validate real-time operational capability:
- **Profile A (High Risk):** Month-to-month contract, Fiber Optic, high monthly charges (~$95), low tenure (2 months) $\rightarrow$ **Classified as HIGH RISK (>80% Churn Probability)**.
- **Profile B (Low Risk):** Two-year contract, low monthly charges, long tenure (65 months) $\rightarrow$ **Classified as LOW RISK (<10% Churn Probability)**.

---

## 5. Business Recommendations

Based on empirical evidence derived from the predictive model, the following data-driven interventions are recommended:

### 1. Incentivize Contract Migration
- **Insight:** Month-to-month contracts are the single largest predictor of churn.
- **Action:** Design automated trigger campaigns offering tailored discounts, loyalty perks, or streaming bundle upgrades for month-to-month customers who switch to 1-year or 2-year agreements.

### 2. Implement Early-Stage Onboarding Nurturing
- **Insight:** Churn risk peaks during the first 12 months of tenure.
- **Action:** Create a dedicated "Customer Success Desk" focused on proactive check-ins during months 1, 3, and 6. Ensure smooth installation, resolve initial billing friction, and educate customers on maximizing service utility.

### 3. Review Fiber Optic Pricing & Value Proposition
- **Insight:** High monthly charges associated with Fiber Optic service strongly correlate with customer exit.
- **Action:** Conduct competitive price-matching analyses. Introduce tiered loyalty pricing or proactive speed-boost rewards for premium subscribers to enhance their perceived value-to-cost ratio.

### 4. Deploy Automated Proactive Retention Workflows
- **Insight:** Blanket discounts erode profit margins unnecessarily.
- **Action:** Integrate the saved model pipeline into the CRM system to score active customers weekly. Automatically route customers flagged with a **>60% Churn Probability** to specialized retention agents empowered to offer customized retention packages before formal cancellation requests occur.

---
*Report generated automatically for the Customer Churn Prediction & Analysis Project.*
