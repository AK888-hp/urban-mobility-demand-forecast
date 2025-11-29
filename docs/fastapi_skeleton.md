# FastAPI Skeleton — Smart City Mobility Intelligence Hub

This document describes the structure and skeleton needed to implement the FastAPI-based prediction service.  
It contains only placeholders (no real logic) so you can implement everything yourself step by step.

---

# 1. Folder Structure

api/
│
├── main.py
├── __init__.py
│
├── routers/
│   ├── __init__.py
│   └── predict.py
│
├── schemas/
│   ├── __init__.py
│   └── predict_schema.py
│
├── core/
│   ├── __init__.py
│   ├── feature_engineering.py
│   ├── model_loader.py
│   └── utils.py
│
├── models/
│   ├── model.pkl
│   ├── scaler.pkl
│   ├── encoder.pkl
│   └── feature_list.json
│
└── config/
    ├── __init__.py
    └── settings.py

---

# 2. `main.py` — API Entry Point (Skeleton)

```python
from fastapi import FastAPI
from routers.predict import router as predict_router

app = FastAPI(
    title="Smart City Mobility Intelligence Hub API",
    version="1.0.0"
)

# Include prediction routes
app.include_router(predict_router)

@app.get("/health")
def health_check():
    return {"status": "ok"}

3. routers/predict.py — Prediction Endpoint Skeleton
from fastapi import APIRouter
from schemas.predict_schema import PredictionRequest, PredictionResponse
from core.feature_engineering import preprocess_input
from core.model_loader import load_model_and_assets

router = APIRouter(prefix="/predict", tags=["Prediction"])

# Load model + assets once at startup
model, scaler, encoder, feature_list = load_model_and_assets()

@router.post("/", response_model=PredictionResponse)
def predict_next_hour(data: PredictionRequest):
    # Step 1: Feature engineering
    features = preprocess_input(data, scaler, encoder, feature_list)
    
    # Step 2: Model inference (placeholder)
    # prediction = model.predict(features)
    
    return {
        "vehicle_count_next_hour": 0,   # placeholder
        "traffic_condition_next": "UNKNOWN",
        "confidence": 0.0,
        "area_id": "UNASSIGNED",
        "model_version": "v1.0.0"
    }

4. schemas/predict_schema.py — Request/Response Models
from pydantic import BaseModel

class PredictionRequest(BaseModel):
    timestamp: str
    latitude: float
    longitude: float
    weather_condition: str
    sentiment_score: float
    ride_sharing_demand: float
    event_active: int
    event_type: str | None = None
    attendance_estimate: int | None = None

class PredictionResponse(BaseModel):
    vehicle_count_next_hour: float
    traffic_condition_next: str
    confidence: float
    area_id: str
    model_version: str

5. core/model_loader.py — Model Loader Skeleton
def load_model_and_assets():
    # You will implement:
    # 1. Load model.pkl
    # 2. Load scaler.pkl
    # 3. Load encoder.pkl
    # 4. Load feature_list.json
    
    model = None
    scaler = None
    encoder = None
    feature_list = []

    return model, scaler, encoder, feature_list

6. core/feature_engineering.py — Feature Engineering Skeleton
def preprocess_input(data, scaler, encoder, feature_list):
    # Steps to implement:
    # - Parse timestamp
    # - Extract hour, day_of_week, weekend
    # - Map lat/lon → area_id
    # - Encode weather and event fields
    # - Apply scaler and encoder
    # - Match feature_list order
    
    return []

7. core/utils.py — General Utilities
# Optional utility functions later:
# - nearest area_id finder
# - timestamp parser
# - category normalization

8. config/settings.py — Global Configuration Skeleton
class Settings:
    MODEL_VERSION = "v1.0.0"
    AREA_MAPPING_FILE = "docs/area_id_mapping.csv"

settings = Settings()

9. Final Steps After Skeleton Setup

After creating this file + layout:

[ ] Create the folder structure
[ ] Add placeholder .py files
[ ] Write minimal implementations
[ ] Integrate your trained model
[ ] Test /predict locally
[ ] Build Docker image
[ ] Deploy to Azure