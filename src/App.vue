<template>
  <!-- scroll progress bar -->
  <div class="progress-bar" :style="{ transform: `scaleX(${progress})` }"></div>

  <div
    class="cursor"
    :class="{ 'cursor--big': cursorBig }"
    :style="{ left: cx + 'px', top: cy + 'px' }"
  >
    <div class="cursor__dot"></div>
    <div class="cursor__ring"></div>
  </div>

  <NavBar         @hover="cursorBig = true" @unhover="cursorBig = false" />
  <HeroSection    data-theme="dark" />
  <MarqueeBar     data-theme="dark" />
  <AboutSection   data-theme="dark" />
  <WorkSection    data-theme="light"  @hover="cursorBig = true" @unhover="cursorBig = false" />
  <StackSection   data-theme="light" />
  <ContactSection data-theme="dark"  @hover="cursorBig = true" @unhover="cursorBig = false" />
  <FooterSection  data-theme="dark"  />
</template>

<script setup>
import { ref, onMounted, onUnmounted } from 'vue'
import NavBar         from './components/NavBar.vue'
import HeroSection    from './components/HeroSection.vue'
import MarqueeBar     from './components/MarqueeBar.vue'
import AboutSection   from './components/AboutSection.vue'
import WorkSection    from './components/WorkSection.vue'
import StackSection   from './components/StackSection.vue'
import ContactSection from './components/ContactSection.vue'
import FooterSection  from './components/FooterSection.vue'

const cx        = ref(-100)
const cy        = ref(-100)
const cursorBig = ref(false)
const progress  = ref(0)

function onScroll() {
  const scrollTop    = window.scrollY
  const docHeight    = document.documentElement.scrollHeight - window.innerHeight
  progress.value     = docHeight > 0 ? scrollTop / docHeight : 0
}

onMounted(() => {
  document.addEventListener('mousemove', e => {
    cx.value = e.clientX
    cy.value = e.clientY
  })

  window.addEventListener('scroll', onScroll, { passive: true })

  // ── reveal observer ──
  const revealObserver = new IntersectionObserver(entries => {
    entries.forEach(el => {
      if (el.isIntersecting) {
        el.target.classList.add('is-visible')
        revealObserver.unobserve(el.target)
      }
    })
  }, { threshold: 0.08 })

  const scan = () => document.querySelectorAll('.reveal').forEach(el => revealObserver.observe(el))
  scan()
  setTimeout(scan, 300)

  // ── theme observer ──
  const sections = document.querySelectorAll('[data-theme]')

  const themeObserver = new IntersectionObserver(entries => {
    entries.forEach(entry => {
      if (entry.isIntersecting) {
        const theme = entry.target.getAttribute('data-theme')
        if (theme === 'dark') {
          document.body.classList.add('theme-dark')
        } else {
          document.body.classList.remove('theme-dark')
        }
      }
    })
  }, {
    threshold: 0,
    rootMargin: '-50% 0px -50% 0px',
  })

  sections.forEach(s => themeObserver.observe(s))
})

onUnmounted(() => {
  window.removeEventListener('scroll', onScroll)
})
</script>

<style>
/* ── Progress bar ── */
.progress-bar {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 2px;
  background: var(--fg);
  transform-origin: left;
  transform: scaleX(0);
  z-index: 99998;
  pointer-events: none;
  transition: background 0.7s var(--ease-out);
}

/* ── Cursor ── */
.cursor {
  position: fixed;
  pointer-events: none;
  z-index: 99999;
  transform: translate(-50%, -50%);
}

.cursor__dot {
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%) scale(1);
  width: 5px;
  height: 5px;
  background: var(--fg, #111);
  border-radius: 50%;
  transition: transform 0.35s cubic-bezier(0.16, 1, 0.3, 1);
}

.cursor__ring {
  width: 26px;
  height: 26px;
  border: 0.75px solid var(--fg, #111);
  border-radius: 50%;
  transform: scale(1);
  opacity: 1;
  transition:
    transform 0.35s cubic-bezier(0.16, 1, 0.3, 1),
    opacity   0.35s cubic-bezier(0.16, 1, 0.3, 1);
}

/* hover state — ring collapses, dot blooms */
.cursor--big .cursor__ring {
  transform: scale(0);
  opacity: 0;
}
.cursor--big .cursor__dot {
  transform: translate(-50%, -50%) scale(5);
}

/* hide native cursor everywhere */
*, *::before, *::after {
  cursor: none !important;
}
</style>