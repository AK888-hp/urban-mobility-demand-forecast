# Urban Mobility Demand Forecasting System

**Author:** K Anantha Krishna Rao  
**Location:** Mysore, India

## Project overview
## 🌆 Smart City Mobility Intelligence Hub — Theme

This project is built with the Smart City Intelligence aesthetic inspired by Microsoft’s design language.  
It uses:

- Azure Blue for digital urban identity  
- Neutral Gray for clarity and readability  
- White for a clean layout  
- Purple accents to highlight analytics or important components

The interface and documentation follow a minimalist, geometric, modern style focused on clarity and insight — similar to Microsoft’s internal AI tools and dashboards.


## Quick structure
- data/               — raw and processed datasets
- notebooks/          — EDA and experiments
- src/                — data pipeline and utilities
- models/             — trained model artifacts
- api/                — prediction service (design only)
- dashboard/          — visualization app (design only)
- docker/             — Dockerfile and container configs
- azure/              — deployment scripts and notes
- docs/               — analysis, feature decisions, AB test report

## Datasets (planned)
- Traffic dataset: [source placeholder]
- Weather dataset: OpenWeather or local weather API
- Events dataset: manual/CSV of city events

## Goals / Deliverables
- Master processed dataset (`data/processed/master_data.csv`)
- EDA notebook with key insights (`notebooks/EDA.ipynb`)
- 2-3 trained models with evaluation (`models/`)
- A/B test report comparing models (`docs/ab_test_results.md`)
- Design for `POST /predict` API (`docs/api_design.md`)
- Docker-ready API (containerized)
- Deployed endpoint on Azure + demo dashboard

## How to proceed
Follow the step-by-step plan guided by the project roadmap. Start with data collection, then SQL & EDA, feature engineering, modeling, A/B testing, API design, containerization, Azure deployment, and dashboard.

