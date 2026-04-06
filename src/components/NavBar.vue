<template>
  <nav class="nav">
    <div class="nav__logo-placeholder"></div>
    <a href="#contact" class="nav__cta u-label"
      @mouseenter="$emit('hover')" @mouseleave="$emit('unhover')">
      Get in touch →
    </a>
  </nav>
</template>

<script setup>
defineEmits(['hover', 'unhover'])
</script>

<style scoped>
.nav {
  position: fixed; top: 0; left: 0; right: 0; z-index: 500;
  display: flex; align-items: center; justify-content: space-between;
  padding: 1.8rem 3rem;
}
.nav__logo-placeholder { width: 120px; }
.nav__cta {
  font-size: 0.62rem;
  letter-spacing: 0.15em;
  text-transform: uppercase;
  color: var(--fg);
  border-bottom: 1px solid var(--fg);
  padding-bottom: 1px;
  transition: opacity 0.2s, color 0.7s var(--ease-out), border-color 0.7s var(--ease-out);
  text-decoration: none;
}
.nav__cta:hover { opacity: 0.4; }

@media (max-width: 700px) {
  .nav { padding: 1.4rem 1.5rem; }
  .nav__cta { display: none; }
}
</style>