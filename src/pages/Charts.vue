<template>
  <div class="charts-container">

    <!-- Back Button -->
    <button class="back-btn" @click="$router.back()">← Back</button>

    <h1 class="title">World Region Statistics</h1>

    <!-- Region Selector -->
    <select v-model="selectedRegion" @change="updateCharts" class="selector">
      <option value="">All Regions</option>
      <option v-for="r in regions" :key="r" :value="r">{{ r }}</option>
    </select>

    <!-- Chart Grid -->
    <div class="charts-grid">
      <div ref="pieChart" class="chart-box"></div>
      <div ref="barChart" class="chart-box"></div>
      <div ref="lineChart" class="chart-box"></div>
    </div>

  </div>
</template>

<script>
import * as echarts from "echarts";

export default {
  data() {
    return {
      allCountries: [],
      selectedRegion: "",
      regions: ["Africa", "Americas", "Asia", "Europe", "Oceania"],

      // Region Colors
      regionColors: {
        Africa: "#f97316",     // Orange
        Americas: "#10b981",   // Green
        Asia: "#3b82f6",       // Blue
        Europe: "#8b5cf6",     // Purple
        Oceania: "#eab308"     // Yellow
      },

      pieInstance: null,
      barInstance: null,
      lineInstance: null,
    };
  },

  async mounted() {
    // Lightweight API (fast)
    const res = await fetch(
      "https://restcountries.com/v3.1/all?fields=name,region,population,area"
    );
    const data = await res.json();
    this.allCountries = data;

    this.initCharts();
  },

  methods: {
    initCharts() {
      this.pieInstance = echarts.init(this.$refs.pieChart);
      this.barInstance = echarts.init(this.$refs.barChart);
      this.lineInstance = echarts.init(this.$refs.lineChart);

      this.updateCharts();
    },

    updateCharts() {
  const filtered = this.selectedRegion
    ? this.allCountries.filter(c => c.region === this.selectedRegion)
    : this.allCountries;

  const activeRegions = this.selectedRegion
    ? [this.selectedRegion]
    : this.regions;

  /* ------------------ PIE CHART (Population) ------------------ */
  const pieData = activeRegions.map(r => ({
    name: r,
    value: this.allCountries
      .filter(c => c.region === r)
      .reduce((sum, c) => sum + c.population, 0),
    itemStyle: { color: this.regionColors[r] }
  }));

  this.pieInstance.setOption({
    title: { text: "Population by Region", left: "center" },
    tooltip: {
      trigger: "item",
      formatter: params => {
        return `
          <b>${params.name}</b><br>
          Population: ${params.value.toLocaleString()}
        `;
      }
    },
    series: [
      {
        type: "pie",
        radius: "60%",
        data: pieData,
        animationDuration: 1200
      }
    ]
  });

  /* ------------------ BAR CHART (Country Count) ------------------ */
  this.barInstance.setOption({
    title: { text: "Countries per Region", left: "center" },
    tooltip: {
      trigger: "item",
      formatter: params => {
        return `
          <b>${params.name}</b><br>
          Countries: ${params.value}
        `;
      }
    },
    xAxis: { type: "category", data: activeRegions },
    yAxis: { type: "value" },
    series: [
      {
        type: "bar",
        data: activeRegions.map(r => ({
          value: this.allCountries.filter(c => c.region === r).length,
          itemStyle: { color: this.regionColors[r] }
        })),
        animationDuration: 1400
      }
    ]
  });

  /* ------------------ LINE CHART (Total Area) ------------------ */
  this.lineInstance.setOption({
    title: { text: "Total Area per Region", left: "center" },
    tooltip: {
      trigger: "item",
      formatter: params => {
        return `
          <b>${params.name}</b><br>
          Area: ${params.value.toLocaleString()} km²
        `;
      }
    },
    xAxis: { type: "category", data: activeRegions },
    yAxis: { type: "value" },
    series: [
      {
        type: "line",
        smooth: true,
        animationDuration: 1500,
        data: activeRegions.map(r => ({
          value: this.allCountries
            .filter(c => c.region === r)
            .reduce((sum, c) => sum + c.area, 0),
          itemStyle: { color: this.regionColors[r] }
        })),
        lineStyle: {
          width: 3,
          color: this.selectedRegion
            ? this.regionColors[this.selectedRegion]
            : "#6366f1"
        },
        symbol: "circle"
      }
    ]
  });
}

  }
};
</script>

<style scoped>
.charts-container {
  max-width: 1100px;
  margin: auto;
  padding: 30px;
}

.title {
  font-size: 32px;
  text-align: center;
  margin-bottom: 20px;
}

.selector {
  display: block;
  margin: 0 auto 20px;
  padding: 10px;
  font-size: 16px;
}

/* GRID */
.charts-grid {
  display: grid;
  grid-template-columns: 1fr;
  gap: 30px;
}

.chart-box {
  width: 100%;
  height: 350px;
  border-radius: 12px;
  box-shadow: 0 4px 10px #0001;
}

/* BIG SCREENS */
@media (min-width: 900px) {
  .charts-grid {
    grid-template-columns: repeat(3, 1fr);
  }
}

/* Back Button */
.back-btn {
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

.back-btn:hover {
  transform: translateY(-2px);
  box-shadow: 0 4px 12px rgba(0,0,0,0.1);
}
</style>
