# Change Log — 2025-11-24

- Implemented specifications for inputs  
- Implemented an activity schema  
- Implemented selection thresholds per block for time and distance  
- Added guardrails  
- Added edge-case handling for specific scenarios  
- Implemented changes to the daily loop logic  
- Defined the wanted output (per day)

---

# **Module 2 — Plan Builder (Options → Days)**

## Inputs (Required for Planning)

- Lodging location (coordinates or neighborhood; if missing, default to city center and warn user)  
- Trip dates and daily time window (start/end hours)  
- Mobility mode (walking, transit, rideshare, driving)  
- Budget policy (per-day or total-trip cap)  
- User preferences (themes, pace, accessibility needs)  
- Weather forecast and holiday calendar

---

## Activity Schema (Per Candidate)

- Type (attraction, restaurant, park, event, etc.)  
- Theme tags (culture, food, nature, shopping, entertainment, wellness)  
- Estimated duration (hours)  
- Cost range (low, medium, high)  
- Distance/time from anchor (lodging or prior activity)  
- Opening hours and booking requirements  
- Indoor/outdoor and weather sensitivity  
- Accessibility notes (ramps, transit access, etc.)

---

## Selection Thresholds (Per Time Block)

- **Morning:**  
  - ≤ 15 min from lodging  
  - Duration 2–3h

- **Midday:**  
  - ≤ 10–15 min from Morning activity  
  - Duration 1.5–2h

- **Afternoon:**  
  - ≤ 25–30 min from Midday activity  
  - Duration 2–3h  
  - Must differ in theme

- **Evening:**  
  - ≤ 20 min from Afternoon or lodging  
  - Duration 1.5–2h  
  - Restaurant or optional event

---

## Guardrails

- **Weather:**  
  Prefer indoor activities during poor conditions; maintain swap list for outdoor blocks.

- **Budget:**  
  Track cumulative per-day spend; downgrade or swap activities if nearing cap.

- **Missing Data:**  
  If opening hours, cost, or distance are unknown, assume mid-range defaults and flag activity as *flex*.

- **Time Overflow:**  
  If activities exceed daily window:  
  1. Trim Midday first  
  2. Compress Afternoon  
  3. Mark Evening optional

---

## Edge-Case Handling

- **Holiday closures:**  
  Replace closed venues with markets, neighborhoods, or outdoor walks.

- **Accessibility needs:**  
  Prioritize flat routes, ramps, shorter transfers, and transit-friendly options.

- **Public transport availability:**  
  Adjust proximity thresholds to realistic travel times.

- **Unknown lodging:**  
  Default to city center anchor and warn user.

---

## Daily Loop (Pseudocode): 
```
for each day:
pick Morning activity (near lodging, open, fits budget, accessible)
pick Midday activity (close by, open, fits budget, pace ok)
pick Afternoon activity (different theme, open, within travel threshold)
pick Evening restaurant or optional event (near lodging or Afternoon, open)
  
apply weather guardrails
apply budget guardrails
apply missing data defaults
handle edge cases (holiday closures, accessibility, transport, lodging) 
```
  ---

## Output (Per Day)

- **Morning:**  
  Name, type, theme, duration, cost, distance/time, opening hours note

- **Midday:**  
  Same fields + estimated transfer time

- **Afternoon:**  
  Same fields, ensure theme differs

- **Evening:**  
  Restaurant/event with reservation note + optional alternative
