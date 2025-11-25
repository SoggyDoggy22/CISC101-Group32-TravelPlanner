> Change Log (2025-11-24): 
> - Implemented specifications for inputs.
> - Implemented a activity schema.
> - Amplemented selection thresholds per block for time and distance (in time).
> - Added gaurdrails.
> - Addededge-case handling for specific scenarios.
> - Implemented added changes to the loop (per day).
> - Defined the wanred output (per day).

### **Module 2 — Plan Builder (Options → Days)**

## Inputs (required for planning):

  Lodging location (coordinates or neighborhood; if missing, default to city center and warn user)
  
  Trip dates and daily time window (start/end hours)
  
  Mobility mode (walking, transit, rideshare, driving)
  
  Budget policy (per-day or total-trip cap)
  
  User preferences (themes, pace, accessibility needs)
  
  Weather forecast and holiday calendar

## Activity schema (each candidate includes):
  
  Type (attraction, restaurant, park, event, etc.)
  
  Theme tags (culture, food, nature, shopping, entertainment, wellness)
  
  Estimated duration (hours)
  
  Cost range (low, medium, high)
  
  Distance/time from anchor (lodging or prior activity)
  
  Opening hours and booking requirements
  
  Indoor/outdoor and weather sensitivity
  
  Accessibility notes (ramps, transit access, etc.)

## Selection thresholds (per block):
  
  Morning: ≤ 15 min from lodging; duration 2–3h
  
  Midday: ≤ 10–15 min from Morning activity; duration 1.5–2h
  
  Afternoon: ≤ 25–30 min from Midday; duration 2–3h; enforce different theme
  
  Evening: ≤ 20 min from Afternoon or lodging; duration 1.5–2h; restaurant or optional event

## Guardrails:
  
  Weather: Prefer indoor activities during poor conditions; maintain swap list for outdoor blocks
  
  Budget: Track cumulative per-day spend; downgrade or swap activities if nearing cap
  
  Missing data: If opening hours, cost, or distance are unknown, assume mid-range defaults and flag activity as “flex”
  
  Time overflow: If blocks exceed daily window, trim Midday first, compress Afternoon, mark Evening optional

## Edge-case handling:
  
  Holiday closures: Replace closed venues with markets, neighborhoods, or outdoor walks
  
  Accessibility needs: Prioritize flat routes, indoor ramps, shorter transfers, and transit-friendly options
  
  Public transport availability: Adjust proximity thresholds to reflect realistic travel times
  
  Unknown lodging: Default to city center anchor and warn user to provide lodging details
  
## Loop (per day):
  for each day:
    pick Morning activity (near lodging, open, fits budget, accessible)
    pick Midday activity (close by, open, fits budget, pace ok)
    pick Afternoon activity (different theme, open, within travel threshold)
    pick Evening restaurant or optional event (near lodging or Afternoon, open)
    
    apply weather guardrails
    apply budget guardrails
    apply missing data defaults
    handle edge cases (holiday closures, accessibility, transport, lodging)

## Output (per day):

  Morning: name, type, theme, duration, cost, distance/time, hours note
  
  Midday: same fields + transfer estimate
  
  Afternoon: same fields, ensure theme differs
  
  Evening: restaurant/event with reservation note + optional alternative
---
