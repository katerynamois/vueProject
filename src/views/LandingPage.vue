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
  emits: [],
}
</script>

<template>
  <v-app>
    <v-app-bar>
      <v-btn @click="$emit('navigate', 'home')">Home</v-btn>
      <v-btn @click="$emit('navigate', 'films')">Film</v-btn>
    </v-app-bar>
    <v-main>
      <v-container>
        <h1>Velkommen til Film App</h1>
        <p>Oplev en verden af film og anmeldelser!</p>
      </v-container>
    </v-main>
  </v-app>
</template>

<style scoped></style>
