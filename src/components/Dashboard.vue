<script setup lang="ts">
import { ref, computed } from 'vue'
import { Bar } from 'vue-chartjs'
import {
  Chart as ChartJS,
  CategoryScale,
  LinearScale,
  BarElement,
  Title,
  Tooltip,
  Legend,
} from 'chart.js'
import hospitalData from '../data/hospital-metrics-2025.json'

ChartJS.register(
  CategoryScale,
  LinearScale,
  BarElement,
  Title,
  Tooltip,
  Legend
)

interface HospitalMetric {
  month: string
  totalPatientVisits: number
  emergencyDepartmentVisits: number
  inpatientAdmissions: number
  overallBedOccupancyRate: number
  icuBedOccupancyRate: number
  averageEdWaitTimeMinutes: number
  averageEdToInpatientBedTimeHours: number
  totalStaffCount: number
  nursingStaffCount: number
  staffToPatientRatio: number
  vacancyRate: number
  totalOvertimeHours: number
}

const selectedMonth = ref<string>('ALL')
const months = ref<string[]>([
  'ALL',
  '2025-01',
  '2025-02',
  '2025-03',
  '2025-04',
  '2025-05',
  '2025-06',
  '2025-07',
  '2025-08',
  '2025-09',
  '2025-10',
  '2025-11',
  '2025-12',
])

const monthLabels = {
  ALL: 'All Months (2025)',
  '2025-01': 'January',
  '2025-02': 'February',
  '2025-03': 'March',
  '2025-04': 'April',
  '2025-05': 'May',
  '2025-06': 'June',
  '2025-07': 'July',
  '2025-08': 'August',
  '2025-09': 'September',
  '2025-10': 'October',
  '2025-11': 'November',
  '2025-12': 'December',
}

// Always use all data
const allData = computed(() => hospitalData as HospitalMetric[])

// Get selected month data for metric cards
const selectedMonthData = computed(() => {
  if (selectedMonth.value === 'ALL') {
    return null
  }
  const found = allData.value.find((d) => d.month === selectedMonth.value)
  return found || null
})

// All month labels for chart
const allMonthLabels = computed(() => 
  hospitalData.map((d) => (monthLabels as any)[d.month])
)

// Helper function to get colors - highlight selected month
const getBarColors = (isSecondary: boolean = false) => {
  const colors = isSecondary ? '#FF6B6B' : '#7E57C2'
  const dimmedColors = isSecondary ? 'rgba(255, 107, 107, 0.3)' : 'rgba(126, 87, 194, 0.3)'
  
  return allData.value.map((d) => {
    const isSelected = d.month === selectedMonth.value
    if (selectedMonth.value === 'ALL') {
      return colors
    }
    return isSelected ? colors : dimmedColors
  })
}

// Patient Volume Chart Data
const patientVolumeChartData = computed(() => ({
  labels: allMonthLabels.value,
  datasets: [
    {
      label: 'Total Patient Visits',
      data: allData.value.map((d) => d.totalPatientVisits),
      backgroundColor: getBarColors(false),
      borderColor: '#7E57C2',
      borderWidth: selectedMonth.value === 'ALL' ? 0 : 1,
    },
    {
      label: 'ED Visits',
      data: allData.value.map((d) => d.emergencyDepartmentVisits),
      backgroundColor: getBarColors(true),
      borderColor: '#FF6B6B',
      borderWidth: selectedMonth.value === 'ALL' ? 0 : 1,
    },
  ],
}))

// Bed Occupancy Chart Data
const bedOccupancyChartData = computed(() => ({
  labels: allMonthLabels.value,
  datasets: [
    {
      label: 'Overall Bed Occupancy %',
      data: allData.value.map((d) => d.overallBedOccupancyRate),
      backgroundColor: allData.value.map((d) => {
        const isSelected = d.month === selectedMonth.value
        if (selectedMonth.value === 'ALL') return '#4ECDC4'
        return isSelected ? '#4ECDC4' : 'rgba(78, 205, 196, 0.3)'
      }),
      borderColor: '#4ECDC4',
      borderWidth: selectedMonth.value === 'ALL' ? 0 : 1,
    },
    {
      label: 'ICU Bed Occupancy %',
      data: allData.value.map((d) => d.icuBedOccupancyRate),
      backgroundColor: allData.value.map((d) => {
        const isSelected = d.month === selectedMonth.value
        if (selectedMonth.value === 'ALL') return '#FFD93D'
        return isSelected ? '#FFD93D' : 'rgba(255, 217, 61, 0.3)'
      }),
      borderColor: '#FFD93D',
      borderWidth: selectedMonth.value === 'ALL' ? 0 : 1,
    },
  ],
}))

