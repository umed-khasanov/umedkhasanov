<script setup lang="ts">
import { ref, onMounted, onUnmounted } from 'vue'

const keyStacks = ['Java 21', 'JPA(HIBERNATE)', 'Spring Boot 3', 'Microservices', 'Docker', 'PostgreSQL', 'REST API', 'Apache Kafka', 'JWT']
const creativeTechnologies = ['UI/UX', 'Unity 3D', 'Blender', 'Adobe Ps, IL']

const projects = [
  {
    name: 'GradeTracker',
    description: 'Backend application for managing student academic records, built with Spring Boot. The system provides RESTful operations for student management, grade updates, and data validation while following clean architecture principles.',
    tech: ['Spring Boot 3', 'Docker Compose', 'PostgreSQL', 'Java 21', 'Hibernate JPA', 'REST API']
  },
  {
    name: 'Microservices Ticket Stream',
    description: 'Microservices architecture built with Apache Kafka and Spring Security. The distributed system separates write-heavy ticket ingestion (Command Service), event processing streams (Kafka Broker), and role-based cryptographic network filters (JWT Gateway) for a scalable and resilient infrastructure.',
    tech: ['Spring Boot 3', 'Docker Compose', 'PostgreSQL', 'Hibernate JPA', 'Java 21', 'Apache Kafka', 'REST API', 'Spring Security', 'JWT']
  }
]

const contactForm = ref({
  name: '',
  email: '',
  message: ''
})

const handleFormSubmit = async (e: Event) => {
  const form = e.target as HTMLFormElement

  try {
    await fetch('https://formspree.io', {
      method: 'POST',
      headers: { 'Content-Type': 'application/json' },
      body: JSON.stringify(contactForm.value)
    })

    alert('Thank you! Your message has been sent.')
    // Полностью очищаем поля формы после успешного действия
    contactForm.value = { name: '', email: '', message: '' }
  } catch (error) {
    alert('Oops! There was a problem submitting your form.')
  }
}

const mouseX = ref(-1000)
const mouseY = ref(-1000)

const handleMouseMove = (e: MouseEvent) => {
  mouseX.value = e.clientX
  mouseY.value = e.clientY
}

const showLogo = ref(false)

const activeSection = ref('about')
let observer: IntersectionObserver | null = null

onMounted(() => {
  window.addEventListener('mousemove', handleMouseMove)

  setTimeout(() => {
    showLogo.value = true
  }, 100)

  observer = new IntersectionObserver((entries) => {
    entries.forEach((entry) => {
      if (entry.isIntersecting) {
        activeSection.value = entry.target.id
      }
    })
  }, {
    rootMargin: '-25% 0px -55% 0px'
  })

  document.querySelectorAll('section[id]').forEach((section) => {
    observer?.observe(section)
  })
})

onUnmounted(() => {
  window.removeEventListener('mousemove', handleMouseMove)
  observer?.disconnect()
})
</script>


