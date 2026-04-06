<template>
  <section id="work" class="work">

    <div class="work__header reveal">
      <h2 class="work__heading u-serif">
        My<br><em>Projects</em>
      </h2>
      <div class="work__header-right">
        <span class="u-label">{{ projects.length }} projects</span>
        <div class="work__scroll-hint u-label">
          <span>scroll</span>
          <span class="work__scroll-arrow">→</span>
        </div>
      </div>
    </div>

    <div class="work__track-wrap" ref="trackWrap">
      <ul class="work__track">
        <li
          v-for="(p, i) in projects"
          :key="p.name"
          class="work__card reveal"
          :style="{ transitionDelay: (i * 0.06) + 's' }"
          @mouseenter="$emit('hover')"
          @mouseleave="$emit('unhover')"
          @click="openModal(p)"
        >
          <!-- bg media: show first item only on card -->
          <div class="work__card-bg">
            <template v-if="p.media && p.media.length">
              <video
                v-if="p.media[0].type === 'video'"
                :src="p.media[0].src"
                autoplay loop muted playsinline
                class="work__card-media"
              />
              <img
                v-else
                :src="p.media[0].src"
                :alt="p.name"
                class="work__card-media"
              />
            </template>
          </div>
          <div class="work__card-overlay"></div>

          <div class="work__card-inner">
            <span class="work__num u-label">{{ String(i + 1).padStart(2, '0') }}</span>
            <div class="work__card-body">
              <h3 class="work__name" v-html="p.name"></h3>
              <p class="work__desc">{{ p.desc }}</p>
            </div>
            <div class="work__card-footer">
              <span class="work__tag u-label">{{ p.type }}</span>
              <span class="work__lang u-label">{{ p.lang }}</span>
              <span class="work__arrow">↗</span>
            </div>
          </div>
        </li>
      </ul>
    </div>

    <!-- ── Modal ── -->
    <Teleport to="body">
      <Transition name="modal">
        <div v-if="active" class="modal" @click.self="closeModal">

          <!-- slideshow bg -->
          <div class="modal__bg">
            <template v-if="active.media && active.media.length">
              <TransitionGroup name="slide">
                <template
                  v-for="(m, mi) in active.media"
                  :key="mi"
                >
                  <video
                    v-if="m.type === 'video' && mi === slideIndex"
                    :src="m.src"
                    autoplay loop muted playsinline
                    class="modal__bg-media"
                  />
                  <img
                    v-else-if="m.type !== 'video' && mi === slideIndex"
                    :src="m.src"
                    :alt="active.name"
                    class="modal__bg-media"
                  />
                </template>
              </TransitionGroup>
            </template>
            <!-- no media fallback -->
            <div v-else class="modal__bg-empty"></div>
          </div>
          <div class="modal__bg-overlay"></div>

          <!-- slideshow controls — only if more than 1 media -->
          <div v-if="active.media && active.media.length > 1" class="modal__slides">
            <button
              class="modal__slide-btn u-label"
              @click="prevSlide"
              @mouseenter="$emit('hover')"
              @mouseleave="$emit('unhover')"
            >←</button>
            <div class="modal__slide-dots">
              <span
                v-for="(_, mi) in active.media"
                :key="mi"
                class="modal__dot"
                :class="{ 'modal__dot--active': mi === slideIndex }"
                @click="slideIndex = mi"
              ></span>
            </div>
            <button
              class="modal__slide-btn u-label"
              @click="nextSlide"
              @mouseenter="$emit('hover')"
              @mouseleave="$emit('unhover')"
            >→</button>
          </div>

          <!-- close -->
          <button
            class="modal__close u-label"
            @click="closeModal"
            @mouseenter="$emit('hover')"
            @mouseleave="$emit('unhover')"
          >✕ close</button>

          <!-- project number -->
          <span class="modal__num u-label">
            {{ String(projects.indexOf(active) + 1).padStart(2, '0') }} / {{ String(projects.length).padStart(2, '0') }}
          </span>

          <!-- centered content -->
          <div class="modal__content">

            <div class="modal__tags">
              <span
                v-for="t in active.type.split('·').map(s => s.trim())"
                :key="t"
                class="modal__tag u-label"
              >{{ t }}</span>
            </div>

            <h2 class="modal__title u-serif" v-html="active.name"></h2>
            <p class="modal__desc">{{ active.desc }}</p>

            <div class="modal__divider"></div>

            <div class="modal__bottom">
              <div class="modal__icons">
                <span
                  v-for="icon in active.icons"
                  :key="icon.class"
                  class="modal__icon-wrap"
                  :title="icon.label"
                >
                  <i :class="['devicon-' + icon.class, 'modal__icon']"></i>
                  <span class="modal__icon-label u-label">{{ icon.label }}</span>
                </span>
              </div>

              <div class="modal__links">
                <a
                  :href="active.live || '#'"
                  class="modal__btn modal__btn--primary u-label"
                  target="_blank"
                  @mouseenter="$emit('hover')"
                  @mouseleave="$emit('unhover')"
                >View Live ↗</a>
                <a
                  :href="active.github || '#'"
                  class="modal__btn modal__btn--ghost u-label"
                  target="_blank"
                  @mouseenter="$emit('hover')"
                  @mouseleave="$emit('unhover')"
                >GitHub ↗</a>
              </div>
            </div>

          </div>

        </div>
      </Transition>
    </Teleport>

  </section>
