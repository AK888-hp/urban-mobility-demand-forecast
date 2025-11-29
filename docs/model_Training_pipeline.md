# Model Training Pipeline — Smart City Mobility Intelligence Hub

## Purpose
Document a reproducible, end-to-end model training pipeline for forecasting next-hour vehicle count and traffic condition. This document is the blueprint for the training script and the notebooks.

---

## 1) Inputs & Outputs

### Inputs
- `data/processed/master_data.csv` — cleaned & merged master dataset
- `docs/area_id_mapping.csv` — area mapping (for reproducibility)
- `models/` (existing artifacts if re-training)

### Outputs
- Trained model artifact: `models/model.pkl`
- Scaler: `models/scaler.pkl`
- Encoder: `models/encoder.pkl`
- Feature list: `models/feature_list.json`
- Model metadata: `models/model_meta.json`
- Test predictions CSV: `docs/test_predictions_{model_name}.csv`
- Evaluation report: `docs/ab_test_results.md` (update)

---

## 2) Environment
- Python 3.10
- Key packages (pinned in `requirements.txt` later):
  - numpy, pandas, scikit-learn, joblib
  - xgboost / lightgbm / catboost (optional)
  - shap (optional for explainability)
  - mlflow (optional for tracking)

---

## 3) Pipeline Overview (high-level steps)
1. Load master_data.csv  
2. Preprocess (impute / scale / encode)  
3. Feature engineering (lags, rolling windows, time features)  
4. Train/validation/test split (time-based)  
5. Train baseline models  
6. Train candidate ML models  
7. Hyperparameter tuning (optional)  
8. Evaluate on validation, pick best model(s)  
9. Evaluate on test set, compute metrics  
10. Save artifacts & update `docs/ab_test_results.md`

---

## 4) Detailed Steps

### 4.1 Load & Quick Validation
- Read `master_data.csv`
- Confirm sorted by timestamp
- Confirm no duplicate rows
- Quick stats: counts, min/max, NA counts

### 4.2 Preprocessing
- Convert `timestamp` to datetime
- Fill small missing values:
  - numeric columns → forward-fill then median (time-aware)
  - categorical → mode or "UNKNOWN"
- Clip or drop extreme outliers per `traffic_cleaning_plan.md`

### 4.3 Feature Engineering (core)
- Time features: hour, day_of_week, is_weekend, is_peak_hour
- Lag features (shift by 1h, 2h, 3h):
  - `vehicle_count_lag_1h`, `vehicle_count_lag_2h` ...
- Rolling means:
  - 3-hour rolling mean of vehicle_count, speed, occupancy
- Event features:
  - `event_active` (0/1)
  - `attendance_estimate`
  - event_type one-hot encoded
- Weather features:
  - weather condition encoding
  - numeric weather features if available (rain, temp)
- Spatial features:
  - area_id one-hot or embedding
- Contextual features:
  - sentiment_score scaled
  - ride_sharing_demand scaled
  - parking_availability mapped

### 4.4 Target Construction
- Regression target:
  - `vehicle_count_next_hour` = `vehicle_count` shifted -1 hour
- Classification target (optional):
  - `traffic_condition_next` = `traffic_condition` shifted -1 hour

Remove rows where target is NA (e.g., last timestamp rows).

### 4.5 Train / Validation / Test Split
- Use **time-based** split:
  - Train: first 70%
  - Validation: next 15%
  - Test: final 15%
- Save indices or timestamp boundaries for reproducibility.

### 4.6 Baseline Models
- Last Value prediction
- Moving Average (3-hour)
- Linear Regression (simple baseline)
Compute baseline metrics and store in the results table.

### 4.7 ML Models (recommended order)
- Random Forest Regressor
- Gradient Boosting (XGBoost or LightGBM)
- CatBoost (handles categorical features well)
- (Optional) Neural models (LSTM) if sequence modeling required

### 4.8 Hyperparameter Tuning
- Use validation set or time-series cross-validation (rolling origin)
- Keep search small initially (grid/random)
- Save best params in `models/model_meta.json`

### 4.9 Final Evaluation
- Select best model based on MAE on validation
- Evaluate chosen model on test set (compute MAE, RMSE, MAPE)
- Save `docs/test_predictions_{model_name}.csv` with columns:
  - timestamp, area_id, actual, prediction, error, event_active, weather_condition

### 4.10 Explainability & Feature Importance
- For tree models use `.feature_importances_`
- Optionally compute SHAP values for top N samples and save plots under `docs/figures/`

---

## 5) Save & Version Model Artifacts
- Save model binary: `joblib.dump(model, "models/model.pkl")`
- Save scaler: `joblib.dump(scaler, "models/scaler.pkl")`
- Save encoder: `joblib.dump(encoder, "models/encoder.pkl")`
- Save feature list:
models/feature_list.json
{
"features": ["hour","day_of_week","vehicle_count_lag_1h", ...]
}

yaml
Copy code
- Save metadata: `models/model_meta.json` with fields:
- model_name, version, train_date, MAE, RMSE, notes

---

## 6) Reproducibility Notes
- Fix random seeds for model training
- Save train/val/test time boundaries
- Save exact package versions in `requirements.txt`
- Consider using MLFlow or DVC (optional) for model tracking & data versioning

---

## 7) Example Minimal Training Script Structure (pseudo-code)
PSEUDO
load master_data.csv
preprocess and engineer features
create targets
split train/val/test by time
fit baseline models
fit RF, XGB, CatBoost on X_train/y_train
evaluate on val; store metrics
choose best model; evaluate on test; store metrics
save artifacts and test predictions
update docs/ab_test_results.md

yaml
Copy code

---

## 8) Checklist (to mark off)
[ ] master_data.csv loaded  
[ ] Preprocessing implemented  
[ ] Feature engineering implemented  
[ ] Targets created  
[ ] Splits created and saved  
[ ] Baseline models computed  
[ ] ML models trained  
[ ] Best model selected & tested  
[ ] Artifacts saved in `models/`  
[ ] Evaluation plots saved in `docs/figures/`  
[ ] ab_test_results.md updated

---

## 9) Notes on Compute & Time
- Start with small samples (one area_id) to debug.
- Use low-compute models first (LR, RF with small estimators) to get baseline.
- For production-grade model (LightGBM/CatBoost), use a stronger CPU or cloud training VM if needed.

---

## 10) Next Steps after training
- Integrate saved model into `api/core/model_loader.py`
- Run local end-to-end test: sample input → predict → compare with offline results
- Build Docker image including `models/` artifacts