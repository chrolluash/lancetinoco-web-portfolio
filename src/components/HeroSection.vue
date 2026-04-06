<template>
  <section class="hero">

    <ul class="hero__disciplines">
      <li v-for="(d, i) in disciplines" :key="d"
          class="hero__disc u-label"
          :style="{ animationDelay: (0.3 + i * 0.12) + 's' }">
        {{ d }}
      </li>
    </ul>

    <div class="hero__photo-wrap">
      <img src="/profile.jpg" alt="Lance Tinoco" class="hero__photo" />
    </div>

    <div class="hero__name-wrap" ref="nameWrapRef">

      <div class="hero__name-ghost" aria-hidden="true">
        <span>Lance</span>
        <span>Tinoco</span>
      </div>

      <h1
        class="hero__name"
        :class="{ 'hero__name--fixed': isFixed }"
        :style="isFixed ? fixedStyle : {}"
      >
        <span class="hero__name-line hero__name-line--1">Lance</span>
        <span
          class="hero__name-line hero__name-line--2"
          :style="tinocoStyle"
        >Tinoco</span>
      </h1>

    </div>

    <div class="hero__bottom">
      <p class="hero__status u-label">Available for opportunities</p>
      <p class="hero__location u-label">Muntinlupa, PH · 2026</p>
    </div>

    <span class="hero__bg-word" aria-hidden="true">Build.</span>
  </section>
</template>

<script setup>
import { ref, computed, onMounted, onUnmounted } from 'vue'

const disciplines = ['Full-Stack Web Dev', 'IoT', 'IT Undergrad']

const nameWrapRef = ref(null)
const progress    = ref(0)
const isFixed     = ref(false)

let originTop  = 0
let originLeft = 0
let rafId      = null
let lastScroll = -1

const lerp     = (a, b, t) => a + (b - a) * t
const isMobile = () => window.innerWidth <= 600

const NAV_TOP  = 28
const NAV_LEFT = 48

const fixedStyle = computed(() => {
  const p      = progress.value
  const mobile = isMobile()
  const size   = mobile ? lerp(6, 0.85, p) : lerp(13, 0.95, p)
  const top    = lerp(originTop, NAV_TOP, p)
  const left   = lerp(originLeft, mobile ? 24 : NAV_LEFT, p)
  const isRow  = p > 0.4

  return {
    position:      'fixed',
    top:           `${top}px`,
    left:          `${left}px`,
    fontSize:      `clamp(0.9rem, ${size}rem, ${mobile ? '6rem' : '15rem'})`,
    zIndex:        '600',
    transform:     'none',
    display:       'flex',
    flexDirection: isRow ? 'row' : 'column',
    alignItems:    'baseline',
    gap:           isRow ? `${lerp(0, 0.25, (p - 0.4) / 0.6)}em` : '0',
    lineHeight:    `${lerp(0.88, 1.2, p)}`,
    transition:    'flex-direction 0.4s var(--ease-out), gap 0.4s var(--ease-out)',
    color:         'var(--fg)',
  }
})

const tinocoStyle = computed(() => {
  const p         = progress.value
  const stroke    = Math.max(0, lerp(1.5, 0, p / 0.6))
  const alpha     = Math.min(1, lerp(0, 1, p / 0.8))
  const fontStyle = p > 0.5 ? 'normal' : 'italic'

  return {
    WebkitTextStroke: `${stroke}px var(--fg)`,
    color:            `color-mix(in srgb, var(--fg) ${Math.round(alpha * 100)}%, transparent)`,
    fontStyle:        fontStyle,
    transition:       'color 0.15s linear, font-style 0.3s ease, -webkit-text-stroke 0.15s linear',
    display:          'block',
  }
})

function measure() {
  const rect = nameWrapRef.value?.getBoundingClientRect()
  originTop  = rect ? rect.top + window.scrollY : 0
  originLeft = rect ? rect.left : (isMobile() ? 24 : NAV_LEFT)
}

