# Healthcare Hospital Operations Dashboard

A real-time operational analytics dashboard designed to help hospital department directors make data-driven staffing decisions based on patient volume, bed occupancy, wait times, and current staffing levels.

## About

This project was built as part of the Protogen Academy 301 capstone. It demonstrates a modern Vue 3 + TypeScript + Vuetify single-page application with interactive data visualization using Chart.js.

## Purpose

The dashboard enables hospital directors to:
- **Monitor demand signals** (patient volume, ED visits, wait times)
- **Assess capacity constraints** (bed occupancy, staff-to-patient ratio)
- **Identify system strain** (vacancy rates, overtime hours)
- **Make informed staffing decisions** across the hospital with budget awareness

The dashboard focuses specifically on staffing optimization—helping directors understand whether they need to increase, decrease, or reallocate staff based on real workload patterns and seasonal trends.

## Features

- 📊 **Interactive Charts** – Patient volume, bed occupancy, wait times, and staffing trends across 12 months
- 📅 **Month-based Filtering** – View individual months or aggregate metrics across the full year
- 🎨 **Dark Theme** – Modern, accessible dark interface optimized for long viewing sessions
- 📱 **Responsive Design** – Works seamlessly on desktop, tablet, and mobile devices
- 🎯 **Decision-focused Layout** – Organized signal → lever → strain flow to guide decision-making

## Tech Stack

- **Frontend Framework**: Vue 3 with TypeScript
- **UI Components**: Vuetify 3
- **Data Visualization**: Chart.js with vue-chartjs
- **Build Tool**: Vite
- **Data**: Mock hospital metrics (JSON)

## Prerequisites

- **Node.js** 16.x or higher
- **npm** 7.x or higher

## Installation

1. **Clone or download the repository**
   ```bash
   cd 301-operational-dashboard
   ```

2. **Install dependencies**
   ```bash
   npm install
   ```

## Running Locally

Start the development server:

```bash
npm run dev
```

The application will be available at `http://localhost:5173` (or the next available port if 5173 is in use).

The dev server includes:
- Hot module replacement (HMR) – changes appear instantly
- TypeScript compilation with type checking
- Development-optimized build

## Building for Production

Create an optimized production build:

```bash
npm run build
```

The compiled output will be in the `dist/` directory.

Preview the production build locally:

```bash
npm run preview
```

## Project Structure

```
301-operational-dashboard/
├── src/
│   ├── components/
│   │   ├── Dashboard.vue       # Main dashboard component
│   │   └── HelloWorld.vue      # Example component
│   ├── data/
│   │   └── hospital-metrics-2025.json  # Mock hospital data
│   ├── App.vue                 # Root Vue component
│   ├── main.ts                 # Application entry point
│   └── style.css               # Global styles
├── public/                      # Static assets
├── package.json                # Dependencies and scripts
├── vite.config.ts              # Vite configuration
├── tsconfig.json               # TypeScript configuration
├── PLAN.md                      # Project planning document
└── README.md                    # This file
```

## How It Works

### Data Flow
1. Hospital metrics data is loaded from `src/data/hospital-metrics-2025.json`
2. The Dashboard component processes and filters data based on the selected month
3. Chart.js visualizations render the filtered data
4. Metric cards display aggregated statistics

### Viewing Modes
- **All Months (2025)** – Shows yearly averages and 12-month trends. Perfect for identifying seasonal patterns.
- **Individual Month** – Displays single-month snapshot with total and average values for tactical decision-making.

### Metric Organization
The dashboard groups metrics into three decision-focused sections:

**Demand & Stress Signals** (What's happening?)
- Patient Visits, ED Visits, ED Wait Time

**Capacity & Staffing Levers** (What tools do I have?)
- Bed Occupancy, Staff Count, Staff-to-Patient Ratio, Nursing Staff

**Strain Indicators** (What's the cost?)
- Vacancy Rate, Overtime Hours

## Key Insights from the Data

The mock dataset (January–December 2025) simulates realistic hospital behavior:
- **Winter peaks**: Higher patient volumes and wait times (flu season, cold-related injuries)
- **Summer dips**: Lower volumes, better wait times
- **Staffing patterns**: Staff count remains relatively stable; seasonal variation comes from vacation schedules
- **Overtime correlation**: Rises when patient volume spikes (system strain indicator)

## Design Decisions

- **Single-page dashboard**: Keeps the director focused on one core job—staffing optimization
- **12-month view**: Enables pattern recognition and evaluation of past staffing decisions
- **Staff-to-Patient Ratio highlighted**: Direct connection between workload and staffing constraints
- **Dark theme**: Reduces eye strain for extended monitoring sessions
- **No finance/quality metrics**: Deliberately scoped to prevent distraction from core staffing decisions

## Known Limitations

- Mock data only (replace `hospital-metrics-2025.json` with real data source for production)
- Single-month selection shows charts with limited data points (single-value scaling edge case)
- No historical comparison or year-over-year analysis
- No user authentication or role-based access control

## Future Enhancements

- Real-time data integration with hospital EHR/staffing systems
- Department-level drill-down views
- Predictive staffing recommendations based on trends
- Custom date range selection beyond monthly granularity
- Export functionality for reports
- Mobile app version

## Contributing

This is a capstone project. If you have feedback or suggestions, feel free to reach out.

## License

Internal project – Protogen Academy 301 Capstone
