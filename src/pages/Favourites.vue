<template>
  <div class="container">
    <button @click="$router.back()" class="back">← Back</button>

    <h2 class="title">❤️ Favourite Countries</h2>

    <div v-if="fav.length === 0" class="empty-box">
      <img 
        src="https://cdn-icons-png.flaticon.com/512/4076/4076632.png" 
        class="empty-img"
      />
      <p>No favourite countries yet...</p>
    </div>

    <div v-else class="grid">
      <CountryCard 
        v-for="c in fav" 
        :key="c.cca3"
        :country="c"
        :isFavourite="true"
        @toggle="toggleFav"
      />
    </div>
  </div>
</template>

<script>
import CountryCard from "../components/CountryCard.vue";

export default {
  components: { CountryCard },
  data() {
    return { fav: [] };
  },
  mounted() {
    this.loadFav();
  },
  methods: {
    loadFav() {
      this.fav = JSON.parse(localStorage.getItem("fav")) || [];
    },
    toggleFav(country) {
      this.fav = this.fav.filter(c => c.cca3 !== country.cca3);
      localStorage.setItem("fav", JSON.stringify(this.fav));
      this.loadFav();
    }
  }
};
</script>

<style scoped>
.container { 
  padding: 24px; 
  max-width: 1200px;
  margin: 0 auto;
}

.back {
  padding: 8px 14px;
  border-radius: 8px;
  border: 1px solid var(--input-border);
  background: var(--input-bg);
  color: var(--text);
  cursor: pointer;
  margin-bottom: 18px;
  font-size: 0.9rem;
}

.back:hover {
  background: var(--accent-soft);
}

.title {
  margin-bottom: 18px;
}

.grid {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(230px, 1fr));
  gap: 20px;
}

.empty-box {
  text-align: center;
  margin-top: 40px;
  color: var(--muted);
}

.empty-img {
  width: 150px;
  opacity: 0.7;
  margin-bottom: 10px;
}

/* phones */
@media (max-width: 480px) {
  .container {
    padding: 16px;
  }
}
</style>