function onScroll() {
  if (rafId) return
  rafId = requestAnimationFrame(() => {
    rafId = null

    const scrollY = window.scrollY
    if (scrollY === lastScroll) return
    lastScroll = scrollY

    if (scrollY <= 0) {
      progress.value = 0
      isFixed.value  = false
      return
    }

    isFixed.value  = true
    progress.value = Math.min(1, scrollY / (window.innerHeight * 0.65))
  })
}

onMounted(() => {
  requestAnimationFrame(measure)
  window.addEventListener('resize', measure)
  window.addEventListener('scroll', onScroll, { passive: true })
})

onUnmounted(() => {
  window.removeEventListener('scroll', onScroll)
  window.removeEventListener('resize', measure)
  if (rafId) cancelAnimationFrame(rafId)
})
</script>

<style scoped>
.hero {
  position: relative;
  min-height: 100vh;
  display: flex;
  flex-direction: column;
  justify-content: flex-end;
  padding: 2rem 3rem 3.5rem;
  overflow: hidden;
  background: var(--bg);
  transition: background 0.7s var(--ease-out);
}

/* ── Photo ── */
.hero__photo-wrap {
  position: absolute;
  right: 0;
  top: 0;
  width: clamp(260px, 32vw, 460px);
  height: 100%;
  overflow: hidden;
  opacity: 0;
  animation: fadeIn 1.1s var(--ease-out) 0.4s forwards;
  z-index: 1;
}

/* grain overlay */
.hero__photo-wrap::before {
  content: '';
  position: absolute;
  inset: 0;
  z-index: 2;
  pointer-events: none;
  opacity: 0.18;
  background-image: url("data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' width='200' height='200'%3E%3Cfilter id='g'%3E%3CfeTurbulence type='fractalNoise' baseFrequency='0.85' numOctaves='4' stitchTiles='stitch'/%3E%3C/filter%3E%3Crect width='200' height='200' filter='url(%23g)' opacity='0.15'/%3E%3C/svg%3E");
  background-size: 180px 180px;
  mix-blend-mode: overlay;
}

.hero__photo {
  width: 100%;
  height: 100%;
  object-fit: cover;
  object-position: center 15%;
  filter: contrast(1.05);
  mix-blend-mode: luminosity;
}

/* ── Disciplines ── */
.hero__disciplines {
  position: absolute;
  top: 3rem;
  right: clamp(270px, 34vw, 480px);
  list-style: none;
  display: flex;
  flex-direction: column;
  align-items: flex-end;
  gap: 0.6rem;
  z-index: 2;
}
.hero__disc {
  opacity: 0;
  transform: translateX(12px);
  animation: slideIn 0.7s var(--ease-out) forwards;
  color: var(--fg-muted);
}

/* ── Name wrap ── */
.hero__name-wrap {
  position: relative;
  z-index: 2;
}

.hero__name-ghost {
  font-family: var(--font-display);
  font-size: clamp(5.5rem, 14vw, 15rem);
  font-weight: 300;
  line-height: 0.88;
  letter-spacing: -0.03em;
  display: flex;
  flex-direction: column;
  opacity: 0;
  pointer-events: none;
  user-select: none;
}

.hero__name {
  font-family: var(--font-display);
  font-size: clamp(5.5rem, 14vw, 15rem);
  font-weight: 300;
  line-height: 0.88;
  letter-spacing: -0.03em;
  color: var(--fg);
  display: flex;
  flex-direction: column;
  position: absolute;
  bottom: 0;
  left: 0;
  transition: color 0.7s var(--ease-out);
}

.hero__name-line {
  display: block;
  opacity: 0;
  transform: translateY(40px);
  animation: fadeUp 1s var(--ease-out) forwards;
}
.hero__name-line--1 { animation-delay: 0.1s; }
.hero__name-line--2 {
  animation-delay: 0.25s;
  font-style: italic;
  -webkit-text-stroke: 1.5px var(--fg);
  color: transparent;
  transition: -webkit-text-stroke 0.7s var(--ease-out);
}

