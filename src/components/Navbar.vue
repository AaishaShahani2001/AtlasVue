<template>
  <nav class="nav">
    <h2>🌍 Country Explorer</h2>

    <button @click="$emit('toggleTheme')" class="theme-btn">
      {{ theme === 'light' ? 'Dark Mode' : 'Light Mode' }}
    </button>

    <router-link to="/favourites" class="fav-link">❤️ Favourites</router-link>
  </nav>
</template>

<script>
export default {
  props: ["theme"],
};
</script>

<style scoped>
.nav {
  display: flex;
  justify-content: space-between;
  padding: 18px;
  background: white;
  align-items: center;
  border-bottom: 1px solid #ddd;
}

.theme-btn {
  background: #eee;
  border: none;
  padding: 8px 12px;
  border-radius: 8px;
  cursor: pointer;
}

.fav-link {
  margin-left: 20px;
  text-decoration: none;
  font-weight: bold;
}
</style>