</template>

<script setup>
import { ref, onMounted, onUnmounted, watch } from 'vue'

defineEmits(['hover', 'unhover'])

// ── Modal & slideshow ──
const active = ref(null)
const slideIndex = ref(0)

function openModal(p) {
  active.value = p
  slideIndex.value = 0
  document.body.style.overflow = 'hidden'
}

function closeModal() {
  active.value = null
  slideIndex.value = 0
  document.body.style.overflow = ''
}

function nextSlide() {
  if (!active.value) return
  slideIndex.value = (slideIndex.value + 1) % active.value.media.length
}

function prevSlide() {
  if (!active.value) return
  slideIndex.value = (slideIndex.value - 1 + active.value.media.length) % active.value.media.length
}

// auto-advance slideshow every 4s
let slideTimer = null
watch(active, (val) => {
  clearInterval(slideTimer)
  if (val && val.media && val.media.length > 1) {
    slideTimer = setInterval(nextSlide, 4000)
  }
})

function onKeydown(e) {
  if (e.key === 'Escape') closeModal()
  if (e.key === 'ArrowRight') nextSlide()
  if (e.key === 'ArrowLeft') prevSlide()
}

// ── Horizontal scroll lock ──
const trackWrap = ref(null)
let isLocked = false
let ticking = false

function lockScroll() {
  if (isLocked) return
  isLocked = true
  document.body.style.overflow = 'hidden'
  const section = document.getElementById('work')
  if (section) section.scrollIntoView({ behavior: 'smooth', block: 'start' })
}

function unlockScroll() {
  if (!isLocked) return
  isLocked = false
  if (!active.value) document.body.style.overflow = ''
}

function onWheel(e) {
  if (active.value) return
  const el = trackWrap.value
  if (!el) return

  const atEnd   = el.scrollLeft + el.clientWidth >= el.scrollWidth - 2
  const atStart = el.scrollLeft <= 1

  if (isLocked) {
    e.preventDefault()
    if (atEnd && e.deltaY > 0) { unlockScroll(); return }
    if (atStart && e.deltaY < 0) { unlockScroll(); return }
    el.scrollLeft += e.deltaY * 1.2
    return
  }

  const section = document.getElementById('work')
  if (!section) return
  const rect = section.getBoundingClientRect()
  const nearTop = rect.top >= -10 && rect.top <= 10

  if (nearTop && e.deltaY > 0 && !atEnd) {
    e.preventDefault()
    lockScroll()
    el.scrollLeft += e.deltaY * 1.2
  }
}

function onScroll() {
  if (isLocked || ticking || active.value) return
  ticking = true
  requestAnimationFrame(() => {
    const section = document.getElementById('work')
    if (!section) { ticking = false; return }
    const rect = section.getBoundingClientRect()
    if (rect.top <= 1 && rect.top >= -10) {
      const el = trackWrap.value
      if (el && el.scrollLeft < el.scrollWidth - el.clientWidth - 2) lockScroll()
    }
    ticking = false
  })
}

