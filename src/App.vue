<template>
  <!-- The root element wraps everything -->
  <div class="app">

    <!-- ── App Header ── -->
    <AppHeader @export="handleExport" />

    <!-- ── Main Layout: two columns ── -->
    <main class="app-layout">

      <!-- LEFT: Editor (all form sections) -->
      <section class="editor-column">
        <h2 class="column-label">✏️ Edit Your Resume</h2>

        <!--
          PersonalForm: handles name, email, phone etc.
          :data="resume.personal"  → sends data DOWN as a prop
          @update="..."           → listens for changes coming UP
        -->
        <PersonalForm
          :data="resume.personal"
          @update="resume.personal = $event"
        />

        <!--
          ExperienceForm: handles job entries (array)
          We pass the whole array and listen for the updated array
        -->
        <ExperienceForm
          :entries="resume.experience"
          @update="resume.experience = $event"
        />

        <!--
          EducationForm: handles school entries (array)
        -->
        <EducationForm
          :entries="resume.education"
          @update="resume.education = $event"
        />

        <!--
          SkillsForm: handles skills (array of strings)
        -->
        <SkillsForm
          :skills="resume.skills"
          @update="resume.skills = $event"
        />

      </section>

      <!-- RIGHT: Live preview of the resume -->
      <section class="preview-column">
        <h2 class="column-label">👁️ Live Preview</h2>

        <!--
          ResumePreview: a READ-ONLY display of the resume.
          It only receives props, never emits back.
        -->
        <div id="resume-preview">
        <ResumePreview :resume="resume" />
        </div>
      </section>

    </main>
  </div>
</template>


<script>
// ── Import all child components ──────────────────────────
import AppHeader       from './components/AppHeader.vue'
import PersonalForm    from './components/PersonalForm.vue'
import ExperienceForm  from './components/ExperienceForm.vue'
import EducationForm   from './components/EducationForm.vue'
import SkillsForm      from './components/SkillsForm.vue'
import ResumePreview   from './components/ResumePreview.vue'
import html2pdf from "html2pdf.js"

export default {
  // Component name (useful for Vue DevTools)
  name: 'App',

  // Register child components so we can use them in <template>
  components: {
    AppHeader,
    PersonalForm,
    ExperienceForm,
    EducationForm,
    SkillsForm,
    ResumePreview,
  },

  data() {
    // ── data() returns the reactive state ──────────────
    // This object is the SINGLE SOURCE OF TRUTH.
    // All child components read from here.
    return {
      resume: {

        // Personal info (flat object)
        personal: {
          name:     'Alex Rivera',
          title:    'Frontend Developer',
          email:    'alex@example.com',
          phone:    '+1 555 987 6543',
          location: 'San Francisco, CA',
          website:  'alexrivera.dev',
          summary:  'Enthusiastic developer with 3 years building responsive web apps. Passionate about clean code, accessibility, and creating delightful user experiences.',
        },

        // Work experience (array of objects)
        experience: [
          {
            id:          1,
            jobTitle:    'Junior Frontend Developer',
            company:     'Bright Labs',
            startDate:   'Jun 2022',
            endDate:     'Present',
            description: 'Built React components for a SaaS dashboard used by 10k+ customers. Improved page load speed by 35% through code splitting.',
          },
          {
            id:          2,
            jobTitle:    'Web Design Intern',
            company:     'Pixel Studio',
            startDate:   'Jan 2022',
            endDate:     'May 2022',
            description: 'Designed and coded landing pages for 8 client projects using HTML, CSS, and JavaScript.',
          },
        ],

        // Education (array of objects)
        education: [
          {
            id:       1,
            degree:   'B.Sc. Computer Science',
            school:   'State University',
            year:     '2018 – 2022',
            details:  'GPA 3.7 · Web Development Club President',
          },
        ],

        // Skills (simple array of strings)
        skills: ['Vue.js', 'React', 'HTML/CSS', 'JavaScript', 'Git', 'Figma'],
      },
    }
  },

  methods: {
    // Called when the user clicks "Export PDF" in AppHeader
    handleExport() {

      const element = document.getElementById("resume-preview")

      const options = {
        margin: 0,
        filename: "resume.pdf",
        image: { type: "jpeg", quality: 0.98 },
        html2canvas: { scale: 2 },
        jsPDF: { unit: "mm", format: "a4", orientation: "portrait" }
      }

      html2pdf().set(options).from(element).save()

    },
  },
}
</script>


<style>
/* ── Global Styles (applied to entire app) ─────────────── */

/* Import Google Fonts */
@import url('https://fonts.googleapis.com/css2?family=Sora:wght@300;400;500;600;700&family=Lora:ital,wght@0,400;0,600;1,400&family=JetBrains+Mono:wght@400;500&display=swap');