// Wait Times Chart Data
const waitTimesChartData = computed(() => ({
  labels: allMonthLabels.value,
  datasets: [
    {
      label: 'ED Wait Time (minutes)',
      data: allData.value.map((d) => d.averageEdWaitTimeMinutes),
      backgroundColor: getBarColors(false),
      borderColor: '#FF6B6B',
      borderWidth: selectedMonth.value === 'ALL' ? 0 : 1,
    },
    {
      label: 'ED to Bed Time (hours)',
      data: allData.value.map((d) => d.averageEdToInpatientBedTimeHours),
      backgroundColor: allData.value.map((d) => {
        const isSelected = d.month === selectedMonth.value
        if (selectedMonth.value === 'ALL') return '#95E1D3'
        return isSelected ? '#95E1D3' : 'rgba(149, 225, 211, 0.3)'
      }),
      borderColor: '#95E1D3',
      borderWidth: selectedMonth.value === 'ALL' ? 0 : 1,
    },
  ],
}))

// Staffing Levels Chart Data
const staffingChartData = computed(() => ({
  labels: allMonthLabels.value,
  datasets: [
    {
      label: 'Total Staff',
      data: allData.value.map((d) => d.totalStaffCount),
      backgroundColor: getBarColors(false),
      borderColor: '#7E57C2',
      borderWidth: selectedMonth.value === 'ALL' ? 0 : 1,
    },
    {
      label: 'Nursing Staff',
      data: allData.value.map((d) => d.nursingStaffCount),
      backgroundColor: allData.value.map((d) => {
        const isSelected = d.month === selectedMonth.value
        if (selectedMonth.value === 'ALL') return '#F38181'
        return isSelected ? '#F38181' : 'rgba(243, 129, 129, 0.3)'
      }),
      borderColor: '#F38181',
      borderWidth: selectedMonth.value === 'ALL' ? 0 : 1,
    },
  ],
}))

const chartOptions = {
  indexAxis: 'x' as const,
  responsive: true,
  maintainAspectRatio: true,
  interaction: {
    intersect: false,
    mode: 'index' as const,
  },
  plugins: {
    legend: {
      display: true,
      labels: {
        color: '#E0E0E0',
        font: {
          size: 12,
          weight: 'normal' as const,
        },
      },
    },
    tooltip: {
      backgroundColor: 'rgba(0, 0, 0, 0.8)',
      titleColor: '#FFFFFF',
      bodyColor: '#E0E0E0',
      borderColor: '#555',
      borderWidth: 1,
      padding: 8,
      displayColors: true,
    },
  },
  scales: {
    x: {
      ticks: {
        color: '#B0B0B0',
        font: {
          size: 10,
        },
        maxRotation: 45,
        minRotation: 45,
      },
      grid: {
        color: 'rgba(200, 200, 200, 0.1)',
      },
    },
    y: {
      beginAtZero: true,
      ticks: {
        color: '#B0B0B0',
        font: {
          size: 11,
        },
      },
      grid: {
        color: 'rgba(200, 200, 200, 0.1)',
      },
    },
  },
}

const getMetricCard = (label: string, value: string | number, unit: string = '') => ({
  label,
  value,
  unit,
})