onMounted(() => {
  const el = trackWrap.value
  if (el) el.addEventListener('wheel', onWheel, { passive: false })
  window.addEventListener('scroll', onScroll, { passive: true })
  window.addEventListener('keydown', onKeydown)
})

onUnmounted(() => {
  clearInterval(slideTimer)
  const el = trackWrap.value
  if (el) el.removeEventListener('wheel', onWheel)
  window.removeEventListener('scroll', onScroll)
  window.removeEventListener('keydown', onKeydown)
  unlockScroll()
})

// ── Projects ──
// media: array of { type: 'image' | 'video', src: '/path/to/file' }
// if only 1 item → single bg; if 2+ → auto slideshow with controls
const projects = [
  {
    name: '<em>Task</em> List',
    desc: 'Task management web app with user authentication and can check off completed tasks.',
    type: 'Web App',
    lang: 'Vue.js / Supabase / PostgreSQL',
    media: [
      { type: 'video', src: '/projects/tasklist.mp4' },
    ],
    live: 'https://tasklistdffy.vercel.app',
    github: '#',
    icons: [
      { class: 'vuejs-plain', label: 'Vue.js' },
      { class: 'supabase-plain', label: 'Supabase' },
      { class: 'postgresql-plain', label: 'PostgreSQL' },
    ],
  },
  {
    name: 'Store <em>Attendance</em> using HF4000',
    desc: 'Fingerprint-based employee attendance system with admin management for employee enrollment with integration of HF4000 fingerprint scanner.',
    type: 'Web App · IoT · Internship',
    lang: 'Vue.js / Laravel',
    media: [
      { type: 'video', src: '/projects/storetime.mp4' },
    ],
    live: '#',
    github: '#',
    icons: [
      { class: 'vuejs-plain', label: 'Vue.js' },
      { class: 'laravel-plain', label: 'Laravel' },
    ],
  },
  {
    name: '<em>Fingerprint</em> Scanner Test and Template using HF4000',
    desc: 'Browser dev & testing template for HF4000 Fingerprint Scanner. Enrollment and fingerprint comparison using WebSocket. Built with HF4000 manufacturerSDK.',
    type: 'Dev Tool · Internship',
    lang: 'JavaScript',
    media: [
      { type: 'image', src: 'https://okmbstzhmjqscmgoimxm.supabase.co/storage/v1/object/public/videos/fingerprint.png' },
      { type: 'image', src: 'https://okmbstzhmjqscmgoimxm.supabase.co/storage/v1/object/public/videos/fingerprint2.png' },
    ],
    live: '#',
    github: '#',
    icons: [
      { class: 'javascript-plain', label: 'JavaScript' },
    ],
  },
  {
    name: 'Employee <em>Leave</em> Request',
    desc: 'Modernized leave request system with improved UI/UX and category-based monitoring. Built during internship.',
    type: 'Web App · Internship',
    lang: 'Blade / PHP',
    media: [
      { type: 'video', src: '/projects/leaverequest.mp4' },
    ],
    live: '#',
    github: '#',
    icons: [
      { class: 'laravel-plain', label: 'Laravel' },
      { class: 'php-plain', label: 'PHP' },
    ],
  },
  {
    name: 'RENT-<em>CONNECT</em>',
    desc: 'Digitized rental platform between landlord and tenant with real-time chat, Leaflet geolocation, property listings, and application management. Capstone project.',
    type: 'Web App',
    lang: 'PHP / JavaScript',
    media: [
      { type: 'video', src: '/projects/rentconnect.mp4' },
    ],
    live: '#',
    github: '#',
    icons: [
      { class: 'php-plain', label: 'PHP' },
      { class: 'javascript-plain', label: 'JavaScript' },
    ],
  },
  {
    name: 'BIG4K-<em>MERCH</em>',
    desc: 'K-pop merchandise e-commerce with admin dashboard and full user storefront. First web app project.',
    type: 'Web App · E-Commerce',
    lang: 'PHP',
    media: [
      { type: 'video', src: '/projects/big4k.mp4' },
    ],
    live: '#',
    github: '#',
    icons: [
      { class: 'php-plain', label: 'PHP' },
    ],
  },
  {
    name: 'LOCALS',
    desc: 'E-commerce mobile app for local PH clothing brands. Built with Flutter + PHP as backend. First Flutter project.',
    type: 'Mobile App · E-Commerce',
    lang: 'Dart / Flutter',
    media: [
      { type: 'video', src: '/projects/locals.mp4' },
    ],
    live: '#',
    github: '#',
    icons: [
      { class: 'dart-plain', label: 'Dart' },
      { class: 'flutter-plain', label: 'Flutter' },
      { class: 'php-plain', label: 'PHP' },
    ],
  },
  {
    name: '<em>Weather</em> Today',
    desc: 'Minimal weather app with clean aesthetics powered by Weather API, covering Luzon. First API Project',
    type: 'Web App',
    lang: 'JavaScript',
    media: [
      { type: 'video', src: '/projects/weathertoday.mp4' },
    ],
    live: 'https://weathertoday-by-lancetinoco.netlify.app',
    github: '#',
    icons: [
      { class: 'javascript-plain', label: 'JavaScript' },
    ],
  },
]
</script>

