<!--
  ╔══════════════════════════════════════════════════════╗
  ║  AppHeader.vue  —  TOP NAVIGATION BAR                ║
  ║                                                      ║
  ║  Beginner Concepts Demonstrated:                    ║
  ║   • $emit  → sending events UP to the parent        ║
  ║   • Simple presentational component                 ║
  ║   • Scoped styles                                   ║
  ╚══════════════════════════════════════════════════════╝
-->

<template>
  <header class="app-header">

    <!-- Logo / Brand -->
    <div class="brand">
      <span class="brand-icon">📄</span>
      <span class="brand-name">ResumeMaker</span>
    </div>

    <!-- Right-side actions -->
    <div class="header-actions">

      <!-- 
        When clicked, we $emit an event called "export"
        The parent (App.vue) is listening with @export="handleExport"
        and will call pdf export
      -->
      <button class="btn btn-primary" @click="$emit('export')">
        ⬇ Export PDF
      </button>

    </div>
  </header>
</template>


<script>
export default {
  name: 'AppHeader',

  // emits: declare what events this component can send up
  // This is optional but good practice — it self-documents the component
  emits: ['export'],

  // No data or methods needed — this is a simple "presentational" component
}
</script>


<!-- "scoped" means these styles ONLY apply inside this component -->
<style scoped>
.app-header {
  height: 60px;
  background: var(--ink);
  color: white;
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 0 28px;
  position: sticky;
  top: 0;
  z-index: 50;
  box-shadow: 0 2px 12px rgba(0,0,0,0.2);
}

.brand {
  display: flex;
  align-items: center;
  gap: 8px;
}

.brand-icon { font-size: 1.3rem; }

.brand-name {
  font-family: var(--font-serif);
  font-size: 1.1rem;
  font-weight: 600;
  letter-spacing: -0.02em;
}

.brand-tag {
  font-size: 0.65rem;
  font-weight: 500;
  background: rgba(255,255,255,0.12);
  color: rgba(255,255,255,0.65);
  padding: 2px 8px;
  border-radius: 99px;
  letter-spacing: 0.04em;
}

.header-actions { display: flex; gap: 10px; }
</style>