// Demand & Stress Signals
const demandCards = computed(() => {
  if (selectedMonth.value === 'ALL') {
    const data = allData.value
    const avgTotalVisits = Math.round(data.reduce((sum, d) => sum + d.totalPatientVisits, 0) / data.length)
    const avgEdVisits = Math.round(data.reduce((sum, d) => sum + d.emergencyDepartmentVisits, 0) / data.length)
    const avgWaitTime = Math.round(data.reduce((sum, d) => sum + d.averageEdWaitTimeMinutes, 0) / data.length)

    return [
      getMetricCard('Patient Visits (Avg/Month)', avgTotalVisits),
      getMetricCard('ED Visits (Avg/Month)', avgEdVisits),
      getMetricCard('ED Wait Time (Avg Monthly)', avgWaitTime, 'min'),
    ]
  } else {
    const data = selectedMonthData.value
    if (!data) {
      return [
        getMetricCard('Total Patient Visits', '—'),
        getMetricCard('ED Visits', '—'),
        getMetricCard('ED Wait Time', '—', 'min'),
      ]
    }
    return [
      getMetricCard('Patient Visits (Month Total)', data.totalPatientVisits),
      getMetricCard('ED Visits (Month Total)', data.emergencyDepartmentVisits),
      getMetricCard('ED Wait Time (Monthly Avg)', data.averageEdWaitTimeMinutes, 'min'),
    ]
  }
})

// Capacity & Staffing Levers (with staff-to-patient ratio prominent)
const capacityCards = computed(() => {
  if (selectedMonth.value === 'ALL') {
    const data = allData.value
    const avgOccupancy = (data.reduce((sum, d) => sum + d.overallBedOccupancyRate, 0) / data.length).toFixed(1)
    const avgStaff = Math.round(data.reduce((sum, d) => sum + d.totalStaffCount, 0) / data.length)
    const avgNursingStaff = Math.round(data.reduce((sum, d) => sum + d.nursingStaffCount, 0) / data.length)

    return [
      getMetricCard('Bed Occupancy (Avg Monthly Rate)', avgOccupancy, '%'),
      getMetricCard('Staff Count (Avg/Month)', avgStaff),
      getMetricCard('Nursing Staff (Avg/Month)', avgNursingStaff),
    ]
  } else {
    const data = selectedMonthData.value
    if (!data) {
      return [
        getMetricCard('Bed Occupancy', '—', '%'),
        getMetricCard('Staff Count', '—'),
        getMetricCard('Nursing Staff', '—'),
      ]
    }
    return [
      getMetricCard('Bed Occupancy (Monthly Rate)', data.overallBedOccupancyRate, '%'),
      getMetricCard('Staff Count (Month Snapshot)', data.totalStaffCount),
      getMetricCard('Nursing Staff (Month Snapshot)', data.nursingStaffCount),
    ]
  }
})

// Strain Indicators
const strainCards = computed(() => {
  if (selectedMonth.value === 'ALL') {
    const data = allData.value
    const avgVacancy = (data.reduce((sum, d) => sum + d.vacancyRate, 0) / data.length).toFixed(1)
    const totalOvertime = Math.round(data.reduce((sum, d) => sum + d.totalOvertimeHours, 0))

    return [
      getMetricCard('Vacancy Rate (Avg Monthly)', avgVacancy, '%'),
      getMetricCard('Overtime Hours (2025 Total)', totalOvertime),
    ]
  } else {
    const data = selectedMonthData.value
    if (!data) {
      return [
        getMetricCard('Vacancy Rate', '—', '%'),
        getMetricCard('Overtime Hours', '—'),
      ]
    }
    return [
      getMetricCard('Vacancy Rate (Monthly Rate)', data.vacancyRate, '%'),
      getMetricCard('Overtime Hours (Month Total)', data.totalOvertimeHours),
    ]
  }
})

// Prominent Staff-to-Patient Ratio
const staffRatioMetric = computed(() => {
  if (selectedMonth.value === 'ALL') {
    const data = allData.value
    const avgStaffRatio = (data.reduce((sum, d) => sum + d.staffToPatientRatio, 0) / data.length).toFixed(2)
    return avgStaffRatio
  } else {
    const data = selectedMonthData.value
    if (!data) return '—'
    return data.staffToPatientRatio.toFixed(2)
  }
})
</script>

