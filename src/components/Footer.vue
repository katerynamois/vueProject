<script>
export default {
  name: 'Footer',
}
</script>

<template>
  <footer class="retro-footer">
    <div class="footer-inner">
      <div class="footer-copy">© 1948–2025 · Alle rettigheder forbeholdes</div>
      <nav class="footer-links" aria-label="Footer navigation">
        <a href="#" class="footer-link">Kontakt</a>
        <a href="#" class="footer-link">Presse</a>
        <a href="#" class="footer-link">Privatlivspolitik</a>
      </nav>
    </div>
  </footer>
</template>

<style scoped>
.retro-footer {
  background: #1a1008;
  border-top: 3px solid #c0392b;
  font-family: 'Georgia', serif;
  font-size: 0.85rem;
  letter-spacing: 1px;
}

.footer-inner {
  display: flex;
  align-items: center;
  justify-content: space-between;
  flex-wrap: wrap;
  gap: 8px;
  padding: 14px 32px;
}

.footer-copy {
  color: #8b7355;
}

.footer-links {
  display: flex;
  gap: 16px;
}
.footer-link {
  color: #8b7355;
  cursor: pointer;
  text-decoration: none;
  transition: color 0.2s;
}
.footer-link:hover {
  color: #d4a843;
}
</style>
