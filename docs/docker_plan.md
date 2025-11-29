# Docker Plan — Smart City Mobility Intelligence Hub

## Purpose
Containerize the prediction API so it can run reliably on any host and be deployed to Azure.

## Image & Name
- Image name (example): smartcity/mobility-hub
- Tag: v1.0.0

## Dockerfile (final to be placed at project root or /docker)
FROM python:3.10-slim

WORKDIR /app

Install runtime dependencies
COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt

Copy application code
COPY . .

Expose port for the API
EXPOSE 8000

Run the API
CMD ["uvicorn", "main:app", "--host", "0.0.0.0", "--port", "8000"]

markdown
Copy code

## Build & Run (local)
- Build:
docker build -t smartcity/mobility-hub:v1.0.0 .

diff
Copy code
- Run:
docker run -p 8000:8000 --env-file .env smartcity/mobility-hub:v1.0.0

markdown
Copy code

## Files to include in image
- src/ (API code, main.py)
- models/ (model.pkl, scaler.pkl, etc.)
- requirements.txt
- docs/ (optional)

## Size & Performance
- Keep image small: use slim base images and `--no-cache-dir` when pip installing.
- Aim: < 500MB final image if possible.

## Health & Logging
- Add a `/health` endpoint returning 200 for liveness.
- Log to stdout/stderr for container logs.

## Security
- Do NOT bake secrets into the image. Use environment variables or Azure Key Vault for production.

## Next actions after this doc
- Create `requirements.txt` with exact pinned versions
- Add `main.py` (FastAPI entry) in src/ and reference it in Dockerfile
- Build locally and test `/predict` endpoint inside conta