<style scoped>
.work {
  background: var(--bg);
  width: 100%;
  padding: 2rem 0;
  transition: background 0.7s var(--ease-out);
  overflow: hidden;
}

/* ── Header ── */
.work__header {
  display: flex;
  justify-content: space-between;
  align-items: flex-end;
  padding: 5rem 3rem 3rem;
  border-bottom: 1px solid var(--lightgray);
  margin: 0 3rem;
}
.work__heading {
  font-size: clamp(2.8rem, 6vw, 7rem);
  font-weight: 300;
  line-height: 1;
  letter-spacing: -0.03em;
  color: var(--fg);
}
.work__heading em { font-style: italic; }

.work__header-right {
  display: flex;
  flex-direction: column;
  align-items: flex-end;
  gap: 0.8rem;
}
.work__scroll-hint {
  display: flex;
  align-items: center;
  gap: 0.4rem;
  opacity: 0.5;
}
.work__scroll-arrow {
  display: inline-block;
  animation: nudge 1.6s var(--ease-out) infinite;
}
@keyframes nudge {
  0%, 100% { transform: translateX(0); }
  50%       { transform: translateX(5px); }
}

/* ── Track ── */
.work__track-wrap {
  overflow-x: auto;
  overflow-y: hidden;
  -webkit-overflow-scrolling: touch;
  scrollbar-width: none;
  padding: 3rem 3rem 4rem;
  cursor: none;
}
.work__track-wrap::-webkit-scrollbar { display: none; }

.work__track {
  display: flex;
  gap: 1.5rem;
  list-style: none;
  width: max-content;
  align-items: stretch;
}

/* ── Card ── */
.work__card {
  position: relative;
  width: 340px;
  flex-shrink: 0;
  border: 1px solid var(--lightgray);
  background: var(--bg);
  cursor: none;
  display: flex;
  flex-direction: column;
  overflow: hidden;
  transition:
    border-color 0.5s var(--ease-out),
    background 0.4s var(--ease-out),
    transform 0.4s var(--ease-out);
}
.work__card:hover {
  border-color: transparent;
  transform: translateY(-6px);
  background: var(--black);
}

.work__card-bg {
  position: absolute;
  inset: 0;
  z-index: 0;
  opacity: 0;
  transition: opacity 0.6s var(--ease-out);
}
.work__card:hover .work__card-bg { opacity: 1; }

.work__card-media {
  width: 100%;
  height: 100%;
  object-fit: cover;
  object-position: top;
}

.work__card-overlay {
  position: absolute;
  inset: 0;
  z-index: 1;
  background: linear-gradient(160deg, rgba(10,10,10,0.85) 0%, rgba(10,10,10,0.65) 100%);
  opacity: 0;
  transition: opacity 0.6s var(--ease-out);
}
.work__card:hover .work__card-overlay { opacity: 1; }

