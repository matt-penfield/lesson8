## My Dashboard

This project is intentionally set up at the workspace root so all commands run from here.

### First 60 Seconds

1. Open a terminal in this root folder.
2. Run `npm install`.
3. Run `npm run dev`.
4. Open http://localhost:5173/.
5. Start editing `src/views/DashboardView.vue` for the main dashboard.

### Run From Root

```bash
npm install
npm run dev
```

### Root Layout

```text
.
|-- index.html
|-- package.json
|-- vite.config.ts
|-- tsconfig.json
|-- tsconfig.app.json
|-- tsconfig.node.json
|-- src/
|   |-- main.ts
|   |-- App.vue
|   |-- style.css
|   |-- env.d.ts
|   `-- views/
|       |-- DashboardView.vue
|       `-- ReportsView.vue
|-- public/
|-- PLAN.md
`-- README.md
```

### What Runs From Here

- `npm run dev`: starts Vite dev server from root.
- `npm run build`: type-checks and builds production assets.
- `npm run preview`: serves the built `dist/` output locally.

### Key Files

- `package.json`: scripts and dependencies for router, Vuetify, icons, and charts.
- `src/main.ts`: app bootstrap, Vuetify setup, and router registration.
- `src/App.vue`: top-level layout with app bar and route outlet.
- `src/views/DashboardView.vue`: KPI cards and line chart view.
- `src/views/ReportsView.vue`: bar chart reports view.

### Stack

- Vue 3 + Vite + TypeScript
- Vue Router 4
- Vuetify 3
- Material Design Icons (`@mdi/font`)
- Chart.js + vue-chartjs
