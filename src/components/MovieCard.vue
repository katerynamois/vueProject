<script>
import CommentList from '@/components/CommentList.vue'

export default {
  name: 'MovieCard',
  components: { CommentList },
  props: {
    movie: {
      type: Object,
      required: true,
    },
  },
  emits: ['add-like', 'add-comment', 'set-watched'],
  data() {
    return {
      newComment: '',
      showComments: false,
    }
  },
  computed: {
    likesColor() {
      if (this.movie.likes <= 5) return 'likes-grey'
      if (this.movie.likes <= 10) return 'likes-blue'
      return 'likes-green'
    },
  },
  methods: {
    addLike() {
      this.$emit('add-like')
    },
    addComment() {
      if (this.newComment.trim() !== '') {
        this.$emit('add-comment', this.newComment.trim())
        this.newComment = ''
      }
    },
    clearInput() {
      this.newComment = ''
    },
    toggleComments() {
      this.showComments = !this.showComments
    },
    onWatchedChange(value) {
      this.$emit('set-watched', value)
    },
  },
}
</script>

<template>
  <v-card :class="['movie-card', { 'movie-watched': movie.watched }]" @click="toggleComments">
    <v-img :src="'https://image.tmdb.org/t/p/w500' + movie.poster_path" height="300px" cover></v-img>

    <v-card-title>{{ movie.title }}</v-card-title>

    <v-card-text @click.stop>
      <p class="movie-year">{{ movie.release_date.slice(0, 4) }}</p>

      <a :href="'https://www.imdb.com/title/' + movie.imdb_id" target="_blank" class="imdb-link">Se på IMDB</a>

      <p :class="['likes-count', likesColor]">Likes: {{ movie.likes }}</p>
      <v-btn size="small" class="like-btn" @click="addLike">Like</v-btn>

      <v-checkbox
        :model-value="movie.watched"
        :label="movie.watched ? 'Set' : 'Ikke set'"
        @update:model-value="onWatchedChange"
      ></v-checkbox>

      <p class="comments-hint">{{ showComments ? 'Skjul kommentarer ▲' : 'Vis kommentarer ▼' }}</p>

      <div v-if="showComments" class="comments-section">
        <CommentList :comments="movie.comments" />

        <v-text-field
          v-model="newComment"
          placeholder="Skriv en kommentar..."
          density="compact"
          @keyup.enter="addComment"
        ></v-text-field>
        <div class="comment-actions">
          <v-btn size="small" @click="addComment">Post</v-btn>
          <v-btn size="small" @click="clearInput">Slet alt</v-btn>
        </div>
      </div>
    </v-card-text>
  </v-card>
</template>

<style scoped>
.movie-card {
  cursor: pointer;
  transition: background-color 0.3s;
}

.movie-watched {
  background-color: #e8f5e9 !important;
  border: 2px solid #2E8B6E !important;
}

.movie-watched :deep(.v-card-title),
.movie-watched :deep(.v-card-text),
.movie-watched :deep(p),
.movie-watched :deep(li) {
  color: #1a1a1a !important;
}

.movie-year {
  color: #8b7355;
  font-size: 0.85rem;
  margin-bottom: 4px;
}

.imdb-link {
  display: inline-block;
  margin-bottom: 8px;
  color: #c0392b;
  font-size: 0.85rem;
}

.likes-count {
  font-weight: bold;
  margin: 8px 0 4px;
}
.likes-grey { color: grey; }
.likes-blue { color: blue; }
.likes-green { color: #2E8B6E; }

.like-btn {
  margin-bottom: 8px;
}

.comments-hint {
  font-size: 0.75rem;
  color: #8b7355;
  cursor: pointer;
  margin-top: 4px;
  margin-bottom: 0;
}

.comments-section {
  margin-top: 8px;
}

.comment-actions {
  display: flex;
  gap: 8px;
}
</style>
