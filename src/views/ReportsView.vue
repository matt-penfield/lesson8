<script setup lang="ts">
import {
  Chart as ChartJS,
  Title,
  Tooltip,
  Legend,
  BarElement,
  CategoryScale,
  LinearScale,
} from 'chart.js'
import { Bar } from 'vue-chartjs'

ChartJS.register(Title, Tooltip, Legend, BarElement, CategoryScale, LinearScale)

const chartData = {
  labels: ['Q1', 'Q2', 'Q3', 'Q4'],
  datasets: [
    {
      label: 'Sales',
      data: [32000, 41000, 38000, 46000],
      backgroundColor: ['#1976d2', '#26a69a', '#ffb300', '#ef5350'],
      borderRadius: 8,
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
      <v-col cols="12">
        <v-card rounded="lg" elevation="2">
          <v-card-title>Quarterly Sales Report</v-card-title>
          <v-card-text>
            <div style="height: 380px">
              <Bar :data="chartData" :options="chartOptions" />
            </div>
          </v-card-text>
        </v-card>
      </v-col>
    </v-row>
  </v-container>
</template>