<template>
  <div class="top-left-logo">
    <img src="./assets/logo.png" class="logo-image" alt="Umed Khasanov" />
  </div>

  <div class="portfolio-container">
    <div
        class="spotlight"
        :style="{
        transform: `translate(${mouseX}px, ${mouseY}px)`
      }"
    ></div>

    <aside class="sidebar">
      <nav class="nav-links">
        <a href="#about" class="nav-item" :class="{ active: activeSection === 'about' }">About</a>
        <a href="#stack" class="nav-item" :class="{ active: activeSection === 'stack' }">Core Stack</a>
        <a href="#projects" class="nav-item" :class="{ active: activeSection === 'projects' }">Projects</a>
        <a href="#connect" class="nav-item" :class="{ active: activeSection === 'connect' }">Connect</a>
      </nav>

      <div class="sidebar-panel contact-panel">
        <div class="panel-title">Contact Info</div>
        <div class="contact-list">
          <a href="mailto:humed256@gmail.com" class="contact-link-item">
            <span class="contact-icon">✉</span> humed256@gmail.com
          </a>
          <div class="contact-text-item">
            <span class="contact-icon">⚲</span> Limerick, Ireland
          </div>
          <div class="resume-wrapper">
            <a href="/umedkhasanovCV.pdf" target="_blank" class="resume-button">
              View Resume ↗
            </a>

          </div>
        </div>
      </div>
    </aside>

    <main class="main-content">

      <section id="about" class="about-section">
        <h1 class="hero-name">Umed Khasanov</h1>
        <p class="hero-title">Backend Engineer</p>

        <div class="bio-text">
          <p>Hi! I'm Umed Khasanov, a Creative Digital Computing student focused on building secure and scalable backend systems with Java and Spring Boot.</p>
          <p>I specialize in designing REST APIs, database-driven applications, and reliable server-side solutions using modern development practices.</p>
          <p>Alongside backend development, I have experience with frontend technologies and UI/UX design, allowing me to create complete and user-focused applications.</p>
          <p>Currently pursuing a BSc in Creative Digital Computing at TUS Limerick and seeking a Software Engineering internship where I can contribute, learn, and grow as a developer.</p>
        </div>
        <div class="socials-block">
          <h3 class="findIcon">Get in touch</h3>
          <div class="social-icons">
            <a href="https://github.com/umed-khasanov" target="_blank" class="social-link" aria-label="GitHub">
              <svg viewBox="0 0 24 24" class="icon"><path d="M12 .297c-6.63 0-12 5.373-12 12 0 5.303 3.438 9.8 8.205 11.385.6.113.82-.258.82-.577 0-.285-.01-1.04-.015-2.04-3.338.724-4.042-1.61-4.042-1.61C4.422 18.07 3.633 17.7 3.633 17.7c-1.087-.744.084-.729.084-.729 1.205.084 1.838 1.236 1.838 1.236 1.07 1.835 2.809 1.305 3.495.998.108-.776.417-1.305.76-1.605-2.665-.3-5.466-1.332-5.466-5.93 0-1.31.465-2.38 1.235-3.22-.135-.303-.54-1.523.105-3.176 0 0 1.005-.322 3.3 1.23.96-.267 1.98-.399 3-.405 1.02.006 2.04.138 3 .405 2.28-1.552 3.285-1.23 3.285-1.23.645 1.653.24 2.873.12 3.176.765.84 1.23 1.91 1.23 3.22 0 4.61-2.805 5.625-5.475 5.92.42.36.81 1.096.81 2.22 0 1.606-.015 2.896-.015 3.286 0 .315.21.69.825.57C20.565 22.092 24 17.592 24 12.297c0-6.627-5.373-12-12-12"/></svg>
            </a>
            <a href="https://linkedin.com" target="_blank" class="social-link" aria-label="LinkedIn">
              <svg viewBox="0 0 24 24" class="icon"><path d="M19 0h-14c-2.761 0-5 2.239-5 5v14c0 2.761 2.239 5 5 5h14c2.762 0 5-2.239 5-5v-14c0-2.761-2.238-5-5-5zm-11 19h-3v-11h3v11zm-1.5-12.268c-.966 0-1.75-.79-1.75-1.764s.784-1.764 1.75-1.764 1.75.79 1.75 1.764-.783 1.764-1.75 1.764zm13.5 12.268h-3v-5.604c0-3.368-4-3.113-4 0v5.604h-3v-11h3v1.765c1.396-2.586 7-2.777 7 2.476v6.759z"/></svg>
            </a>
            <a href="https://www.instagram.com/umed1_o?igsh=MTYyZnVibXh0eDYxYQ==" target="_blank" class="social-link" aria-label="Instagram">
              <svg viewBox="0 0 24 24" class="icon"><path d="M12 2.163c3.204 0 3.584.012 4.85.07 3.252.148 4.771 1.691 4.919 4.919.058 1.265.069 1.645.069 4.849 0 3.205-.012 3.584-.069 4.849-.149 3.225-1.664 4.771-4.919 4.919-1.266.058-1.644.07-4.85.07-3.204 0-3.584-.012-4.849-.07-3.26-.149-4.771-1.699-4.919-4.92-.058-1.265-.07-1.644-.07-4.849 0-3.204.013-3.583.07-4.849.149-3.227 1.664-4.771 4.919-4.919 1.266-.057 1.645-.069 4.849-.069zm0-2c-3.259 0-3.667.014-4.947.072-4.358.2-6.78 2.618-6.98 6.98-.059 1.281-.073 1.689-.073 4.948 0 3.259.014 3.668.072 4.948.2 4.358 2.618 6.78 6.98 6.98 1.281.058 1.689.072 4.948.072 3.259 0 3.668-.014 4.948-.072 4.354-.2 6.782-2.618 6.979-6.98.059-1.28.073-1.689.073-4.948 0-3.259-.014-3.667-.072-4.947-.196-4.354-2.617-6.78-6.979-6.98-1.281-.059-1.69-.073-4.949-.073zm0 5.838c-3.403 0-6.162 2.759-6.162 6.162s2.759 6.163 6.162 6.163 6.162-2.759 6.162-6.163c0-3.403-2.759-6.162-6.162-6.162zm0 10.162c-2.209 0-4-1.79-4-4 0-2.209 1.791-4 4-4s4 1.791 4 4c0 2.21-1.791 4-4 4zm6.406-11.845c-.796 0-1.441.645-1.441 1.44s.645 1.44 1.441 1.44c.795 0 1.439-.645 1.439-1.44s-.644-1.44-1.439-1.44z"/></svg>
            </a>
          </div>
        </div>
      </section>

      <hr class="section-divider" />

      <!-- Секция CORE STACK -->
      <section id="stack" class="stack-section">
        <div class="stack-block">
          <div class="stack-header">
            <span class="dash-title">—</span>
            <h2 class="section-title">Key Stacks</h2>
          </div>
          <div class="tech-grid">
            <template v-for="(tech, index) in keyStacks" :key="tech">
              <span class="tech-item">{{ tech }}</span>
              <span v-if="index < keyStacks.length - 1" class="dash">—</span>
            </template>
          </div>
        </div>

        <div class="stack-block" style="margin-top: 60px;">
          <div class="stack-header">
            <span class="dash-title">—</span>
            <h2 class="section-title">Creative Technologies</h2>
          </div>
          <div class="tech-grid">
            <template v-for="(tech, index) in creativeTechnologies" :key="tech">
              <span class="tech-item">{{ tech }}</span>
              <span v-if="index < creativeTechnologies.length - 1" class="dash">—</span>
            </template>
          </div>
        </div>
      </section>

      <hr class="section-divider" />

      <section id="projects" class="projects-section">
        <h2 class="main-projects-title">Projects</h2>
        <div v-for="project in projects" :key="project.name" class="project-card">
          <h3 class="project-name">{{ project.name }}</h3>
          <p class="project-description">{{ project.description }}</p>
          <div class="project-tech-list">
            <template v-for="(tech, index) in project.tech" :key="tech">
              <span>{{ tech }}</span>
              <span v-if="index < project.tech.length - 1" class="dash">—</span>
            </template>
          </div>
          <a href="#" class="view-link">View Tech Specs & Code ↗</a>
        </div>
      </section>

      <hr class="section-divider" />

      <section id="connect" class="connect-section">
        <h2 class="main-projects-title">Let's Work Together</h2>
        <p class="project-description" style="margin-bottom: 30px;">
          I’m currently seeking Java Backend Developer opportunities where I can contribute to building reliable, scalable applications while continuing to grow my skills in Spring Boot, REST APIs, and backend architecture. If you have an opportunity, want to discuss a project, or simply connect, feel free to get in touch!
        </p>

        <form @submit.prevent="handleFormSubmit" class="connect-form">
          <div class="form-group">
            <label class="form-label" for="name">Name</label>
            <input v-model="contactForm.name" class="form-input" id="name" type="text" placeholder="Your Name" required />
          </div>
          <div class="form-group">
            <label class="form-label" for="email">Email</label>
            <input v-model="contactForm.email" class="form-input" id="email" type="email" placeholder="email@example.com" required />
          </div>
          <div class="form-group">
            <label class="form-label" for="message">Message</label>
            <textarea v-model="contactForm.message" class="form-textarea" id="message" rows="5" placeholder="Hi Umed, I stumbled upon your portfolio..." required></textarea>
          </div>

          <button type="submit" class="submit-button">Send Message </button>
        </form>
      </section>

    </main>
  </div>
