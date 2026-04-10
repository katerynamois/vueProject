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
    addLike(movieId) {
      const movie = this.movies.find((m) => m.id === movieId)
      if (movie) movie.likes++
    },
    addComment(movieId, comment) {
      const movie = this.movies.find((m) => m.id === movieId)
      if (movie) movie.comments.push(comment)
    },
    setWatched(movieId, value) {
      const movie = this.movies.find((m) => m.id === movieId)
      if (movie) movie.watched = value
    },
    fetchMovies() {
      const movieIds = [155, 27205, 680, 13, 122]
      const promises = movieIds.map((id) =>
        fetch(`https://api.themoviedb.org/3/movie/${id}`, {
          headers: {
            Authorization: 'Bearer ' + import.meta.env.VITE_TMDB_TOKEN,
          },
        }).then((r) => r.json()),
      )
      Promise.all(promises).then((movies) => {
        this.movies = movies.map((movie) => ({
          ...movie,
          likes: 0,
          comments: [],
          watched: false,
        }))
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
      <MovieCard
        :movie="movie"
        @add-like="addLike(movie.id)"
        @add-comment="addComment(movie.id, $event)"
        @set-watched="setWatched(movie.id, $event)"
      />
    </v-col>
  </v-row>
</template>

<style scoped></style>
