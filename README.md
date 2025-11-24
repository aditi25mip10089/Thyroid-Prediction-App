
# Thyroid Disease Recurrence Prediction Model

This repository contains a machine learning pipeline built using **Random Forest Classifier** to predict the likelihood of thyroid disease recurrence based on clinical, demographic, and pathological features.

---

## Project Overview

Thyroid disease recurrence prediction is valuable for clinicians to plan follow-ups and treatment.  
This model uses structured patient data and applies preprocessing + classification to estimate recurrence probability.

The workflow includes:

- Data loading & cleaning
- Binary target encoding (`Yes` → 1, `No` → 0)
- Train-test split
- Categorical One-Hot Encoding
- Random Forest Classification
- Performance evaluation
- Single-patient prediction
- Saving model for future predictions

---

## Dataset Structure

The dataset must contain the following columns:

```
Age, Gender, Smoking, Hx Smoking, Hx Radiothreapy,
Thyroid Function, Physical Examination, Adenopathy,
Pathology, Focality, Risk, T, N, M, Stage, Response, Recurred
```

Target variable:

| Value | Meaning |
|--------|---------|
| Yes | Recurrence happened |
| No | No recurrence |

---

## Model Pipeline

```python
preprocessor = ColumnTransformer(
    transformers=[
        ("cat", OneHotEncoder(handle_unknown="ignore"), categorical_features),
        ("num", "passthrough", numeric_features)
    ]
)

model = RandomForestClassifier(
    n_estimators=300,
    class_weight="balanced",
    random_state=42
)

pipeline = Pipeline(steps=[
    ("preprocess", preprocessor),
    ("model", model)
])
```

---

## Model Performance

Example classification report:

```
              precision    recall  f1-score   support
           0       0.96      1.00      0.98        69
           1       1.00      0.89      0.94        27

    accuracy                           0.97        96
   macro avg       0.98      0.94      0.96        96
weighted avg       0.97      0.97      0.97        96
```

---

## Making Predictions

```python
new_patient = {
    "Age": 31,
    "Gender": "F",
    "Smoking": "No",
    "Hx Smoking": "No",
    "Hx Radiothreapy": "No",
    "Thyroid Function": "Euthyroid",
    "Physical Examination": "Single nodular goiter-right",
    "Adenopathy": "No",
    "Pathology": "Papillary",
    "Focality": "Uni-Focal",
    "Risk": "Low",
    "T": "T1a",
    "N": "N0",
    "M": "M0",
    "Stage": "I",
    "Response": "Excellent"
}

df_new = pd.DataFrame([new_patient])
prediction = pipeline.predict(df_new)[0]
probability = pipeline.predict_proba(df_new)[0][1]

print("Recurred:", "Yes" if prediction==1 else "No")
print("Probability of recurrence:", round(probability, 4))
```

---

## Output Example

```
Recurred: No
Probability of recurrence: 0.0033
```

---

## Requirements

```
pandas
scikit-learn
joblib
```

Install dependencies:

```bash
pip install -r requirements.txt
```

---

