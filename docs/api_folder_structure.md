# API Folder Structure — Smart City Mobility Intelligence Hub

## Purpose
Define a clean folder structure for the prediction API so it works smoothly with Docker, FastAPI, and Azure.

## Final Folder Layout

api/
│
├── main.py                # FastAPI entry point
├── __init__.py
│
├── routers/               # Additional API routes
│   ├── __init__.py
│   └── predict.py
│
├── core/                  # Core utilities
│   ├── __init__.py
│   ├── feature_engineering.py
│   ├── model_loader.py
│   └── utils.py
│
├── models/                # ML artifacts
│   ├── model.pkl
│   ├── scaler.pkl
│   ├── encoder.pkl
│   └── feature_list.json
│
├── schemas/               # Request / Response models
│   ├── __init__.py
│   └── predict_schema.py
│
└── config/
    ├── __init__.py
    └── settings.py


## Rationale
- Separation of concerns:
  - routers/ → API endpoints
  - core/ → feature engineering + inference logic
  - schemas/ → Pydantic classes
  - models/ → saved ML models
- Makes it easy to maintain and deploy on Azure.
- Reduces Docker build issues by keeping things modular.

## Next Actions
1. Create `api/main.py`
2. Create `api/core/feature_engineering.py`
3. Create `api/schemas/predict_schema.py`
4. Add minimal FastAPI code (later)