</template>

<style scoped>
.top-left-logo {
  position: fixed;
  top: 40px;
  left: 60px;
  display: flex;
  align-items: center;
  gap: 12px;
  z-index: 10000;
  animation: pulse-fade 4s ease-in-out infinite;
}

.logo-image {
  height: 40px;
  width: auto;
  display: block;
}

@keyframes pulse-fade {
  0%, 100% {
    opacity: 0.2;
    transform: scale(0.98);
  }
  30% {
    opacity: 1;
    transform: scale(1);
  }
}

.spotlight {
  position: fixed;
  top: 0;
  left: 0;
  width: 700px;
  height: 700px;
  margin-left: -350px;
  margin-top: -350px;
  background: radial-gradient(circle, rgba(185, 179, 166, 0.12) 0%, transparent 65%);
  pointer-events: none;
  z-index: 9999;
  mix-blend-mode: screen;
  will-change: transform;
}

.portfolio-container {
  display: flex;
  justify-content: space-between;
  max-width: 1200px;
  min-height: 100vh;
  margin: 0 auto;
  padding: 140px 60px 100px 60px;
  background-color: #100808;
  overflow: hidden;
}

.sidebar {
  width: 220px;
  flex-shrink: 0;
  position: fixed;
  top: 180px;
  height: fit-content;
  display: flex;
  flex-direction: column;
  gap: 35px;
}

