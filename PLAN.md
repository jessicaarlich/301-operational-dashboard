## Operational Dashboard Plan
A single analytics dashboard showing business metrics for Healthcare Hospital. This should be a simple analytics view. The primary audience for this is a department director who needs information to make important decisions.

# Data
Generate 12 months of realistic fake hospital data from January 2025 through December 2025. Return the data as a JSON file.

Include the following metrics for each month:

**Patient Volume**
- Total patient visits
- Emergency department visits
- Inpatient admissions

**Bed Occupancy**
- Overall bed occupancy rate (%)
- ICU bed occupancy rate (%)

**Wait Times**
- Average ED wait time in minutes
- Average time from ED to inpatient bed in hours

**Staffing Levels**
- Total staff count
- Nursing staff count
- Staff-to-patient ratio
- Vacancy rate (%)
- Total overtime hours

Make the data realistic by following these rules:
- Higher patient volumes in winter months and flu season, lower in summer
- When patient volume goes up, bed occupancy and wait times should also go up
- Overtime hours should increase when patient volume is high
- Staffing should dip slightly in summer and around holidays due to vacations
- Add some natural randomness so the data doesn't look too perfect or predictable

Base the numbers on a mid-size community hospital with around 250 beds.

# Layout
- v-app-bar at the top with the dashboard title and company name
- there should also be a month picker in the bar
- the month picker should default to showing ALL months
- when a specific month is selected, all charts and cards will filter to that month
- when ALL is selected, it will show the average of all months
- below the app bar, show a row with two charts: patient volume and bed occupancy.
- below that, show another row with two more charts: wait times and staffing levels.
- use v-container, v-row, and v-col for a responsive grid layout

# Interactions
- the month picker filters everything, all charts
- when a month is selected, all charts highlight that particular month
- when "all" is selected, the charts show yearly totals and averages

# Style
- dark theme by default (Vuetify dark theme)
- clean, minimal with lots of white space
- accessible using wcag standards
- charts should use a cohesive color palette
- needs to be mobile-responsive

# Tech
- vue 3, typescript and vuetify 3
- chart.js via vue-charts for all charts
- fake data from the local JSON file
- single page chart