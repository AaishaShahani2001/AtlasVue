<template>
  <footer class="footer">

    <div class="footer-content">

      <!-- LEFT: Navigation Links -->
      <div class="footer-links">
        <router-link to="/">Home</router-link>
        <router-link to="/favourites">Favourites</router-link>
        <router-link to="/charts">Charts</router-link>
      </div>

      <!-- RIGHT: Copyright -->
      <div class="footer-copy">
        © {{ year }} AtlasVue. Built with ❤️ using Vue.js
      </div>

    </div>

  </footer>
</template>

<script>
export default {
  data() {
    return {
      year: new Date().getFullYear()
    };
  }
};
</script>

<style scoped>
.footer {
  margin-top: 40px;
  padding: 18px 25px;
  background: var(--nav-bg);
  border-top: 1px solid var(--nav-border);
  backdrop-filter: blur(12px);
  color: var(--text);
  animation: fadeInUp 0.7s ease forwards;
  opacity: 0;
}

@keyframes fadeInUp {
  from {
    opacity: 0;
    transform: translateY(12px);
  }
  to {
    opacity: 1;
    transform: translateY(0);
  }
}

.footer-content {
  max-width: 1100px;
  margin: auto;

  display: flex;
  justify-content: space-between;
  align-items: center;

  flex-wrap: wrap;
  gap: 12px;
}

/* LEFT SIDE LINKS */
.footer-links {
  display: flex;
  gap: 20px;
}

.footer-links a {
  text-decoration: none;
  font-size: 0.9rem;
  color: var(--text);
  transition: all 0.25s ease;;
}

.footer-links a:hover {
  color: var(--accent);
  text-shadow: 0 0 6px var(--accent);
  transform: translateY(-2px);
}

/* RIGHT SIDE COPYRIGHT */
.footer-copy {
  font-size: 0.9rem;
  color: var(--muted);
}

/* MOBILE */
@media (max-width: 640px) {
  .footer-content {
    flex-direction: column;
    text-align: center;
  }
}
</style>