.nav-links {
  display: flex;
  flex-direction: column;
  gap: 22px;
}

.nav-item {
  display: flex;
  align-items: center;
  color: #5A5247 !important;
  font-size: 16px;
  font-weight: 500;
  transition: color 0.4s ease;
  text-decoration: none;
}

.nav-item::before {
  content: '';
  display: inline-block;
  width: 15px;
  height: 1px;
  background-color: #5A5247;
  margin-right: 12px;
  transition: width 0.4s ease, background-color 0.4s ease;
}

.nav-item:hover,
.nav-item.active {
  color: #B9B3A6 !important;
}

.nav-item:hover::before,
.nav-item.active::before {
  width: 38px;
  background-color: #B9B3A6;
}

.sidebar-panel {
  border-top: 1px solid #40382E;
  padding-top: 20px;
}

.panel-title {
  font-size: 13px;
  font-weight: 600;
  color: #B9B3A6;
  text-transform: uppercase;
  letter-spacing: 1px;
  margin-bottom: 15px;
}

.chart-container {
  display: flex;
  flex-direction: column;
  gap: 10px;
}

.chart-row {
  display: flex;
  align-items: center;
  justify-content: space-between;
  gap: 10px;
}

.chart-label {
  font-size: 12px;
  color: #8E877D;
  width: 60px;
}

.chart-bar-wrapper {
  flex-grow: 1;
  height: 4px;
  background-color: #261F18;
  border-radius: 2px;
  overflow: hidden;
}

.chart-bar {
  height: 100%;
  background-color: #B9B3A6;
  opacity: 0.8;
  transition: width 0.5s ease;
}

.contact-list {
  display: flex;
  flex-direction: column;
  gap: 10px;
}

.contact-link-item, .contact-text-item {
  font-size: 13px;
  color: #8E877D;
  text-decoration: none;
  display: flex;
  align-items: center;
  gap: 8px;
  transition: color 0.3s ease;
}

