# Azure Deployment Guide  
### Smart City Mobility Intelligence Hub  
### Deploy FastAPI + ML Model + Dashboard on Azure

This guide explains step-by-step how to deploy the backend API, dashboard, and model assets to Microsoft Azure.

---

# ✅ 1. Overview of Deployment Architecture

User → Dashboard (Streamlit / React)
↓
FastAPI Prediction Service (Azure App Service)
↓
ML Model Files (Azure Blob Storage)

yaml
Copy code

You will deploy:

### ✔ FastAPI backend → Azure App Service  
### ✔ ML model artifacts → Azure Blob Storage  
### ✔ Dashboard →  
- Streamlit Cloud OR  
- Azure Static Web Apps (if React)

---

# 🎯 2. Requirements

Install locally:
pip install fastapi uvicorn
pip install azure-storage-blob
pip install python-dotenv
pip install docker

objectivec
Copy code

Install Azure CLI:
https://learn.microsoft.com/en-us/cli/azure/install-azure-cli

makefile
Copy code

Login:
az login

yaml
Copy code

---

# 🗂 3. Azure Resources Needed

You will create:

1. **Resource Group**
2. **App Service Plan**
3. **Web App (FastAPI backend)**
4. **Storage Account**
5. **Blob Container** (for model files)
6. **Static Web App** (only if React dashboard)

---

# 🧱 4. Step 1 — Create Resource Group

```sh
az group create \
  --name smartcity-rg \
  --location eastus
📦 5. Step 2 — Create Storage Account (Model Storage)
sh
Copy code
az storage account create \
  --resource-group smartcity-rg \
  --name smartcitystorage123 \
  --location eastus \
  --sku Standard_LRS
Then create a container:

sh
Copy code
az storage container create \
  --account-name smartcitystorage123 \
  --name model-files \
  --auth-mode login
Upload your model files:

markdown
Copy code
models/
  - model.pkl
  - scaler.pkl
  - encoder.pkl
  - feature_list.json
Upload using Azure CLI:

sh
Copy code
az storage blob upload-batch \
  --account-name smartcitystorage123 \
  --destination model-files \
  --source models/
☁ 6. Step 3 — Update model_loader.py to load from Azure Blob
(You add real logic later — this is just a reminder section)

python
Copy code
# pseudo-code:
load blob file → download locally → load into pickle
🐳 7. Step 4 — Containerize FastAPI (Docker)
In docker/Dockerfile:

sql
Copy code
FROM python:3.10

WORKDIR /app

COPY api/ api/
COPY models/ models/
COPY requirements.txt .

RUN pip install --no-cache-dir -r requirements.txt

CMD ["uvicorn", "api.main:app", "--host", "0.0.0.0", "--port", "8000"]
Build locally:

sh
Copy code
docker build -t traffic-api .
Run locally:

sh
Copy code
docker run -p 8000:8000 traffic-api
Check:

arduino
Copy code
http://127.0.0.1:8000/health
🚀 8. Step 5 — Push Container to Azure Container Registry
Create ACR:

sh
Copy code
az acr create \
  --resource-group smartcity-rg \
  --name smartcityacr123 \
  --sku Basic
Login:

sh
Copy code
az acr login --name smartcityacr123
Tag image:

sh
Copy code
docker tag traffic-api smartcityacr123.azurecr.io/traffic-api:v1
Push:

sh
Copy code
docker push smartcityacr123.azurecr.io/traffic-api:v1
🌐 9. Step 6 — Deploy FastAPI to Azure App Service
Create App Service plan:

sh
Copy code
az appservice plan create \
  --name smartcity-plan \
  --resource-group smartcity-rg \
  --sku B1 \
  --is-linux
Create Web App:

sh
Copy code
az webapp create \
  --resource-group smartcity-rg \
  --plan smartcity-plan \
  --name smartcity-traffic-api \
  --deployment-container-image-name smartcityacr123.azurecr.io/traffic-api:v1
Connect ACR:

sh
Copy code
az webapp config container set \
  --name smartcity-traffic-api \
  --resource-group smartcity-rg \
  --docker-custom-image-name smartcityacr123.azurecr.io/traffic-api:v1 \
  --docker-registry-server-url https://smartcityacr123.azurecr.io
Restart:

sh
Copy code
az webapp restart \
  --name smartcity-traffic-api \
  --resource-group smartcity-rg
API URL:

arduino
Copy code
https://smartcity-traffic-api.azurewebsites.net
Health check:

bash
Copy code
GET /health
🖥 10. Step 7 — Deploy Dashboard
Streamlit (Simplest)
Push project to GitHub.

Login:

arduino
Copy code
https://streamlit.io/cloud
Deploy:

Select the repo

Select dashboard/streamlit_app.py

Add environment variables if needed:

ini
Copy code
API_URL=https://smartcity-traffic-api.azurewebsites.net
React Dashboard (Professional)
sh
Copy code
az staticwebapp create \
  --resource-group smartcity-rg \
  --name smartcity-dashboard \
  --source dashboard/react-app \
  --location eastus \
  --app-location "/" \
  --output-location "build"
Dashboard URL example:

arduino
Copy code
https://white-flower-0ac234.azurestaticapps.net
🔄 11. Step 8 — Continuous Deployment (Optional)
Use GitHub Actions:

Auto rebuild Docker image when model updates

Auto redeploy App Service

Auto redeploy Static Web App

🧪 12. Step 9 — Final Testing Checklist
API:
[ ] /predict works
[ ] Correct model version returned
[ ] Confidence included
[ ] Area mapping correct

Dashboard:
[ ] Prediction form working
[ ] Charts loading
[ ] Error handling correct

Azure:
[ ] Storage keys secured
[ ] App Service logs clean
[ ] Static Web App loads fast