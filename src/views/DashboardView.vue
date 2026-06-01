<script setup lang="ts">
import {
  Chart as ChartJS,
  Title,
  Tooltip,
  Legend,
  LineElement,
  PointElement,
  CategoryScale,
  LinearScale,
} from 'chart.js'
import { Line } from 'vue-chartjs'

ChartJS.register(
  Title,
  Tooltip,
  Legend,
  LineElement,
  PointElement,
  CategoryScale,
  LinearScale,
)

const chartData = {
  labels: ['Mon', 'Tue', 'Wed', 'Thu', 'Fri', 'Sat', 'Sun'],
  datasets: [
    {
      label: 'Revenue',
      data: [1200, 980, 1450, 1720, 1600, 2010, 1950],
      borderColor: '#3f51b5',
      backgroundColor: 'rgba(63, 81, 181, 0.18)',
      tension: 0.3,
      fill: true,
    },
  ],
}

const chartOptions = {
  responsive: true,
  maintainAspectRatio: false,
}
</script>

<template>
  <v-container class="py-6" fluid>
    <v-row>
      <v-col cols="12" md="4">
        <v-card rounded="lg" elevation="2">
          <v-card-title>Active Users</v-card-title>
          <v-card-text class="text-h4">2,481</v-card-text>
        </v-card>
      </v-col>
      <v-col cols="12" md="4">
        <v-card rounded="lg" elevation="2">
          <v-card-title>Conversion Rate</v-card-title>
          <v-card-text class="text-h4">4.8%</v-card-text>
        </v-card>
      </v-col>
      <v-col cols="12" md="4">
        <v-card rounded="lg" elevation="2">
          <v-card-title>Open Tickets</v-card-title>
          <v-card-text class="text-h4">17</v-card-text>
        </v-card>
      </v-col>
    </v-row>

    <v-row class="mt-1">
      <v-col cols="12">
        <v-card rounded="lg" elevation="2">
          <v-card-title>Weekly Revenue Trend</v-card-title>
          <v-card-text>
            <div style="height: 340px">
              <Line :data="chartData" :options="chartOptions" />
            </div>
          </v-card-text>
        </v-card>
      </v-col>
    </v-row>
  </v-container>
</template>
