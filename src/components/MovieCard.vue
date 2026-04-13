<script>
// MovieCard displays a single movie with all interactive elements.
// It receives movie data via props and emits events up to MovieList for state updates.
import CommentList from '@/components/CommentList.vue'

export default {
  name: 'MovieCard',
  components: { CommentList },

  props: {
    // Full movie object including TMDB data and local state (likes, comments, watched)
    movie: {
      type: Object,
      required: true,
    },
  },

  // Events emitted to parent (MovieList) to update the single source of truth
  emits: ['add-like', 'add-comment', 'set-watched'],

  mounted() {
    // 为每个组件实例创建独立的方法引用，避免闭包问题
    this._boundAddLike = () => this.addLike()
    this._boundAddComment = (comment) => this.addComment(comment)
    this._boundSetWatched = (value) => this.onWatchedChange(value)
  },

  data() {
    return {
      newComment: '',      // Bound to the comment input field
      showComments: false, // Controls visibility of the comments section
      imageError: false,   // Tracks if poster image failed to load
    }
  },

  computed: {
    // Returns a CSS class based on the current like count
    // 0-5: grey, 6-10: blue, 11+: green
    likesColor() {
      if (this.movie.likes <= 5) return 'likes-grey'
      if (this.movie.likes <= 10) return 'likes-blue'
      return 'likes-green'
    },

    // Rounds vote_average to one decimal
    rating() {
      return this.movie.vote_average ? this.movie.vote_average.toFixed(1) : '—'
    },

    // Extracts year from release_date (format: YYYY-MM-DD)
    year() {
      return this.movie.release_date ? this.movie.release_date.slice(0, 4) : ''
    },
  },

  methods: {
    // Emits add-like event — parent receives movie object via $event and extracts id
    addLike() {
      // 发送整个 movie 对象，让父组件通过 $event 获取 id
      this.$emit('add-like', this.movie)
    },

    // Validates input, emits the comment to parent with movie object, then clears the field
    addComment() {
      if (this.newComment.trim() !== '') {
        // 发送 movie 对象和评论内容
        this.$emit('add-comment', this.movie, this.newComment.trim())
        this.newComment = ''
      }
    },

    // Clears the comment input field without posting
    clearInput() {
      this.newComment = ''
    },

    // Handles poster image load error by showing placeholder
    onImageError() {
      this.imageError = true
    },

    // Toggles the comments section open/closed when the card is clicked
    toggleComments() {
      this.showComments = !this.showComments
    },

    // Emits the new watched value to parent when checkbox changes
    onWatchedChange(value) {
      // 发送 movie 对象和 watched 状态
      this.$emit('set-watched', this.movie, value)
    },
  },
}
</script>

<template>
  <!-- Dynamic class 'movie-watched' changes the card style when the film is marked as seen -->
  <div :class="['movie-card', { 'movie-watched': movie.watched }]" @click="toggleComments">

    <!-- Poster image fetched from TMDB image CDN with fallback -->
    <img
      v-if="!imageError && movie.poster_path"
      :src="'https://image.tmdb.org/t/p/w500' + movie.poster_path"
      :alt="movie.title"
      class="movie-poster"
      @error="onImageError"
    />
    <!-- Fallback placeholder when image fails to load or no poster available -->
    <div v-else class="movie-poster-placeholder">
      <div class="placeholder-content">
        <span class="placeholder-icon">🎬</span>
        <span class="placeholder-text">{{ movie.title }}</span>
        <span class="placeholder-year">{{ year }}</span>
      </div>
    </div>

    <!-- click.stop prevents card click (toggleComments) from firing inside the body -->
    <div class="movie-body" @click.stop>
      <h3 class="movie-title">{{ movie.title }}</h3>

      <!-- Rating + year + IMDB in one row -->
      <div class="movie-meta">
        <span class="movie-rating">★ {{ rating }}</span>
        <span class="movie-year">{{ year }}</span>
        <a :href="'https://www.imdb.com/title/' + movie.imdb_id" target="_blank" class="imdb-link">IMDB ↗</a>
      </div>

      <!-- Likes count with dynamic color class, and like button -->
      <div class="likes-row">
        <span :class="['likes-count', likesColor]">♥ {{ movie.likes }}</span>
        <button class="like-btn" @click="addLike">Like</button>
      </div>

      <!-- Watched checkbox — two-way via :checked + @change -->
      <label class="watched-label">
        <input type="checkbox" :checked="movie.watched" @change="onWatchedChange($event.target.checked)" />
        {{ movie.watched ? 'Set' : 'Ikke set' }}
      </label>

      <!-- Toggle button text changes based on current state -->
      <p class="comments-hint" @click="toggleComments">
        {{ showComments ? 'Skjul kommentarer ▲' : 'Vis kommentarer ▼' }}
      </p>

      <!-- Comments section — only rendered when showComments is true -->
      <div v-if="showComments" class="comments-section">
        <!-- Nested CommentList component renders the list of comments -->
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

/* Applied when movie.watched is true */
.movie-watched {
  background: #e8f5ee;
  border-color: #2E8B6E;
  box-shadow: 3px 3px 0 #2E8B6E;
}

.movie-poster {
  width: 100%;
  display: block;
}

.movie-poster-placeholder {
  width: 100%;
  aspect-ratio: 2/3;
  background: linear-gradient(135deg, #3a2a18 0%, #1a1008 100%);
  display: flex;
  align-items: center;
  justify-content: center;
  min-height: 300px;
}

.placeholder-content {
  display: flex;
  flex-direction: column;
  align-items: center;
  text-align: center;
  padding: 20px;
  color: #d4c5a9;
}

.placeholder-icon {
  font-size: 3rem;
  margin-bottom: 12px;
  opacity: 0.8;
}

.placeholder-text {
  font-family: 'Georgia', serif;
  font-size: 1rem;
  font-weight: bold;
  color: #fff8ee;
  margin-bottom: 8px;
  line-height: 1.3;
}

.placeholder-year {
  font-size: 0.85rem;
  color: #8b7355;
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


.movie-meta {
  display: flex;
  align-items: center;
  gap: 10px;
  margin-bottom: 10px;
}

.movie-rating {
  font-size: 0.8rem;
  color: #d4a843;
  font-weight: bold;
}

.movie-year {
  font-size: 0.75rem;
  color: #8b7355;
}

.imdb-link {
  font-size: 0.75rem;
  color: #c0392b;
  text-decoration: none;
  letter-spacing: 1px;
  margin-left: auto;
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
/* Color classes determined by likesColor computed property */
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
