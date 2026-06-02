<script setup lang="ts">
import { computed } from 'vue'
import {
  Chart as ChartJS,
  Title,
  Tooltip,
  Legend,
  BarElement,
  LineElement,
  PointElement,
  CategoryScale,
  LinearScale,
  Filler,
} from 'chart.js'
import { Bar, Line } from 'vue-chartjs'
import metrics from '../data/metrics.json'

ChartJS.register(
  Title, Tooltip, Legend,
  BarElement, LineElement, PointElement,
  CategoryScale, LinearScale, Filler,
)

const props = defineProps<{ selectedMonth: string }>()

const allLabels = metrics.map((m) => m.month)

const selectedIndex = computed(() =>
  props.selectedMonth === 'All' ? -1 : allLabels.indexOf(props.selectedMonth)
)

// Summary computations
const summary = computed(() => {
  if (selectedIndex.value === -1) {
    const totalRevenue = metrics.reduce((s, m) => s + m.revenue, 0)
    const totalVisitors = metrics.reduce((s, m) => s + m.visitors, 0)
    const avgConversions = metrics.reduce((s, m) => s + m.conversions, 0) / metrics.length
    const totalOrders = metrics.reduce((s, m) => s + m.orders, 0)
    return { revenue: totalRevenue, visitors: totalVisitors, conversions: avgConversions, orders: totalOrders }
  }
  const m = metrics[selectedIndex.value]
  return { revenue: m.revenue, visitors: m.visitors, conversions: m.conversions, orders: m.orders }
})

// Change from previous month
const change = computed(() => {
  const idx = selectedIndex.value
  if (idx <= 0) return { revenue: 0, visitors: 0, conversions: 0, orders: 0 }
  const cur = metrics[idx]
  const prev = metrics[idx - 1]
  return {
    revenue: ((cur.revenue - prev.revenue) / prev.revenue) * 100,
    visitors: ((cur.visitors - prev.visitors) / prev.visitors) * 100,
    conversions: cur.conversions - prev.conversions,
    orders: ((cur.orders - prev.orders) / prev.orders) * 100,
  }
})

const showChange = computed(() => selectedIndex.value > 0)

// Colors
const palette = {
  blue: '#42a5f5',
  blueFill: 'rgba(66,165,245,0.15)',
  teal: '#26a69a',
  tealFill: 'rgba(38,166,154,0.2)',
  purple: '#ab47bc',
  purpleFill: 'rgba(171,71,188,0.2)',
}

// Revenue bar chart
const revenueChartData = computed(() => {
  const bgColors = allLabels.map((_, i) =>
    selectedIndex.value === -1 || i === selectedIndex.value ? palette.blue : 'rgba(66,165,245,0.25)'
  )
  return {
    labels: allLabels,
    datasets: [{
      label: 'Revenue ($)',
      data: metrics.map((m) => m.revenue),
      backgroundColor: bgColors,
      borderRadius: 6,
    }],
  }
})

// Visitors line chart
const visitorsChartData = computed(() => ({
  labels: allLabels,
  datasets: [{
    label: 'Visitors',
    data: metrics.map((m) => m.visitors),
    borderColor: palette.teal,
    backgroundColor: palette.tealFill,
    tension: 0.35,
    fill: true,
    pointBackgroundColor: allLabels.map((_, i) =>
      selectedIndex.value === -1 || i === selectedIndex.value ? palette.teal : 'transparent'
    ),
    pointRadius: allLabels.map((_, i) =>
      selectedIndex.value === -1 ? 4 : i === selectedIndex.value ? 7 : 3
    ),
  }],
}))

// Conversion area chart
const conversionChartData = computed(() => ({
  labels: allLabels,
  datasets: [{
    label: 'Conversion Rate (%)',
    data: metrics.map((m) => m.conversions),
    borderColor: palette.purple,
    backgroundColor: palette.purpleFill,
    tension: 0.35,
    fill: true,
    pointBackgroundColor: allLabels.map((_, i) =>
      selectedIndex.value === -1 || i === selectedIndex.value ? palette.purple : 'transparent'
    ),
    pointRadius: allLabels.map((_, i) =>
      selectedIndex.value === -1 ? 4 : i === selectedIndex.value ? 7 : 3
    ),
  }],
}))

const chartOptions = {
  responsive: true,
  maintainAspectRatio: false,
  plugins: {
    legend: { display: false },
  },
  scales: {
    x: { grid: { display: false } },
    y: { grid: { color: 'rgba(255,255,255,0.06)' } },
  },
}

