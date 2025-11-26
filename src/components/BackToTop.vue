<template>
  <button 
    v-show="visible"
    class="back-to-top"
    @click="scrollToTop"
  >
    ⬆
  </button>
</template>

<script>
export default {
  data() {
    return { visible: false };
  },

  mounted() {
    window.addEventListener("scroll", this.checkScroll);
  },
  beforeUnmount() {
    window.removeEventListener("scroll", this.checkScroll);
  },

  methods: {
    checkScroll() {
      this.visible = window.scrollY > 300;
    },
    scrollToTop() {
      window.scrollTo({ top: 0, behavior: "smooth" });
    }
  }
};
</script>

<style scoped>
.back-to-top {
  position: fixed;
  right: 22px;
  bottom: 22px;
  width: 48px;
  height: 48px;
  border-radius: 50%;
  border: none;
  cursor: pointer;
  font-size: 1.3rem;
  background: var(--card-bg);
  color: var(--text);
  box-shadow: var(--shadow-soft);
  transition: all 0.25s ease;
  z-index: 999;
}

.back-to-top:hover {
  transform: translateY(-3px);
  box-shadow: 0 6px 16px rgba(0, 0, 0, 0.25);
}
</style>
