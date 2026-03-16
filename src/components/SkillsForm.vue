<!--
  ╔══════════════════════════════════════════════════════╗
  ║  SkillsForm.vue  —  SKILLS TAG EDITOR               ║
  ║                                                      ║
  ║  Beginner Concepts Demonstrated:                    ║
  ║   • Local data() state (newSkill input value)       ║
  ║   • @keydown.enter — keyboard event shortcut        ║
  ║   • Filtering arrays to "remove" an item            ║
  ║   • v-if — conditional rendering                    ║
  ╚══════════════════════════════════════════════════════╝
-->

<template>
  <AccordionSection title="Skills" icon="⚡">

    <!-- Tag cloud: show existing skills as removable pills -->
    <div class="tags-wrap">

      <!--
        v-for over skills array.
        Each tag has an × button to remove it.
      -->
      <span
        v-for="(skill, index) in skills"
        :key="index"
        class="skill-tag"
      >
        {{ skill }}
        <!-- @click removes this skill by index -->
        <button class="tag-remove" @click="removeSkill(index)" title="Remove">×</button>
      </span>

      <!-- Show hint when no skills -->
      <span v-if="skills.length === 0" class="empty-hint">No skills yet.</span>
    </div>

    <!-- Input row to add a new skill -->
    <div class="add-skill-row">
      <!--
        v-model binds newSkill to this input (two-way).
        @keydown.enter = pressing Enter calls addSkill().
        This is a shorthand for @keydown="if key is Enter, call addSkill".
      -->
      <input
        type="text"
        v-model="newSkill"
        @keydown.enter="addSkill"
        placeholder="Type a skill and press Enter..."
        class="skill-input"
      />
      <button class="btn btn-primary" @click="addSkill">Add</button>
    </div>

    <!-- Tip for the user -->
    <p class="tip">💡 Tip: Press <kbd>Enter</kbd> to quickly add a skill</p>

  </AccordionSection>
</template>


<script>
import AccordionSection from './AccordionSection.vue'

export default {
  name: 'SkillsForm',
  components: { AccordionSection },

  props: {
    skills: {
      type: Array,
      required: true,
    },
  },

  emits: ['update'],

  // ── data() ───────────────────────────────────────────
  // LOCAL state: only lives inside this component.
  // The parent doesn't need to know about newSkill.
  data() {
    return {
      newSkill: '', // what's currently typed in the input
    }
  },

  methods: {

    addSkill() {
      // trim() removes whitespace from start and end
      const skill = this.newSkill.trim()

      // Don't add if empty, or if already in the list
      if (!skill) return
      if (this.skills.includes(skill)) {
        alert(`"${skill}" is already in your skills list.`)
        return
      }

      // Emit new array = all existing skills + new one
      this.$emit('update', [...this.skills, skill])

      // Clear the input field
      this.newSkill = ''
    },

    removeSkill(index) {
      // filter returns array without the item at `index`
      const updated = this.skills.filter((_, i) => i !== index)
      this.$emit('update', updated)
    },

  },
}
</script>


<style scoped>
/* Container for skill tags */
.tags-wrap {
  display: flex;
  flex-wrap: wrap;
  gap: 8px;
  min-height: 36px;
  margin-bottom: 14px;
  padding: 10px;
  background: #faf9f7;
  border: 1.5px solid var(--border);
  border-radius: var(--radius);
}

/* Individual skill tag */
.skill-tag {
  display: inline-flex;
  align-items: center;
  gap: 5px;
  background: var(--ink);
  color: white;
  font-size: 0.78rem;
  font-weight: 500;
  padding: 4px 10px 4px 12px;
  border-radius: 99px;
  font-family: var(--font-body);
  animation: popIn 0.18s ease;
}

@keyframes popIn {
  from { opacity: 0; transform: scale(0.8); }
  to   { opacity: 1; transform: scale(1); }
}

/* Remove button inside each tag */
.tag-remove {
  background: none;
  border: none;
  color: rgba(255,255,255,0.5);
  font-size: 1rem;
  line-height: 1;
  cursor: pointer;
  padding: 0;
  transition: color 0.15s;
}
.tag-remove:hover { color: #fca5a5; }

/* Input row */
.add-skill-row {
  display: flex;
  gap: 8px;
}

.skill-input {
  font-family: var(--font-body);
  font-size: 0.88rem;
  color: var(--ink);
  background: #faf9f7;
  border: 1.5px solid var(--border);
  border-radius: 7px;
  padding: 8px 11px;
  flex: 1;
  outline: none;
  transition: all 0.18s;
}
.skill-input:focus {
  border-color: var(--accent);
  background: white;
  box-shadow: 0 0 0 3px rgba(37,99,235,0.10);
}

/* Keyboard shortcut hint */
.tip {
  font-size: 0.72rem;
  color: var(--ink-muted);
  margin-top: 8px;
}

kbd {
  background: var(--border);
  border-radius: 4px;
  padding: 1px 5px;
  font-family: var(--font-mono);
  font-size: 0.72rem;
  color: var(--ink-soft);
}

.empty-hint {
  font-size: 0.8rem;
  color: var(--ink-muted);
  font-style: italic;
  align-self: center;
}
</style>
