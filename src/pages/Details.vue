<template>
  <div class="details">
    <button @click="$router.back()" class="back">← Back</button>

    <div v-if="country" class="info-box">
      <img :src="country.flags.png" />

      <div>
        <h2>{{ country.name.common }}</h2>
        <p><b>Capital:</b> {{ country.capital?.[0] }}</p>
        <p><b>Population:</b> {{ country.population.toLocaleString() }}</p>
        <p><b>Region:</b> {{ country.region }}</p>
        <p><b>Subregion:</b> {{ country.subregion }}</p>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return { country: null };
  },
  async mounted() {
    const code = this.$route.params.code;
    const res = await fetch(`https://restcountries.com/v3.1/alpha/${code}`);
    this.country = (await res.json())[0];
  }
};
</script>

<style scoped>
.details { padding: 30px; }
.back { padding: 8px 14px; border-radius: 8px; }
.info-box {
  display: flex;
  gap: 30px;
  margin-top: 20px;
}
img {
  width: 300px;
  border-radius: 12px;
}
</style>
