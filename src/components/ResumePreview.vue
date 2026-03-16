<!--
  ╔══════════════════════════════════════════════════════╗
  ║  ResumePreview.vue  —  LIVE RESUME DISPLAY          ║
  ║                                                      ║
  ║  Beginner Concepts Demonstrated:                    ║
  ║   • Receiving a complex object as a prop            ║
  ║   • v-if for conditional rendering                  ║
  ║   • v-for to loop data arrays                       ║
  ║   • Computed properties for derived data            ║
  ║   • This component is PURELY display — no emits     ║
  ╚══════════════════════════════════════════════════════╝
-->

<template>
  <!-- Paper sheet wrapper -->
  <div class="sheet">

    <!-- ── HEADER ──────────────────────────────────── -->
    <header class="resume-header">

      <!-- Name (fallback text if empty) -->
      <h1 class="r-name">{{ resume.personal.name || 'Your Name' }}</h1>

      <!-- Job title: only show if not empty -->
      <p v-if="resume.personal.title" class="r-title">
        {{ resume.personal.title }}
      </p>

      <!-- Contact info row -->
      <div class="r-contact">
        <!-- Each contact item only shows if it has a value -->
        <span v-if="resume.personal.email">✉ {{ resume.personal.email }}</span>
        <span v-if="resume.personal.phone">✆ {{ resume.personal.phone }}</span>
        <span v-if="resume.personal.location">📍 {{ resume.personal.location }}</span>
        <span v-if="resume.personal.website">🔗 {{ resume.personal.website }}</span>
      </div>

    </header>

    <!-- ── BODY ─────────────────────────────────────── -->
    <div class="resume-body">

      <!-- Summary section -->
      <!-- v-if: only renders this block if summary is not empty -->
      <section v-if="resume.personal.summary" class="r-section">
        <h2 class="r-section-title">Summary</h2>
        <p class="r-summary">{{ resume.personal.summary }}</p>
      </section>

      <!-- Experience section -->
      <!-- Only show section if there is at least 1 experience entry -->
      <section v-if="resume.experience.length > 0" class="r-section">
        <h2 class="r-section-title">Experience</h2>

        <!--
          Loop through each experience entry.
          :key should be a unique identifier (we use entry.id)
        -->
        <div
          v-for="entry in resume.experience"
          :key="entry.id"
          class="r-entry"
        >
          <div class="r-entry-top">
            <div>
              <div class="r-entry-title">{{ entry.jobTitle }}</div>
              <div class="r-entry-sub">{{ entry.company }}</div>
            </div>
            <div class="r-entry-date">
              <!-- 
                hasDate is a computed property (see below).
                We call it as a method here with the entry object.
              -->
              {{ formatDate(entry.startDate, entry.endDate) }}
            </div>
          </div>
          <p v-if="entry.description" class="r-entry-desc">{{ entry.description }}</p>
        </div>
      </section>

      <!-- Education section -->
      <section v-if="resume.education.length > 0" class="r-section">
        <h2 class="r-section-title">Education</h2>

        <div
          v-for="entry in resume.education"
          :key="entry.id"
          class="r-entry"
        >
          <div class="r-entry-top">
            <div>
              <div class="r-entry-title">{{ entry.degree }}</div>
              <div class="r-entry-sub">{{ entry.school }}</div>
            </div>
            <div class="r-entry-date">{{ entry.year }}</div>
          </div>
          <p v-if="entry.details" class="r-entry-desc">{{ entry.details }}</p>
        </div>
      </section>

      <!-- Skills section -->
      <section v-if="resume.skills.length > 0" class="r-section">
        <h2 class="r-section-title">Skills</h2>
        <div class="r-skills">
          <span
            v-for="(skill, index) in resume.skills"
            :key="index"
            class="r-skill-badge"
          >
            {{ skill }}
          </span>
        </div>
      </section>

    </div><!-- end resume-body -->

  </div><!-- end sheet -->
</template>


<script>
export default {
  name: 'ResumePreview',

  props: {
    // We receive the entire resume object from App.vue
    resume: {
      type: Object,
      required: true,
    },
  },

  // No emits — this component is READ-ONLY (display only)

  methods: {
    // ── formatDate ─────────────────────────────────
    // A helper method to format the date range nicely
    formatDate(start, end) {
      if (!start && !end) return ''
      if (!end) return start
      return `${start} – ${end}`
    },
  },
}
</script>


<style scoped>
/* The "paper" sheet */
.sheet {
  width: 100%;
  min-height: 1040px;
  background: white;
  box-shadow: 0 4px 40px rgba(0,0,0,0.15);
  border-radius: 4px;
  overflow: hidden;
  font-family: var(--font-body);
  color: var(--ink);
}

/* ── Header ──────────────────────────────────────── */
.resume-header {
  background: var(--ink);
  color: white;
  padding: 36px 44px 28px;
}

.r-name {
  font-family: var(--font-serif);
  font-size: 2rem;
  font-weight: 600;
  letter-spacing: -0.02em;
  line-height: 1.1;
}

.r-title {
  font-size: 0.88rem;
  color: rgba(255,255,255,0.65);
  margin-top: 5px;
  font-weight: 400;
  letter-spacing: 0.06em;
  text-transform: uppercase;
}

.r-contact {
  display: flex;
  flex-wrap: wrap;
  gap: 14px;
  margin-top: 16px;
  padding-top: 14px;
  border-top: 1px solid rgba(255,255,255,0.15);
  font-size: 0.76rem;
  color: rgba(255,255,255,0.7);
}

/* ── Body ────────────────────────────────────────── */
.resume-body {
  padding: 28px 44px 40px;
}

/* Section (summary, experience, etc.) */
.r-section {
  margin-bottom: 24px;
}

.r-section-title {
  font-size: 0.66rem;
  font-weight: 700;
  text-transform: uppercase;
  letter-spacing: 0.14em;
  color: var(--accent);
  border-bottom: 2px solid var(--accent);
  padding-bottom: 5px;
  margin-bottom: 12px;
}

/* Summary text */
.r-summary {
  font-size: 0.84rem;
  color: var(--ink-soft);
  line-height: 1.7;
}

/* Individual entry row */
.r-entry {
  margin-bottom: 14px;
  padding-left: 12px;
  border-left: 2.5px solid #e0e7ff;
}

.r-entry-top {
  display: flex;
  justify-content: space-between;
  align-items: flex-start;
}

.r-entry-title {
  font-size: 0.9rem;
  font-weight: 600;
  color: var(--ink);
}

.r-entry-sub {
  font-size: 0.8rem;
  color: var(--accent);
  font-weight: 500;
}

.r-entry-date {
  font-family: var(--font-mono);
  font-size: 0.72rem;
  color: var(--ink-muted);
  white-space: nowrap;
  padding-top: 2px;
}

.r-entry-desc {
  font-size: 0.81rem;
  color: var(--ink-soft);
  line-height: 1.65;
  margin-top: 5px;
}

/* Skills */
.r-skills {
  display: flex;
  flex-wrap: wrap;
  gap: 7px;
}

.r-skill-badge {
  font-size: 0.76rem;
  font-weight: 500;
  background: #eff6ff;
  color: var(--accent);
  border: 1.5px solid #bfdbfe;
  padding: 3px 11px;
  border-radius: 99px;
}

/* Print: remove box shadow */
@media print {
  .sheet {
    box-shadow: none;
    border-radius: 0;
    width: 100%;
  }
}
</style>
