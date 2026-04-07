<template>
  <footer class="footer">
    <span class="footer__copy u-label">© 2026 · chrolluash · All rights reserved</span>
    <a
      href="#"
      class="footer__top"
      @mouseenter="$emit('hover')"
      @mouseleave="$emit('unhover')"
    >↑</a>
  </footer>
</template>

<script setup>
defineEmits(['hover', 'unhover'])
</script>

<style scoped>
.footer {
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 2.5rem 3rem;
}

.footer__copy {
  color: var(--fg-muted);
  opacity: 0.5;
}

.footer__top {
  font-size: 1.4rem;
  color: var(--fg);
  text-decoration: none;
  opacity: 0.5;
  animation: float 2.4s var(--ease-out) infinite;
  transition: opacity 0.3s;
  display: block;
}

.footer__top:hover {
  opacity: 1;
}

@keyframes float {
  0%, 100% { transform: translateY(0);    }
  50%       { transform: translateY(-6px); }
}

@media (max-width: 700px) {
  .footer { padding: 2rem 1.5rem; }
}
</style>