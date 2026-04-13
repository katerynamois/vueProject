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
    <v-container class="d-flex align-center justify-space-between" style="padding: 0 32px">
      <div class="logo-wrap" @click="$emit('navigate', 'home')">
        <img src="@/assets/Logo.png" class="logo-img" alt="CinéRetro logo" />
      </div>

      <nav class="d-flex align-center" style="gap: 28px">
        <button
          v-for="link in navLinks"
          :key="link.view"
          class="nav-link"
          :class="{ active: currentPage === link.view }"
          @click="$emit('navigate', link.view)"
        >
          {{ link.name }}
        </button>
      </nav>
    </v-container>
  </v-app-bar>
</template>

<style scoped>
.logo-wrap {
  cursor: pointer;
}
.logo-img {
  height: 50px;
  width: auto;
  display: block;
}

.retro-header {
  background: #1a1008 !important;
  border-bottom: 3px solid #c0392b !important;
  box-shadow: 0 2px 0 #d4a843 !important;
}

.nav-link {
  font-family: 'Georgia', serif;
  font-size: 0.85rem;
  letter-spacing: 3px;
  color: #8b7355;
  cursor: pointer;
  padding-bottom: 2px;
  border: none;
  border-bottom: 2px solid transparent;
  background: none;
  transition:
    color 0.2s,
    border-color 0.2s;
}
.nav-link:hover,
.nav-link.active {
  color: #d4a843;
  border-bottom-color: #c0392b;
}
</style>
