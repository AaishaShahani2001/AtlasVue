<template>
  <div :class="['app', theme]">
    <Navbar @toggleTheme="toggleTheme" :theme="theme" />
    <router-view />
    <BackToTop />
    <Footer />
  </div>
</template>

<script>
import Navbar from "./components/Navbar.vue";
import Footer from "./components/Footer.vue";
import BackToTop from "./components/BackToTop.vue";

export default {
  components: { Navbar, Footer, BackToTop },
  data() {
    return { theme: "light" };
  },
  created() {
    const saved = localStorage.getItem("ce-theme");
    if (saved === "light" || saved === "dark") {
      this.theme = saved;
    }
  },
  methods: {
    toggleTheme() {
      this.theme = this.theme === "light" ? "dark" : "light";
      localStorage.setItem("ce-theme", this.theme);
    }
  }
};
</script>

<style>
.app {
  min-height: 100vh;
  background: var(--bg);
  color: var(--text);
  transition: background 0.3s ease, color 0.3s ease;
}

/* LIGHT */
.app.light {
  --bg: #f5f5f9;
  --card-bg: #ffffff;
  --nav-bg: #ffffff;
  --nav-border: #e5e7eb;
  --nav-shadow: 0 6px 18px rgba(15, 23, 42, 0.08);
  --accent: #6366f1;
  --accent-soft: rgba(99, 102, 241, 0.08);
  --input-bg: #ffffff;
  --input-border: #d4d4d8;
  --input-placeholder: #9ca3af;
  --text: #111827;
  --muted: #6b7280;
  --shadow-soft: 0 10px 25px rgba(15, 23, 42, 0.08);
}

/* DARK */
.app.dark {
  --bg: #020617;
  --card-bg: #0b1120;
  --nav-bg: rgba(15, 23, 42, 0.96);
  --nav-border: #1e293b;
  --nav-shadow: 0 12px 32px rgba(56, 189, 248, 0.35);
  --accent: #22d3ee;
  --accent-soft: rgba(34, 211, 238, 0.12);
  --input-bg: #020617;
  --input-border: #1f2937;
  --input-placeholder: #64748b;
  --text: #e5e7eb;
  --muted: #9ca3af;
  --shadow-soft: 0 0 25px rgba(56, 189, 248, 0.35);
}
</style>
