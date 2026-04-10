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
  <div :class="['movie-card', { 'movie-watched': movie.watched }]" @click="toggleComments">
    <img
      :src="'https://image.tmdb.org/t/p/w500' + movie.poster_path"
      :alt="movie.title"
      class="movie-poster"
    />

    <div class="movie-body" @click.stop>
      <h3 class="movie-title">{{ movie.title }}</h3>
      <p class="movie-year">{{ movie.release_date.slice(0, 4) }}</p>

      <a :href="'https://www.imdb.com/title/' + movie.imdb_id" target="_blank" class="imdb-link">Se på IMDB ↗</a>

      <div class="likes-row">
        <span :class="['likes-count', likesColor]">♥ {{ movie.likes }}</span>
        <button class="like-btn" @click="addLike">Like</button>
      </div>

      <label class="watched-label">
        <input type="checkbox" :checked="movie.watched" @change="onWatchedChange($event.target.checked)" />
        {{ movie.watched ? 'Set' : 'Ikke set' }}
      </label>

      <p class="comments-hint" @click="toggleComments">
        {{ showComments ? 'Skjul kommentarer ▲' : 'Vis kommentarer ▼' }}
      </p>

      <div v-if="showComments" class="comments-section">
        <CommentList :comments="movie.comments" />
        <input
          v-model="newComment"
          class="comment-input"
          placeholder="Skriv en kommentar..."
          @keyup.enter="addComment"
          @click.stop
        />
        <div class="comment-actions">
          <button class="comment-btn" @click="addComment">Post</button>
          <button class="comment-btn" @click="clearInput">Slet alt</button>
        </div>
      </div>
    </div>
  </div>
</template>

<style scoped>
.movie-card {
  background: #fff8ee;
  border: 1.5px solid #d4c5a9;
  border-radius: 4px;
  max-width: 320px;
  margin: 0 auto;
  cursor: pointer;
  box-shadow: 3px 3px 0 #c0b090;
  transition: box-shadow 0.2s, transform 0.2s;
  overflow: hidden;
}
.movie-card:hover {
  transform: translate(-2px, -2px);
  box-shadow: 5px 5px 0 #c0b090;
}

.movie-watched {
  background: #e8f5ee;
  border-color: #2E8B6E;
  box-shadow: 3px 3px 0 #2E8B6E;
}

.movie-poster {
  width: 100%;
  display: block;
}

.movie-body {
  padding: 12px 16px 16px;
}

.movie-title {
  font-family: 'Georgia', serif;
  font-size: 1rem;
  font-weight: bold;
  color: #1a1008;
  margin: 0 0 2px;
}

.movie-year {
  font-size: 0.8rem;
  color: #8b7355;
  margin: 0 0 6px;
}

.imdb-link {
  display: inline-block;
  font-size: 0.75rem;
  color: #c0392b;
  text-decoration: none;
  letter-spacing: 1px;
  margin-bottom: 10px;
}
.imdb-link:hover {
  text-decoration: underline;
}

.likes-row {
  display: flex;
  align-items: center;
  gap: 10px;
  margin-bottom: 8px;
}

.likes-count {
  font-family: 'Georgia', serif;
  font-size: 0.9rem;
  font-weight: bold;
}
.likes-grey { color: #999; }
.likes-blue { color: #3a6ea8; }
.likes-green { color: #2E8B6E; }

.like-btn {
  font-size: 0.7rem;
  letter-spacing: 2px;
  background: #c0392b;
  color: #fff8ee;
  border: none;
  padding: 4px 12px;
  cursor: pointer;
  font-family: 'Georgia', serif;
  border-radius: 2px;
}
.like-btn:hover {
  background: #a93226;
}

.watched-label {
  display: flex;
  align-items: center;
  gap: 8px;
  font-size: 0.8rem;
  color: #5a4a35;
  cursor: pointer;
  margin-bottom: 8px;
}

.comments-hint {
  font-size: 0.72rem;
  color: #8b7355;
  letter-spacing: 1px;
  cursor: pointer;
  margin: 0;
  user-select: none;
}

.comments-section {
  margin-top: 10px;
  border-top: 1px solid #d4c5a9;
  padding-top: 10px;
}

.comment-input {
  width: 100%;
  border: 1px solid #d4c5a9;
  border-radius: 2px;
  padding: 6px 8px;
  font-size: 0.8rem;
  font-family: 'Georgia', serif;
  background: #fff;
  color: #1a1008;
  margin-bottom: 8px;
  box-sizing: border-box;
}

.comment-actions {
  display: flex;
  gap: 8px;
}

.comment-btn {
  font-size: 0.7rem;
  letter-spacing: 1px;
  background: #1a1008;
  color: #f5f0e8;
  border: none;
  padding: 4px 12px;
  cursor: pointer;
  font-family: 'Georgia', serif;
  border-radius: 2px;
}
.comment-btn:hover {
  background: #3a2a18;
}
</style>
