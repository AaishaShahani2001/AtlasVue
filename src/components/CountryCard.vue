<template>
  <div class="card" @click="goToDetails">
    <div class="heart" @click.stop="toggleFav">
      <span v-if="isFavourite">❤️</span>
      <span v-else>🤍</span>
    </div>

    <img :src="country.flags.png" />
    <h3>{{ country.name.common }}</h3>

    <p>Population: {{ country.population.toLocaleString() }}</p>
    <p>Region: {{ country.region }}</p>
  </div>
</template>

<script>
export default {
  props: ["country", "isFavourite"],

  methods: {
    goToDetails() {
      this.$router.push(`/country/${this.country.cca3}`);
    },

    toggleFav() {
      this.$emit("toggle", this.country);
    }
  }
};
</script>

<style scoped>
.card {
  background: white;
  padding: 15px;
  border-radius: 12px;
  box-shadow: 0 3px 10px rgba(0,0,0,0.1);
  cursor: pointer;
  transition: 0.2s;
  position: relative;
}

.card:hover {
  transform: scale(1.03);
}

.heart {
  position: absolute;
  top: 10px;
  right: 12px;
  font-size: 22px;
  cursor: pointer;
}

img {
  width: 100%;
  height: 140px;
  object-fit: cover;
  border-radius: 10px;
}
</style>
