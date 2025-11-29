# EDA Plan — Smart City Mobility Intelligence Hub

## 1) Purpose
Perform Exploratory Data Analysis (EDA) on the cleaned master dataset to understand:
- Traffic patterns over time (hour, day, weekend)
- Spatial variations across area_id
- Correlations among traffic, weather, events, and contextual features
- Event impact on congestion
- Weather impact on speed & occupancy
- Identify anomalies and feature importance before modeling

---

## 2) Dataset Required
Input file:
- `data/processed/master_data.csv`

This file must contain:
- time features
- traffic features
- contextual features
- events
- area_id

---

## 3) Basic Checks
- Verify row count, column count
- Identify data types (numeric / categorical / datetime)
- Verify timestamp ordering
- Check for duplicates
- Check for remaining missing values
- Check basic statistics:
  - mean, median, std, min, max
  - percentile values (5%, 25%, 50%, 75%, 95%)

Deliverables:
- Summary table in notebook
- Notes on any unusual findings

---

## 4) Time-Based Analysis
### Goals:
Understand how traffic behaves over different time scales.

### Tasks:
- Plot traffic volume vs hour of day
- Plot traffic speed vs hour of day
- Compare weekday vs weekend patterns
- Compare different day_of_week trends
- Monthly / weekly seasonality (if dataset spans longer)

### Key Insights Expected:
- Peak hours (morning & evening)
- Weekend differences
- Hourly speed drop patterns

---

## 5) Spatial Analysis (area_id)
### Goals:
Find which parts of the city have heavier traffic.

### Tasks:
- Group by area_id and compute:
  - avg vehicle_count
  - avg speed
  - avg occupancy
- Bar plots for each metric by area_id
- Identify top 2–3 congested zones
- Identify zones with lowest speeds

### Key Insights Expected:
- Bottleneck areas
- High-impact locations for events & accidents

---

## 6) Weather Impact Analysis
### Goals:
Understand how weather affects traffic behavior.

### Tasks:
- Compare vehicle_count under different weather conditions
- Compare speeds under: SUNNY vs CLOUDY vs RAINY
- Check if rainy weather increases occupancy & congestion
- Compute correlation of:
  - rainfall (if available)
  - visibility (if available)
  - weather_condition (encoded)

### Key Insights Expected:
- Rain reduces speed
- Bad weather → higher occupancy & slower flow

---

## 7) Event Impact Analysis
### Goals:
Measure how events increase local congestion.

### Tasks:
- Compare speed before/during/after events
- Compare vehicle_count for:
  - event_active = 0 vs 1
- Analyze impact by event_type (Concert, Parade, Sports)
- Analyze impact by attendance_estimate level

### Key Insights Expected:
- Events increase congestion significantly
- Stadium area shows peak load spikes
- High-attendance events cause slowdowns across multiple areas

---

## 8) Correlation Analysis
### Goals:
Find which variables influence congestion/traffic speed the most.

### Tasks:
- Compute correlation matrix (numeric columns)
- Heatmap visualization
- Focus on correlation of:
  - vehicle_count
  - traffic_speed_kmh
  - road_occupancy_pct
  with:
  - sentiment_score
  - ride_sharing_demand
  - parking_availability
  - emissions & energy usage
  - events
  - weather

### Key Insights Expected:
- Negative correlation between speed & occupancy
- Events positively correlated with congestion
- Sentiment_score correlations (if any)
- Parking availability → congestion link

---

## 9) Outlier & Anomaly Detection
### Goals:
Identify abnormal rows for cleaning/flagging.

### Tasks:
- Boxplots for speed, occupancy, vehicle_count
- Identify zero-speed rows during non-event times
- Identify speed > 200, occupancy > 100
- Check unrealistic timestamps
- Compare anomalies across area_id

### Key Insights Expected:
- Outliers may be sensor glitches
- Some areas may have more erroneous readings

---

## 10) Feature Insights Summary
After EDA, write a summary including:
- 5 most important takeaways from time analysis
- 3–4 critical spatial insights
- Weather patterns
- Event effects
- Which features look useful for modeling

Include this summary as:docs/eda_summary.md


---

## 11) EDA Notebook Deliverable
Create:notebooks/EDA.ipynb


The notebook must include:
- Basic stats
- Time plots
- Spatial analysis plots
- Weather-event correlations
- Heatmap
- Outlier visuals
- Final summary markdown cell

---

## 12) Final EDA Checklist

[ ] master_data.csv loaded  
[ ] Data types verified  
[ ] Summary stats generated  
[ ] Time-based patterns plotted  
[ ] Spatial patterns plotted  
[ ] Weather impact analyzed  
[ ] Event impact analyzed  
[ ] Correlation matrix created  
[ ] Outliers detected  
[ ] Insights documented in eda_summary.md  
[ ] EDA.ipynb saved  



