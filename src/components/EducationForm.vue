<!--
  ╔══════════════════════════════════════════════════════╗
  ║  EducationForm.vue  —  EDUCATION EDITOR             ║
  ║                                                      ║
  ║  Very similar structure to ExperienceForm.          ║
  ║  Same patterns: v-for, updateField, add, remove.    ║
  ║  Study ExperienceForm.vue first, then this one.     ║
  ╚══════════════════════════════════════════════════════╝
-->

<template>
  <AccordionSection title="Education" icon="🎓">

    <div
      v-for="(entry, index) in entries"
      :key="entry.id"
      class="entry-block"
    >

      <div class="entry-header">
        <span class="entry-label">
          Entry #{{ index + 1 }}
          <span v-if="entry.school" class="entry-sublabel">— {{ entry.school }}</span>
        </span>
        <button class="btn btn-danger" @click="removeEntry(index)">✕ Remove</button>
      </div>

      <!-- Degree + School -->
      <div class="two-col">
        <div class="field">
          <label>Degree</label>
          <input
            type="text"
            :value="entry.degree"
            @input="updateField(index, 'degree', $event.target.value)"
            placeholder="e.g. B.Sc. Computer Science"
          />
        </div>
        <div class="field">
          <label>School</label>
          <input
            type="text"
            :value="entry.school"
            @input="updateField(index, 'school', $event.target.value)"
            placeholder="e.g. MIT"
          />
        </div>
      </div>

      <!-- Year + Details -->
      <div class="two-col">
        <div class="field">
          <label>Years</label>
          <input
            type="text"
            :value="entry.year"
            @input="updateField(index, 'year', $event.target.value)"
            placeholder="e.g. 2018 – 2022"
          />
        </div>
        <div class="field">
          <label>GPA / Honours</label>
          <input
            type="text"
            :value="entry.details"
            @input="updateField(index, 'details', $event.target.value)"
            placeholder="e.g. GPA 3.8"
          />
        </div>
      </div>

    </div>

    <p v-if="entries.length === 0" class="empty-hint">
      No education added yet.
    </p>

    <button class="btn btn-add" @click="addEntry">
      + Add Education
    </button>

  </AccordionSection>
</template>


<script>
import AccordionSection from './AccordionSection.vue'

export default {
  name: 'EducationForm',
  components: { AccordionSection },

  props: {
    entries: {
      type: Array,
      required: true,
    },
  },

  emits: ['update'],

  methods: {
    updateField(index, field, value) {
      const updated = [...this.entries]
      updated[index] = { ...updated[index], [field]: value }
      this.$emit('update', updated)
    },

    addEntry() {
      this.$emit('update', [
        ...this.entries,
        { id: Date.now(), degree: '', school: '', year: '', details: '' },
      ])
    },

    removeEntry(index) {
      this.$emit('update', this.entries.filter((_, i) => i !== index))
    },
  },
}
</script>


<style scoped>
.entry-block {
  border: 1.5px solid var(--border);
  border-radius: var(--radius);
  padding: 14px;
  margin-bottom: 12px;
  background: #faf9f7;
  transition: border-color 0.2s;
}
.entry-block:hover { border-color: var(--accent); }

.entry-header {
  display: flex;
  align-items: center;
  justify-content: space-between;
  margin-bottom: 12px;
}

.entry-label { font-size: 0.82rem; font-weight: 600; color: var(--ink-soft); }
.entry-sublabel { font-weight: 400; color: var(--ink-muted); }

.empty-hint {
  font-size: 0.82rem;
  color: var(--ink-muted);
  text-align: center;
  padding: 16px 0 8px;
  font-style: italic;
}
</style>