function formatCurrency(n: number) {
  return '$' + n.toLocaleString()
}
function formatNumber(n: number) {
  return n.toLocaleString()
}
</script>

<template>
  <v-container fluid class="pa-6">
    <!-- KPI Cards -->
    <v-row>
      <v-col cols="12" sm="6" lg="3">
        <v-card rounded="lg" elevation="0" color="grey-darken-3">
          <v-card-title class="text-caption text-medium-emphasis">Revenue</v-card-title>
          <v-card-text class="text-h5 font-weight-bold">
            {{ formatCurrency(summary.revenue) }}
            <span v-if="showChange" class="text-caption ml-2" :class="change.revenue >= 0 ? 'text-green' : 'text-red'">
              <v-icon size="14">{{ change.revenue >= 0 ? 'mdi-arrow-up' : 'mdi-arrow-down' }}</v-icon>
              {{ Math.abs(change.revenue).toFixed(1) }}%
            </span>
          </v-card-text>
        </v-card>
      </v-col>
      <v-col cols="12" sm="6" lg="3">
        <v-card rounded="lg" elevation="0" color="grey-darken-3">
          <v-card-title class="text-caption text-medium-emphasis">Visitors</v-card-title>
          <v-card-text class="text-h5 font-weight-bold">
            {{ formatNumber(summary.visitors) }}
            <span v-if="showChange" class="text-caption ml-2" :class="change.visitors >= 0 ? 'text-green' : 'text-red'">
              <v-icon size="14">{{ change.visitors >= 0 ? 'mdi-arrow-up' : 'mdi-arrow-down' }}</v-icon>
              {{ Math.abs(change.visitors).toFixed(1) }}%
            </span>
          </v-card-text>
        </v-card>
      </v-col>
      <v-col cols="12" sm="6" lg="3">
        <v-card rounded="lg" elevation="0" color="grey-darken-3">
          <v-card-title class="text-caption text-medium-emphasis">Conversions</v-card-title>
          <v-card-text class="text-h5 font-weight-bold">
            {{ summary.conversions.toFixed(1) }}%
            <span v-if="showChange" class="text-caption ml-2" :class="change.conversions >= 0 ? 'text-green' : 'text-red'">
              <v-icon size="14">{{ change.conversions >= 0 ? 'mdi-arrow-up' : 'mdi-arrow-down' }}</v-icon>
              {{ Math.abs(change.conversions).toFixed(1) }}pp
            </span>
          </v-card-text>
        </v-card>
      </v-col>
      <v-col cols="12" sm="6" lg="3">
        <v-card rounded="lg" elevation="0" color="grey-darken-3">
          <v-card-title class="text-caption text-medium-emphasis">Orders</v-card-title>
          <v-card-text class="text-h5 font-weight-bold">
            {{ formatNumber(summary.orders) }}
            <span v-if="showChange" class="text-caption ml-2" :class="change.orders >= 0 ? 'text-green' : 'text-red'">
              <v-icon size="14">{{ change.orders >= 0 ? 'mdi-arrow-up' : 'mdi-arrow-down' }}</v-icon>
              {{ Math.abs(change.orders).toFixed(1) }}%
            </span>
          </v-card-text>
        </v-card>
      </v-col>
    </v-row>

    <!-- Revenue Bar + Visitors Line -->
    <v-row class="mt-2">
      <v-col cols="12" md="6">
        <v-card rounded="lg" elevation="0" color="grey-darken-3">
          <v-card-title class="text-body-1 font-weight-medium">Monthly Revenue</v-card-title>
          <v-card-text>
            <div style="height: 280px">
              <Bar :data="revenueChartData" :options="chartOptions" />
            </div>
          </v-card-text>
        </v-card>
      </v-col>
      <v-col cols="12" md="6">
        <v-card rounded="lg" elevation="0" color="grey-darken-3">
          <v-card-title class="text-body-1 font-weight-medium">Visitors Over Time</v-card-title>
          <v-card-text>
            <div style="height: 280px">
              <Line :data="visitorsChartData" :options="chartOptions" />
            </div>
          </v-card-text>
        </v-card>
      </v-col>
    </v-row>

    <!-- Conversion Area Chart -->
    <v-row class="mt-2">
      <v-col cols="12">
        <v-card rounded="lg" elevation="0" color="grey-darken-3">
          <v-card-title class="text-body-1 font-weight-medium">Conversion Rate Trend</v-card-title>
          <v-card-text>
            <div style="height: 260px">
              <Line :data="conversionChartData" :options="chartOptions" />
            </div>
          </v-card-text>
        </v-card>
      </v-col>
    </v-row>
  </v-container>
</template>
