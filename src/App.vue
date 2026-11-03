<script setup>
import { computed, onMounted, onUnmounted, ref } from 'vue'

const navItems = [
  { label: 'Home', id: 'home' },
  { label: 'About', id: 'about' },
  { label: 'Portfolio', id: 'portfolio' },
  { label: 'Contact', id: 'contacts' },
]

const roles = ['Informatics Fresh Graduate', 'Tech Enthusiast']
const roleIndex = ref(0)
const menuOpen = ref(false)
const activeCategory = ref('All')
const formStatus = ref('')
const activeProjectSlides = ref({})
const lightbox = ref(null)
let roleTimer

onMounted(() => {
  roleTimer = window.setInterval(() => {
    roleIndex.value = (roleIndex.value + 1) % roles.length
  }, 2800)
  window.addEventListener('keydown', handleLightboxKeydown)
})

onUnmounted(() => {
  window.clearInterval(roleTimer)
  window.removeEventListener('keydown', handleLightboxKeydown)
})

const categories = ['All', 'Web Development', 'Data & AI', 'Python']

const projects = [
  {
    title: 'Koko Petshop',
    category: 'Web Development',
    description: 'Aplikasi pengelolaan stok, transaksi, dan laporan persediaan petshop.',
    tech: ['Laravel', 'PHP', 'MySQL'],
    link: 'https://github.com/marsaardita/koko-petshop',
    images: [
      { src: '/projects/koko-petshop/login.png', label: 'Login', alt: 'Halaman login KoKo Petshop' },
      { src: '/projects/koko-petshop/dashboard.png', label: 'Dashboard', alt: 'Dashboard KoKo Petshop' },
      { src: '/projects/koko-petshop/inventory.png', label: 'Persediaan', alt: 'Halaman persediaan barang KoKo Petshop' },
      { src: '/projects/koko-petshop/categories.png', label: 'Master Data', alt: 'Halaman master data kategori KoKo Petshop' },
      { src: '/projects/koko-petshop/reports.png', label: 'Laporan', alt: 'Halaman laporan persediaan KoKo Petshop' },
    ],
  },
  {
    title: 'Sewa Alat Camping',
    category: 'Web Development',
    description: 'Aplikasi penyewaan perlengkapan camping untuk mengelola pelanggan dan transaksi.',
    tech: ['React', 'Vite', 'Tailwind CSS', 'PHP', 'MySQL'],
    link: 'https://github.com/marsaardita/Sewa-Alat-Camping',
    images: [
      { src: '/projects/sewa-alat-camping/login.png', label: 'Login', alt: 'Halaman login Cikarang Outdoor Gear' },
      { src: '/projects/sewa-alat-camping/dashboard.png', label: 'Dashboard', alt: 'Dashboard aplikasi Cikarang Outdoor Gear' },
      { src: '/projects/sewa-alat-camping/customers.png', label: 'Data Pelanggan', alt: 'Halaman data pelanggan Cikarang Outdoor Gear' },
      { src: '/projects/sewa-alat-camping/inventory.png', label: 'Data Barang', alt: 'Halaman data barang Cikarang Outdoor Gear' },
      { src: '/projects/sewa-alat-camping/transactions.png', label: 'Transaksi', alt: 'Halaman transaksi penyewaan Cikarang Outdoor Gear' },
    ],
  },
  {
    title: 'Klasifikasi Penyakit Kulit Kucing',
    category: 'Data & AI',
    description: 'Klasifikasi penyakit kulit kucing menggunakan ResNet50 dan transfer learning.',
    tech: ['Python', 'Deep Learning', 'ResNet50'],
    link: 'https://github.com/marsaardita/Klasifikasi-Penyakit-Kulit-Kucing-ResNet50',
    images: [
      { src: '/projects/klasifikasi-kulit-kucing/upload.png', label: 'Upload Gambar', alt: 'Halaman upload gambar pada sistem klasifikasi penyakit kulit kucing' },
      { src: '/projects/klasifikasi-kulit-kucing/prediction.png', label: 'Hasil Prediksi', alt: 'Hasil prediksi penyakit kulit kucing beserta distribusi probabilitas' },
    ],
  },
  {
    title: 'PCOS Detection CNN',
    category: 'Data & AI',
    description: 'Klasifikasi citra ultrasonografi ovarium untuk mendeteksi PCOS menggunakan CNN.',
    tech: ['Python', 'CNN', 'Deep Learning'],
    link: 'https://github.com/marsaardita/PCOS-Detection-CNN',
    images: [
      { src: '/projects/pcos-detection-cnn/prediction.png', label: 'Hasil Prediksi', alt: 'Hasil klasifikasi citra ultrasonografi pada proyek PCOS Detection CNN' },
    ],
  },
  {
    title: 'BMI Calculator Python',
    category: 'Python',
    description: 'Kalkulator indeks massa tubuh berdasarkan tinggi dan berat badan.',
    tech: ['Python'],
    link: 'https://github.com/marsaardita/BMI-Calculator-Python',
    images: [
      { src: '/projects/bmi-calculator-python/calculator.png', label: 'BMI Calculator', alt: 'Tampilan aplikasi BMI Calculator Python' },
      { src: '/projects/bmi-calculator-python/result.png', label: 'Hasil Perhitungan', alt: 'Hasil perhitungan BMI 23,3 dengan kategori Normal' },
    ],
  },
]

