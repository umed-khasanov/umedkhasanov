<script setup lang="ts">
import { ref, computed, onMounted, onUnmounted } from 'vue'

const cvUrl = '/umedkhasanovCV.pdf'
const cvDownloadName = 'Umedchon-Khasanov-CV.pdf'

const skills = [
  { title: 'Backend', items: ['Java 21', 'Spring Boot', 'Spring Data JPA / Hibernate', 'Spring Security, JWT', 'REST APIs', 'PostgreSQL', 'Docker', 'Maven', 'Git'] },
  { title: 'Frontend and design', items: ['Vue.js', 'JavaScript', 'HTML, CSS', 'Figma', 'UI/UX design', 'Adobe Photoshop, Illustrator', 'After Effects, Premiere Pro'] }
]

type Category = 'Backend' | 'Design'
const filters: Array<'All' | Category> = ['All', 'Backend', 'Design']
const activeFilter = ref<'All' | Category>('All')

const projects: Array<{ name: string; category: Category; description: string; tech: string[]; link?: string; note?: string }> = [
  {
    name: 'TicketStream',
    category: 'Backend',
    description: 'Secure ticket booking REST API with JWT authentication and role-based access, request validation and a PostgreSQL database.',
    tech: ['Java 21', 'Spring Boot 4', 'Spring Data JPA', 'Spring Security', 'PostgreSQL'],
    link: 'https://github.com/umed-khasanov/TicketStream-Secure-Ticket-Booking-REST-API'
  },
  {
    name: 'GradeTracker',
    category: 'Backend',
    description: 'REST API for managing student academic records: CRUD operations, validation, centralised exception handling and a clean layered architecture.',
    tech: ['Java', 'Spring Boot', 'Spring Data JPA', 'PostgreSQL'],
    link: 'https://github.com/umed-khasanov/gradetracker'
  },
  {
    name: 'Creato',
    category: 'Design',
    description: 'Online student gallery where students showcase creative work and employers discover talent. Group project (IDM3). I lead the brand identity and UI design.',
    tech: ['Figma', 'Brand identity', 'UI/UX'],
    note: 'Case study coming soon'
  }
]

const visibleProjects = computed(() =>
  activeFilter.value === 'All' ? projects : projects.filter((p) => p.category === activeFilter.value)
)

const education = [
  {
    period: '2024 — 2028',
    title: 'BSc (Hons) Creative Digital Computing',
    org: 'Technological University of the Shannon (TUS)',
    location: 'Limerick',
    points: [
      'Specialising in Java backend development: Spring Boot, Spring Data JPA, PostgreSQL and Docker.',
      'Coursework also covers UI/UX design, Unity and interactive media.'
    ]
  }
]

const experience = [
  {
    period: 'May 2025 — Present',
    title: 'Commis Chef',
    org: "Rosie's Bar",
    location: 'Aghada, Co. Cork',
    points: [
      'Prep, cook and plate dishes during busy service as part of a small kitchen team.',
      'Work to food safety and hygiene standards under time pressure.'
    ]
  },
  {
    period: 'Oct 2023 — Feb 2024',
    title: 'Sales Assistant',
    org: 'Penneys',
    location: 'Killarney, Co. Kerry',
    points: [
      'Served customers on the shop floor and at the till.',
      'Kept stock replenished and the floor organised during peak trading.'
    ]
  }
]

const contactForm = ref({ name: '', email: '', message: '' })
const formStatus = ref<'idle' | 'sending' | 'sent' | 'error'>('idle')

const handleFormSubmit = async () => {
  formStatus.value = 'sending'
  try {
    const res = await fetch('https://formspree.io/f/meeylyrr', {
      method: 'POST',
      headers: { 'Content-Type': 'application/json', Accept: 'application/json' },
      body: JSON.stringify(contactForm.value)
    })
    if (!res.ok) throw new Error(`Formspree responded with ${res.status}`)
    formStatus.value = 'sent'
    contactForm.value = { name: '', email: '', message: '' }
  } catch (error) {
    console.error(error)
    formStatus.value = 'error'
  }
}

const activeSection = ref('top')
let observer: IntersectionObserver | null = null

onMounted(() => {
  observer = new IntersectionObserver((entries) => {
    entries.forEach((entry) => {
      if (entry.isIntersecting) activeSection.value = entry.target.id
    })
  }, { rootMargin: '-30% 0px -60% 0px' })
  document.querySelectorAll('section[id]').forEach((section) => observer?.observe(section))
})

onUnmounted(() => observer?.disconnect())
</script>

