<template>
  <v-app>
    <v-app-bar color="indigo-darken-3" density="comfortable">
      <v-app-bar-title>My Dashboard</v-app-bar-title>
      <v-spacer />
      <v-btn to="/" variant="text" prepend-icon="mdi-view-dashboard">Dashboard</v-btn>
      <v-btn to="/reports" variant="text" prepend-icon="mdi-chart-bar">Reports</v-btn>
    </v-app-bar>
    <v-main>
      <router-view />
    </v-main>
  </v-app>
</template>

<style scoped>
.v-main {
  background: #f5f7fa;
  min-height: calc(100vh - 64px);
}
</style>
