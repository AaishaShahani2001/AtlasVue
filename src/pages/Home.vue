<template>
  <div class="container">
    <SearchBar 
      v-model:modelValue="search"
      :regions="['Africa', 'Americas', 'Asia', 'Europe', 'Oceania']"
      v-model:region="region"
      v-model:sort="sort"
    />

    <div class="grid">
      <CountryCard 
        v-for="c in paginated" 
        :key="c.cca3"
        :country="c"
        :isFavourite="isFav(c)"
        @toggle="toggleFav"
      />
    </div>

    <Pagination 
      :total="filtered.length"
      :page="page"
      :perPage="15"
      @change="page = $event"
    />
  </div>
</template>

<script>
import CountryCard from "../components/CountryCard.vue";
import SearchBar from "../components/SearchBar.vue";
import Pagination from "../components/Pagination.vue";

export default {
  components: { CountryCard, SearchBar, Pagination },

  data() {
    return {
      all: [],
      search: "",
      region: "",
      sort: "",
      page: 1,
    };
  },

  async mounted() {
    try {
      const res = await fetch(
        "https://restcountries.com/v3.1/all?fields=name,flags,cca3,region,subregion,capital,population"
      );

      if (!res.ok) return console.error("API failed:", res.status);

      const data = await res.json();

      if (Array.isArray(data)) {
        this.all = data;
        console.log("Countries loaded:", data.length);
      }
    } catch (err) {
      console.error("API error:", err);
    }
  },

  computed: {
    filtered() {
      if (!Array.isArray(this.all)) return [];

      let list = [...this.all];

      if (this.search)
        list = list.filter(c =>
          c.name.common.toLowerCase().includes(this.search.toLowerCase())
        );

      if (this.region)
        list = list.filter(c => c.region === this.region);

      if (this.sort === "asc")
        list.sort((a, b) => a.population - b.population);

      if (this.sort === "desc")
        list.sort((a, b) => b.population - a.population);

      return list;
    },

    paginated() {
      const start = (this.page - 1) * 15;
      return this.filtered.slice(start, start + 15);
    }
  },

  methods: {
    isFav(c) {
      const fav = JSON.parse(localStorage.getItem("fav")) || [];
      return fav.some(f => f.cca3 === c.cca3);
    },

    toggleFav(country) {
      let fav = JSON.parse(localStorage.getItem("fav")) || [];

      const exists = fav.some(c => c.cca3 === country.cca3);

      if (exists) {
        fav = fav.filter(c => c.cca3 !== country.cca3);
      } else {
        fav.push(country);
      }

      localStorage.setItem("fav", JSON.stringify(fav));
    }
  }
};
</script>

<style scoped>
.container {
  padding: 30px;
}
.grid {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(230px, 1fr));
  gap: 20px;
}
</style>