.contact-link-item:hover {
  color: #B9B3A6;
}

.contact-icon {
  color: #5A5247;
  font-size: 14px;
}
.resume-wrapper {
  margin-top: 20px;
}

.resume-button {
  display: inline-block;
  width: 100%;
  text-align: center;
  font-size: 14px;
  font-weight: 600;
  color: #B9B3A6;
  border: 1px solid #40382E;
  padding: 10px 16px;
  border-radius: 2px;
  transition: color 0.3s ease, border-color 0.3s ease, background-color 0.3s ease;
}

.resume-button:hover {
  color: #100808;
  background-color: #B9B3A6;
  border-color: #B9B3A6;
}

.main-content {
  width: 640px;
  flex-shrink: 0;
  display: flex;
  flex-direction: column;
  gap: 60px;
  margin-left: auto;
}

.section-divider {
  border: none;
  border-top: 1px solid #40382E;
  width: 100%;
  margin: 0;
  opacity: 0.5;
}

.hero-name {
  font-size: 42px;
  font-weight: 700;
  color: #B9B3A6;
  margin-bottom: 6px;
  letter-spacing: -0.5px;
}

.hero-title {
  font-size: 20px;
  color: #B9B3A6;
  margin-bottom: 45px;
}

.bio-text p {
  font-size: 17px;
  line-height: 1.75;
  color: #8E877D;
  margin-bottom: 24px;
}

.stack-header {
  display: flex;
  align-items: center;
  gap: 14px;
  margin-bottom: 24px;
}

.dash-title {
  color: #5A5247;
  font-size: 20px;
}

.section-title {
  font-size: 18px;
  font-weight: 600;
  color: #B9B3A6;
  text-transform: uppercase;
  letter-spacing: 0.5px;
}

.tech-grid {
  display: flex;
  flex-wrap: wrap;
  align-items: center;
  gap: 8px 12px;
  line-height: 2;
}

.tech-item {
  font-size: 16px;
  color: #8E877D;
}

.dash {
  color: #8E877D;
  font-size: 16px;
  user-select: none;
}

.main-projects-title {
  font-size: 22px;
  color: #B9B3A6;
  margin-bottom: 45px;
  font-weight: 700;
}

.project-card {
  margin-bottom: 75px;
}

.project-name {
  font-size: 18px;
  color: #B9B3A6;
  margin-bottom: 14px;
  font-weight: 600;
}

.project-description {
  font-size: 17px;
  line-height: 1.65;
  color: #8E877D;
  margin-bottom: 22px;
}

.project-tech-list {
  display: flex;
  flex-wrap: wrap;
  gap: 6px 12px;
  font-size: 16px;
  color: #635C52;
  margin-bottom: 25px;
}

.view-link {
  display: inline-block;
  font-size: 13px;
  color: #8E877D;
  border-bottom: 1px dashed #40382E;
  padding-bottom: 3px;
  transition: color 0.3s, border-color 0.3s;
}

.view-link:hover {
  color: #B9B3A6;
  border-bottom-color: #B9B3A6;
}

.connect-section {
  margin-bottom: 60px;
}

.connect-description {
  font-size: 17px;
  line-height: 1.75;
  color: #8E877D;
  margin-bottom: 35px;
}

.connect-form {
  display: flex;
  flex-direction: column;
  gap: 20px;
  max-width: 500px;
}

.form-group {
  display: flex;
  flex-direction: column;
  gap: 8px;
}

.form-label {
  font-size: 14px;
  font-weight: 500;
  color: #B9B3A6;
}

.form-input, .form-textarea {
  background-color: #170F0F;
  border: 1px solid #40382E;
  border-radius: 4px;
  padding: 12px;
  color: #B9B3A6;
  font-size: 15px;
  transition: border-color 0.3s ease;
}

.form-input:focus, .form-textarea:focus {
  outline: none;
  border-color: #B9B3A6;
}

