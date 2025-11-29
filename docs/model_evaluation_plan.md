# Model Evaluation Plan — Smart City Mobility Intelligence Hub

## 1) Purpose
Define a consistent, reproducible evaluation strategy for all forecasting models built in this project.  
This ensures:
- scientific rigor
- reproducibility
- fairness in model comparison
- strong presentation for recruiters or researchers

This document complements `ab_test_results.md`.

---

## 2) Dataset Setup

### 2.1 Input Dataset
Use:
data/processed/master_data.csv

yaml
Copy code

### 2.2 Target Columns
Primary:
- vehicle_count_next_hour (regression)

Secondary:
- traffic_condition_next (classification)

### 2.3 Train / Validation / Test Split
Time-based split (no shuffle):
- Train → 70%
- Validation → 15%
- Test → 15%

---

## 3) Baseline Evaluation
Before training ML models, compute baselines:

### Baseline A — Last Value Model
y_pred = current hour value

shell
Copy code

### Baseline B — Moving Average
y_pred = mean of last 3 hours

yaml
Copy code

### Baseline C — Linear Regression
Trains on all features but no non-linearity.

Baselines give minimum acceptable performance.

---

## 4) Metrics to Compute

### Regression Metrics
| Metric | Purpose |
|--------|---------|
| **MAE** | Primary metric, interpretable |
| **RMSE** | Penalizes large errors |
| **MAPE** | Percentage error |
| **R²** | Variance explained (optional) |

### Classification Metrics (optional)
| Metric | Purpose |
|--------|---------|
| Accuracy | Simple overall measure |
| F1-score (macro) | For imbalanced classes |
| Confusion Matrix | Error type breakdown |

---

## 5) Evaluation Procedure

### Step 1 — Prepare feature matrix
- Apply scaling (saved as scaler.pkl)
- Apply encoding (saved as encoder.pkl)
- Maintain consistent feature order (feature_list.json)

### Step 2 — Train on training data
- fit() model on X_train, y_train

### Step 3 — Validate
- Predict on X_val
- Compute all metrics
- Tune hyperparameters

### Step 4 — Test
- Final evaluation on X_test
- Save predictions as CSV:
docs/test_predictions_{model_name}.csv

yaml
Copy code

---

## 6) Special Evaluation Scenarios

### 6.1 Event-Hour Evaluation
Compare model performance on:
- event_active = 1
- event_active = 0

Helps understand:
- event congestion sensitivity
- performance drop during crowds

### 6.2 Weather-Specific Evaluation
Evaluate separately on:
- rainy hours
- clear hours

Insight:
- weather sensitivity
- prediction robustness

### 6.3 Peak vs Off-Peak Hours
Split test data into:
- peak hours: 7–10 AM, 5–8 PM
- off-peak hours: rest of the day

---

## 7) Feature Importance Analysis

For tree models:
- feature_importances_

For linear models:
- absolute coefficients

For deep learning:
- SHAP or integrated gradients (optional)

Save plot:
docs/figures/feature_importance_{model}.png

yaml
Copy code

Interpret:
- top 5 positive features
- top 5 negative features
- any surprising influences

---

## 8) Error Analysis

### 8.1 Residual Plots
Plot residuals vs:
- time
- area_id
- weather
- event_active

### 8.2 Error Buckets
Bucket predictions into:
- very low error (<10%)
- low error (<20%)
- moderate error (<30%)
- high error (>30%)

### 8.3 Worst-Case Scenarios
List worst 10 predictions with:
- timestamp  
- input conditions  
- prediction vs actual  

---

## 9) Model Stability

Evaluate:
- metric stability across days
- sensitivity to missing or noisy values
- reproducibility with different seeds

---

## 10) Exporting the Final Model

When all evaluation is complete:

Export:
model.pkl
scaler.pkl
encoder.pkl
feature_list.json
model_meta.json

go
Copy code

`model_meta.json` should include:
{
"model_name": "CatBoostRegressor",
"version": "v1.0.0",
"train_date": "YYYY-MM-DD",
"mae": ,
"rmse": ,
"mape": ,
"notes": ""
}

css
Copy code

---

## 11) Final Evaluation Checklist

[ ] Train/val/test split complete  
[ ] Baseline metrics computed  
[ ] All ML models evaluated  
[ ] Test metrics recorded  
[ ] Event-hour analysis done  
[ ] Weather-hour analysis done  
[ ] Feature importance saved  
[ ] Residual plots saved  
[ ] Final model selected  
[ ] Model artifacts exported  