const filteredProjects = computed(() => {
  if (activeCategory.value === 'All') return projects
  return projects.filter((project) => project.category === activeCategory.value)
})

const skills = [
  { name: 'HTML', logo: '/tool-logos/html5.svg' },
  { name: 'CSS', logo: '/tool-logos/css.svg' },
  { name: 'JavaScript', logo: '/tool-logos/javascript.svg' },
  { name: 'Vue.js', logo: '/tool-logos/vue.svg' },
  { name: 'Tailwind CSS', logo: '/tool-logos/tailwindcss.svg' },
  { name: 'PHP', logo: '/tool-logos/php.svg' },
  { name: 'Laravel', logo: '/tool-logos/laravel.svg' },
  { name: 'CodeIgniter', logo: '/tool-logos/codeigniter.svg' },
  { name: 'Python', logo: '/tool-logos/python.svg' },
  { name: 'MySQL', logo: '/tool-logos/mysql.svg' },
  { name: 'PostgreSQL', logo: '/tool-logos/postgresql.svg' },
  { name: 'SQL Server Management Studio', logo: '/tool-logos/sql-server.svg' },
  { name: 'Oracle', logo: '/tool-logos/oracle.svg' },
]

const certificates = [
  {
    title: 'Junior Web Programmer',
    issuer: 'BNSP',
    category: 'Professional Certification',
    file: '/sertifikat/BNSP Junior Web Programmer.pdf',
  },
  {
    title: 'Studi Independen ASAH',
    issuer: 'Dicoding in Association with Accenture',
    category: 'Independent Study Program',
    file: '/sertifikat/Sertifikat dan Transkrip Nilai ASAH.pdf',
  },
  {
    title: 'Asisten Praktikum DGX',
    issuer: 'Universitas Gunadarma',
    category: 'Academic Experience',
    file: '/sertifikat/Sertifikat_AsistenDGX_Marsa Ardita.pdf',
  },
  {
    title: 'Narasumber FGDDGX',
    issuer: 'Universitas Gunadarma',
    category: 'Academic Experience',
    file: '/sertifikat/Sertifikat_NarasumberFGDDGX_Marsa Ardita.pdf',
  },
  {
    title: 'SQL Server for Intermediate',
    issuer: 'Universitas Gunadarma',
    category: 'Database',
    file: '/sertifikat/SQL SERVER FOR INTERMEDIATE.pdf',
  },
  {
    title: 'Oracle for Intermediate',
    issuer: 'Universitas Gunadarma',
    category: 'Database',
    file: '/sertifikat/ORACLE FOR INTERMEDIATE.pdf',
  },
  {
    title: 'JavaScript Programming Language Fundamental',
    issuer: 'Universitas Gunadarma',
    category: 'Web Development',
    file: '/sertifikat/JavaScript Programming Language Fundamental.pdf',
  },
  {
    title: 'Basic Web Application Design',
    issuer: 'Universitas Gunadarma',
    category: 'Web Development',
    file: '/sertifikat/Basic Web Application Design.pdf',
  },
  {
    title: 'Basic Web Application Development',
    issuer: 'Universitas Gunadarma',
    category: 'Web Development',
    file: '/sertifikat/Basic Web Application Development.pdf',
  },
  {
    title: 'Sertifikat Penulisan Ilmiah',
    issuer: 'Universitas Gunadarma',
    category: 'Academic',
    file: '/sertifikat/Sertifikat Penulisan Ilmiah (PI).pdf',
  },
]

const socialLinks = [
  { label: 'GitHub', url: 'https://github.com/marsaardita', handle: 'marsaardita' },
  { label: 'LinkedIn', url: 'https://www.linkedin.com/in/marsaardita/', handle: 'Marsa Ardita' },
  { label: 'Instagram', url: 'https://www.instagram.com/marsaardita/', handle: '@marsaardita' },
  { label: 'TikTok', url: 'https://www.tiktok.com/@marsaardita', handle: '@marsaardita' },
]

