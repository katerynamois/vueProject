<script>
import Header from '/src/components/Header.vue'
import Footer from '/src/components/Footer.vue'
import LandingPage from '/src/views/LandingPage.vue'
import MovieList from '/src/views/MovieList.vue'

export default {
  name: '',
  components: {
    Header,
    Footer,
    LandingPage,
    MovieList,
  },
  data() {
    return {
      currentPage: 'home',
    }
  },
  props: {},
  computed: {},
  methods: {},
  watch: {},
  emits: [],
}
</script>

<template>
  <v-app>
    <Header @navigate="currentPage = $event" />

    <v-main>
      <v-container>
        <LandingPage v-if="currentPage === 'home'" />
        <MovieList v-else-if="currentPage === 'films'" />
      </v-container>
    </v-main>

    <Footer />
  </v-app>
</template>

<style scoped></style>