/* ── Bottom bar ── */
.hero__bottom {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-top: 3.5rem;
  position: relative;
  z-index: 2;
  opacity: 0;
  animation: fadeUp 0.9s var(--ease-out) 0.7s forwards;
}
.hero__status {
  color: rgba(255, 255, 255, 0.45);
}
.hero__location {
  color: rgba(255, 255, 255, 0.45);
}

/* ── Background word ── */
.hero__bg-word {
  position: absolute;
  bottom: -0.5rem;
  left: -0.5rem;
  font-family: var(--font-display);
  font-style: italic;
  font-size: clamp(10rem, 24vw, 26rem);
  font-weight: 300;
  color: transparent;
  -webkit-text-stroke: 1px rgba(255,255,255,0.055);
  pointer-events: none;
  user-select: none;
  line-height: 1;
  z-index: 0;
}

/* ── Keyframes ── */
@keyframes fadeUp {
  to { opacity: 1; transform: translateY(0); }
}
@keyframes fadeIn {
  to { opacity: 1; }
}
@keyframes slideIn {
  to { opacity: 1; transform: translateX(0); }
}

/* ── Tablet (≤900px) ── */
@media (max-width: 900px) {
  .hero__photo-wrap {
    width: clamp(180px, 38vw, 280px);
  }
  .hero__disciplines {
    top: 5.5rem;
    right: clamp(190px, 40vw, 295px);
  }
}

/* ── Mobile (≤600px) ── */
@media (max-width: 600px) {
  .hero {
    padding: 0;
    min-height: 100svh;
    justify-content: flex-end;
  }

  .hero__name,
  .hero__name-ghost {
    font-size: clamp(4.5rem, 24vw, 6rem);
  }

  /* photo diagonal clip from right */
  .hero__photo-wrap {
    position: absolute;
    top: 0;
    right: 0;
    left: auto;
    bottom: auto;
    width: 72%;
    height: 100%;
    clip-path: polygon(22% 0%, 100% 0%, 100% 100%, 0% 100%);
  }

  /* left-to-right fade so name area stays clean */
  .hero__photo-wrap::after {
    content: '';
    position: absolute;
    inset: 0;
    background: linear-gradient(
      to right,
      var(--bg) 0%,
      transparent 35%
    );
    z-index: 1;
    transition: background 0.7s var(--ease-out);
  }

  .hero__photo {
    object-position: center 15%;
  }

  /* disciplines top-left */
  .hero__disciplines {
    position: absolute;
    top: 4.5rem;
    left: 1.5rem;
    right: auto;
    align-items: flex-start;
    flex-direction: column;
    z-index: 3;
    background: none;
    padding: 0;
    gap: 0.3rem;
  }
  .hero__disc {
    transform: translateX(-12px);
    color: var(--fg-muted);
  }

  /* name bottom-left */
  .hero__name-wrap {
    position: relative;
    z-index: 3;
    padding: 0 1.5rem 0 2rem;
  }

  .hero__bottom {
    flex-direction: row;
    justify-content: space-between;
    align-items: flex-end;
    padding: 0 1.5rem 0 2rem;
    margin-top: 0.75rem;
    margin-bottom: 2.5rem;
    position: relative;
    z-index: 3;
    gap: 1rem;
  }

  .hero__status {
    flex-shrink: 1;
  }

  .hero__location {
    flex-shrink: 0;
    text-align: right;
    margin-left: auto;
  }

  /* Build. repositioned on mobile so it doesn't hide behind photo */
  .hero__bg-word {
    display: block;
    font-size: clamp(4rem, 28vw, 6.5rem);
    bottom: 1.5rem;
    left: 0;
    right: auto;
    z-index: 4;
    -webkit-text-stroke: 1px rgba(255, 255, 255, 0.08);
  }
}
</style>