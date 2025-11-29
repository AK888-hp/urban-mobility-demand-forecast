# Event Dataset Builder — Smart City Mobility Intelligence Hub

## Purpose
Describe a reproducible, realistic process to build an events dataset for the mobility project. This document contains the schema, rules, and clear pseudo-code so anyone can recreate or extend the events dataset.

---

## 1) Files & Locations
- Final CSV path: `data/raw/events_raw.csv`
- Backup / versioning: `data/raw/events_raw_v1.csv`
- Sample preview: `docs/figures/events_preview.png` (optional)

---

## 2) Schema (final)
event_id,date,start_time,end_time,event_type,area_id,attendance_estimate,description,recurrence

markdown
Copy code

### Field definitions
- `event_id` — integer or unique string ID
- `date` — ISO date `YYYY-MM-DD`
- `start_time` — `HH:MM` (24-hour)
- `end_time` — `HH:MM` (24-hour)
- `event_type` — categorical; examples: `public_holiday`, `cultural_event`, `sports_event`, `concert`, `education_event`, `marathon`
- `area_id` — string; must match values in `docs/area_id_mapping.csv` (e.g., `MYS_CENTRE`)
- `attendance_estimate` — integer (approx attendees)
- `description` — short text
- `recurrence` — optional: `none`, `weekly`, `annual`, `monthly`

---

## 3) Data Sources & Rules
1. **Authoritative references**
   - City calendars (municipal websites)
   - College / stadium event pages
   - Public holiday calendars (national/state)
2. **Sensible assumptions**
   - When attendance unknown, use conservative estimate for type:
     - `public_holiday` → city-wide large number (e.g., 100k+)
     - `cultural_event` → 5k–50k depending on location
     - `concert` → 3k–20k
     - `sports_event` → 5k–30k
     - `education_event` → 500–5k
   - If event spans midnight (e.g., festival), set start/end across dates or treat as whole-day event.
3. **Timezone**
   - All dates/times must be local time (city timezone). Save as local; mention timezone in README.

---

## 4) Quality Rules
- No duplicate event rows (same date, same area, same description).
- Attendance must be > 0 for event types not `none`.
- `start_time` < `end_time` (unless event spans midnight — handle as `end_time` on next day or mark `recurrence`).
- `area_id` must exist in `docs/area_id_mapping.csv`. If not, use `MYS_CITY` as fallback.

---

## 5) Minimal Realism Heuristics
- Add at least one major city festival per year (e.g., Mysuru Dasara).
- Add weekly recurring attractions (e.g., weekend exhibitions) with `recurrence=weekly` (optional).
- Add education exam periods as `education_event` with a lower attendance estimate but wide timespan (multiple days).

---

## 6) Manual Build Steps (no code required)
1. Create the file `data/raw/events_raw.csv` with header row (matching schema).
2. Populate 20–50 rows for 6–12 months (mix of holidays, festivals, sports, college events).
3. Ensure `area_id` values match `docs/area_id_mapping.csv`.
4. Save, then create a backup copy `data/raw/events_raw_v1.csv`.
5. Commit to git (but do NOT upload raw dataset to a public repo if the events file contains private data).

---

## 7) Automated (Pseudo-code) — optional when you want to generate programmatically
This is pseudo-code to help later automation (not runnable code):

PSEUDO
load holiday_calendar for country/state
load city_event_list (scraped or manual)
define default_attendance_by_type = {public_holiday:500000, cultural_event:20000, concert:10000, sports_event:15000, education_event:2000}
events = []

for date in date_range:
if date in holiday_calendar:
events.append(create_row(date, "00:00", "23:59", "public_holiday", "MYS_CITY", default_attendance...))
for raw_event in city_event_list:
# raw_event contains title, date, start, end, area
attendance = raw_event.get("attendance") or default_attendance_by_type[raw_event.type]
events.append(mapping_row...)

deduplicate and validate area_id
save events as CSV

yaml
Copy code

---

## 8) Example Rows
event_id,date,start_time,end_time,event_type,area_id,attendance_estimate,description,recurrence
1,2025-10-15,18:00,22:00,cultural_event,MYS_CENTRE,50000,Dasara Procession,annual
2,2025-07-20,06:00,12:00,sports_event,MYS_STADIUM,10000,Mysuru Marathon,none
3,2025-08-15,09:00,12:00,public_holiday,MYS_CITY,600000,Independence Day,annual

yaml
Copy code

---

## 9) Notes on Ethics & Sharing
- Events dataset can be shared publicly; however if attendance estimates are sensitive, consider redacting precise counts.
- Always document that events are synthetic or manually aggregated.
- Cite sources for any scraped or third-party data.

---

## 10) Versioning & Provenance
- Include a `generated_on` timestamp in the CSV header or a separate `docs/events_metadata.md` describing:
  - who created it
  - which sources were used
  - date of creation
  - version tag (`v1.0`)

---

## 11) Next steps after building
- Merge events into traffic as per `traffic_cleaning_plan.md`
- Use `event_active` binary and `attendance_estimate` as features in modeling
- Optionally create rolling buffers (e.g., event influence for ±2 hours) and encode as features