<template>
  <div class="page">
    <header class="topbar">
      <a href="#top" class="brand">Umedchon Khasanov<span class="brand-dot" aria-hidden="true"></span></a>
      <nav class="nav" aria-label="Sections">
        <a href="#work" :class="{ active: activeSection === 'work' }">Work</a>
        <a href="#skills" :class="{ active: activeSection === 'skills' }">Skills</a>
        <a href="#resume" :class="{ active: activeSection === 'resume' }">Resume</a>
        <a href="#contact" :class="{ active: activeSection === 'contact' }">Contact</a>
      </nav>
    </header>

    <main class="content">
      <section id="top" class="hero">
        <p class="eyebrow">Creative Digital Computing / TUS Midwest / Limerick</p>
        <h1 class="hero-name">Umedchon<br />Khasanov<span class="hero-dot" aria-hidden="true">.</span></h1>
        <div class="hero-grid">
          <p class="hero-text">
            I build secure, well-structured backend systems with Java and Spring Boot. Alongside that I work in UI/UX design, so I think about how software works and how it feels to use.
          </p>
          <div class="hero-side">
            <p class="availability">
              <span class="availability-dot" aria-hidden="true"></span>
              Open to a work placement from January 2027
            </p>
            <div class="hero-links">
              <a :href="cvUrl" target="_blank" rel="noopener noreferrer" class="btn btn--solid">View CV</a>
              <a href="https://github.com/umed-khasanov" target="_blank" rel="noopener noreferrer" class="btn">GitHub</a>
              <a href="https://www.linkedin.com/in/umed-khasanov-6b5770303" target="_blank" rel="noopener noreferrer" class="btn">LinkedIn</a>
              <a href="https://www.behance.net/umedkhasanov" target="_blank" rel="noopener noreferrer" class="btn">Behance</a>
            </div>
          </div>
        </div>
      </section>

      <section id="work" class="block">
        <div class="block-head">
          <h2 class="block-title">Selected work</h2>
          <div class="filters" role="group" aria-label="Filter projects">
            <button
              v-for="f in filters"
              :key="f"
              type="button"
              class="filter"
              :class="{ active: activeFilter === f }"
              @click="activeFilter = f"
            >{{ f }}</button>
          </div>
        </div>

        <ol class="projects">
          <li v-for="(project, i) in visibleProjects" :key="project.name" class="project">
            <span class="project-no">{{ String(i + 1).padStart(2, '0') }}</span>
            <div class="project-main">
              <h3 class="project-name">{{ project.name }}</h3>
              <p class="project-desc">{{ project.description }}</p>
              <p class="project-tech">{{ project.tech.join(' / ') }}</p>
            </div>
            <div class="project-side">
              <span class="project-cat">{{ project.category }}</span>
              <a v-if="project.link" :href="project.link" target="_blank" rel="noopener noreferrer" class="project-link">Source code ↗</a>
              <span v-else class="project-note">{{ project.note }}</span>
            </div>
          </li>
        </ol>
      </section>

      <section id="skills" class="block">
        <h2 class="block-title">Skills</h2>
        <div class="skills">
          <div v-for="group in skills" :key="group.title" class="skill-col">
            <h3 class="skill-title">{{ group.title }}</h3>
            <ul class="skill-list">
              <li v-for="item in group.items" :key="item">{{ item }}</li>
            </ul>
          </div>
        </div>
      </section>

      <section id="resume" class="block">
        <div class="block-head">
          <h2 class="block-title">Resume</h2>
          <a :href="cvUrl" :download="cvDownloadName" class="btn btn--solid">Download CV (PDF)</a>
        </div>

        <h3 class="sub-title">Education</h3>
        <ol class="timeline">
          <li v-for="item in education" :key="item.title" class="timeline-item">
            <span class="timeline-date">{{ item.period }}</span>
            <div>
              <h4 class="timeline-title">{{ item.title }}</h4>
              <p class="timeline-org">{{ item.org }}, {{ item.location }}</p>
              <ul class="timeline-points">
                <li v-for="point in item.points" :key="point">{{ point }}</li>
              </ul>
            </div>
          </li>
        </ol>

        <h3 class="sub-title">Experience</h3>
        <ol class="timeline">
          <li v-for="job in experience" :key="job.title + job.org" class="timeline-item">
            <span class="timeline-date">{{ job.period }}</span>
            <div>
              <h4 class="timeline-title">{{ job.title }}</h4>
              <p class="timeline-org">{{ job.org }}, {{ job.location }}</p>
              <ul class="timeline-points">
                <li v-for="point in job.points" :key="point">{{ point }}</li>
              </ul>
            </div>
          </li>
        </ol>
      </section>

      <section id="contact" class="block">
        <h2 class="block-title">Contact</h2>
        <div class="contact-grid">
          <p class="contact-text">
            I'm looking for a work placement from January 2027, ideally in Java backend development. If you have an opportunity or want to talk about a project, send me a message or email
            <a href="mailto:humed256@gmail.com" class="inline-link">humed256@gmail.com</a>.
          </p>

          <form @submit.prevent="handleFormSubmit" class="form">
            <label class="label" for="name">Name</label>
            <input v-model="contactForm.name" class="input" id="name" name="name" type="text" autocomplete="name" required />

            <label class="label" for="email">Email</label>
            <input v-model="contactForm.email" class="input" id="email" name="email" type="email" autocomplete="email" required />

            <label class="label" for="message">Message</label>
            <textarea v-model="contactForm.message" class="input" id="message" name="message" rows="5" required></textarea>

            <button type="submit" class="btn btn--solid submit" :disabled="formStatus === 'sending'">
              {{ formStatus === 'sending' ? 'Sending…' : 'Send message' }}
            </button>

            <p v-if="formStatus === 'sent'" class="status" role="status">Message sent. I'll reply to the email address you gave.</p>
            <p v-else-if="formStatus === 'error'" class="status status--error" role="alert">
              Your message didn't send. Try again, or email me at
              <a href="mailto:humed256@gmail.com" class="inline-link">humed256@gmail.com</a>.
            </p>
          </form>
        </div>
      </section>
    </main>

    <footer class="footer">
      <p>Umedchon Khasanov, Limerick and Cork, Ireland</p>
      <p>Designed in Figma, built with Vue.js, deployed on Vercel.</p>
    </footer>
  </div>
