<!--
  ╔══════════════════════════════════════════════════════╗
  ║  PersonalForm.vue  —  PERSONAL INFORMATION FORM     ║
  ║                                                      ║
  ║  Beginner Concepts Demonstrated:                    ║
  ║   • props   → receiving data from parent            ║
  ║   • $emit   → sending updated data back up          ║
  ║   • computed → deriving a local copy to edit        ║
  ║   • v-model → two-way binding on inputs             ║
  ╚══════════════════════════════════════════════════════╝
-->

<template>
  <!-- 
    AccordionSection is a reusable wrapper component.
    We pass it a title and icon, and it handles open/close.
  -->
  <AccordionSection title="Personal Info" icon="👤">

    <!-- Name (full width) -->
    <div class="field">
      <label>Full Name</label>
      <input
        type="text"
        :value="localData.name"
        @input="update('name', $event.target.value)"
        placeholder="e.g. Shruti Hegde"
      />
    </div>

    <!-- Job title (full width) -->
    <div class="field">
      <label>Job Title</label>
      <input
        type="text"
        :value="localData.title"
        @input="update('title', $event.target.value)"
        placeholder="e.g. Frontend Developer"
      />
    </div>

    <!-- Email + Phone (side by side) -->
    <div class="two-col">
      <div class="field">
        <label>Email</label>
        <input
          type="email"
          :value="localData.email"
          @input="update('email', $event.target.value)"
          placeholder="you@email.com"
        />
      </div>
      <div class="field">
        <label>Phone</label>
        <input
          type="tel"
          :value="localData.phone"
          @input="update('phone', $event.target.value)"
          placeholder="+1 555 000 0000"
        />
      </div>
    </div>

    <!-- Location + Website (side by side) -->
    <div class="two-col">
      <div class="field">
        <label>Location</label>
        <input
          type="text"
          :value="localData.location"
          @input="update('location', $event.target.value)"
          placeholder="City, State"
        />
      </div>
      <div class="field">
        <label>Website</label>
        <input
          type="text"
          :value="localData.website"
          @input="update('website', $event.target.value)"
          placeholder="yoursite.com"
        />
      </div>
    </div>

    <!-- Summary (full width, textarea) -->
    <div class="field">
      <label>Summary</label>
      <textarea
        :value="localData.summary"
        @input="update('summary', $event.target.value)"
        placeholder="Write 2–3 sentences about yourself..."
        rows="3"
      ></textarea>
    </div>

  </AccordionSection>
</template>


<script>
import AccordionSection from './AccordionSection.vue'

export default {
  name: 'PersonalForm',

  components: { AccordionSection },

  // ── props ────────────────────────────────────────────
  // Props are data the PARENT sends DOWN to this component.
  // We declare them with their expected type so Vue can warn us if wrong.
  props: {
    data: {
      type: Object,    // we expect an object
      required: true,  // parent MUST provide this
    },
  },

  // ── emits ────────────────────────────────────────────
  // Events this component can send back UP to the parent
  emits: ['update'],

  // ── computed ─────────────────────────────────────────
  // A computed property is like a derived variable.
  // It recalculates whenever its dependencies change.
  computed: {
    // Make a local copy of the prop so we can read from it
    localData() {
      return this.data
    },
  },

  methods: {
    // Called when any input changes
    // key = which field changed (e.g. 'name')
    // value = new value from the input
    update(key, value) {
      // We can't directly mutate a prop (Vue will warn you).
      // Instead, we create a NEW object with the updated field
      // and emit it UP to the parent so the parent updates its data.
      const updated = {
        ...this.data,  // spread: copy ALL existing fields
        [key]: value,  // then override just the one that changed
      }

      // $emit('update', payload) → parent listens with @update="..."
      this.$emit('update', updated)
    },
  },
}
</script>

<!-- No extra styles needed — using global styles from App.vue -->
