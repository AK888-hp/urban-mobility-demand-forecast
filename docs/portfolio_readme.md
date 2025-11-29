# 🌆 Smart City Mobility Intelligence Hub  
### Predictive Urban Traffic Intelligence using Machine Learning & Event-Aware Modeling

**Author:** K Anantha Krishna Rao  
**Location:** Mysore, India  
**Tech Stack:** Python · FastAPI · Machine Learning · SQL · Streamlit/React · Docker · Azure  

---

## 🎯 Project Overview

This project predicts city-wide mobility demand (vehicle_count_next_hour) using:
- Traffic sensor data  
- Weather conditions  
- City events  
- Sentiment & ride-sharing signals  
- Geo-location features  

Inspired by Microsoft’s AI/Data Science roles, this project demonstrates:
- Data engineering  
- Feature engineering  
- Real ML modeling  
- A/B testing  
- API development  
- Dashboard design  
- Deployment (Azure)  

All components work together to form one cohesive, production-style system.

---

# 📁 Repository Structure

project-root/
│
├── data/
│ ├── raw/
│ └── processed/
│
├── notebooks/
│ ├── eda.ipynb
│ └── feature_engineering.ipynb
│
├── src/
│ ├── build_dataset.py
│ └── utils.py
│
├── models/
│ ├── model.pkl
│ ├── scaler.pkl
│ ├── encoder.pkl
│ └── feature_list.json
│
├── api/
│ ├── main.py
│ ├── routers/
│ ├── core/
│ ├── schemas/
│ └── config/
│
├── dashboard/
│ ├── streamlit_app.py (or react-app/)
│
├── docker/
│ ├── Dockerfile
│ └── docker-compose.yml
│
├── docs/
│ ├── eda_plan.md
│ ├── modeling_plan.md
│ ├── ab_test_results.md
│ ├── model_evaluation_plan.md
│ ├── dashboard_design.md
│ ├── fastapi_skeleton.md
│ ├── azure_deployment_guide.md
│ └── project_report.pdf
│
└── README.md ← YOU ARE HERE

markdown
Copy code

---

# 🧠 Key Features

### 1. **Traffic Forecasting Model**
Predicts:
- Next-hour vehicle count  
- Next-hour traffic condition  
- Congestion risk  
- Confidence score  

### 2. **Event-Aware Modeling**
Model reacts to:
- Concerts
- Sports events
- Festivals
- Traffic jams
- Weather alerts  

### 3. **Dynamic Feature Engineering**
Includes:
- Lag features  
- Rolling statistics  
- Encoded weather  
- Location clustering  
- Sentiment normalization  

### 4. **FastAPI Prediction Service**
Handled through endpoint:
POST /predict

diff
Copy code

Returns:
- prediction  
- traffic_condition  
- confidence  
- model version  

### 5. **Microsoft-Style Dashboard**
Built using:
- Streamlit OR React + Fluent UI  
- Predictive cards  
- Heatmaps  
- Hourly trends  
- Event impact charts  

### 6. **Dockerized Deployment**
docker build .
docker run -p 8000:8000 app

markdown
Copy code

### 7. **Azure Integration**
- App Service (API)
- Static Web Apps (Dashboard)
- Blob storage for models
- CI/CD pipeline (optional)

---

# 🧪 Modeling Workflow

### ✔ Dataset Preparation  
- Merging traffic, weather, events  
- Creating event dataset manually  
- Timestamp standardization  
- Area mapping  

### ✔ EDA  
- Time-series analysis  
- Hourly/weekly patterns  
- Weather/event overlays  

### ✔ Modeling  
- Baseline models  
- ML models (RF/GBM/CatBoost)  
- Optional deep learning (LSTM)  

### ✔ A/B Testing  
Documented in:  
docs/ab_test_results.md

yaml
Copy code

### ✔ Feature Importance  
- Tree importance  
- SHAP values (optional)  

---

# 📊 Dashboard Preview

Dashboard pages:
1. Overview  
2. Traffic Explorer  
3. Event/Weather Impact  
4. Prediction Panel  

Designed using the Microsoft Fluent UI style:
- Azure Blue theme  
- Segoe UI  
- Minimalist cards  

Full design here:
docs/dashboard_design.md

yaml
Copy code

---

# 🚀 Deployment

API:
docker build -t traffic-api .
docker run -p 8000:8000 traffic-api

yaml
Copy code

Dashboard:
- Streamlit Cloud / Azure  
- React App on Azure Static Web Apps  

---

# 📚 Documentation

All major documents are stored in:

docs/

yaml
Copy code

Includes:
- EDA plan  
- Modeling plan  
- A/B test results  
- Evaluation strategy  
- Dashboard design  
- FastAPI skeleton  
- Azure deployment guide  

---

# 📌 Status

| Component | Status |
|----------|--------|
| Dataset Prep | ✅ Done |
| Event Dataset | ⚡ In progress |
| EDA Plan | ✅ Done |
| Modeling Plan | ✅ Done |
| A/B Testing Template | ✅ Done |
| Evaluation Plan | ✅ Done |
| API Skeleton | ✅ Done |
| Dashboard Design | ✅ Done |
| Azure Deployment Guide | 🔜 Next |
| Model Training | 🔜 Soon |
| Dashboard Building | 🔜 Soon |

---

# 📝 Author Notes

This project is part of a long-term goal to master:
- ML Engineering  
- Data Science  
- Backend APIs  
- Deployment pipelines  
- Product thinking  

It will be continuously improved over time.

---

# ⭐ If you like this project  
Give it a **star⭐** on GitHub — it helps **a LOT**!