<template>
  <v-app class="dashboard-app">
    <!-- App Bar -->
    <v-app-bar flat color="#1E1E1E" class="app-bar">
      <v-app-bar-title class="font-weight-bold text-h5">
        <span class="accent-color">Healthcare Hospital</span> Operations Dashboard
      </v-app-bar-title>
      <v-spacer></v-spacer>

      <!-- Month Picker -->
      <v-select
        v-model="selectedMonth"
        :items="months"
        label="Select Month"
        outlined
        dense
        hide-details
        class="month-picker"
        :item-title="(item) => monthLabels[item as keyof typeof monthLabels]"
        :item-value="(item) => item"
      ></v-select>
    </v-app-bar>

    <!-- Main Content -->
    <v-main class="pa-6 pt-16">
      <v-container fluid>
        <!-- Page Title and Description -->
        <v-row class="mb-6">
          <v-col cols="12">
            <h1 class="text-h4 font-weight-bold mb-2">Dashboard Overview</h1>
            <p class="text-subtitle1">
              Viewing: <strong>{{ monthLabels[selectedMonth as keyof typeof monthLabels] }}</strong>
            </p>
            <p class="text-caption text-grey mt-1">
              Timeframe: metrics are monthly by default. In "All Months (2025)", values are average per month unless labeled as a yearly total.
            </p>
          </v-col>
        </v-row>

        <!-- SECTION 1: Demand & Stress Signals -->
        <v-row class="mb-2">
          <v-col cols="12">
            <h2 class="text-subtitle1 font-weight-bold text-grey mb-3">Demand & Stress Signals</h2>
          </v-col>
        </v-row>
        <v-row class="mb-6">
          <v-col
            v-for="(card, index) in demandCards"
            :key="`demand-${index}`"
            cols="12"
            sm="6"
            md="4"
          >
            <v-card class="metric-card signal-card" elevation="1">
              <div class="pa-4">
                <div class="text-caption text-grey">{{ card.label }}</div>
                <div class="text-h6 font-weight-bold mt-2">
                  {{ card.value }}<span class="text-caption ml-1">{{ card.unit }}</span>
                </div>
              </div>
            </v-card>
          </v-col>
        </v-row>

        <!-- CHARTS ROW 1: Signal Indicators (Patient Volume + Wait Times) -->
        <v-row class="mb-6">
          <v-col cols="12" md="6">
            <v-card class="chart-card" elevation="1">
              <div class="pa-4">
                <h3 class="text-h6 font-weight-bold mb-4">Patient Volume Trends</h3>
                <Bar :key="`pv-${selectedMonth}`" :data="patientVolumeChartData" :options="chartOptions" />
              </div>
            </v-card>
          </v-col>
          <v-col cols="12" md="6">
            <v-card class="chart-card" elevation="1">
              <div class="pa-4">
                <h3 class="text-h6 font-weight-bold mb-4">Wait Times</h3>
                <Bar :key="`wt-${selectedMonth}`" :data="waitTimesChartData" :options="chartOptions" />
              </div>
            </v-card>
          </v-col>
        </v-row>

        <!-- SECTION 2: Capacity & Staffing Levers (with prominent Staff-to-Patient Ratio) -->
        <v-row class="mb-2">
          <v-col cols="12">
            <h2 class="text-subtitle1 font-weight-bold text-grey mb-3">Capacity & Staffing Levers</h2>
          </v-col>
        </v-row>
        <v-row class="mb-6">
          <!-- Prominent Staff-to-Patient Ratio -->
          <v-col cols="12" sm="6" md="4">
            <v-card class="metric-card lever-card prominent-card" elevation="2">
              <div class="pa-4">
                <div class="text-caption text-grey">Staff-to-Patient Ratio</div>
                <div class="text-h4 font-weight-bold mt-3 lever-highlight">{{ staffRatioMetric }}</div>
                <div class="text-caption text-grey mt-2">← Staff per patient</div>
              </div>
            </v-card>
          </v-col>
          <!-- Other Capacity Cards -->
          <v-col
            v-for="(card, index) in capacityCards"
            :key="`capacity-${index}`"
            cols="12"
            sm="6"
            md="4"
          >
            <v-card class="metric-card lever-card" elevation="1">
              <div class="pa-4">
                <div class="text-caption text-grey">{{ card.label }}</div>
                <div class="text-h6 font-weight-bold mt-2">
                  {{ card.value }}<span class="text-caption ml-1">{{ card.unit }}</span>
                </div>
              </div>
            </v-card>
          </v-col>
        </v-row>

        <!-- CHARTS ROW 2: Lever Indicators (Bed Occupancy + Staffing) -->
        <v-row class="mb-6">
          <v-col cols="12" md="6">
            <v-card class="chart-card" elevation="1">
              <div class="pa-4">
                <h3 class="text-h6 font-weight-bold mb-4">Bed Occupancy Rates</h3>
                <Bar :key="`bo-${selectedMonth}`" :data="bedOccupancyChartData" :options="chartOptions" />
              </div>
            </v-card>
          </v-col>
          <v-col cols="12" md="6">
            <v-card class="chart-card" elevation="1">
              <div class="pa-4">
                <h3 class="text-h6 font-weight-bold mb-4">Staffing Levels</h3>
                <Bar :key="`sl-${selectedMonth}`" :data="staffingChartData" :options="chartOptions" />
              </div>
            </v-card>
          </v-col>
        </v-row>

        <!-- SECTION 3: Strain Indicators -->
        <v-row class="mb-2">
          <v-col cols="12">
            <h2 class="text-subtitle1 font-weight-bold text-grey mb-3">Strain Indicators</h2>
          </v-col>
        </v-row>
        <v-row>
          <v-col
            v-for="(card, index) in strainCards"
            :key="`strain-${index}`"
            cols="12"
            sm="6"
            md="4"
          >
            <v-card class="metric-card strain-card" elevation="1">
              <div class="pa-4">
                <div class="text-caption text-grey">{{ card.label }}</div>
                <div class="text-h6 font-weight-bold mt-2">
                  {{ card.value }}<span class="text-caption ml-1">{{ card.unit }}</span>
                </div>
              </div>
            </v-card>
          </v-col>
        </v-row>
      </v-container>
    </v-main>
  </v-app>
