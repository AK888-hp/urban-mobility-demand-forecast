# Traffic Cleaning Plan — Smart City Mobility Intelligence Hub

## 1) Traffic dataset column names (raw)
'Timestamp',
'Latitude',
'Longitude',
'Vehicle_Count',
'Traffic_Speed_kmh',
'Road_Occupancy_%',
'Traffic_Light_State',
'Weather_Condition',
'Accident_Report',
'Sentiment_Score',
'Ride_Sharing_Demand',
'Parking_Availability',
'Emission_Levels_g_km',
'Energy_Consumption_L_h',
'Traffic_Condition'

## 2) Timestamp Cleaning Plan
- Current format: DD-MM-YYYY HH:MM:SS
- Convert to standard: YYYY-MM-DD HH:MM:SS
- Extract later:
  - date
  - hour
  - day_of_week
  - is_weekend

## 3) Geolocation Validation
- Validate latitude range: -90 <= Latitude <= 90
- Validate longitude range: -180 <= Longitude <= 180
- If any lat/long outside these ranges → mark as invalid and flag rows
- Plan to create `area_id` by clustering lat/long into coarse zones:
  - Suggested area_ids: MYS_NORTH, MYS_SOUTH, MYS_CENTRE, MYS_PALACE, MYS_STADIUM, MYS_GROUNDS
- Save mapping file later as: docs/area_id_mapping.md

## 4) Traffic Feature Cleaning Rules
- Check for unrealistic values:
  - Traffic_Speed_kmh > 200 → mark as outlier
  - Traffic_Speed_kmh < 0 → invalid
  - Road_Occupancy_% outside 0–100 → invalid or cap to nearest valid value
  - Vehicle_Count < 0 → invalid
- Normalize Traffic_Light_State:
  Expected categories: RED, YELLOW, GREEN
  Convert all text to uppercase for consistency.
- Standardize Traffic_Condition:
  Expected: FREE_FLOW, MODERATE, HEAVY, JAM
  Unify casing and remove spaces if needed.

## 5) Contextual Feature Cleaning Rules
- Weather_Condition → clean to categories like: SUNNY, CLOUDY, RAINY
- Accident_Report → convert to 0 (No) / 1 (Yes)
- Sentiment_Score → normalize later (range either -1 to 1 or 0 to 1)
- Ride_Sharing_Demand → ensure numeric; handle extreme spikes
- Parking_Availability → standardize (LOW / MEDIUM / HIGH) or keep numeric if provided
- Emission_Levels_g_km → ensure numeric; handle negative or impossible values
- Energy_Consumption_L_h → ensure numeric; check for outliers

## 6) Event Merging Plan
- Join traffic data with events on:
  - Same date
  - Traffic timestamp falls between event start_time and end_time
- Add new columns after merge:
  - event_active (0/1)
  - event_type
  - attendance_estimate
  - event_area_id
- If multiple events overlap, choose the one with highest attendance_estimate

## 7) Final Cleaning Checklist

[ ] Timestamp converted to YYYY-MM-DD HH:MM:SS  
[ ] Date, hour, day_of_week, is_weekend extracted  
[ ] Latitude/Longitude validated  
[ ] area_id mapping created  
[ ] Speed outliers handled  
[ ] Occupancy outliers handled  
[ ] Traffic light state standardized  
[ ] Traffic condition standardized  
[ ] Weather_Condition cleaned  
[ ] Accident_Report normalized  
[ ] Sentiment_Score normalized  
[ ] Parking_Availability standardized  
[ ] Emission and Energy fields validated  
[ ] Events merged into traffic  
[ ] Ready to build master_data.csv
