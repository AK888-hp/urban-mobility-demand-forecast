# Architecture Diagram & Component Overview — Smart City Mobility Intelligence Hub

## Purpose
Explain the high-level architecture, components, and data flow for the end-to-end system. Use this doc as a guide to draw a visual diagram (PNG/SVG) for your repo or portfolio.

---

## 1) High-level Components (Textual Diagram)

+----------------+ +----------------------+ +-----------------+
| Data Sources | ---> | Data Processing | ---> | Master Data |
| (Traffic CSV, | | & Cleaning Pipeline | | (CSV/DB) |
| Weather API, | | (notebooks / src) | | data/processed |
| Events CSV) | +----------------------+ +-----------------+
| |
| v
| +---------------------+
| | Feature Engine |
| | (offline/online) |
| +---------------------+
| |
v v
+----------------+ +---------------------+ +---------------------+
| model training | <--| master_data | ---------->| trained model |
| (notebooks, | | (data/processed) | | models/*.pkl |
| scripts) | +---------------------+ +---------------------+
+----------------+ |
| v
| +-----------------+
| | Model Store |
| | (models/ or |
v | Azure Blob) |
+----------------+ +-----------------+
| FastAPI API | <--- (loads model on startup) --- | Model Artifacts |
| (Dockerized) | +-----------------+
+----------------+ |
| v
| +-----------------+
| | Dashboard |
v | (Streamlit/React)|
+----------------+ +-----------------+
| Users / UI | <--- Requests / Responses ---> | Visualizations |
+----------------+ +-----------------+

markdown
Copy code

---

## 2) Component Descriptions

### A. Data Sources
- **Traffic CSV** — primary time-series sensor data
- **Weather** — external API (OpenWeather) or archived sample
- **Events** — manual events CSV (we created `events_raw.csv`)

### B. Data Processing Layer
- Notebooks / `src/` scripts that:
  - Standardize timestamps
  - Clean sensor data
  - Map lat/long → area_id
  - Generate `master_data.csv` (in `data/processed/`)

### C. Master Data
- Single source of truth for modeling and historical dashboard views
- Stored as CSV or a small DB (sqlite/parquet) depending on scale

### D. Model Training
- Local notebook or cloud VM
- Produces `models/model.pkl` and supporting scalers/encoders

### E. Model Store
- Local `models/` folder for quick dev
- Azure Blob Storage for production artifacts (recommended)

### F. FastAPI Prediction Service
- Loads model on startup
- Provides `/predict` endpoint
- Containerized in Docker and deployed on Azure App Service or ACI
- Handles feature engineering for incoming requests

### G. Dashboard
- Streamlit or React-based UI
- Reads `master_data.csv` for historical visuals
- Calls FastAPI `/predict` for live predictions

### H. Users & Clients
- Recruiters, product managers, or demo viewers interact via dashboard or curl requests

---

## 3) Data Flow (detailed sequence)
1. `Traffic CSV` + `Weather` + `Events` → data processing scripts → `master_data.csv`
2. `master_data.csv` → feature generation → `X_train`/`y_train`
3. `X_train` → train model → `model.pkl` saved in `models/`
4. `model.pkl` uploaded (optional) to Azure Blob Storage
5. FastAPI app starts → downloads model artifact (or loads local model) → serves `/predict`
6. Dashboard reads `master_data.csv` (historical) and calls `/predict` for live inference
7. User sees visuals + predictions in dashboard

---

## 4) Security & Ops Considerations
- Do NOT store secrets in repo. Use `.env` locally and Azure Key Vault in production.
- Logs:
  - Application logs to stdout for container logging
  - Use Azure Monitor for App Service logs
- Health checks:
  - `/health` endpoint
  - Liveness/readiness for container orchestrators
- Monitoring:
  - Track model performance over time
  - Log predictions and compare with observed values (if available)
- Scaling:
  - For low volume use Azure App Service tiers
  - For high scale use Azure Container Instances / AKS

---

## 5) Visual Diagram Suggestions (for README/portfolio)
- Create a single PNG showing the blocks above with arrows
- Add small captions under each block (one sentence)
- Optionally provide two diagrams:
  - Dev diagram (local flow)
  - Prod diagram (Azure resources)

---

## 6) Minimal Legend (for diagram)
- Blue boxes: Data & Storage
- Green boxes: Compute & Model
- Orange boxes: API & Services
- Grey boxes: UI & Users

---

## 7) Final Notes
- Keep architecture simple for a portfolio demo.
- Highlight in your README that the system is modular:
  - Data → Model → API → Dashboard
- Emphasize reproducibility: versioned data, saved artifacts, documented pipelines.
