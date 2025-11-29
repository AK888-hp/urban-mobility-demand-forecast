# Dashboard Design — Smart City Mobility Intelligence Hub

## 1) Purpose
Create a clean, Microsoft-style dashboard to visualize:
- real-time or batch predictions
- traffic patterns across time
- spatial congestion patterns
- event & weather impact
- model performance metrics
- city-level KPIs

The dashboard should be simple, modern, and aligned with Microsoft Fluent Design.

---

## 2) Dashboard Layout

The dashboard will contain 4 core sections:

### 1. Overview Page (Home)
- City-wide traffic condition (current)
- Predicted next-hour mobility demand
- Weather + event summary
- Key KPIs:
  - Avg_speed_now
  - Current congestion level
  - Event-active areas
  - Rain impact indicator
  - Forecasted traffic for next hour

### 2. Traffic Explorer
- Time-based charts:
  - vehicle_count by hour
  - traffic_speed by hour
  - occupancy by hour
- Filters:
  - Date range selector
  - Area_id selector
- Heatmaps:
  - hour vs day vs avg speed
  - hour vs area_id vs vehicle_count

### 3. Event & Weather Impact
- Comparison charts:
  - event vs non-event congestion levels
  - rainy vs clear hour statistics
- Bar charts:
  - speed drop during events
  - increase in occupancy during bad weather
- Timeline:
  - congestion trend during major events

### 4. Prediction Panel
- Input form:
  - timestamp
  - area_id / latitude & longitude
  - weather_condition
  - ride_sharing_demand
  - sentiment_score
  - event_active
- Output:
  - predicted next-hour vehicle count
  - predicted next-hour traffic condition
  - confidence score
  - area_id mapping
- Visuals:
  - gauge meter for congestion
  - predicted trend for next 3 hours (optional)

---

## 3) UI Theme & Styling

### Color Palette (Microsoft Fluent Style)
- Azure Blue (#0078D4) — Primary highlight
- Light Gray (#F3F2F1) — Background
- Dark Gray (#201F1E) — Text
- Pure White (#FFFFFF) — Cards & panels
- Accent Purple (#6B4C9A) — Analytics emphasis

### Typography
- Titles: Segoe UI Bold
- Body: Segoe UI Regular
- Metrics: Segoe UI Semibold

### Components
- KPI cards
- Line charts
- Bar charts
- Heatmaps
- Input forms
- Modal boxes for detailed views

### Design Rules
- Use Fluent UI spacing (8px grid)
- Minimalist, flat, high-clarity visuals
- Consistent color semantics:
  - Red → high congestion
  - Green → free flow
  - Yellow → moderate traffic
- Smooth, subtle transitions

---

## 4) Dashboard Data Flow

### Data Sources Used:
1. **Static Historical Data**
   - `master_data.csv`
   - Used for charts, trends, heatmaps

2. **Live API Predictions**
   - POST → `/predict`
   - Uses user inputs
   - Updates output section instantly

---

### Data Flow Diagram (Conceptual)

User Input  
→ Dashboard Prediction Form  
→ API `/predict`  
→ ML Model  
→ Prediction Result  
→ Display on Dashboard (cards + charts)

Historical Data  
→ Pre-loaded once  
→ Used for charts/filters  
→ Combined with predictions visually

---

### Dashboard Actions

#### On page load:
- Load master_data.csv
- Precompute all summary tables:
  - hourly averages
  - area-level summaries
  - weather overlay
  - event overlay

#### On filter change:
- Recompute filtered charts
- Update heatmaps
- Refresh KPIs

#### On clicking “Predict Next Hour”:
- Collect form inputs
- Send POST request to API
- Display:
  - prediction values
  - confidence
  - next-hour conditions

---

### Caching Strategy
- Cache static data on first load
- Cache last 10 API responses for fast viewing
- Cache event & weather derived tables

---

## 5) Dashboard Technology Choices

You may choose ONE based on what you want to showcase.

### **Option A — Streamlit (Recommended)**
- Python-based
- Quickest to build
- Clean results
- Super easy API calls

Use if you want:
- Fast implementation  
- Clean UI without React  

### **Option B — React + Fluent UI**
- Most professional, closest to Microsoft dashboards
- Works perfectly with Azure Static Web Apps
- Full frontend freedom

Use if you want:
- A software engineering–style portfolio  
- Perfect UI Polish  

### **Option C — Power BI Dashboard**
- Best enterprise dashboard look
- Microsoft-native
- Easy drill-downs & analytics

Use if:
- You want a very “Microsoft interview friendly” portfolio  
- You prefer no-code or low-code dashboards  

### **Option D — Plotly Dash**
- Python-based but more customizable than Streamlit
- Highly interactive
- Clean graphs

Use if:
- You want a more flexible Python dashboard  

---

## 6) Dashboard Pages (Detailed Specs)

### Overview Page
- Cards:
  - Current avg speed
  - Current congestion score
  - Weather summary (icon + label)
  - Event count today
- Live prediction box
- City map (optional)

### Traffic Explorer
- Line charts:
  - Hourly speed  
  - Hourly vehicle_count  
- Heatmaps:
  - Hour vs Day  
  - Hour vs Area  
- Dropdown:
  - Select Area  
  - Select Date Range  

### Event & Weather Impact
- Event-hour vs non-event-hour plot
- Rain vs clear impact plot
- Attendance vs congestion scatter plot

### Prediction Panel
- API input form
- Card with prediction result
- Congestion gauge meter
- Confidence percentage  
- Next 3 hour projected traffic (optional)

---

## 7) Dashboard Deployment Options

### Streamlit:
- Streamlit Cloud (free)
- Azure App Service

### React:
- Azure Static Web Apps (best choice)
- Netlify/Vercel (optional)

### Power BI:
- Power BI Service
- Embedded Power BI on a website

---

## 8) Final Dashboard Checklist

[ ] Layout completed  
[ ] Styling + theme aligned with Fluent UI  
[ ] Filters implemented  
[ ] Charts implemented  
[ ] Event-weather overlay completed  
[ ] Prediction form connected to `/predict`  
[ ] Live inference working  
[ ] Deployment script ready  
