# Streamlit Dashboard Skeleton  
### Smart City Mobility Intelligence Hub  
### Microsoft-Style Fluent UI Dashboard

This document gives you the **complete Streamlit folder structure + UI layout skeleton**.  
There is **no real logic or model code**, only placeholders — so you build it yourself.

---

# 1. Folder Structure

dashboard/
│
├── streamlit_app.py
├── pages/
│   ├── 1_Overview.py
│   ├── 2_Traffic_Explorer.py
│   ├── 3_Event_Weather_Impact.py
│   └── 4_Prediction_Panel.py
│
├── assets/
│   ├── logo.png
│   └── theme.css
│
└── utils/
    ├── __init__.py
    └── api_client.py

---

# 2. Global Theme Styling (Optional)

In:
dashboard/assets/theme.css

css
Copy code

Use Fluent colors:

```css
/* Microsoft Fluent Theme */
:root {
    --primary-blue: #0078D4;
    --accent-purple: #6B4C9A;
    --light-gray: #F3F2F1;
    --dark-gray: #201F1E;
}

body {
    background-color: var(--light-gray);
    font-family: "Segoe UI", sans-serif;
}
Inject into Streamlit with:

python
Copy code
st.markdown("<style>" + open("assets/theme.css").read() + "</style>", unsafe_allow_html=True)
3. streamlit_app.py (Root File Skeleton)
python
Copy code
import streamlit as st

st.set_page_config(
    page_title="Smart City Mobility Hub",
    page_icon="🚦",
    layout="wide",
)

# Load theme CSS
with open("assets/theme.css") as f:
    st.markdown(f"<style>{f.read()}</style>", unsafe_allow_html=True)

st.title("🌆 Smart City Mobility Intelligence Hub")
st.subheader("Microsoft Fluent-style Dashboard for Traffic Forecasting")

st.write("""
Welcome to your unified traffic intelligence workspace.
Use the sidebar to navigate through pages:
- Overview
- Traffic Explorer
- Event & Weather Impact
- Prediction Panel
""")

st.image("assets/logo.png", width=200)
4. Page 1 — pages/1_Overview.py
python
Copy code
import streamlit as st

st.title("📊 Overview Dashboard")

st.write("High-level metrics about current city mobility.")

# KPI placeholders
col1, col2, col3, col4 = st.columns(4)

col1.metric("Avg Speed (km/h)", "—")
col2.metric("Congestion Level", "—")
col3.metric("Active Events", "—")
col4.metric("Weather", "—")

st.write("---")

st.subheader("City-wide Traffic Trend")
st.line_chart([])  # placeholder

st.subheader("Hourly Speed Heatmap")
st.write("Heatmap placeholder")
5. Page 2 — pages/2_Traffic_Explorer.py
python
Copy code
import streamlit as st

st.title("🛰 Traffic Explorer")
st.write("Explore traffic patterns using filters.")

# Filters
area = st.selectbox("Select Area", ["—"])
date_range = st.date_input("Select Date Range")

st.write("---")

st.subheader("Hourly Vehicle Count")
st.line_chart([])

st.subheader("Traffic Speed Over Time")
st.line_chart([])

st.subheader("Occupancy Heatmap")
st.write("Heatmap placeholder")
6. Page 3 — pages/3_Event_Weather_Impact.py
python
Copy code
import streamlit as st

st.title("🎤 Event & Weather Impact Analysis")

st.write("""
Analyze how concerts, matches, festivals, and weather
impact city-wide congestion.
""")

event = st.selectbox("Select Event Type", ["—"])
weather = st.selectbox("Select Weather Condition", ["—"])

st.write("---")

col1, col2 = st.columns(2)
col1.subheader("Event vs Non-Event Speeds")
col1.bar_chart([])

col2.subheader("Rain vs Clear Speed Drop")
col2.bar_chart([])

st.write("---")

st.subheader("Event Timeline Effect")
st.line_chart([])
7. Page 4 — pages/4_Prediction_Panel.py
python
Copy code
import streamlit as st
from utils.api_client import predict

st.title("🔮 Prediction Panel")
st.write("Send features to FastAPI and get next-hour traffic predictions.")

st.write("### Input Features")

# Form
with st.form("predict_form"):
    timestamp = st.text_input("Timestamp (DD-MM-YYYY HH:MM:SS)")
    latitude = st.number_input("Latitude")
    longitude = st.number_input("Longitude")
    weather = st.selectbox("Weather Condition", ["Clear", "Rain", "Snow", "Fog"])
    sentiment = st.slider("Sentiment Score", -1.0, 1.0, 0.0)
    ride = st.slider("Ride Sharing Demand", 0, 100, 10)
    event_active = st.selectbox("Event Active?", [0, 1])
    event_type = st.text_input("Event Type (optional)")
    attendance = st.number_input("Attendance Estimate (optional)", min_value=0)

    submitted = st.form_submit_button("Predict")

if submitted:
    st.write("📡 Sending request to API...")
    
    response = predict(
        timestamp=timestamp,
        latitude=latitude,
        longitude=longitude,
        weather=weather,
        sentiment=sentiment,
        ride=ride,
        event_active=event_active,
        event_type=event_type,
        attendance=attendance,
    )
    
    st.write("### Prediction Result:")
    st.json(response)
8. API Client — utils/api_client.py
python
Copy code
import requests

API_URL = "https://your-api-url.azurewebsites.net/predict/"

def predict(**kwargs):
    # send POST request
    try:
        res = requests.post(API_URL, json=kwargs)
        return res.json()
    except Exception as e:
        return {"error": str(e)}
9. Final Steps
Checklist
[ ] Add real charts
[ ] Load real data
[ ] Connect to your real /predict API
[ ] Add theme.css
[ ] Add logo
[ ] Deploy to Streamlit Cloud or Azure