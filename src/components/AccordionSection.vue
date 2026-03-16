<!--
  ╔══════════════════════════════════════════════════════╗
  ║  AccordionSection.vue  —  REUSABLE COLLAPSIBLE WRAP ║
  ║                                                      ║
  ║  Beginner Concepts Demonstrated:                    ║
  ║   • Slots → inserting content from the parent       ║
  ║   • Local toggle state (isOpen)                     ║
  ║   • v-show vs v-if                                  ║
  ║   • Dynamic classes with :class binding             ║
  ╚══════════════════════════════════════════════════════╝
-->

<template>
  <div class="accordion" :class="{ open: isOpen }">

    <!-- Clickable header row -->
    <button class="accordion-header" @click="isOpen = !isOpen">
      <span class="acc-icon">{{ icon }}</span>
      <span class="acc-title">{{ title }}</span>

      <!-- Arrow rotates based on isOpen -->
      <span class="acc-arrow" :class="{ rotated: isOpen }">›</span>
    </button>

    <!--
      v-show = toggle CSS display (element stays in DOM)
      v-if  = add/remove element from DOM entirely

      Use v-show for things toggled frequently (like accordion),
      use v-if for things rarely shown (like error messages).
    -->
    <div v-show="isOpen" class="accordion-body">
      <!--
        <slot /> is a placeholder for whatever the parent puts inside.
        Example usage in App.vue:
          <AccordionSection title="Personal Info" icon="👤">
            ... form fields go here (they fill the slot) ...
          </AccordionSection>
      -->
      <slot />
    </div>

  </div>
</template>


<script>
export default {
  name: 'AccordionSection',

  props: {
    // The heading text
    title: {
      type: String,
      required: true,
    },
    // The emoji icon
    icon: {
      type: String,
      default: '📌', // default value if parent doesn't pass one
    },
    // Start open or closed?
    defaultOpen: {
      type: Boolean,
      default: false,
    },
  },

  data() {
    return {
      // isOpen controls whether the body is visible
      // initialised from the defaultOpen prop
      isOpen: this.defaultOpen,
    }
  },
}
</script>


<style scoped>
.accordion {
  border: 1.5px solid var(--border);
  border-radius: var(--radius);
  overflow: hidden;
  transition: border-color 0.2s;
}
.accordion.open { border-color: #c7d2fe; }

/* Header button */
.accordion-header {
  width: 100%;
  display: flex;
  align-items: center;
  gap: 8px;
  padding: 13px 16px;
  background: #f8f7f5;
  border: none;
  cursor: pointer;
  text-align: left;
  transition: background 0.18s;
}
.accordion-header:hover { background: #f0eee9; }
.accordion.open .accordion-header { background: var(--accent-bg); }

.acc-icon { font-size: 1rem; }

.acc-title {
  font-size: 0.82rem;
  font-weight: 700;
  text-transform: uppercase;
  letter-spacing: 0.07em;
  color: var(--ink-soft);
  flex: 1;
}
.accordion.open .acc-title { color: var(--accent); }

/* The › arrow, rotates 90° when open */
.acc-arrow {
  font-size: 1.1rem;
  color: var(--ink-muted);
  transition: transform 0.22s ease;
  display: inline-block;
}
.acc-arrow.rotated { transform: rotate(90deg); }

/* Body content area */
.accordion-body {
  padding: 16px;
  border-top: 1.5px solid var(--border);
  background: white;
  max-height: calc(100% - 40px);
  overflow-y: auto;
}
</style>
