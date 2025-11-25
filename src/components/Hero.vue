<template>
  <section class="hero">
    <div class="worldmap"></div>
    <div class="blend-layer"></div>

    <div class="content">
      <h1>Explore Countries Around the World</h1>
      <p>Search, filter and discover insights instantly.</p>

      <div class="search-slot">
        <slot></slot>
      </div>
    </div>
  </section>
</template>

<script>
import worldMap from "@/assets/world_map.jpg";

export default {
  data() {
    return { worldMap };
  }
};
</script>

<style scoped>
.hero {
  position: relative;
  height: 790px;              /* FIXED: No more cropping */
  border-radius: 20px;
  overflow: hidden;
  margin-bottom: 30px;
  background: var(--card-bg);
  box-shadow: var(--shadow-soft);
}

/* Map background */
.worldmap {
  position: absolute;
  inset: 0;
  background-image: url("../assets/world_map.jpg");
  background-size: cover;
  background-position: center;
  background-repeat: no-repeat;

  opacity: var(--map-opacity);
  filter: var(--map-filter);
}

/* Light mode */
.app.light .worldmap {
  --map-opacity: 0.40;
  --map-filter: brightness(1) contrast(1.05) saturate(0.9);
}

/* Dark mode */
.app.dark .worldmap {
  --map-opacity: 0.55;
  --map-filter: brightness(1.6) contrast(1.3) saturate(0.8);
}

/* Blending layer */
.blend-layer {
  position: absolute;
  inset: 0;
  pointer-events: none;

  background: linear-gradient(
    to bottom,
    rgba(255,255,255,0.75) 0%,
    rgba(255,255,255,0.45) 20%,
    rgba(255,255,255,0) 60%
  );

  mix-blend-mode: soft-light;
}

.app.dark .blend-layer {
  background: linear-gradient(
    to bottom,
    rgba(2,6,23,0.85) 0%,
    rgba(2,6,23,0.55) 20%,
    rgba(2,6,23,0.15) 60%
  );

  mix-blend-mode: overlay;
}

/* Hero content */
.content {
  position: relative;
  z-index: 2;
  text-align: center;
  padding-top: 200px;
}

h1 {
  margin: 0;
  font-size: 3rem;
  font-weight: 700;
  color: var(--text);
}

p {
  margin-top: 6px;
  font-size: 1.5rem;
  color: var(--muted);
}

.search-slot {
  margin-top: 80px;
  width: 92%;
  max-width: 900px;
  margin-inline: auto;
}

/* Responsive heights */
@media (max-width: 1024px) {
  .hero {
    height: 650px;
  }
}

@media (max-width: 768px) {
  .hero {
    height: 700px;
  }
}
</style>
