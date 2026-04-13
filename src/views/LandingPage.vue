<script>
export default {
  name: 'LandingPage',
  emits: ['navigate'],
}
</script>

<template>
  <section class="landing-page">
    <v-container class="hero-section text-center py-12">
      <div class="badge-label mb-4">— NOW PRESENTING —</div>
      <h1 class="hero-title">The Greatest<br />Films Ever Made</h1>
      <p class="hero-sub mt-4">
        En håndplukket samling af tidløse klassikere,<br />
        kurateret med kærlighed til det store lærred.
      </p>

      <div class="mt-8">
        <v-btn class="ticket-btn" size="x-large" @click="$emit('navigate', 'films')">
          SE ALLE FILM
        </v-btn>
      </div>
    </v-container>

  </section>
</template>

<style scoped>
.landing-page {
  background-color: #f5f0e8;
}

.badge-label {
  font-size: clamp(0.55rem, 1.5vw, 0.65rem);
  letter-spacing: 5px;
  color: #8b7355;
  font-family: 'Georgia', serif;
}

.hero-title {
  font-family: 'Georgia', serif;
  font-size: clamp(1.8rem, 7vw, 4rem);
  font-weight: bold;
  color: #1a1008;
  line-height: 1.1;
  text-transform: uppercase;
  letter-spacing: 2px;
  display: inline-block;
}
.hero-title::after {
  content: '';
  display: block;
  width: 60%;
  height: 3px;
  background: #c0392b;
  margin: 12px auto 0;
}

.hero-sub {
  font-size: clamp(0.85rem, 2vw, 1rem);
  color: #5a4a35;
  line-height: 1.7;
  padding: 0 1rem;
}

.ticket-btn {
  background: #c0392b !important;
  color: #f5f0e8 !important;
  font-size: clamp(0.65rem, 1.5vw, 0.75rem) !important;
  letter-spacing: 4px !important;
  font-family: 'Georgia', serif !important;
  border: 2px dashed #d4a843 !important;
  border-radius: 2px !important;
  padding: 0 1.5rem !important;
  height: 52px !important;
  box-shadow: 4px 4px 0 #1a1008 !important;
  transition:
    transform 0.1s,
    box-shadow 0.1s !important;
  white-space: normal !important;
  word-break: break-word;
}
.ticket-btn:hover {
  transform: translate(-2px, -2px) !important;
  box-shadow: 6px 6px 0 #1a1008 !important;
}
.ticket-btn:active {
  transform: translate(2px, 2px) !important;
  box-shadow: 2px 2px 0 #1a1008 !important;
}
.ticket-btn::before {
  content: '🎟';
  margin-right: 10px;
}
.ticket-btn::after {
  content: '→';
  margin-left: 10px;
}

@media (max-width: 600px) {
  .hero-sub br {
    display: none;
  }
}
</style>
