<script>
export default {
  name: 'CommentList',
  props: {
    comments: {
      type: Array,
      required: true,
    },
  },
}
</script>

<template>
  <ul v-if="comments.length > 0" class="comments-list">
    <li v-for="(comment, index) in comments" :key="index" class="comment-item">
      {{ comment }}
    </li>
  </ul>
  <p v-else class="no-comments">Ingen kommentarer endnu.</p>
</template>

<style scoped>
.comments-list {
  padding-left: 16px;
  margin-bottom: 8px;
}

.comment-item {
  font-size: 0.85rem;
  color: #5a4a35;
  margin-bottom: 4px;
}

.no-comments {
  font-size: 0.8rem;
  color: #aaa;
  margin-bottom: 8px;
}
</style>
