<script setup lang="ts">
import DashboardView from './views/DashboardView.vue'
import { ref } from 'vue'

const selectedMonth = ref('All')
const months = ['All', 'Jan', 'Feb', 'Mar', 'Apr', 'May', 'Jun', 'Jul', 'Aug', 'Sep', 'Oct', 'Nov', 'Dec']
</script>

<template>
  <v-app>
    <v-app-bar color="grey-darken-4" density="comfortable" elevation="0">
      <v-app-bar-title class="font-weight-bold">
        <v-icon class="mr-2">mdi-view-dashboard</v-icon>
        My Dashboard
      </v-app-bar-title>
      <v-spacer />
      <v-select
        v-model="selectedMonth"
        :items="months"
        density="compact"
        variant="outlined"
        hide-details
        style="max-width: 160px"
        class="mr-4"
        label="Month"
      />
    </v-app-bar>
    <v-main>
      <DashboardView :selected-month="selectedMonth" />
    </v-main>
  </v-app>
</template>