</template>

<style scoped>
.page { max-width: 1120px; margin: 0 auto; padding: 0 32px; }

.topbar {
  position: sticky; top: 0; z-index: 10;
  display: flex; justify-content: space-between; align-items: center;
  padding: 20px 0; background: var(--paper); border-bottom: 1px solid var(--ink);
}
.brand { font-family: var(--font-head); font-weight: 700; font-size: 17px; letter-spacing: -0.01em; }
.brand-dot { display: inline-block; width: 8px; height: 8px; margin-left: 4px; border-radius: 50%; background: var(--orange); }
.nav { display: flex; gap: 28px; font-size: 14px; }
.nav a { color: var(--muted); padding-bottom: 2px; border-bottom: 1px solid transparent; }
.nav a:hover, .nav a.active { color: var(--ink); border-bottom-color: var(--orange); }

.hero { padding: 88px 0 72px; }
.eyebrow { font-size: 12px; letter-spacing: 0.14em; text-transform: uppercase; color: var(--muted); margin-bottom: 28px; }
.hero-name {
  font-family: var(--font-head); font-weight: 700;
  font-size: clamp(52px, 11vw, 132px); line-height: 0.95; letter-spacing: -0.04em;
}
.hero-dot { color: var(--orange); }
.hero-grid { display: grid; grid-template-columns: 1.2fr 1fr; gap: 56px; margin-top: 56px; align-items: start; }
.hero-text { font-size: 20px; line-height: 1.55; max-width: 560px; }
.availability { display: flex; align-items: center; gap: 10px; font-size: 14px; margin-bottom: 22px; }
.availability-dot { width: 9px; height: 9px; border-radius: 50%; background: var(--orange); flex-shrink: 0; }
.hero-links { display: flex; flex-wrap: wrap; gap: 10px; }

.btn {
  display: inline-block; padding: 10px 18px; font-size: 14px; font-weight: 500; font-family: var(--font-body);
  border: 1px solid var(--ink); background: transparent; color: var(--ink); cursor: pointer;
  transition: background 0.15s, color 0.15s;
}
.btn:hover { background: var(--ink); color: var(--paper); }
.btn--solid { background: var(--ink); color: var(--paper); }
.btn--solid:hover { background: var(--cobalt); border-color: var(--cobalt); }
.btn:disabled { opacity: 0.6; cursor: default; }

.block { padding: 72px 0; border-top: 1px solid var(--ink); }
.block-head { display: flex; justify-content: space-between; align-items: center; gap: 16px; flex-wrap: wrap; margin-bottom: 40px; }
.block-title { font-family: var(--font-head); font-weight: 600; font-size: 14px; letter-spacing: 0.14em; text-transform: uppercase; }
.block > .block-title { margin-bottom: 40px; }
.block-head .block-title { margin-bottom: 0; }

