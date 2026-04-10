<script>
import MovieCard from '@/components/MovieCard.vue'
export default {
  name: '',
  components: {
    MovieCard,
  },
  data() {
    return {
      movies: [],
    }
  },
  props: {},
  computed: {},
  methods: {
    fetchMovies() {
      const movieIds = [155, 27205, 680, 13, 122]
      const promises = movieIds.map((id) =>
        fetch(`https://api.themoviedb.org/3/movie/${id}`, {
          headers: {
            Authorization:
              'Bearer eyJhbGciOiJIUzI1NiJ9.eyJhdWQiOiI5YmZkODVmYzFiMmI5ZmNlNTlmZDZkODg1MDllYjVjMSIsIm5iZiI6MTc3MTU3ODQ5Ni4zMywic3ViIjoiNjk5ODI0ODA4MTQyZDVlY2FhYWI5MTI1Iiwic2NvcGVzIjpbImFwaV9yZWFkIl0sInZlcnNpb24iOjF9.uXRZdg70CQZ1ilW9M2bkLTA3U6KNu84jyxJxgSKx870',
          },
        }).then((r) => r.json()),
      )
      Promise.all(promises).then((movies) => {
        this.movies = movies
        console.log(this.movies)
      })
    },
  },
  watch: {},
  mounted() {
    this.fetchMovies()
  },
}
</script>

<template>
  <v-row>
    <v-col v-for="movie in movies" :key="movie.id" cols="12" md="6" lg="4">
      <MovieCard :movie="movie" />
    </v-col>
  </v-row>
</template>

<style scoped></style>
