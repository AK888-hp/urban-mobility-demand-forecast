# A/B Test Results — Smart City Mobility Intelligence Hub

## 1) Purpose
Compare multiple models on the same dataset using identical evaluation methods, then select the final model for production.  
This report will hold the experiment results & analysis.

---

## 2) Models Compared

### Baseline Models
- Last Value Model
- Moving Average (3-hour window)
- Linear Regression
- Ridge Regression

### Machine Learning Models
- Random Forest Regressor
- Gradient Boosted Trees (XGBoost/LightGBM)
- CatBoost Regressor

### (Optional) Deep Learning Models
- LSTM
- GRU
- TCN

---

## 3) Dataset Used
- Training: first 70%
- Validation: next 15%
- Test: final 15%
- Source: `data/processed/master_data.csv`
- Target: `vehicle_count_next_hour`

No shuffling (time-series).

---

## 4) Metrics Used

### Regression:
- MAE (preferred)
- RMSE
- MAPE
- R² (optional)

### Classification (if predicting traffic_condition_next):
- Accuracy
- Precision
- Recall
- F1-score
- Confusion Matrix

---

## 5) Experimental Setup

### Feature Engineering:
- lag features (1h, 2h, 3h)
- rolling window (3h mean)
- encoded weather_condition
- encoded event_type
- scaled sentiment, parking, emissions
- area_id one-hot encoded

### Hardware:
(e.g., your laptop’s CPU/GPU)

### Software:
- Python 3.x
- scikit-learn
- pandas
- joblib
- xgboost/lightgbm/catboost (if used)

---

## 6) Results Table (Fill After Running Models)

Create a results table like:

| Model                  | MAE   | RMSE  | MAPE | Notes |
|-----------------------|-------|-------|------|-------|
| Last Value            |       |       |      |       |
| Moving Average        |       |       |      |       |
| Linear Regression     |       |       |      |       |
| Ridge Regression      |       |       |      |       |
| Random Forest         |       |       |      |       |
| XGBoost / LightGBM    |       |       |      |       |
| CatBoost              |       |       |      |       |
| LSTM / GRU (optional) |       |       |      |       |

---

## 7) A/B Comparison

Select two models:

- **Model A:**  
- **Model B:**  

Compute improvement:Improvement = (MAE_A - MAE_B) / MAE_A

Also compare:
- event-only hours  
- rainy hours  
- peak hours  

---

## 8) Visual Analysis (Optional but Strong for Portfolio)

Include plots (saved in:
docs/figures/

yaml
Copy code
):

- MAE vs time
- Error distribution
- Residual plots
- Feature importance comparison
- Event-hour prediction plots
- Weather prediction sensitivity plots

---

## 9) Decision & Notes

### Selected Final Model:
- Model name:
- Version:
- MAE on test:
- Reason for selection:

### Strengths:
- 

### Weaknesses:
-

### Edge Cases:
-

---

## 10) Production Versioning
Save final artifacts into:

models/
├── model.pkl
├── scaler.pkl
├── encoder.pkl
└── feature_list.json

javascript
Copy code

Set model version:
v1.0.0

yaml
Copy code

---

## 11) Final Checklist

[ ] All models trained  
[ ] Metrics computed  
[ ] A/B tests completed  
[ ] Plots saved  
[ ] Final model selected  
[ ] Model exported  
[ ] API updated with correct model 

