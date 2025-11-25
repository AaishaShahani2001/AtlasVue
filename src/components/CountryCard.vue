<template>
  <div class="card" @click="goToDetails">
    <button class="heart" @click.stop="toggleFav">
      <span :class="{ active: isFavourite }">❤</span>
    </button>

    <img :src="country.flags.png" alt="Flag" />
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
  background: var(--card-bg);
  padding: 16px;
  border-radius: 18px;
  box-shadow: var(--shadow-soft);
  cursor: pointer;
  transition: transform 0.18s ease, box-shadow 0.18s ease;
  position: relative;
  color: var(--text);
}

.card:hover {
  transform: translateY(-4px);
  box-shadow: 0 14px 32px rgba(15, 23, 42, 0.22);
}

.heart {
  position: absolute;
  top: 10px;
  right: 12px;
  border: none;
  background: transparent;
  cursor: pointer;
  padding: 0;
}

.heart span {
  font-size: 1.3rem;
  color: var(--muted);
  transition: transform 0.15s ease, color 0.15s ease, text-shadow 0.15s ease;
}

.heart span.active {
  color: #ef4444;
  text-shadow: 0 0 14px rgba(239, 68, 68, 0.8);
  transform: scale(1.1);
}

img {
  width: 100%;
  height: 140px;
  object-fit: cover;
  border-radius: 14px;
  margin-bottom: 10px;
}

h3 {
  margin: 6px 0;
  font-size: 1rem;
}

p {
  margin: 2px 0;
  font-size: 0.85rem;
  color: var(--muted);
}

/* small phones: make image a bit shorter */
@media (max-width: 480px) {
  img {
    height: 120px;
  }
}
</style>
