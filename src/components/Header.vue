<script>
export default {
  name: 'Header',
  props: {
    currentPage: {
      type: String,
      required: true,
    },
  },
  emits: ['navigate'],
  data() {
    return {
      navLinks: [
        { name: 'HJEM', view: 'home' },
        { name: 'FILM', view: 'films' },
      ],
    }
  },
}
</script>

<template>
  <v-app-bar flat class="retro-header" height="70">
    <v-container class="d-flex align-center justify-space-between pa-0">

      <div class="logo-wrap d-flex align-center" style="gap: 10px;">
        <v-img src="@/assets/Logo.png" max-height="44" max-width="44" />
        <div class="logo-text">
          <div class="logo-title">CinéRetro</div>
          <div class="logo-sub">THE CLASSIC FILM HOUSE</div>
        </div>
      </div>

      <nav class="d-flex align-center" style="gap: 28px;">
        <span
          v-for="link in navLinks"
          :key="link.view"
          class="nav-link"
          :class="{ active: currentPage === link.view }"
          @click="$emit('navigate', link.view)"
        >
          {{ link.name }}
        </span>
      </nav>

    </v-container>
  </v-app-bar>
</template>

<style scoped>
.retro-header {
  background: #1A1008 !important;
  border-bottom: 3px solid #C0392B !important;
  box-shadow: 0 2px 0 #D4A843 !important;
}

.logo-title {
  font-family: 'Georgia', serif;
  font-size: 1.4rem;
  font-weight: bold;
  color: #D4A843;
  letter-spacing: 2px;
  line-height: 1;
}
.logo-sub {
  font-size: 0.55rem;
  color: #8B7355;
  letter-spacing: 3px;
  margin-top: 2px;
}

.nav-link {
  font-size: 0.7rem;
  letter-spacing: 3px;
  color: #8B7355;
  cursor: pointer;
  padding-bottom: 2px;
  border-bottom: 2px solid transparent;
  transition: color 0.2s, border-color 0.2s;
}
.nav-link:hover,
.nav-link.active {
  color: #D4A843;
  border-bottom-color: #C0392B;
}
</style>
