# API Design — Smart City Mobility Intelligence Hub

## 1) Purpose
Provide a clean, simple API that accepts city mobility inputs and returns:
- Predicted vehicle_count for the next hour
- Predicted traffic_condition
- Optional confidence score

The API must be lightweight, scalable, containerizable, and Azure-ready.

---

## 2) Overall Architecture

### Components:
- **FastAPI backend**
- **Model artifacts** (model.pkl, scaler.pkl, encoder.pkl)
- **Feature engineering layer** inside API
- **Docker container**
- **Azure deployment** (Container Apps or App Service)

### Flow:
Request → Feature engineering → Model inference → Response

---

## 3) Endpoint Summary

### Primary Endpoint:
POST /predict


### Optional Endpoints:
GET /health → returns API status
GET /model_info → model metadata & version


---

## 4) `/predict` Endpoint Specification

### Method
POST


### URL
/predict


### Request Body Schema (JSON)
{
"timestamp": "2025-02-10 15:00:00",
"latitude": 12.305,
"longitude": 76.655,
"weather_condition": "RAINY",
"sentiment_score": 0.62,
"ride_sharing_demand": 180,
"event_active": 1,
"event_type": "CONCERT",
"attendance_estimate": 15000
}


### Notes:
- `latitude` + `longitude` will be converted → `area_id` internally
- The API will generate:
  - lag features (from recent stored values)
  - rolling averages
  - time-based features
- Missing optional fields will be handled gracefully (fallback defaults)

---

## 5) `/predict` Response Schema

### Example Response:
{
"vehicle_count_next_hour": 540,
"traffic_condition_next": "HEAVY",
"confidence": 0.87,
"model_version": "v1.0.0",
"area_id": "MYS_CENTRE"
}


### Fields Explained:
- **vehicle_count_next_hour** → numeric regression output
- **traffic_condition_next** → classification output
- **confidence** → softmax probability or scaled confidence
- **area_id** → calculated by API
- **model_version** → helpful for production tracking

---

## 6) Error Handling

### Missing required fields:
422 Unprocessable Entity


### Model not loaded / internal error:

500 Internal Server Error
{
"error": "Model loading failed"
}


### Invalid timestamp format:


400 Bad Request
{
"error": "Invalid timestamp format. Expected YYYY-MM-DD HH:MM:SS"
}


---

## 7) Performance Requirements

- Average response time: < 100ms
- Max response time (API cold start): < 400ms
- Concurrent requests supported: 50+
- Memory usage: < 512MB

---

## 8) Model Integration

### Required files in `/models`:
- `model.pkl`
- `scaler.pkl`
- `encoder.pkl`
- `feature_list.json`

### Load on startup:


@app.on_event("startup")
def load_model():
global model, scaler, encoder


### Feature engineering inside API:
- Convert timestamp → hour/day/weekend
- Add event-related logic
- Map lat/lon → area_id
- Apply scaler + encoder
- Select correct feature order

---

## 9) Docker & Deployment

### Dockerfile (structure only)


FROM python:3.10
WORKDIR /app
COPY requirements.txt .
RUN pip install -r requirements.txt
COPY . .
CMD ["uvicorn", "main:app", "--host", "0.0.0.0", "--port", "8000"]


### Exposed Port:


8000


### Azure Deployment Options:
- Azure Container Apps (recommended)
- Azure App Service (Docker)
- Azure Kubernetes Service (advanced)

---

## 10) API Test Examples

### Curl Command:


curl -X POST "http://localhost:8000/predict
"
-H "Content-Type: application/json"
-d '{"timestamp":"2025-02-10 15:00:00","latitude":12.31,
"longitude":76.65,"weather_condition":"SUNNY",
"sentiment_score":0.4,"ride_sharing_demand":220,
"event_active":0}'


### Python Test:


import requests

url = "http://localhost:8000/predict
"
payload = {
"timestamp": "2025-02-10 15:00:00",
"latitude": 12.31,
"longitude": 76.65,
"weather_condition": "SUNNY",
"sentiment_score": 0.4,
"ride_sharing_demand": 220,
"event_active": 0
}

response = requests.post(url, json=payload)
print(response.json())


---

## 11) Final API Checklist

[ ] Request schema finalised  
[ ] Response schema finalised  
[ ] Feature-engineering rules defined  
[ ] Model artifacts planned  
[ ] Dockerfile ready  
[ ] Endpoint documented  
[ ] Deployment plan written  
[ ] API monitoring plan to be added later  