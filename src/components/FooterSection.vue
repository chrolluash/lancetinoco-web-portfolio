<template>
  <footer class="footer">
    <a
      href="#"
      class="footer__top u-label"
      @mouseenter="$emit('hover')"
      @mouseleave="$emit('unhover')"
    >↑</a>
    <span class="footer__copy u-label">© 2026 · chrolluash · All rights reserved</span>
  </footer>
</template>

<script setup>
defineEmits(['hover', 'unhover'])
</script>

<style scoped>
.footer {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  gap: 1rem;
  padding: 3rem;
  text-align: center;
}

.footer__top {
  color: var(--fg);
  text-decoration: none;
  border-bottom: 1px solid var(--lightgray);
  padding-bottom: 1px;
  transition: border-color 0.2s, opacity 0.2s;
}
.footer__top:hover {
  opacity: 0.4;
  border-color: var(--fg);
}

.footer__copy {
  color: var(--fg-muted);
}
</style>