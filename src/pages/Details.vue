<template>
  <div class="details">

    <button @click="$router.back()" class="back">← Back</button>

    <div v-if="country" class="info-box">
      <img :src="country.flags.png" />

      <div class="text">
        <h2>{{ country.name.common }}</h2>

        <p><b>Capital:</b> {{ country.capital?.[0] }}</p>
        <p><b>Population:</b> {{ country.population.toLocaleString() }}</p>
        <p><b>Region:</b> {{ country.region }}</p>
        <p><b>Subregion:</b> {{ country.subregion }}</p>
        <p><b>Languages:</b> {{ Object.values(country.languages || {}).join(", ") }}</p>
        <p><b>Currencies:</b> {{ Object.values(country.currencies || {}).map(c => c.name).join(", ") }}</p>
        <p><b>Area:</b> {{ country.area.toLocaleString() }} km²</p>
      </div>
    </div>

    <div v-if="country" class="map-section">
      <h3>Geographical Position</h3>
      <div ref="geoMap" class="geo-map"></div>
    </div>

  </div>
</template>

<script>
import * as echarts from "echarts";

export default {
  data() {
    return { country: null };
  },

  async mounted() {
    const code = this.$route.params.code;

    const res = await fetch(`https://restcountries.com/v3.1/alpha/${code}`);
    const data = await res.json();
    this.country = data[0];

    // LOAD WORLD MAP GEOJSON SAFELY
    const worldGeo = await fetch(
      "https://fastly.jsdelivr.net/npm/echarts@latest/map/json/world.json"
    ).then(res => res.json());

    echarts.registerMap("world", worldGeo);

    this.$nextTick(() => {
      this.initGeoMap();
    });
  },

  methods: {
    initGeoMap() {
      if (!this.country?.latlng) return;

      const map = echarts.init(this.$refs.geoMap);
      const [lat, lng] = this.country.latlng;

      map.setOption({
        backgroundColor: "transparent",

        geo: {
          map: "world",
          roam: true,
          zoom: 2.2,
          center: [lng, lat],
          itemStyle: {
            areaColor: "#d4d4d8",
            borderColor: "#71717a"
          },
          emphasis: {
            itemStyle: {
              areaColor: "#6366f1"
            }
          }
        },

        series: [
          {
            type: "scatter",
            coordinateSystem: "geo",
            symbol: "circle",
            symbolSize: 12,
            itemStyle: {
              color: "#ef4444",
              shadowBlur: 15,
              shadowColor: "#ef4444"
            },
            data: [
              {
                name: this.country.name.common,
                value: [lng, lat]
              }
            ]
          }
        ]
      });

      window.addEventListener("resize", () => map.resize());
    }
  }
};
</script>

<style scoped>
.details {
  padding: 30px;
}

.back {
  padding: 8px 14px;
  border: none;
  border-radius: 8px;
  background: var(--card-bg);
  color: var(--text);
  box-shadow: var(--shadow-soft);
  cursor: pointer;
  font-size: 0.9rem;
  margin-bottom: 16px;
  transition: all 0.2s ease;
}

.back:hover {
  transform: translateY(-2px);
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.12);
}


.info-box {
  display: flex;
  gap: 30px;
  margin-top: 20px;
  flex-wrap: wrap;
}

img {
  width: 300px;
  border-radius: 12px;
  box-shadow: var(--shadow-soft);
}

.map-section {
  margin-top: 40px;
}

.geo-map {
  width: 100%;
  height: 300px; /* Mobile default */
  border-radius: 14px;
  box-shadow: var(--shadow-soft);
  margin-top: 10px;
}

/* Tablets (min-width: 600px) */
@media (min-width: 600px) {
  .geo-map {
    height: 400px;
  }
}

/* Laptops (min-width: 900px) */
@media (min-width: 900px) {
  .geo-map {
    height: 500px;
  }
}

/* Large desktops (min-width: 1200px) */
@media (min-width: 1200px) {
  .geo-map {
    height: 600px;
  }
}

</style>
