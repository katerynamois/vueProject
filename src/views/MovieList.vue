<script>
// MovieList is the single source of truth for all movie data.
// It fetches movies from TMDB and extends each with local state (likes, comments, watched).
import MovieCard from '@/components/MovieCard.vue'

export default {
  name: 'MovieList',
  components: {
    MovieCard,
  },
  data() {
    return {
      // All movies including TMDB data and local state
      movies: [],
    }
  },
  methods: {
    // Increments likes for the given movie object
    addLike(movie) {
      movie.likes++
    },

    // Appends a new comment to the given movie object
    addComment(movie, comment) {
      movie.comments.push(comment)
    },

    // Updates the watched status for the given movie object
    setWatched(movie, value) {
      movie.watched = value
    },

    // Fetches movie details from TMDB API for each selected movie id.
    // Local state (likes, comments, watched) is merged into each movie object.
    fetchMovies() {
      const movieIds = [289, 15873, 11970, 770, 10529, 26566, 3122, 4111, 10803]

      const promises = movieIds.map((id) =>
        fetch(`https://api.themoviedb.org/3/movie/${id}`, {
          headers: {
            // API token stored in .env.local as VITE_TMDB_TOKEN
            Authorization: 'Bearer ' + import.meta.env.VITE_TMDB_TOKEN,
          },
        }).then((r) => r.json()),
      )

      // Wait for all requests to finish, then add local state to each movie
      Promise.all(promises).then((movies) => {
        this.movies = movies.map((movie) =>
          Object.assign({}, movie, {
            likes: 0,
            comments: [],
            watched: false,
          }),
        )
      })
    },
  },

  // Fetch movies as soon as the component is mounted
  mounted() {
    this.fetchMovies()
  },
}
</script>

<template>
  <!-- Responsive grid: 1 col on mobile, 2 on tablet, 3 on desktop, 4 on large screens -->
  <v-container class="movie-list-container">
    <v-row>
      <v-col v-for="movie in movies" :key="movie.id" cols="12" sm="6" lg="4" xl="3">
        <!-- Pass full movie object down and listen for emitted events to update state -->
        <MovieCard
          :movie="movie"
          @add-like="addLike"
          @add-comment="addComment"
          @set-watched="setWatched"
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
