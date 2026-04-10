<script>
export default {
  name: '',
  components: {},
  data() {
    return {}
  },
  props: {},
  computed: {},
  methods: {},
  watch: {},
  emits: ['navigate'],
}
</script>

<template>
  <v-app-bar>
    <v-btn @click="$emit('navigate', 'home')">Home</v-btn>
    <v-btn @click="$emit('navigate', 'films')">Film</v-btn>
  </v-app-bar>
</template>

<style scoped></style>
