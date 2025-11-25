<template>
  <div class="container">
    <Hero>
      <SearchBar 
        v-model:modelValue="search"
        :regions="['Africa', 'Americas', 'Asia', 'Europe', 'Oceania']"
        v-model:region="region"
        v-model:sort="sort"
      />
    </Hero>

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
      :perPage="16"
      @change="page = $event"
    />
  </div>
</template>

<script>
import Hero from "../components/Hero.vue";
import CountryCard from "../components/CountryCard.vue";
import SearchBar from "../components/SearchBar.vue";
import Pagination from "../components/Pagination.vue";

export default {
  components: { Hero, CountryCard, SearchBar, Pagination },

  data() {
    return {
      all: [],
      search: "",
      region: "",
      sort: "",
      page: 1,
      favourites: []
    };
  },

  async mounted() {
    try {
      const res = await fetch(
        "https://restcountries.com/v3.1/all?fields=name,flags,cca3,region,subregion,capital,population"
      );
      if (!res.ok) {
        console.error("API failed:", res.status);
        return;
      }
      const data = await res.json();
      if (Array.isArray(data)) this.all = data;
    } catch (err) {
      console.error("API error:", err);
    }

    this.loadFav();
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
      const start = (this.page - 1) * 16;
      return this.filtered.slice(start, start + 16);
    }
  },

  methods: {
    loadFav() {
      this.favourites = JSON.parse(localStorage.getItem("fav")) || [];
    },
    isFav(c) {
      return this.favourites.some(f => f.cca3 === c.cca3);
    },
    toggleFav(country) {
      const exists = this.isFav(country);
      if (exists) {
        this.favourites = this.favourites.filter(c => c.cca3 !== country.cca3);
      } else {
        this.favourites = [...this.favourites, country];
      }
      localStorage.setItem("fav", JSON.stringify(this.favourites));
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

.grid {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(230px, 1fr));
  gap: 20px;
  margin-bottom: 24px;
}

/* a bit tighter on phones */
@media (max-width: 480px) {
  .container {
    padding: 16px;
  }
}
</style>
