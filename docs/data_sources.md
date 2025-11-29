# Data Sources — Smart City Mobility Intelligence Hub

## Overview
We will combine three data sources:
1. Traffic dataset — road traffic index / sensor counts / taxi trips
2. Weather dataset — temperature, humidity, precipitation
3. Events dataset — concerts, sports, public holidays, big gatherings

## Planned sources
- Traffic: Kaggle / City open data / public traffic APIs (choose one)
- Weather: OpenWeather API (free tier) or local government weather data
- Events: manual CSV (events.csv) created from city calendars or scraped

## File naming & paths
- data/raw/traffic_raw.csv
- data/raw/weather_raw.csv
- data/raw/events_raw.csv

## Target minimum size
- Combined master dataset: at least ~50,000 rows (time-series rows across locations/hours/days)

## Notes
- Include timestamp column in UTC and local time columns (hour, date)
- Ensure all sources have a common key to join by time and location (e.g., `timestamp`, `area_id`)
- Cache API responses to `data/raw/` to avoid rate-limit issues