.filters { display: flex; gap: 6px; }
.filter {
  padding: 6px 14px; font-size: 13px; font-family: var(--font-body);
  background: transparent; border: 1px solid var(--line); color: var(--muted); cursor: pointer;
}
.filter:hover { border-color: var(--ink); color: var(--ink); }
.filter.active { background: var(--ink); border-color: var(--ink); color: var(--paper); }

.projects { list-style: none; }
.project {
  display: grid; grid-template-columns: 56px 1fr 180px; gap: 24px;
  padding: 32px 0; border-top: 1px solid var(--line);
}
.project:last-child { border-bottom: 1px solid var(--line); }
.project-no { font-family: var(--font-head); font-size: 14px; color: var(--muted); padding-top: 6px; }
.project-name { font-family: var(--font-head); font-weight: 600; font-size: 30px; letter-spacing: -0.02em; line-height: 1.15; }
.project-desc { margin-top: 10px; max-width: 620px; color: var(--ink); }
.project-tech { margin-top: 14px; font-size: 13px; color: var(--muted); }
.project-side { display: flex; flex-direction: column; align-items: flex-end; gap: 12px; padding-top: 8px; }
.project-cat { font-size: 12px; letter-spacing: 0.1em; text-transform: uppercase; color: var(--muted); }
.project-link { font-size: 14px; border-bottom: 1px solid var(--orange); padding-bottom: 1px; }
.project-link:hover { color: var(--cobalt); border-bottom-color: var(--cobalt); }
.project-note { font-size: 13px; color: var(--muted); }

.skills { display: grid; grid-template-columns: repeat(2, 1fr); gap: 40px; }
.skill-title { font-family: var(--font-head); font-weight: 600; font-size: 20px; margin-bottom: 16px; }
.skill-list { list-style: none; }
.skill-list li { padding: 8px 0; border-top: 1px solid var(--line); font-size: 15px; }

.sub-title { font-family: var(--font-head); font-weight: 600; font-size: 20px; margin: 8px 0 18px; }
.timeline { list-style: none; margin-bottom: 40px; }
.timeline-item { display: grid; grid-template-columns: 200px 1fr; gap: 24px; padding: 20px 0; border-top: 1px solid var(--line); }
.timeline-date { font-size: 14px; color: var(--muted); }
.timeline-title { font-size: 17px; font-weight: 600; }
.timeline-org { font-size: 14px; color: var(--muted); margin: 2px 0 10px; }
.timeline-points { padding-left: 18px; font-size: 15px; }
.timeline-points li { margin-bottom: 4px; }

.contact-grid { display: grid; grid-template-columns: 1fr 1fr; gap: 56px; align-items: start; }
.contact-text { font-size: 20px; line-height: 1.55; max-width: 480px; }
.inline-link { border-bottom: 1px solid var(--orange); }
.inline-link:hover { color: var(--cobalt); }
.form { display: flex; flex-direction: column; }
.label { font-size: 12px; letter-spacing: 0.1em; text-transform: uppercase; color: var(--muted); margin: 16px 0 6px; }
.label:first-of-type { margin-top: 0; }
.input {
  width: 100%; padding: 12px 14px; font-family: var(--font-body); font-size: 15px;
  background: transparent; border: 1px solid var(--ink); color: var(--ink); border-radius: 0; resize: vertical;
}
.input:focus { border-color: var(--cobalt); }
.submit { align-self: flex-start; margin-top: 22px; }
.status { margin-top: 14px; font-size: 14px; }
.status--error { color: #B3261E; }

.footer {
  display: flex; justify-content: space-between; gap: 16px; flex-wrap: wrap;
  padding: 28px 0 40px; border-top: 1px solid var(--ink); font-size: 13px; color: var(--muted);
}

@media (max-width: 860px) {
  .page { padding: 0 20px; }
  .hero { padding: 56px 0 48px; }
  .hero-grid, .contact-grid { grid-template-columns: 1fr; gap: 32px; }
  .skills { grid-template-columns: 1fr; gap: 32px; }
  .project { grid-template-columns: 36px 1fr; }
  .project-side { grid-column: 2; flex-direction: row; align-items: center; gap: 18px; padding-top: 0; }
  .timeline-item { grid-template-columns: 1fr; gap: 6px; }
  .nav { gap: 16px; }
  .block { padding: 48px 0; }
}
@media (max-width: 520px) {
  .nav a:nth-child(2) { display: none; }
  .brand { font-size: 15px; }
  .hero-text, .contact-text { font-size: 18px; }
}
</style>