</template>

<style scoped>
.dashboard-app {
  background-color: #121212;
}

.app-bar {
  border-bottom: 1px solid rgba(255, 255, 255, 0.12);
}

.accent-color {
  color: #7e57c2;
  font-weight: bold;
}

.month-picker {
  min-width: 180px;
  margin-right: 10px;
}

.metric-card {
  background-color: #1e1e1e;
  border-left: 4px solid #7e57c2;
  transition: all 0.3s ease;
}

.metric-card:hover {
  transform: translateY(-2px);
  box-shadow: 0 8px 16px rgba(126, 87, 194, 0.2) !important;
}

.chart-card {
  background-color: #1e1e1e;
  border: 1px solid rgba(255, 255, 255, 0.12);
}

.chart-card :deep(h3) {
  color: #e0e0e0;
}

.chart-card :deep(.pa-4) {
  position: relative;
  min-height: 300px;
  display: flex;
  flex-direction: column;
}

/* Signal Cards - Demand & Stress (Red/Orange accent) */
.metric-card.signal-card {
  border-left-color: #FF6B6B;
}

.metric-card.signal-card:hover {
  box-shadow: 0 8px 16px rgba(255, 107, 107, 0.2) !important;
}

/* Lever Cards - Capacity & Staffing (Cyan accent) */
.metric-card.lever-card {
  border-left-color: #4ECDC4;
}

.metric-card.lever-card:hover {
  box-shadow: 0 8px 16px rgba(78, 205, 196, 0.2) !important;
}

/* Prominent Card - Staff-to-Patient Ratio */
.metric-card.prominent-card {
  background: linear-gradient(135deg, #1e1e1e 0%, #2a2a3e 100%);
  border-left-color: #FFD93D;
  border: 2px solid rgba(255, 217, 61, 0.4);
  border-left: 6px solid #FFD93D;
}

.metric-card.prominent-card:hover {
  transform: translateY(-4px);
  box-shadow: 0 12px 24px rgba(255, 217, 61, 0.3) !important;
}

.lever-highlight {
  color: #FFD93D;
}

/* Strain Cards - Warnings (Pink accent) */
.metric-card.strain-card {
  border-left-color: #F38181;
}

.metric-card.strain-card:hover {
  box-shadow: 0 8px 16px rgba(243, 129, 129, 0.2) !important;
}

:deep(.v-select__content) {
  background-color: #1e1e1e;
}

:deep(.v-field__input) {
  color: #e0e0e0 !important;
}

:deep(.v-field__outline) {
  --v-field-border-opacity: 0.3;
}

@media (max-width: 600px) {
  .app-bar {
    flex-wrap: wrap;
  }

  .month-picker {
    min-width: 100%;
    margin-top: 10px;
    margin-right: 0;
  }
}
</style>