const homeSocials = socialLinks.slice(0, 3)

function getProjectSlide(project) {
  return activeProjectSlides.value[project.title] ?? 0
}

function setProjectSlide(project, index) {
  activeProjectSlides.value[project.title] = index
}

function changeProjectSlide(project, direction) {
  const slideCount = project.images.length
  setProjectSlide(project, (getProjectSlide(project) + direction + slideCount) % slideCount)
}

function openProjectImage(project, index) {
  lightbox.value = { project, index }
}

function closeProjectImage() {
  lightbox.value = null
}

function changeLightboxImage(direction) {
  if (!lightbox.value) return
  const { project, index } = lightbox.value
  const nextIndex = (index + direction + project.images.length) % project.images.length
  lightbox.value = { project, index: nextIndex }
  setProjectSlide(project, nextIndex)
}

function handleLightboxKeydown(event) {
  if (!lightbox.value) return
  if (event.key === 'Escape') closeProjectImage()
  if (event.key === 'ArrowLeft') changeLightboxImage(-1)
  if (event.key === 'ArrowRight') changeLightboxImage(1)
}

function sendMessage(event) {
  const formData = new FormData(event.currentTarget)
  const name = String(formData.get('name') || '').trim()
  const email = String(formData.get('email') || '').trim()
  const subject = String(formData.get('subject') || '').trim()
  const message = String(formData.get('message') || '').trim()
  const body = `Nama: ${name}\nEmail: ${email}\n\n${message}`
  const mailto = `mailto:marsaardita.career@gmail.com?subject=${encodeURIComponent(subject)}&body=${encodeURIComponent(body)}`

  formStatus.value = 'Email app akan dibuka untuk mengirim pesan Anda.'
  window.location.href = mailto
}
</script>

