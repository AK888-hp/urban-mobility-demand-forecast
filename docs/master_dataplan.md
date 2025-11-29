# Master Data Construction Plan — Smart City Mobility Intelligence Hub

## 1) Purpose
The master dataset will combine:
- cleaned traffic data
- parsed weather data
- events dataset
into a single table used for modeling and analysis.

## 2) Final Columns in master_data.csv

### Time features
- timestamp (cleaned)
- date
- hour
- day_of_week
- is_weekend

### Location features
- latitude
- longitude
- area_id

### Traffic features
- vehicle_count
- traffic_speed_kmh
- road_occupancy_pct
- traffic_light_state
- traffic_condition

### Contextual features
- weather_condition
- accident_report
- sentiment_score
- ride_sharing_demand
- parking_availability
- emission_levels_g_km
- energy_consumption_l_h

### Event features
- event_active
- event_type
- attendance_estimate
- event_area_id

## 3) Merging Logic

### Traffic + Weather
- Join on timestamp (rounded to the nearest hour if needed)
- If weather data has lower frequency, forward-fill or nearest-value fill

### Traffic + Events
- Match on same date
- A traffic row is part of an event if:
    event.start_time <= traffic.timestamp.time <= event.end_time
- If multiple events overlap in the same area:
    keep the event with the highest attendance_estimate

## 4) Area ID Integration
- Use docs/area_id_mapping.csv to assign area_id to each traffic row.
- Match each traffic row by nearest latitude/longitude.
- If exact match isn't available, assign the closest area manually.
- Final master dataset will have a clean `area_id` column.


### Final merge order
1. Start with cleaned traffic data
2. Merge weather
3. Merge events
4. Save result as master_data.csv

## 5) Final Master Dataset Checklist

[ ] Traffic data cleaned  
[ ] Weather data prepared  
[ ] Events data merged  
[ ] area_id assigned using mapping  
[ ] All timestamps standardized  
[ ] All categorical fields normalized  
[ ] Outliers handled  
[ ] Missing values handled  
[ ] Final master_data.csv generated in data/processed/