.work__card-inner {
  position: relative;
  z-index: 2;
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  flex: 1;
  padding: 1.8rem;
  gap: 1rem;
  overflow: hidden;
}

.work__card:hover .work__num,
.work__card:hover .work__name,
.work__card:hover .work__desc,
.work__card:hover .work__tag,
.work__card:hover .work__lang,
.work__card:hover .work__arrow { color: var(--white); }
.work__card:hover .work__tag { border-color: rgba(255,255,255,0.25); }
.work__card:hover .work__card-footer { border-color: rgba(255,255,255,0.15); }

.work__num { display: block; transition: color 0.3s; }

.work__card-body {
  flex: 1;
  display: flex;
  flex-direction: column;
  justify-content: flex-end;
  gap: 0.6rem;
}
.work__name {
  font-family: var(--font-display);
  font-size: clamp(1.3rem, 2vw, 1.8rem);
  font-weight: 300;
  letter-spacing: -0.02em;
  color: var(--fg);
  transition: color 0.3s;
  line-height: 1.2;
}
.work__name em { font-style: italic; }

.work__desc {
  font-size: 0.6rem;
  line-height: 1.9;
  color: var(--gray);
  transition: color 0.3s;
  display: -webkit-box;
  -webkit-line-clamp: 4;
  line-clamp: 4;
  -webkit-box-orient: vertical;
  overflow: hidden;
}

.work__card-footer {
  display: flex;
  align-items: center;
  gap: 0.5rem;
  flex-wrap: wrap;
  border-top: 1px solid var(--lightgray);
  padding-top: 1rem;
  transition: border-color 0.3s;
}
.work__tag {
  border: 1px solid var(--lightgray);
  padding: 0.2rem 0.6rem;
  white-space: nowrap;
  transition: color 0.3s, border-color 0.3s;
}
.work__lang { transition: color 0.3s; }

.work__arrow {
  margin-left: auto;
  font-size: 1rem;
  transition: color 0.3s, transform 0.3s;
}
.work__card:hover .work__arrow { transform: translate(2px, -2px); }

/* ── Modal ── */
.modal {
  position: fixed;
  inset: 0;
  z-index: 9000;
  display: flex;
  align-items: center;
  justify-content: center;
  padding: 3rem;
}

/* slideshow bg */
.modal__bg {
  position: absolute;
  inset: 0;
  z-index: 0;
  background: #0c0c0c;
  overflow: hidden;
}
.modal__bg-media {
  position: absolute;
  inset: 0;
  width: 100%;
  height: 100%;
  object-fit: cover;
  object-position: top;
  display: block;
  filter: blur(0px) brightness(0.35);
  transform: scale(1.04);
}
.modal__bg-empty {
  position: absolute;
  inset: 0;
  background: #0c0c0c;
}

/* slide transition */
.slide-enter-active,
.slide-leave-active {
  transition: opacity 0.8s var(--ease-out);
}
.slide-enter-from,
.slide-leave-to { opacity: 0; }

.modal__bg-overlay {
  position: absolute;
  inset: 0;
  background: rgba(8, 8, 8, 0.55);
  z-index: 1;
}

