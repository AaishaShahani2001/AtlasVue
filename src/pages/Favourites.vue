<template>
  <div class="container">
    <h2>❤️ Favourite Countries</h2>

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
    return {
      fav: []
    };
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
.container { padding: 30px; }

.grid {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(230px, 1fr));
  gap: 20px;
}

.empty-box {
  text-align: center;
  margin-top: 40px;
  color: #555;
}

.empty-img {
  width: 150px;
  opacity: 0.7;
  margin-bottom: 10px;
}
</style>
