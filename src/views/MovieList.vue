<script>
import MovieCard from '@/components/MovieCard.vue'
export default {
  name: 'MovieList',
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
      const movieIds = [238, 289, 539, 389, 11218, 426, 12593, 10590, 11691, 641]
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
  <v-container class="movie-list-container">
    <v-row>
      <v-col v-for="movie in movies" :key="movie.id" cols="12" sm="6" lg="4" xl="3">
        <MovieCard
          :movie="movie"
          @add-like="addLike(movie.id)"
          @add-comment="addComment(movie.id, $event)"
          @set-watched="setWatched(movie.id, $event)"
        />
      </v-col>
    </v-row>
  </v-container>
</template>

<style scoped>
.movie-list-container {
  padding: 32px 16px;
}
</style>