.form-textarea {
  resize: vertical;
}

.form-submit-btn {
  align-self: flex-start;
  background-color: transparent;
  border: 1px solid #B9B3A6;
  color: #fafafa;
  padding: 12px 24px;
  font-size: 14px;
  font-weight: 600;
  border-radius: 4px;
  cursor: pointer;
  transition: background-color 0.3s ease, color 0.3s ease;
}

.form-submit-btn:hover {
  background-color: #B9B3A6;
  color: #100808;
}

.minimal-connect-divider {
  font-size: 14px;
  color: #5A5247;
  text-transform: uppercase;
  margin: 30px 0;
  position: relative;
  text-align: center;
  max-width: 500px;
}

.minimal-connect-divider::before, .minimal-connect-divider::after {
  content: '';
  position: absolute;
  top: 50%;
  width: 44%;
  height: 1px;
  background-color: #40382E;
}

.minimal-connect-divider::before { left: 0; }
.minimal-connect-divider::after { right: 0; }

.minimal-email-box {
  display: flex;
  flex-direction: column;
  gap: 12px;
}

.minimal-email-title {
  font-size: 16px;
  color: #8E877D;
}

.minimal-email-btn {
  align-self: flex-start;
  font-size: 16px;
  color: #B9B3A6;
  text-decoration: none;
  border-bottom: 1px solid transparent;
  transition: border-color 0.3s ease;
}

.minimal-email-btn:hover {
  border-color: #B9B3A6;
}

.socials-block {
  margin-top: 55px;
  display: block;
  width: 100%;
}

.socials-block h3.findIcon {
  font-size: 18px;
  color: #B9B3A6;
  margin-bottom: 18px;
  font-weight: 600;
}

.social-icons {
  display: flex;
  gap: 22px;
}

.social-link {
  display: flex;
  align-items: center;
  justify-content: center;
}

.icon {
  width: 25px;
  height: 25px;
  fill: #8E877D;
  transition: fill 0.3s ease;
}

.social-link:hover .icon {
  fill: #B9B3A6;
}

@media (max-width: 1024px) {
  .portfolio-container {
    flex-direction: column;
    padding: 120px 40px 60px 40px;
  }

  .top-left-logo {
    top: 30px;
    left: 40px;
  }

  .sidebar {
    position: relative;
    top: 0;
    width: 100%;
    margin-bottom: 50px;
    flex-direction: row;
    flex-wrap: wrap;
    justify-content: space-between;
    gap: 30px;
  }

  .nav-links {
    flex-direction: row;
    width: 100%;
    justify-content: flex-start;
    gap: 30px;
    border-bottom: 1px solid #40382E;
    padding-bottom: 15px;
  }

  .sidebar-panel {
    border-top: none;
    padding-top: 0;
    flex: 1;
    min-width: 200px;
  }

  .main-content {
    width: 100%;
    margin-left: 0;
  }
}

@media (max-width: 768px) {
  .portfolio-container {
    padding: 100px 20px 40px 20px;
  }

  .top-left-logo {
    top: 20px;
    left: 20px;
  }

  .sidebar {
    flex-direction: column;
    gap: 25px;
  }

  .nav-links {
    gap: 15px;
    overflow-x: auto;
    white-space: nowrap;
  }

  .nav-item {
    font-size: 15px;
  }

  .nav-item::before {
    display: none;
  }

  .sidebar-panel {
    width: 100%;
    border-top: 1px solid #40382E;
    padding-top: 15px;
  }

  .hero-name {
    font-size: 32px;
  }

  .hero-title {
    font-size: 18px;
    margin-bottom: 30px;
  }

  .bio-text p {
    font-size: 15px;
    line-height: 1.6;
  }

  .tech-item, .dash, .project-tech-list {
    font-size: 14px;
  }

  .project-name {
    font-size: 17px;
  }

  .project-description {
    font-size: 15px;
  }

  .connect-form {
    max-width: 100%;
  }
}
</style>
