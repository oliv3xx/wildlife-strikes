# Wildlife Strikes Analysis

## Business Question
Which wildlife species, flight phases, and times of year are most likely to cause aircraft damage when struck, and what should an airport like Louisville (SDF) prioritize in its wildlife management?

*Original question focused on damage **cost**, but profiling showed cost fields are 98% empty. Pivoted to damage rate (% of strikes causing damage) as the main measure, with cost as supporting context where reported.
Added a local airport focus so the findings lead to a specific,
actionable recommendation.*

## Data Source
- FAA National Wildlife Strike Database (wildlife.faa.gov)
- Downloaded: 2026-11-12 (Excel format)
- 360,607 rows, incidents from 1/2/1990 to 11/9/2026

Data Dictionary
| Column | What it means | % Empty | Keep? |
|---|---|---|---|
| INCIDENT_DATE | Date of strike | 0% | Yes |
| AIRPORT_ID | Aiport code | <1% | Yes (group by this) |
| AIRPORT | Airport name | 0% | Yes (for labels) |
| STATE | State | 14% | Yes |
| AIRCRAFT | Aircraft type | 0% | Maybe |
| PHASE_OF_FLIGHT | Takeoff, approach, etc | 40% | Yes |
| HEIGHT | Feet above ground at time of strike | 30% | Yes (to separate on-airport vs. off-airport strikes) |
| COST_REPAIRS_INFL_ADJ | Repair cost, today's $ | 98% | YES |
| COST_OTHER_INFL_ADJ | Other costs (delays, etc), today's $ | 98% | Yes |
| COST_REPAIRS / COST_OTHER | Unadjusted costs | 98% | No (adjusted version replace them) |
| INDICATED_DAMAGE | Damaged yes/no | 0% | Yes |
| DAMAGE_LEVEL | Severity code | 35% | Yes |
| STR_/DAM_ columns | Part struck/damaged | 0% but a lot of 0s | Maybe |
| SPECIES_ID/SPECIES | Animal | 0% | Yes |

## Decisions & Caveats
- Strike reporting is voluntary, so counts reflect reporting habits, not just actual strikes. FAA warns airport/airline comparisons may be misleading.
- Cost fields are 98% empty, so damage rate and severity are the primary measures. Cost is used only as supporting context where reported.
- Using inflation-adjusted cost columns so costs are comparable across years.
- Filtered to 2016–2025 (10 complete years); 2026 excluded as partial.
- Blank STATE, PHASE_OF_FLIGHT, and DAMAGE_LEVEL values kept and labeled "Unknown" instead of deleted, so damage counts stay complete.
- Grouping by AIRPORT_ID and SPECIES_ID instead of names to avoid spelling inconsistencies.
- Using damage rate (% of strikes causing damage) instead of raw counts, since counts reflect reporting habits and strike frequency, not danger.
- Following the approach FAA/USDA researchers use to rank species hazard (% damage, % substantial damage).
- HEIGHT is 30% empty. Strikes at or below 500 ft are classified as
on-airport (threshold used in FAA/USDA research). When HEIGHT is blank, ground phases (taxi, takeoff run, landing roll, parked) count as on-airport; the rest are Unknown.

## Log
- 2026-11-12: Set up project, downloaded data, profiled columns in Power Query