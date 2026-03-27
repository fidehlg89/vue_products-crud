<template>
  <div class="app-container">
    <RouterView />
  </div>
</template>

<script setup>
import { RouterView } from "vue-router";
</script>

<style>
@import url('https://fonts.googleapis.com/css2?family=Plus+Jakarta+Sans:wght@400;500;600;700&display=swap');

:root {
  --primary: #6366f1;
  --primary-hover: #4f46e5;
  --bg-gradient: linear-gradient(135deg, #f8fafc 0%, #f1f5f9 100%);
  --surface: #ffffff;
  --text-main: #1e293b;
  --text-muted: #64748b;
  --border-radius: 16px;
  --shadow: 0 10px 15px -3px rgba(0, 0, 0, 0.1), 0 4px 6px -2px rgba(0, 0, 0, 0.05);
}

body {
  font-family: 'Plus Jakarta Sans', sans-serif !important;
  background: var(--bg-gradient);
  color: var(--text-main);
  min-height: 100vh;
  margin: 0;
}

.app-container {
  padding: 2rem;
  max-width: 1400px;
  margin: 0 auto;
}

h1 {
  font-weight: 800;
  letter-spacing: -0.025em;
  color: #0f172a;
}

/* Transitions */
.page-fade-enter-active,
.page-fade-leave-active {
  transition: opacity 0.3s ease;
}

.page-fade-enter-from,
.page-fade-leave-to {
  opacity: 0;
}
</style>