<template>
  <section class="stack">

    <div class="stack__header reveal">
      <h2 class="stack__heading u-serif">Tech <em>Stack</em></h2>
      <p class="stack__sub u-label">What I build with</p>
    </div>

    <div class="stack__categories">
      <div
        v-for="(group, i) in groups"
        :key="group.title"
        class="stack__group reveal"
        :style="{ transitionDelay: (i * 0.1) + 's' }"
      >
        <span class="stack__group-label u-label">{{ group.title }}</span>
        <div class="stack__cloud">
          <span
            v-for="item in group.items"
            :key="item.class"
            class="stack__pill"
            :title="item.label"
          >
            <i :class="['devicon-' + item.class, 'stack__pill-icon']"></i>
            <span class="stack__pill-label u-label">{{ item.label }}</span>
          </span>
        </div>
      </div>
    </div>

  </section>
</template>

<script setup>
const groups = [
  {
    title: 'Languages',
    items: [
      { class: 'html5-plain',      label: 'HTML5'      },
      { class: 'css3-plain',       label: 'CSS3'       },
      { class: 'javascript-plain', label: 'JavaScript' },
      { class: 'php-plain',        label: 'PHP'        },
      { class: 'dart-plain',       label: 'Dart'       },
    ],
  },
  {
    title: 'Frameworks',
    items: [
      { class: 'vuejs-plain',          label: 'Vue.js'       },
      { class: 'laravel-plain',        label: 'Laravel'      },
      { class: 'flutter-plain',        label: 'Flutter'      },
      { class: 'tailwindcss-original', label: 'Tailwind CSS' },
    ],
  },
  {
    title: 'Databases',
    items: [
      { class: 'mysql-plain',      label: 'MySQL'      },
      { class: 'mariadb-plain',    label: 'MariaDB'    },
      { class: 'firebase-plain',   label: 'Firebase'   },
      { class: 'postgresql-plain', label: 'PostgreSQL' },
      { class: 'supabase-plain',   label: 'Supabase'   },
    ],
  },
  {
    title: 'Tools & Platforms',
    items: [
      { class: 'git-plain',     label: 'Git'    },
      { class: 'github-plain',  label: 'GitHub' },
      { class: 'arduino-plain', label: 'Arduino'},
    ],
  },
]
</script>

<style scoped>
.stack {
  padding: 8rem 3rem;
}

/* ── Header ── */
.stack__header {
  display: flex;
  justify-content: space-between;
  align-items: flex-end;
  margin-bottom: 4rem;
}
.stack__heading {
  font-size: clamp(2rem, 4vw, 4.5rem);
  font-weight: 300;
  letter-spacing: -0.03em;
  line-height: 1;
}
.stack__heading em { font-style: italic; }

/* ── Categories ── */
.stack__categories {
  display: flex;
  flex-direction: column;
  gap: 0;
}

.stack__group {
  display: grid;
  grid-template-columns: 180px 1fr;
  gap: 2rem;
  align-items: start;
  padding: 2rem 0;
}

.stack__group-label {
  padding-top: 0.6rem;
  color: var(--fg-muted);
}

/* ── Pill cloud per category ── */
.stack__cloud {
  display: flex;
  flex-wrap: wrap;
  gap: 0.5rem;
}

.stack__pill {
  display: inline-flex;
  align-items: center;
  gap: 0.5rem;
  padding: 0.5rem 1rem;
  border: 1px solid var(--lightgray);
  color: var(--fg);
  cursor: default;
  transition:
    background 0.3s var(--ease-out),
    color 0.3s var(--ease-out),
    border-color 0.3s var(--ease-out),
    transform 0.3s var(--ease-out);
}

.stack__pill:hover {
  background: var(--fg);
  color: var(--bg);
  border-color: var(--fg);
  transform: translateY(-2px);
}

.stack__pill-icon {
  font-size: 0.95rem;
  color: inherit;
  line-height: 1;
}

.stack__pill-label {
  color: inherit;
  letter-spacing: 0.18em;
}

/* ── Mobile ── */
@media (max-width: 900px) {
  .stack { padding: 4rem 1.5rem; }
  .stack__group {
    grid-template-columns: 100px 1fr;
    gap: 1rem;
  }
}

@media (max-width: 500px) {
  .stack__group {
    grid-template-columns: 1fr;
    gap: 0.8rem;
  }
  .stack__group-label {
    padding-top: 0;
  }
}
</style>