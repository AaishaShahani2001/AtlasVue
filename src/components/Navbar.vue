<template>
  <nav class="nav">
    <div class="nav-left">
      <div class="logo-circle">🌍</div>
      <div class="brand">
        <span class="title">AtlasVue</span>
        <span class="subtitle">Discover countries at a glance</span>
      </div>
    </div>

    <div class="nav-right">
      <button @click="$emit('toggleTheme')" class="theme-btn">
        <span class="icon">
          {{ theme === "light" ? "🌙" : "☀️" }}
        </span>
        <span>{{ theme === "light" ? "Dark mode" : "Light mode" }}</span>
      </button>

      <router-link to="/favourites" class="fav-link">
        <span class="heart">❤️</span>
        <span>Favourites</span>
      </router-link>
      <router-link to="/charts" class="charts-link">
        <span class="chart-icon">📊</span>
        <span>Charts</span>
      </router-link>
    </div>
  </nav>
</template>

<script>
export default {
  props: ["theme"],
};
</script>

<style scoped>
.nav {
  position: sticky;
  top: 0;
  z-index: 20;
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 14px 20px;
  background: var(--nav-bg);
  border-bottom: 1px solid var(--nav-border);
  box-shadow: var(--nav-shadow);
  backdrop-filter: blur(12px);
  flex-wrap: wrap;
  row-gap: 10px;
}

.nav-left {
  display: flex;
  align-items: center;
  gap: 10px;
}

.logo-circle {
  width: 32px;
  height: 32px;
  border-radius: 999px;
  display: flex;
  align-items: center;
  justify-content: center;
  background: radial-gradient(circle at 20% 20%, #22d3ee, #6366f1);
  box-shadow: 0 0 18px rgba(56, 189, 248, 0.55);
}

.brand .title {
  font-weight: 700;
  font-size: 1.05rem;
}

.brand .subtitle {
  display: block;
  font-size: 0.75rem;
  color: var(--muted);
}

.nav-right {
  display: flex;
  align-items: center;
  gap: 10px;
}

.theme-btn {
  display: inline-flex;
  align-items: center;
  gap: 6px;
  padding: 6px 12px;
  border-radius: 999px;
  border: 1px solid var(--input-border);
  background: var(--input-bg);
  color: var(--text);
  cursor: pointer;
  font-size: 0.85rem;
}

.theme-btn .icon {
  font-size: 1rem;
}

.fav-link {
  display: inline-flex;
  align-items: center;
  gap: 6px;
  padding: 6px 14px;
  border-radius: 999px;
  background: var(--accent-soft);
  color: var(--accent);
  font-weight: 600;
  text-decoration: none;
  border: 1px solid transparent;
  transition: transform 0.15s ease, box-shadow 0.15s ease, border-color 0.15s;
}

.fav-link .heart {
  font-size: 1rem;
}

.fav-link:hover {
  transform: translateY(-1px);
  border-color: var(--accent);
  box-shadow: 0 0 18px rgba(99, 102, 241, 0.45);
}

.charts-link {
  display: inline-flex;
  align-items: center;
  gap: 6px;
  padding: 6px 14px;
  border-radius: 999px;
  background: var(--accent-soft);
  color: var(--accent);
  font-weight: 600;
  text-decoration: none;
  border: 1px solid transparent;
  transition: transform 0.15s ease, box-shadow 0.15s ease, border-color 0.15s;
}

.charts-link .chart-icon {
  font-size: 1rem;
}

.charts-link:hover {
  transform: translateY(-1px);
  border-color: var(--accent);
  box-shadow: 0 0 18px rgba(99, 102, 241, 0.45);
}


/* small screens */
@media (max-width: 640px) {
  .nav {
    align-items: flex-start;
  }

  .nav-right {
    width: 100%;
    justify-content: flex-start;
  }
}
</style>
