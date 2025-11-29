# Area ID Mapping — Smart City Mobility Intelligence Hub

## Purpose
Map latitude/longitude clusters to human-friendly `area_id` values used across datasets (traffic, weather, events).

## Suggested area_ids and descriptions
- MYS_NORTH — Northern zone of Mysuru (includes major markets & suburbs)
- MYS_SOUTH — Southern zone of Mysuru (residential + industrial)
- MYS_CENTRE — City center (commercial district, main roads)
- MYS_PALACE — Mysore Palace & immediate vicinity
- MYS_STADIUM — City stadium and sports complex
- MYS_GROUNDS — Exhibition grounds, fairgrounds, large event areas
- MYS_UNIV — University / college cluster
- MYS_STATION — Main railway station area

## Strategy to create mapping
1. Inspect detector lat/longs and visually group nearby detectors into zones above.
2. If you use a grid/clustering method later, map each cluster id to one of the above `area_id` names.
3. Save final mapping as CSV when ready:
   - `docs/area_id_mapping.csv`
   - Columns: detector_id (or lat,lon), area_id

## Example mapping row format
detector_id,latitude,longitude,area_id
D123,12.3021,76.6553,MYS_PALACE
D124,12.3050,76.6478,MYS_CENTRE


## Notes
- Keep the mapping simple (6–9 zones is enough).
- Use the same `area_id` values in `events_raw.csv` so joins are straightforward.
