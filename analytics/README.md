# Analytics Pipeline - Titanic Survival Prediction (Module 2)

## Overview
This module focuses on the end-to-end data science workflow: from profiling the classic Titanic dataset to building and evaluating a robust predictive modeling pipeline. It transitions from Exploratory Data Analysis (EDA) to rigorous machine learning evaluation.

### Pipeline Architecture
```text
[ Titanic Dataset ]
       |
       v
[ EDA: Profiling & Cleaning ] ---> [ Cleaned Dataset ]
                                          |
                                          v
                                 [ Stratified Split ]
                                 /               \
                         (80%) Train Set     (20%) Test Set
                                |                   |
                                v                   |
                   [ Fit Preprocessing: Scaler/Encoder ]
                                |                   |
                                v                   |
                   [ Train Classifiers: RF, DT, LogReg ]
                                |                   |
                                v                   v
                        [ Transform Test Set ] <-----+
                                |
                                v
                   [ Evaluation: F1, AUC, Confusion Matrix ]
                                |
                                v
                    [ Save as .joblib Pipeline ]
```

## Design Decisions

### 1. Data Profiling & Cleaning
- **Missing Values**: Followed the project threshold rule:
    - **Under 5% missing**: Dropped rows (e.g., `embarked`).
    - **5-30% missing**: Imputed using the median to handle skewed distributions (e.g., `age`).
    - **Over 30% missing**: Dropped the column entirely (e.g., `deck`) as imputation would be unreliable.
- **Outliers**: Used the Interquartile Range (IQR) rule to identify and report outliers in `age` and `fare`.
- **Skewness**: Confirmed `fare` is right-skewed by comparing Mean > Median > Mode.

### 2. Modeling Strategy
- **Pipeline Architecture**: Used `sklearn.pipeline.Pipeline` with `ColumnTransformer` to ensure structural prevention of data leakage.
    - **Scaling**: `StandardScaler` applied to numeric features.
    - **Encoding**: `OneHotEncoder` applied to categorical features with `drop='first'` to avoid the dummy variable trap.
    - **Leakage Prevention**: All preprocessing is fit only on the training split and applied to the test split.
- **Classifiers**: Evaluated Logistic Regression, Decision Tree, and Random Forest.
- **Imbalance Handling**: Compared baseline performance against `class_weight='balanced'` and **SMOTE** (applied only to the training fold) to optimize the F1-score for survivors.
- **Tuning**: Used `GridSearchCV` for the Random Forest, reporting the **Out-of-Bag (OOB) score** as a reliable estimate of generalization performance.

### 3. Regression Task
- Implemented a multivariate linear regression to predict `fare`.
- Analysis of the residual plot confirmed **heteroscedasticity** (non-constant variance), suggesting that linear regression may not be the ideal model for this specific target.

## Implementation Details
- **EDA Story**: Produced 4+ distinct charts with written interpretations to build a coherent argument about survival likelihood based on sex and class.
- **Evaluation**: All models are compared using a comprehensive suite: Confusion Matrix, Accuracy, Precision, Recall, F1-score, and ROC-AUC.
- **Artifact**: The best-performing complete pipeline (preprocessing + estimator) is saved as `titanic_survival_pipeline.joblib` for end-to-end inference on raw data. Note: While imbalance handling (SMOTE) and hyperparameter tuning (Random Forest) were explored as separate experiments to analyze model sensitivity, the Logistic Regression baseline was ultimately persisted as the final artifact due to its superior overall balance of Accuracy and AUC on the test set.

## Installation & Execution
1. **Dependencies**:
   ```bash
   pip install pandas seaborn scikit-learn imbalanced-learn joblib
   ```
2. **Run**:
   Execute `Titanic_Survival_Prediction_Model.ipynb`.
   - The notebook generates `titanic.csv` as an offline fallback.
   - The final pipeline is saved as `titanic_survival_pipeline.joblib`.
