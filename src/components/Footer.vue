<script>
export default {
  name: 'Footer',
}
</script>

<template>
  <v-footer app class="retro-footer">
    <v-container class="d-flex align-center justify-space-between flex-wrap" style="gap: 8px;">
      <div class="footer-brand">🎬 CinéRetro</div>
      <div class="footer-copy">© 1948–2025 · Alle rettigheder forbeholdes</div>
      <div class="d-flex" style="gap: 16px;">
        <span class="footer-link">Kontakt</span>
        <span class="footer-link">Presse</span>
        <span class="footer-link">Privatlivspolitik</span>
      </div>
    </v-container>
  </v-footer>
</template>

<style scoped>
.retro-footer {
  background: #1A1008 !important;
  border-top: 3px solid #C0392B !important;
  color: #8B7355 !important;
  font-size: 0.75rem;
  letter-spacing: 1px;
  min-height: 52px;
}
.footer-brand {
  color: #D4A843;
  font-size: 0.9rem;
  letter-spacing: 2px;
}
.footer-copy { color: #8B7355; }
.footer-link {
  color: #8B7355;
  cursor: pointer;
  transition: color 0.2s;
}
.footer-link:hover { color: #D4A843; }
</style>
