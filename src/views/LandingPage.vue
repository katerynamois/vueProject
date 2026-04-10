<script>
export default {
  name: 'LandingPage',
  emits: ['navigate'],
  data() {
    return {
      features: [
        { icon: '🎞️', title: 'Klassiske Film', desc: 'Håndplukket udvalg af tidløse mesterværker fra det 20. århundrede.' },
        { icon: '🎟️', title: 'Nem Booking',    desc: 'Book din billet på under et minut — ingen konto nødvendig.' },
        { icon: '🎬', title: 'Retro Oplevelse', desc: 'Nyd film som de blev skabt til — på det store lærred.' },
      ],
    }
  },
}
</script>

<template>
  <div class="landing-page">

    <div class="film-strip top-strip">
      <span v-for="n in 12" :key="n" class="strip-hole"></span>
    </div>

    <v-container class="hero-section text-center py-12">
      <div class="badge-label mb-4">— NOW PRESENTING —</div>
      <h1 class="hero-title">The Greatest<br>Films Ever Made</h1>
      <p class="hero-sub mt-4">
        En håndplukket samling af tidløse klassikere,<br>
        kurateret med kærlighed til det store lærred.
      </p>

      <div class="mt-8">
        <v-btn class="ticket-btn" size="x-large" @click="$emit('navigate', 'films')">
          <span class="ticket-left">🎟</span>
          SE ALLE FILM
          <span class="ticket-right">→</span>
        </v-btn>
      </div>
    </v-container>

    <v-container class="pb-12">
      <v-row justify="center">
        <v-col cols="12" sm="4" v-for="feat in features" :key="feat.title">
          <div class="feature-card">
            <div class="feature-icon">{{ feat.icon }}</div>
            <div class="feature-title">{{ feat.title }}</div>
            <div class="feature-desc">{{ feat.desc }}</div>
          </div>
        </v-col>
      </v-row>
    </v-container>

    <div class="film-strip bottom-strip">
      <span v-for="n in 12" :key="n" class="strip-hole"></span>
    </div>

  </div>
</template>

<style scoped>
.landing-page {
  background-color: #F5F0E8;
}

.film-strip {
  display: flex;
  background: #1A1008;
  padding: 6px 0;
  overflow: hidden;
}
.top-strip    { border-bottom: 2px solid #C0392B; }
.bottom-strip { border-top: 2px solid #C0392B; }
.strip-hole {
  flex: 1;
  height: 22px;
  margin: 4px 6px;
  background: #F5F0E8;
  border-radius: 3px;
  opacity: 0.85;
}

.badge-label {
  font-size: 0.65rem;
  letter-spacing: 5px;
  color: #8B7355;
  font-family: 'Georgia', serif;
}

.hero-title {
  font-family: 'Georgia', serif;
  font-size: clamp(2.2rem, 6vw, 4rem);
  font-weight: bold;
  color: #1A1008;
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
  background: #C0392B;
  margin: 12px auto 0;
}

.hero-sub {
  font-size: 1rem;
  color: #5a4a35;
  line-height: 1.7;
}

.ticket-btn {
  background: #C0392B !important;
  color: #F5F0E8 !important;
  font-size: 0.75rem !important;
  letter-spacing: 4px !important;
  font-family: 'Georgia', serif !important;
  border: 2px dashed #D4A843 !important;
  border-radius: 2px !important;
  padding: 0 2rem !important;
  height: 52px !important;
  box-shadow: 4px 4px 0 #1A1008 !important;
  transition: transform 0.1s, box-shadow 0.1s !important;
}
.ticket-btn:hover {
  transform: translate(-2px, -2px) !important;
  box-shadow: 6px 6px 0 #1A1008 !important;
}
.ticket-btn:active {
  transform: translate(2px, 2px) !important;
  box-shadow: 2px 2px 0 #1A1008 !important;
}
.ticket-left  { margin-right: 10px; }
.ticket-right { margin-left: 10px; }

.feature-card {
  background: #fff8ee;
  border: 1.5px solid #8B7355;
  border-radius: 2px;
  padding: 1.5rem;
  text-align: center;
  box-shadow: 3px 3px 0 #8B7355;
  height: 100%;
}
.feature-icon  { font-size: 2rem; margin-bottom: 0.75rem; }
.feature-title {
  font-size: 0.75rem;
  letter-spacing: 3px;
  color: #C0392B;
  font-weight: bold;
  margin-bottom: 0.5rem;
  text-transform: uppercase;
}
.feature-desc  { font-size: 0.9rem; color: #5a4a35; line-height: 1.6; }
</style>
