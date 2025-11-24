# Home Credit Default Risk: Predicting Loan Defaults for Underbanked Populations

## Project Overview

Built predictive models to help Home Credit assess loan default risk for people with little to no credit history. The goal is to improve default prediction while maintaining financial inclusion for underbanked populations.

**Key Result**: Random Forest model achieved **63% sensitivity** in detecting defaults vs. 0% for baseline models, enabling Home Credit to prevent significant loan losses while still approving creditworthy applicants.


## Business Problem

Home Credit provides consumer loans to people traditionally underserved by banks—those without traditional credit scores. Without credit bureau data, they struggle to predict which applicants will default, creating two risks:

- **Approve bad loans** → Defaults damage profitability
- **Reject good applicants** → Miss revenue and fail social mission

**Target Variable**: Binary classification (91.9% repayment, 8.1% default) due to highly imbalanced data

**Challenge**: Build a model that maximizes default detection (sensitivity) while maintaining reasonable accuracy to avoid rejecting too many good customers.

---

## Data Preparation

- **Dataset**: 307,511 loan applications with 220+ variables across 7 relational tables
- **Feature Selection**: Focused on 19 high-signal variables (income, employment, assets, credit history, demographics)
- **Feature Engineering**: Aggregated bureau credit data (average days since credit application - frequent applications signal risk)
- **Class Imbalance**: Applied downsampling to balance the highly skewed default rate (8.1%)

---

## Models Developed

### Logistic Regression
- **AUC**: 0.666
- **Sensitivity**: ~0%
- **Result**: Too simple; became majority classifier

### Random Forest (Selected Model)
- **AUC**: 0.719
- **Sensitivity**: 0.63
- **Result**: Best default detection

### Neural Networks (Various Architectures)
- **AUC**: 0.651-0.695
- **Sensitivity**: 0.16-0.34
- **Result**: Slow to train, poor sensitivity

### Why Random Forest?

- **63% sensitivity**: Catches most potential defaults before approval
- **71% specificity**: Most good applicants still approved automatically
- **Practical**: Trains in 15 minutes (vs. 45+ for neural nets)
- **Interpretable**: Tree-based structure helps understand risk factors (regulatory requirement)

---

## Results and Business Impact

### Model Performance (Validation Set)

**Confusion Matrix**:

|                  | Predicted No Default | Predicted Default |
|------------------|---------------------|-------------------|
| **Actual No Default** | 39,754 | 16,330 |
| **Actual Default** | 1,843 | 3,575 |

**Metrics**:
- **Sensitivity**: 63% (Catches 63% of defaults)
- **Specificity**: 71% (Approves 71% of good loans)
- **Overall Accuracy**: 70%

### What This Means for Home Credit

**Before Model**: Approve all loans → 8.1% default rate, significant losses

**With Model**:
- Catch 3,575 of 5,418 defaults (63%) before approval
- Prevent losses from bad loans (depends on average loan size)
- Trade-off: 16,330 good applicants flagged for manual review (acceptable cost vs. default losses)

### Kaggle Competition Results

- **Public Score**: 0.662 AUC
- **Private Score**: 0.675 AUC
- Scores within 1% → model generalizes well to unseen data

---

## Key Technical Decisions

### 1. Prioritized Sensitivity Over Accuracy

For Home Credit, missing a default (false negative) is more costly than flagging a good customer (false positive). Standard accuracy metrics (91%) are misleading with imbalanced data—the model could predict "no default" for everyone and be "accurate" but useless.

**Key Insight**: I understood that business priorities should drive metric selection. For lending, preventing defaults is more important than overall accuracy.

### 2. Downsampling Beat Class Weighting

Tested multiple approaches to handle 91%-9% class imbalance:
- Random Forest with downsampling: **63% sensitivity**
- Neural networks with 11.25x class weighting: **34% sensitivity**
- Threshold tuning alone: Didn't resolve underlying issue

**Key Insight**: Downsampling the majority class during training balanced the model's learning while preserving the real distribution for evaluation.

### 3. Simpler Model Outperformed Complex Ones

Despite testing neural networks with various architectures, Random Forest consistently performed better.

**Key Insight**: I learned that proper data handling (downsampling) and domain-relevant features matter more than algorithm complexity. Also considered production constraints—Random Forest trains in 15 minutes vs. 45+ for neural nets.

### 4. Feature Engineering from Multiple Tables

Aggregated bureau data to capture credit-seeking behavior.

**Key Insight**: I joined multiple tables and engineered features that captured behavioral patterns—not just static demographic data.

---

## What I Would Do Differently

Given more time, I would:

1. **Use All 7 Tables**: Currently used 2 of 7 available tables; payment history, previous applications, and installment data likely contain strong signals
2. **Advanced Imputation**: Replace NA-dropping with mean/mode or k-NN imputation to retain more training data
3. **Model Ensembling**: Stack Random Forest + XGBoost + LightGBM
4. **Cost-Sensitive Learning**: Assign explicit dollar costs to false negatives vs. false positives to optimize business profit directly

---

## Technical Skills Demonstrated

- **Machine Learning**: Logistic Regression, Random Forest, Neural Networks (MLP)
- **Imbalanced Data**: Downsampling, class weighting, threshold tuning
- **Model Evaluation**: AUC-ROC, sensitivity/specificity, confusion matrices, 5-fold cross-validation
- **Data Engineering**: Multi-table joins, aggregation, feature selection
- **R/tidyverse**: caret, ranger, RWeka, dplyr, ggplot2
- **Business Acumen**: Chose metrics aligned with business costs, not just technical benchmarks

---

## Key Interview Talking Points

**Walk me through your approach to this project.**

Started with business understanding—Home Credit needs to detect defaults while maintaining financial inclusion. Built baseline logistic regression (0% sensitivity due to imbalance), then Random Forest with downsampling (63% sensitivity). Tested neural networks extensively but they didn't justify the computational cost. Selected Random Forest for best sensitivity and practical deployment.

**How did you handle the class imbalance?**

Tried three approaches: downsampling (balanced training data), class weighting (weighted defaults 11.25x), and threshold tuning. Downsampling in Random Forest worked best, achieving 63% sensitivity vs. 34% with class weighting in neural nets.

**Why didn't neural networks perform better?**

With tabular data and limited feature engineering, tree-based methods often outperform deep learning. Also, training time was prohibitive (45+ minutes vs. 15 for Random Forest), and sensitivity remained poor despite extensive hyperparameter tuning.

**What would you improve with more time?**

Three priorities: (1) Feature engineering—create interaction terms and ratios, (2) Use all 7 data tables instead of just 2, (3) Ensemble multiple models like top Kaggle solutions. Could likely reach 0.75-0.80 AUC with these improvements.

**How does this translate to business value?**

If Home Credit processes 10,000 loans/month with 8% default rate, that's 800 defaults. Our model catches 504 of them (63%), preventing significant losses. Trade-off is flagging 1,633 good applicants for manual review, but cost of defaults far exceeds review costs.

---

## Contact

**Astha K C**

📧 asthakc.us@gmail.com | 💼 [LinkedIn](https://www.linkedin.com/in/astha-k-c/)

*Open to opportunities in Data Analytics, Business Analytics, Data Science, ML Engineering, and Risk Analytics*

---

## Acknowledgments

This was a project for MSBA program. This repository contains my individual contributions: logistic regression model, Random Forest model development with downsampling and cross-validation, AUC evaluation framework, and model comparison analysis.

**Course**: MSBA Practice Project, University of Utah, Fall 2025

**Last Updated**: November 23, 2025