<template>
  <div class="site-shell min-h-screen overflow-hidden text-slate-100">
    <header class="site-header sticky top-0 z-50 border-b border-slate-200/80 bg-white/85 shadow-sm shadow-slate-900/[0.03] backdrop-blur-xl">
      <nav class="relative mx-auto flex max-w-6xl items-center justify-between px-6 py-4">
        <a href="#home" class="brand-word text-xl font-black tracking-[0.16em]" aria-label="Marsa, Home">MARSA</a>

        <div class="hidden items-center gap-8 md:flex">
          <a
            v-for="item in navItems"
            :key="item.id"
            :href="`#${item.id}`"
            class="nav-link text-sm font-medium text-slate-300 transition"
          >
            {{ item.label }}
          </a>
        </div>

        <button
          type="button"
          class="flex h-10 w-10 items-center justify-center rounded-xl border border-slate-200 text-slate-200 transition hover:border-emerald-300 md:hidden"
          :aria-expanded="menuOpen"
          :aria-label="menuOpen ? 'Close navigation' : 'Open navigation'"
          @click="menuOpen = !menuOpen"
        >
          <svg v-if="!menuOpen" aria-hidden="true" class="h-5 w-5" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.8">
            <path stroke-linecap="round" d="M4 7h16M4 12h16M4 17h16" />
          </svg>
          <svg v-else aria-hidden="true" class="h-5 w-5" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.8">
            <path stroke-linecap="round" d="m6 6 12 12M18 6 6 18" />
          </svg>
        </button>

        <div
          v-if="menuOpen"
          class="mobile-menu absolute inset-x-4 top-[calc(100%+0.5rem)] z-50 rounded-2xl border border-slate-200 p-3 shadow-xl md:hidden"
        >
          <a
            v-for="item in navItems"
            :key="item.id"
            :href="`#${item.id}`"
            class="block rounded-xl px-4 py-3 text-sm font-medium text-slate-200 transition hover:bg-white/5"
            @click="menuOpen = false"
          >
            {{ item.label }}
          </a>
        </div>
      </nav>
    </header>

    <main>
      <section id="home" class="hero-section scroll-mt-24 relative isolate">
        <div aria-hidden="true" class="hero-orb hero-orb-one"></div>
        <div aria-hidden="true" class="hero-orb hero-orb-two"></div>
        <div class="relative mx-auto grid max-w-6xl items-center gap-12 px-6 py-20 md:min-h-[650px] md:grid-cols-[1.05fr_0.95fr] md:py-24">
          <div class="hero-copy">
            <p class="availability inline-flex items-center gap-2 rounded-full border px-4 py-2 text-sm font-medium">
              <span class="availability-dot"></span>
              Ready to Innovate
            </p>

            <h1 class="mt-7 text-5xl font-bold leading-[1.06] tracking-tight text-white md:text-7xl">
              Hi, I’m
              <span class="hero-name mt-2 block">Marsa Ardita</span>
            </h1>

            <h2 class="role-line mt-5 flex min-h-8 items-center text-lg text-slate-300 md:text-xl">
              <Transition name="role" mode="out-in">
                <span :key="roleIndex">{{ roles[roleIndex] }}</span>
              </Transition>
              <span class="role-caret ml-1" aria-hidden="true"></span>
            </h2>

            <p class="mt-5 max-w-xl text-base leading-7 text-slate-400 md:text-lg">
              Membangun website modern yang inovatif, fungsional, dan user-friendly sesuai kebutuhan pengguna.
            </p>

            <div class="mt-6 flex flex-wrap gap-2">
              <span class="hero-tag">Database &amp; Web Development</span>
              <span class="hero-tag">IT Infrastructure</span>
              <span class="hero-tag">Data &amp; ML Enthusiast</span>
            </div>

            <div class="mt-8 flex flex-wrap gap-3">
              <a href="#projects" class="button-primary inline-flex items-center gap-2 rounded-xl px-5 py-3 font-semibold transition hover:-translate-y-0.5">
                Projects
                <svg aria-hidden="true" class="h-4 w-4" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.8">
                  <path stroke-linecap="round" stroke-linejoin="round" d="M7 17 17 7M7 7h10v10"/>
                </svg>
              </a>
              <a href="#contacts" class="button-secondary inline-flex items-center gap-2 rounded-xl px-5 py-3 font-semibold transition hover:-translate-y-0.5">
                Contacts
                <svg aria-hidden="true" class="h-4 w-4" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.8">
                  <rect x="3" y="5" width="18" height="14" rx="2"/>
                  <path stroke-linecap="round" stroke-linejoin="round" d="m4 7 8 6 8-6"/>
                </svg>
              </a>
            </div>

            <div class="mt-7 flex items-center gap-3" aria-label="Social profiles">
              <a
                v-for="social in homeSocials"
                :key="social.label"
                :href="social.url"
                :aria-label="social.label"
                :title="social.label"
                target="_blank"
                rel="noopener noreferrer"
                class="social-icon"
              >
                <svg v-if="social.label === 'GitHub'" aria-hidden="true" viewBox="0 0 24 24" fill="currentColor">
                  <path d="M12 .9a11.1 11.1 0 0 0-3.51 21.63c.56.1.76-.24.76-.54v-2.1c-3.1.67-3.75-1.32-3.75-1.32-.5-1.28-1.24-1.62-1.24-1.62-1.01-.7.08-.69.08-.69 1.12.08 1.71 1.15 1.71 1.15 1 .1.75 2.17 4.01 1.53.1-.73.39-1.22.7-1.5-2.48-.28-5.09-1.24-5.09-5.52 0-1.22.44-2.21 1.15-2.99-.11-.28-.5-1.42.11-2.95 0 0 .94-.3 3.06 1.14a10.7 10.7 0 0 1 5.57 0c2.12-1.44 3.05-1.14 3.05-1.14.61 1.53.23 2.67.11 2.95.72.78 1.15 1.77 1.15 2.99 0 4.29-2.61 5.23-5.1 5.51.4.35.75 1.02.75 2.06v3.06c0 .3.2.65.77.54A11.1 11.1 0 0 0 12 .9Z"/>
                </svg>
                <svg v-else-if="social.label === 'LinkedIn'" aria-hidden="true" viewBox="0 0 24 24" fill="currentColor">
                  <path d="M5.2 3.5a2.2 2.2 0 1 1 0 4.4 2.2 2.2 0 0 1 0-4.4ZM3.3 9.5h3.8v11.2H3.3V9.5Zm6.1 0H13v1.5h.1a4 4 0 0 1 3.6-2c3.9 0 4.6 2.6 4.6 5.9v5.8h-3.8v-5.1c0-1.2 0-2.8-1.8-2.8s-2.1 1.3-2.1 2.7v5.2H9.4V9.5Z"/>
                </svg>
                <svg v-else aria-hidden="true" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.8">
                  <rect x="3" y="3" width="18" height="18" rx="5"/>
                  <circle cx="12" cy="12" r="4"/>
                  <circle cx="18" cy="6" r=".8" fill="currentColor"/>
                </svg>
              </a>
            </div>
          </div>

          <div class="hero-visual relative mx-auto flex w-full max-w-[500px] items-center justify-center">
            <div class="visual-ring visual-ring-outer" aria-hidden="true"></div>
            <div class="visual-ring visual-ring-inner" aria-hidden="true"></div>
            <div class="visual-window">
              <div class="window-top">
                <span></span><span></span><span></span>
                <i>marsa.dev</i>
              </div>
              <div class="window-code" aria-hidden="true">
                <p><b>const</b> developer = {</p>
                <p class="code-indent"><em>name</em>: <strong>"Marsa Ardita"</strong>,</p>
                <p class="code-indent"><em>focus</em>: [</p>
                <p class="code-indent code-more"><strong>"Web Development"</strong>,</p>
                <p class="code-indent code-more"><strong>"Data &amp; AI"</strong></p>
                <p class="code-indent">]</p>
                <p>}</p>
              </div>
              <div class="window-footer"><span></span> Available for opportunities</div>
            </div>
            <div class="float-chip chip-code" aria-hidden="true">&lt;/&gt;</div>
          </div>
        </div>
        <a href="#about" class="scroll-cue hidden md:flex" aria-label="Scroll to About">
          <span></span>
        </a>
      </section>

      <section id="about" class="section-block scroll-mt-24 py-20 md:py-24">
        <div class="mx-auto max-w-6xl px-6">
          <div class="section-heading">
            <span class="section-kicker">01 / ABOUT</span>
            <h2>About <span>Me</span></h2>
          </div>

          <div class="grid items-center gap-10 md:grid-cols-[1.15fr_0.85fr] md:gap-16">
            <div>
              <p class="text-base leading-8 text-slate-300 md:text-lg">
                Halo! Saya Marsa Ardita, fresh graduate S1 Informatika Universitas Gunadarma dengan IPK 3,81. Saya memiliki pengalaman akademik dalam pengembangan aplikasi web dan IT infrastructure, termasuk merancang serta membangun sistem, mengelola basis data, melakukan troubleshooting perangkat lunak maupun perangkat keras, dan memberikan dukungan teknis kepada pengguna.
              </p>

              <blockquote class="about-quote mt-7 rounded-r-xl px-5 py-4 text-base font-medium italic leading-7 text-emerald-100 md:text-lg">
                “Driving efficiency with AI, keeping creativity uniquely human.”
              </blockquote>

              <div class="mt-7 flex flex-wrap gap-3">
                <a href="/CV_Marsa_Ardita.pdf" download="CV_Marsa_Ardita.pdf" class="button-primary inline-flex items-center gap-2 rounded-xl px-5 py-3 font-semibold transition hover:-translate-y-0.5">
                  Download CV
                  <svg aria-hidden="true" class="h-4 w-4" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.8">
                    <path stroke-linecap="round" stroke-linejoin="round" d="M12 3v12m0 0 4-4m-4 4-4-4M5 17v3h14v-3"/>
                  </svg>
                </a>
                <a href="#projects" class="button-secondary inline-flex items-center gap-2 rounded-xl px-5 py-3 font-semibold transition hover:-translate-y-0.5">
                  Projects
                  <svg aria-hidden="true" class="h-4 w-4" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.8">
                    <path stroke-linecap="round" stroke-linejoin="round" d="M7 17 17 7M7 7h10v10"/>
                  </svg>
                </a>
              </div>
            </div>

            <div class="profile-placeholder-wrap">
              <div class="profile-placeholder">
                <img
                  src="/marsa-profile.png"
                  alt="Marsa Ardita di Jakarta"
                  class="profile-photo"
                  fetchpriority="high"
                />
                <div class="profile-placeholder-frame" aria-hidden="true"></div>
              </div>
            </div>
          </div>

          <div class="mt-16">
            <div class="mb-6 flex items-end justify-between gap-4">
              <div>
                <span class="section-kicker">TOOLS &amp; TECHNOLOGIES</span>
                <h3 class="mt-2 text-xl font-semibold text-white">Tools I Work With</h3>
              </div>
            </div>
            <div class="grid grid-cols-2 gap-3 sm:grid-cols-3 md:grid-cols-4 lg:grid-cols-7">
              <div v-for="skill in skills" :key="skill.name" class="skill-logo-card">
                <img :src="skill.logo" alt="" class="skill-logo-img" loading="lazy" />
                <span>{{ skill.name }}</span>
              </div>
            </div>
          </div>
        </div>
      </section>

      <section id="portfolio" class="section-block scroll-mt-24 py-20 md:py-24">
        <div class="mx-auto max-w-6xl px-6">
          <div class="section-heading">
            <span class="section-kicker">02 / PORTFOLIO</span>
            <h2>Selected <span>Work</span></h2>
          </div>

          <section id="projects" class="scroll-mt-28">
            <div class="subsection-title">
              <span class="section-icon" aria-hidden="true">
                <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.7">
                  <path stroke-linecap="round" stroke-linejoin="round" d="M3 7.5A1.5 1.5 0 0 1 4.5 6H10l2 2h7.5A1.5 1.5 0 0 1 21 9.5v8a1.5 1.5 0 0 1-1.5 1.5h-15A1.5 1.5 0 0 1 3 17.5v-10Z"/>
                </svg>
              </span>
              <div>
                <h3>Projects</h3>
                <p>Selected academic and personal projects</p>
              </div>
            </div>

            <div class="mt-7 flex flex-wrap gap-2">
              <button
                v-for="category in categories"
                :key="category"
                type="button"
                class="filter-chip"
                :class="{ 'filter-chip-active': activeCategory === category }"
                :aria-pressed="activeCategory === category"
                @click="activeCategory = category"
              >
                {{ category }}
              </button>
            </div>

            <div class="mt-7 grid gap-5 md:grid-cols-2 lg:grid-cols-3">
              <article v-for="project in filteredProjects" :key="project.title" class="project-card group">
                <div v-if="project.images" class="project-gallery">
                  <div class="project-gallery-stage" :aria-label="`Screenshot ${project.title}`">
                    <Transition name="gallery-slide" mode="out-in">
                      <button
                        :key="project.images[getProjectSlide(project)].src"
                        type="button"
                        class="project-gallery-image-button"
                        :aria-label="`Perbesar gambar ${project.images[getProjectSlide(project)].label}`"
                        @click="openProjectImage(project, getProjectSlide(project))"
                      >
                        <img
                          :src="project.images[getProjectSlide(project)].src"
                          :alt="project.images[getProjectSlide(project)].alt"
                          loading="lazy"
                        />
                        <span class="project-gallery-zoom" aria-hidden="true">
                          <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.8">
                            <circle cx="10.8" cy="10.8" r="6.8" />
                            <path stroke-linecap="round" d="m16 16 4.5 4.5M10.8 7.8v6m-3-3h6" />
                          </svg>
                        </span>
                      </button>
                    </Transition>
                    <button
                      type="button"
                      class="project-gallery-arrow project-gallery-prev"
                      :aria-label="`Gambar sebelumnya: ${project.title}`"
                      @click="changeProjectSlide(project, -1)"
                    >
                      <svg aria-hidden="true" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
                        <path stroke-linecap="round" stroke-linejoin="round" d="m15 18-6-6 6-6" />
                      </svg>
                    </button>
                    <button
                      type="button"
                      class="project-gallery-arrow project-gallery-next"
                      :aria-label="`Gambar berikutnya: ${project.title}`"
                      @click="changeProjectSlide(project, 1)"
                    >
                      <svg aria-hidden="true" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
                        <path stroke-linecap="round" stroke-linejoin="round" d="m9 18 6-6-6-6" />
                      </svg>
                    </button>
                    <div class="project-gallery-caption" aria-live="polite">
                      <span>{{ project.images[getProjectSlide(project)].label }}</span>
                      <span>{{ getProjectSlide(project) + 1 }} / {{ project.images.length }}</span>
                    </div>
                  </div>
                  <div class="project-gallery-pagination" role="group" :aria-label="`Pilih screenshot ${project.title}`">
                    <button
                      v-for="(image, index) in project.images"
                      :key="image.src"
                      type="button"
                      :class="{ 'project-gallery-dot-active': getProjectSlide(project) === index }"
                      :aria-label="`Tampilkan screenshot ${image.label}`"
                      :aria-pressed="getProjectSlide(project) === index"
                      @click="setProjectSlide(project, index)"
                    ></button>
                  </div>
                </div>
                <div v-else class="project-cover">
                  <span>{{ project.category }}</span>
                  <strong aria-hidden="true">{{ project.title.slice(0, 2).toUpperCase() }}</strong>
                </div>
                <div class="flex flex-1 flex-col p-5">
                  <h4>{{ project.title }}</h4>
                  <p class="mt-2 flex-1 text-sm leading-6 text-slate-400">{{ project.description }}</p>
                  <div class="mt-4 flex flex-wrap gap-2">
                    <span v-for="tech in project.tech" :key="tech" class="tech-pill">{{ tech }}</span>
                  </div>
                  <a :href="project.link" target="_blank" rel="noopener noreferrer" class="project-link mt-5 inline-flex items-center gap-2 text-sm font-semibold">
                    GitHub
                    <svg aria-hidden="true" class="h-4 w-4" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.8">
                      <path stroke-linecap="round" stroke-linejoin="round" d="M7 17 17 7M7 7h10v10"/>
                    </svg>
                  </a>
                </div>
              </article>
            </div>
          </section>

          <section id="certificates" class="mt-20 scroll-mt-28">
            <div class="subsection-title">
              <span class="section-icon" aria-hidden="true">
                <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.7">
                  <path stroke-linecap="round" stroke-linejoin="round" d="M7 3.5h7l4 4V14M7 3.5A1.5 1.5 0 0 0 5.5 5v14A1.5 1.5 0 0 0 7 20.5h6M14 3.5V8h4M9 12h6m-6 4h3m4.5-.5 2 1.5 2-1.5v6l-2-1.3-2 1.3v-6Z"/>
                </svg>
              </span>
              <div>
                <h3>Certificates</h3>
                <p>Professional training and academic achievements</p>
              </div>
            </div>

            <div class="mt-7 grid gap-4 md:grid-cols-2 lg:grid-cols-3">
              <article v-for="certificate in certificates" :key="certificate.title" class="certificate-card">
                <span class="certificate-category">{{ certificate.category }}</span>
                <h4>{{ certificate.title }}</h4>
                <p>{{ certificate.issuer }}</p>
                <a :href="certificate.file" target="_blank" rel="noopener noreferrer" class="certificate-link">
                  Certificate <span aria-hidden="true">↗</span>
                </a>
              </article>
            </div>
          </section>
        </div>
      </section>

      <section id="contacts" class="section-block contact-section scroll-mt-24 py-20 md:py-24">
        <div class="mx-auto max-w-6xl px-6">
          <div class="section-heading">
            <span class="section-kicker">03 / CONTACT</span>
            <h2>Let’s Work <span>Together</span></h2>
          </div>

          <div class="grid gap-12 lg:grid-cols-[0.85fr_1.15fr] lg:gap-16">
            <div>
              <p class="max-w-md text-base leading-7 text-slate-300">
                Saya terbuka untuk peluang kerja entry-level, program pengembangan, dan kolaborasi di bidang web development, data, maupun IT support. Mari bangun solusi digital yang bermanfaat bersama.
              </p>

              <div class="mt-8 space-y-3">
                <a
                  v-for="social in socialLinks"
                  :key="social.label"
                  :href="social.url"
                  target="_blank"
                  rel="noopener noreferrer"
                  class="contact-social"
                >
                  <span class="contact-social-icon" aria-hidden="true">
                    <svg v-if="social.label === 'GitHub'" viewBox="0 0 24 24" fill="currentColor">
                      <path d="M12 .9a11.1 11.1 0 0 0-3.51 21.63c.56.1.76-.24.76-.54v-2.1c-3.1.67-3.75-1.32-3.75-1.32-.5-1.28-1.24-1.62-1.24-1.62-1.01-.7.08-.69.08-.69 1.12.08 1.71 1.15 1.71 1.15 1 .1.75 2.17 4.01 1.53.1-.73.39-1.22.7-1.5-2.48-.28-5.09-1.24-5.09-5.52 0-1.22.44-2.21 1.15-2.99-.11-.28-.5-1.42.11-2.95 0 0 .94-.3 3.06 1.14a10.7 10.7 0 0 1 5.57 0c2.12-1.44 3.05-1.14 3.05-1.14.61 1.53.23 2.67.11 2.95.72.78 1.15 1.77 1.15 2.99 0 4.29-2.61 5.23-5.1 5.51.4.35.75 1.02.75 2.06v3.06c0 .3.2.65.77.54A11.1 11.1 0 0 0 12 .9Z"/>
                    </svg>
                    <svg v-else-if="social.label === 'LinkedIn'" viewBox="0 0 24 24" fill="currentColor">
                      <path d="M5.2 3.5a2.2 2.2 0 1 1 0 4.4 2.2 2.2 0 0 1 0-4.4ZM3.3 9.5h3.8v11.2H3.3V9.5Zm6.1 0H13v1.5h.1a4 4 0 0 1 3.6-2c3.9 0 4.6 2.6 4.6 5.9v5.8h-3.8v-5.1c0-1.2 0-2.8-1.8-2.8s-2.1 1.3-2.1 2.7v5.2H9.4V9.5Z"/>
                    </svg>
                    <svg v-else-if="social.label === 'Instagram'" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.8">
                      <rect x="3" y="3" width="18" height="18" rx="5"/>
                      <circle cx="12" cy="12" r="4"/>
                      <circle cx="18" cy="6" r=".8" fill="currentColor"/>
                    </svg>
                    <svg v-else viewBox="0 0 24 24" fill="currentColor">
                      <path d="M19.6 7.1a5.3 5.3 0 0 1-3.2-1.1 5.3 5.3 0 0 1-1.9-3.2h-3.7v12.8a2.9 2.9 0 1 1-2-2.8V9a6.8 6.8 0 1 0 5.7 6.7V9.2a9 9 0 0 0 5.1 1.6V7.1Z"/>
                    </svg>
                  </span>
                  <span class="flex-1">
                    <strong>{{ social.label }}</strong>
                    <small>{{ social.handle }}</small>
                  </span>
                  <span aria-hidden="true" class="text-slate-500">↗</span>
                </a>
              </div>
            </div>

            <form class="contact-form" @submit.prevent="sendMessage">
              <div class="grid gap-5 sm:grid-cols-2">
                <label class="form-field">
                  <span>Your Name</span>
                  <input name="name" type="text" autocomplete="name" placeholder="Enter your name" required />
                </label>
                <label class="form-field">
                  <span>Email Address</span>
                  <input name="email" type="email" autocomplete="email" placeholder="name@example.com" required />
                </label>
              </div>

              <label class="form-field mt-5">
                <span>Subject</span>
                <input name="subject" type="text" placeholder="What is this about?" required />
              </label>

              <label class="form-field mt-5">
                <span>Message</span>
                <textarea name="message" rows="5" placeholder="Write your message..." required></textarea>
              </label>

              <div class="mt-5 flex flex-wrap items-center gap-4">
                <button type="submit" class="button-primary inline-flex items-center gap-2 rounded-xl px-5 py-3 font-semibold transition hover:-translate-y-0.5">
                  Send Message
                  <svg aria-hidden="true" class="h-4 w-4" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.8">
                    <path stroke-linecap="round" stroke-linejoin="round" d="m22 2-7 20-4-9-9-4 20-7ZM22 2 11 13"/>
                  </svg>
                </button>
                <p v-if="formStatus" role="status" class="text-sm text-emerald-200">{{ formStatus }}</p>
              </div>
              <p class="mt-3 text-xs text-slate-500">Form ini membuka aplikasi email Anda untuk mengirim pesan ke marsaardita.career@gmail.com.</p>
            </form>
          </div>
        </div>
      </section>
    </main>

    <Teleport to="body">
      <div
        v-if="lightbox"
        class="project-lightbox"
        role="dialog"
        aria-modal="true"
        :aria-label="`${lightbox.project.title}: ${lightbox.project.images[lightbox.index].label}`"
        @click.self="closeProjectImage"
      >
        <button type="button" class="lightbox-close" aria-label="Tutup gambar" @click="closeProjectImage">
          <svg aria-hidden="true" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
            <path stroke-linecap="round" d="m6 6 12 12M18 6 6 18" />
          </svg>
        </button>
        <button type="button" class="lightbox-arrow lightbox-prev" aria-label="Gambar sebelumnya" @click="changeLightboxImage(-1)">
          <svg aria-hidden="true" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
            <path stroke-linecap="round" stroke-linejoin="round" d="m15 18-6-6 6-6" />
          </svg>
        </button>
        <figure class="lightbox-figure" :class="{ 'lightbox-figure-pcos': lightbox.project.title === 'PCOS Detection CNN' }">
          <img
            :src="lightbox.project.images[lightbox.index].src"
            :alt="lightbox.project.images[lightbox.index].alt"
          />
          <figcaption>
            <span>{{ lightbox.project.title }} · {{ lightbox.project.images[lightbox.index].label }}</span>
            <span>{{ lightbox.index + 1 }} / {{ lightbox.project.images.length }}</span>
          </figcaption>
        </figure>
        <button type="button" class="lightbox-arrow lightbox-next" aria-label="Gambar berikutnya" @click="changeLightboxImage(1)">
          <svg aria-hidden="true" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
            <path stroke-linecap="round" stroke-linejoin="round" d="m9 18 6-6-6-6" />
          </svg>
        </button>
      </div>
    </Teleport>

    <footer class="border-t border-white/5 px-6 py-6 text-center text-sm text-slate-500">
      © 2026 Marsa Ardita
    </footer>

  </div>
</template>