/* slideshow controls — bottom center */
.modal__slides {
  position: absolute;
  bottom: 2rem;
  left: 50%;
  transform: translateX(-50%);
  z-index: 10;
  display: flex;
  align-items: center;
  gap: 1rem;
}
.modal__slide-btn {
  background: none;
  border: none;
  color: rgba(255,255,255,0.5);
  cursor: none;
  transition: color 0.2s;
  padding: 0;
  font-size: 0.9rem;
}
.modal__slide-btn:hover { color: #fff; }

.modal__slide-dots {
  display: flex;
  gap: 0.4rem;
  align-items: center;
}
.modal__dot {
  width: 5px;
  height: 5px;
  border-radius: 50%;
  background: rgba(255,255,255,0.25);
  cursor: none;
  transition: background 0.3s, transform 0.3s;
}
.modal__dot--active {
  background: #fff;
  transform: scale(1.3);
}

.modal__close {
  position: absolute;
  top: 2rem;
  right: 2.5rem;
  z-index: 10;
  background: none;
  border: none;
  color: rgba(255,255,255,0.5);
  cursor: none;
  transition: color 0.2s;
  padding: 0;
}
.modal__close:hover { color: #fff; }

.modal__num {
  position: absolute;
  top: 2rem;
  left: 2.5rem;
  z-index: 10;
  color: rgba(255,255,255,0.3);
}

.modal__content {
  position: relative;
  z-index: 5;
  width: 100%;
  max-width: 680px;
  display: flex;
  flex-direction: column;
  gap: 1.4rem;
}

.modal__tags {
  display: flex;
  gap: 0.4rem;
  flex-wrap: wrap;
}
.modal__tag {
  border: 1px solid rgba(255,255,255,0.2);
  padding: 0.22rem 0.7rem;
  color: rgba(255,255,255,0.5);
}

.modal__title {
  font-size: clamp(2.8rem, 6vw, 5.5rem);
  font-weight: 300;
  letter-spacing: -0.03em;
  line-height: 1.05;
  color: #fff;
}
.modal__title em { font-style: italic; }

.modal__desc {
  font-size: 0.68rem;
  line-height: 2;
  color: rgba(255,255,255,0.55);
  max-width: 520px;
}

.modal__divider {
  width: 100%;
  height: 1px;
  background: rgba(255,255,255,0.12);
}

.modal__bottom {
  display: flex;
  align-items: center;
  justify-content: space-between;
  gap: 1.5rem;
  flex-wrap: wrap;
}

.modal__icons {
  display: flex;
  gap: 1.2rem;
  flex-wrap: wrap;
  align-items: center;
}
.modal__icon-wrap {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 0.35rem;
}
.modal__icon {
  font-size: 1.5rem;
  color: rgba(255,255,255,0.7);
}
.modal__icon-label {
  color: rgba(255,255,255,0.35);
  font-size: 0.44rem;
  letter-spacing: 0.16em;
}

.modal__links {
  display: flex;
  gap: 0.7rem;
  flex-wrap: wrap;
}
.modal__btn {
  padding: 0.6rem 1.4rem;
  transition: background 0.25s, color 0.25s, border-color 0.25s;
  cursor: none;
  text-decoration: none;
  display: inline-block;
}
.modal__btn--primary {
  background: #fff;
  color: #0c0c0c;
  border: 1px solid #fff;
}
.modal__btn--primary:hover {
  background: transparent;
  color: #fff;
}
.modal__btn--ghost {
  background: transparent;
  color: #fff;
  border: 1px solid rgba(255,255,255,0.3);
}
.modal__btn--ghost:hover { border-color: #fff; }

/* ── Modal transition ── */
.modal-enter-active,
.modal-leave-active { transition: opacity 0.5s var(--ease-out); }
.modal-enter-active .modal__content,
.modal-leave-active .modal__content {
  transition: transform 0.5s var(--ease-out), opacity 0.5s var(--ease-out);
}
.modal-enter-from, .modal-leave-to { opacity: 0; }
.modal-enter-from .modal__content,
.modal-leave-to .modal__content {
  transform: translateY(20px);
  opacity: 0;
}

/* ── Mobile ── */
@media (max-width: 700px) {
  .work__header {
    margin: 0 1.5rem;
    padding: 4rem 0 2rem;
    border-bottom: none;
  }
  .work__track-wrap { padding: 2rem 1.5rem 3rem; }
  .work__card { width: 280px; }
  .work__card-inner { padding: 1.4rem; }

  .modal { padding: 2rem 1.5rem; align-items: flex-end; }
  .modal__content { max-width: 100%; }
  .modal__title { font-size: clamp(2rem, 8vw, 3rem); }
  .modal__bottom { flex-direction: column; align-items: flex-start; }
  .modal__close { top: 1.5rem; right: 1.5rem; }
  .modal__num { top: 1.5rem; left: 1.5rem; }
  .modal__slides { bottom: 1.5rem; }
}
</style>
