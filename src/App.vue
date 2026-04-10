<script>
// Root component — manages global navigation state and renders the correct view
import Header from '/src/components/Header.vue'
import Footer from '/src/components/Footer.vue'
import LandingPage from '/src/views/LandingPage.vue'
import MovieList from '/src/views/MovieList.vue'

export default {
  name: 'App',
  components: { Header, Footer, LandingPage, MovieList },
  data() {
    return {
      // Tracks which page is currently active: 'home' or 'films'
      currentPage: 'home',
    }
  },
}
</script>

<template>
  <v-app class="retro-app">
    <!-- Header receives currentPage to highlight the active nav link -->
    <Header :currentPage="currentPage" @navigate="currentPage = $event" />

    <v-main>
      <!-- Conditionally render the correct view based on currentPage -->
      <LandingPage v-if="currentPage === 'home'" @navigate="currentPage = $event" />
      <MovieList v-else-if="currentPage === 'films'" />
    </v-main>

    <Footer />
  </v-app>
</template>

<style>
.retro-app {
  background-color: #F5F0E8 !important;
  font-family: 'Georgia', 'Times New Roman', serif;
}
</style>
