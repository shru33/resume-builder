<!--
  ╔══════════════════════════════════════════════════════╗
  ║  ExperienceForm.vue  —  WORK EXPERIENCE EDITOR      ║
  ║                                                      ║
  ║  Beginner Concepts Demonstrated:                    ║
  ║   • v-for   → looping through an array             ║
  ║   • :key    → required with v-for for performance   ║
  ║   • Adding / removing array items                   ║
  ║   • Editing items inside an array                   ║
  ║   • Emitting the full updated array upward          ║
  ╚══════════════════════════════════════════════════════╝
-->

<template>
  <AccordionSection title="Work Experience" icon="💼">

    <!--
      v-for loops through the entries array.
      For each item, Vue needs a unique :key.
      We use entry.id as the key.
    -->
    <div
      v-for="(entry, index) in entries"
      :key="entry.id"
      class="entry-block"
    >

      <!-- Entry header: label + delete button -->
      <div class="entry-header">
        <span class="entry-label">
          Job #{{ index + 1 }}
          <span v-if="entry.company" class="entry-sublabel">— {{ entry.company }}</span>
        </span>
        <!-- 
          @click calls removeEntry and passes the index.
          index is the position of this item in the array.
        -->
        <button class="btn btn-danger" @click="removeEntry(index)">✕ Remove</button>
      </div>

      <!-- Job Title + Company (side by side) -->
      <div class="two-col">
        <div class="field">
          <label>Job Title</label>
          <input
            type="text"
            :value="entry.jobTitle"
            @input="updateField(index, 'jobTitle', $event.target.value)"
            placeholder="e.g. Frontend Developer"
          />
        </div>
        <div class="field">
          <label>Company</label>
          <input
            type="text"
            :value="entry.company"
            @input="updateField(index, 'company', $event.target.value)"
            placeholder="e.g. Google"
          />
        </div>
      </div>

      <!-- Start + End date (side by side) -->
      <div class="two-col">
        <div class="field">
          <label>Start Date</label>
          <input
            type="text"
            :value="entry.startDate"
            @input="updateField(index, 'startDate', $event.target.value)"
            placeholder="e.g. Jan 2022"
          />
        </div>
        <div class="field">
          <label>End Date</label>
          <input
            type="text"
            :value="entry.endDate"
            @input="updateField(index, 'endDate', $event.target.value)"
            placeholder="e.g. Present"
          />
        </div>
      </div>

      <!-- Description (full width) -->
      <div class="field">
        <label>Description</label>
        <textarea
          :value="entry.description"
          @input="updateField(index, 'description', $event.target.value)"
          placeholder="Describe your role and achievements..."
          rows="3"
        ></textarea>
      </div>

    </div><!-- end v-for -->

    <!-- Empty state: shown when no entries exist -->
    <p v-if="entries.length === 0" class="empty-hint">
      No experience added yet. Click below to add your first job.
    </p>

    <!-- Add new entry button -->
    <button class="btn btn-add" @click="addEntry">
      + Add Work Experience
    </button>

  </AccordionSection>
</template>


<script>
import AccordionSection from './AccordionSection.vue'

export default {
  name: 'ExperienceForm',

  components: { AccordionSection },

  props: {
    entries: {
      type: Array,     // expect an array
      required: true,
    },
  },

  emits: ['update'],

  methods: {

    // ── updateField ─────────────────────────────────
    // Called when a single field inside one entry changes.
    // index = which entry in the array (0, 1, 2...)
    // field = which property ('jobTitle', 'company', etc.)
    // value = new value from the input
    updateField(index, field, value) {

      // 1. Copy the entire array (we never mutate props directly)
      const updated = [...this.entries]

      // 2. Copy the specific entry object and update its field
      updated[index] = {
        ...updated[index],  // copy all existing fields of this entry
        [field]: value,      // override just the one that changed
      }

      // 3. Emit the new array up to the parent
      this.$emit('update', updated)
    },

    // ── addEntry ────────────────────────────────────
    // Adds a new blank entry to the array
    addEntry() {
      const newEntry = {
        id:          Date.now(), // unique id using timestamp
        jobTitle:    '',
        company:     '',
        startDate:   '',
        endDate:     '',
        description: '',
      }

      // Spread old entries + add new one at the end
      this.$emit('update', [...this.entries, newEntry])
    },

    // ── removeEntry ─────────────────────────────────
    // Removes the entry at the given index
    removeEntry(index) {
      // filter() returns a new array WITHOUT the item at `index`
      const updated = this.entries.filter((_, i) => i !== index)
      this.$emit('update', updated)
    },

  },
}
</script>


<style scoped>
/* A bordered block for each job entry */
.entry-block {
  border: 1.5px solid var(--border);
  border-radius: var(--radius);
  padding: 14px;
  margin-bottom: 12px;
  background: #faf9f7;
  transition: border-color 0.2s;
}
.entry-block:hover { border-color: var(--accent); }

/* Header row of each entry */
.entry-header {
  display: flex;
  align-items: center;
  justify-content: space-between;
  margin-bottom: 12px;
}

.entry-label {
  font-size: 0.82rem;
  font-weight: 600;
  color: var(--ink-soft);
}

.entry-sublabel {
  font-weight: 400;
  color: var(--ink-muted);
}

/* Hint shown when list is empty */
.empty-hint {
  font-size: 0.82rem;
  color: var(--ink-muted);
  text-align: center;
  padding: 16px 0 8px;
  font-style: italic;
}
</style>