/* CSS variables: change these to retheme the whole app */
:root {
  --bg:         #f4f2ee;
  --white:      #ffffff;
  --ink:        #1c1917;
  --ink-soft:   #57534e;
  --ink-muted:  #a8a29e;
  --border:     #e7e5e0;
  --accent:     #2563eb;
  --accent-bg:  #eff6ff;
  --success:    #16a34a;
  --danger:     #dc2626;
  --radius:     10px;
  --shadow:     0 2px 16px rgba(28,25,23,0.08);
  --font-body:  'Sora', sans-serif;
  --font-serif: 'Lora', serif;
  --font-mono:  'JetBrains Mono', monospace;
}

/* Reset */
*, *::before, *::after { box-sizing: border-box; margin: 0; padding: 0; }

body {
  font-family: var(--font-body);
  background: var(--bg);
  color: var(--ink);
  font-size: 15px;
  line-height: 1.6;
  -webkit-font-smoothing: antialiased;
}

/* ── Layout ──────────────────────────────────────────── */
.app { min-height: 100vh; display: flex; flex-direction: column; }

.app-layout {
  display: grid;
  grid-template-columns: 400px 1fr; /* editor | preview */
  gap: 0;
  flex: 1;
  min-height: calc(100vh - 60px);
}

.editor-column {
  background: var(--white);
  border-right: 1.5px solid var(--border);
  overflow-y: auto;
  max-height: calc(100vh - 60px);
  position: sticky;
  top: 60px;
  padding: 24px 20px 48px;
  display: flex;
  flex-direction: column;
  gap: 12px;
}

.preview-column {
  background: #ddd9d3;
  overflow-y: auto;
  padding: 32px 24px;
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 16px;
}

/* Column headings */
.column-label {
  font-size: 0.72rem;
  font-weight: 600;
  text-transform: uppercase;
  letter-spacing: 0.1em;
  color: var(--ink-muted);
  padding-bottom: 8px;
  border-bottom: 1px solid var(--border);
}

/* ── Reusable card (used in forms) ───────────────────── */
.card {
  background: var(--white);
  border: 1.5px solid var(--border);
  border-radius: var(--radius);
  padding: 20px;
  transition: border-color 0.2s;
}
.card:hover { border-color: var(--accent); }

/* ── Section title inside editor ────────────────────── */
.section-heading {
  font-size: 0.8rem;
  font-weight: 700;
  text-transform: uppercase;
  letter-spacing: 0.08em;
  color: var(--ink-soft);
  margin-bottom: 14px;
  display: flex;
  align-items: center;
  gap: 6px;
}

/* ── Form fields ─────────────────────────────────────── */
.field { display: flex; flex-direction: column; gap: 4px; margin-bottom: 10px; }

.field label {
  font-size: 0.7rem;
  font-weight: 600;
  text-transform: uppercase;
  letter-spacing: 0.07em;
  color: var(--ink-soft);
}

.field input,
.field textarea {
  font-family: var(--font-body);
  font-size: 0.88rem;
  color: var(--ink);
  background: #faf9f7;
  border: 1.5px solid var(--border);
  border-radius: 7px;
  padding: 8px 11px;
  width: 100%;
  outline: none;
  transition: all 0.18s;
  resize: vertical;
}

.field input:focus,
.field textarea:focus {
  border-color: var(--accent);
  background: var(--white);
  box-shadow: 0 0 0 3px rgba(37,99,235,0.10);
}

.field textarea { min-height: 72px; }

/* Two-column grid for short fields */
.two-col { display: grid; grid-template-columns: 1fr 1fr; gap: 0 12px; }

/* ── Buttons ─────────────────────────────────────────── */
.btn {
  font-family: var(--font-body);
  font-size: 0.8rem;
  font-weight: 600;
  padding: 8px 16px;
  border-radius: 7px;
  border: none;
  cursor: pointer;
  display: inline-flex;
  align-items: center;
  gap: 5px;
  transition: all 0.18s;
}
.btn-primary   { background: var(--accent); color: white; }
.btn-primary:hover { background: #1d4ed8; transform: translateY(-1px); }
.btn-outline   { background: transparent; border: 1.5px solid var(--border); color: var(--ink-soft); }
.btn-outline:hover { border-color: var(--accent); color: var(--accent); }
.btn-danger    { background: transparent; border: 1.5px solid #fca5a5; color: var(--danger); font-size:0.75rem; padding: 4px 10px; }
.btn-danger:hover { background: var(--danger); color: white; }
.btn-add       { background: var(--accent-bg); color: var(--accent); border: 1.5px dashed var(--accent); width: 100%; justify-content: center; padding: 9px; border-radius: 8px; margin-top: 8px; }
.btn-add:hover { background: var(--accent); color: white; border-style: solid; }


</style>
