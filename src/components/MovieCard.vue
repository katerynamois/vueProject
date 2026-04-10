<script>
export default {
  name: 'MovieCard',
  components: {},
  data() {
    return {
      // Lokal data der ændrer sig i appen
      likes: 0,
      comments: [],
      watched: false,
      newComment: '', // tekst i input-feltet
      showComments: false, // vis/skjul kommentarer
    }
  },
  props: {
    // Modtager ét film-objekt fra MovieList
    movie: {
      type: Object,
      required: true,
    },
  },
  computed: {
    likesColor() {
      if (this.likes <= 5) {
        return 'grey'
      } else if (this.likes <= 10) {
        return 'blue'
      } else {
        return 'green'
      }
    },
  },
  methods: {
    // Tilføjer +1 til likes
    addLike() {
      this.likes++
    },
    // Tilføjer kommentar til listen
    addComment() {
      if (this.newComment.trim() !== '') {
        this.comments.push(this.newComment)
        this.newComment = '' // ryd input-felt
      }
    },
    // Sletter al tekst i input-feltet
    clearInput() {
      this.newComment = ''
    },
    // Skifter mellem vis/skjul kommentarer
    toggleComments() {
      this.showComments = !this.showComments
    },
  },
  watch: {},
  emits: [],
}
</script>

<template>
  <v-card>
    <!-- Poster billede -->
    <v-img :src="'https://image.tmdb.org/t/p/w500' + movie.poster_path" height="300px"></v-img>

    <!-- Titel -->
    <v-card-title>{{ movie.title }}</v-card-title>

    <v-card-text>
      <!-- Årstal -->
      <p>{{ movie.release_date.slice(0, 4) }}</p>

      <!-- IMDB link -->
      <a :href="'https://www.imdb.com/title/' + movie.imdb_id" target="_blank">Se på IMDB</a>

      <!-- Likes -->
      <p :class="likesColor">Likes: {{ likes }}</p>
      <v-btn @click="addLike">Like</v-btn>

      <!-- Watched checkbox -->
      <v-checkbox v-model="watched" :label="watched ? 'Set' : 'Ikke set'"></v-checkbox>

      <!-- Kommentarer -->
      <v-btn @click="toggleComments">
        {{ showComments ? 'Skjul kommentarer' : 'Vis kommentarer' }}
      </v-btn>

      <!-- Liste over kommentarer -->
      <ul v-if="showComments">
        <li v-for="(comment, index) in comments" :key="index">{{ comment }}</li>
      </ul>

      <!-- Input felt til ny kommentar -->
      <v-text-field
        v-model="newComment"
        placeholder="Skriv en kommentar..."
        @keyup.enter="addComment"
      ></v-text-field>
      <v-btn @click="addComment">Post</v-btn>
      <v-btn @click="clearInput">Slet alt</v-btn>
    </v-card-text>
  </v-card>
</template>

<style scoped>
.grey {
  color: grey;
}
.blue {
  color: blue;
}
.green {
  color: green;
}
</style>
