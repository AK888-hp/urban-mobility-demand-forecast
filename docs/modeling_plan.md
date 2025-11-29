# Modeling Plan — Smart City Mobility Intelligence Hub

## 1) Purpose
Build and evaluate models to forecast short-term mobility demand and congestion using the engineered master dataset.  
Primary goals:
- Predict next-hour **Vehicle_Count** (regression)
- Predict next-hour **Traffic_Condition** (classification)
- Identify important features (weather, events, sentiment, occupancy)
- Compare multiple models and keep the best one via A/B testing
- Prepare for deployment (API + Docker)

---

## 2) Define Targets (Labels)

### Regression Target (Primary)
- **vehicle_count_next_hour**
  - Created by shifting Vehicle_Count by -1 hour.

### Classification Target (Secondary)
- **traffic_condition_next**
  - Created by shifting Traffic_Condition by -1 hour.
  - Classes:
    - FREE_FLOW
    - MODERATE
    - HEAVY
    - JAM

These targets will help during model evaluation and future expansion.

---

## 3) Feature Engineering Plan

### 3.1 Time-based Features
- hour
- day_of_week
- is_weekend
- is_peak_hour (7–10 AM, 5–8 PM)

### 3.2 Lag Features
Lag both Volume and Speed:
- vehicle_count_lag_1h
- vehicle_count_lag_2h
- vehicle_count_lag_3h
- traffic_speed_lag_1h

### 3.3 Rolling Window Features
- vehicle_count_rolling_mean_3h  
- traffic_speed_rolling_mean_3h  
- occupancy_rolling_mean_3h  

### 3.4 Weather Features
- Encode Weather_Condition using one-hot or integer mapping
- Rain/Cloud indicator
- Temperature (if available later)

### 3.5 Event Features
- event_active (0/1)
- attendance_estimate
- one-hot encode event_type

### 3.6 Spatial Features
- area_id one-hot encoding
- lat/long normalized (optional)

### 3.7 Contextual Features
- sentiment_score normalized
- ride_sharing_demand normalized
- parking_availability mapped to numeric (e.g., LOW=0, MEDIUM=1, HIGH=2)
- emissions and energy consumption scaled

---

## 4) Models To Build

### 4.1 Baseline Models (Start Simple)
- **Last Value Model**
  - Predict next hour = current hour value
- **Moving Average Model**
  - 3-hour average prediction
- **Linear Regression**
- **Ridge Regression**

These establish minimum accuracy expectations.

### 4.2 Machine Learning Models (Core Models)
- Random Forest Regressor
- Gradient Boosting Regressor (XGBoost or LightGBM)
- CatBoost Regressor (recommended)

### 4.3 Deep Learning Models (Optional, if time)
- LSTM model for time-series prediction
- GRU model
- Temporal CNN (TCN)

---

## 5) Train/Validation/Test Strategy

### 5.1 Split Method
**Time-based split** (IMPORTANT: do NOT shuffle)
- Train: first 70%
- Validation: next 15%
- Test: final 15%

### 5.2 Sliding Window Testing
Optional:
- Use rolling-origin evaluation to test model stability over time.

---

## 6) Evaluation Metrics

### Regression Metrics
- MAE (Mean Absolute Error) → preferred
- RMSE
- MAPE

### Classification Metrics (for traffic conditions)
- Accuracy
- F1-score (macro)
- Confusion matrix

### Additional Metrics
- Event-hour error vs non-event-hour error
- Weather-impact error (rainy vs non-rainy)

---

## 7) Model Comparison & A/B Testing

### 7.1 A/B Test Setup
Compare two models at a time:
- Model A = Baseline (e.g., Linear Regression)
- Model B = ML model (e.g., Random Forest)

### 7.2 Test Method
- Evaluate on same test split
- Compute relative improvement:
  improvement = (MAE_A - MAE_B) / MAE_A

### 7.3 Decision
Choose model with:
- Lower MAE
- More stable performance
- Better event-hour accuracy

Document results in:docs/ab_test_results.md


---

## 8) Final Model Packaging Plan

### 8.1 Save Model Artifacts
Store in:
models/
Artifacts:
- model.pkl
- scaler.pkl
- encoder.pkl
- feature_list.json

### 8.2 API Inputs
API `/predict` will require only:
- timestamp
- latitude, longitude OR area_id
- weather_condition
- ride_sharing_demand
- sentiment_score
- event_active

(Other features will be engineered inside the backend.)

### 8.3 API Output
- predicted_vehicle_count_next_hour  
- predicted_traffic_condition_next  
- confidence score  
- contributing_features (optional)

---

## 9) Deployment Readiness

### 9.1 API Containerization
- Build FastAPI service
- Build Dockerfile
- Expose endpoint: `/predict`

### 9.2 Azure Deployment
Deploy container to:
- Azure Container Apps  
or  
- Azure App Service

---

## 10) Final Checklist

[ ] Target column created  
[ ] Feature engineering complete  
[ ] Baseline models built  
[ ] ML models trained  
[ ] Metrics compared  
[ ] A/B test completed  
[ ] Best model selected  
[ ] Model saved in models/  
[ ] API design ready  
[ ] Docker setup ready  
[ ] Azure deployment plan ready  
