<template>
  <div class="marquee">
    <div class="marquee__track">
      <span v-for="(item, i) in items" :key="i" class="marquee__item">
        <i
          :class="['devicon-' + item.class, 'marquee__icon', { 'marquee__icon--colored': item.colored }]"
          :title="item.label"
        ></i>
        <span class="marquee__label u-label">{{ item.label }}</span>
        <span class="marquee__sep" aria-hidden="true">—</span>
      </span>
    </div>
  </div>
</template>

<script setup>
const base = [
  { class: 'vuejs-plain',          label: 'Vue.js' },
  { class: 'laravel-plain',        label: 'Laravel' },
  { class: 'flutter-plain',        label: 'Flutter' },
  { class: 'php-plain',            label: 'PHP' },
  { class: 'javascript-plain',     label: 'JavaScript' },
  { class: 'dart-plain',           label: 'Dart' },
  { class: 'mysql-plain',          label: 'MySQL' },
  { class: 'firebase-plain',       label: 'Firebase' },
  { class: 'tailwindcss-original', label: 'Tailwind CSS' },
  { class: 'arduino-plain',        label: 'Arduino' },
  { class: 'html5-plain',          label: 'HTML' },
  { class: 'css3-plain',           label: 'CSS' },
  { class: 'github-plain',      label: 'GitHub' },
  { class: 'postgresql-plain',     label: 'PostgreSQL' },
  { class: 'supabase-plain',       label: 'Supabase' },
]

const items = [...base, ...base]
</script>

<style scoped>
.marquee {
  overflow: hidden;
  padding: 0.9rem 0;
  background: var(--white);
}

.marquee__track {
  display: flex;
  align-items: center;
  width: max-content;
  animation: marquee 40s linear infinite;
}

.marquee__item {
  display: flex;
  align-items: center;
  gap: 0.5rem;
  padding: 0 1.2rem;
  white-space: nowrap;
}

.marquee__icon {
  font-size: 1.1rem;
  color: var(--black);
  transition: color 0.7s var(--ease-out);
}

/* colored icons like supabase, tailwind, github — grayscale to match theme */
.marquee__icon--colored {
  filter: grayscale(100%) brightness(0);
  transition: filter 0.7s var(--ease-out);
}

.marquee__label {
  color: var(--black);
}

.marquee__sep {
  color: var(--lightgray);
  margin-left: 0.6rem;
}

@keyframes marquee {
  from { transform: translateX(0); }
  to   { transform: translateX(-50%); }
}
</style>