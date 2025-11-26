<template>
  <section class="hero">
    <div class="worldmap"></div>
    <div class="blend-layer"></div>

    <!-- ✈️ Animated Plane -->
    <div class="plane"></div>

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
  height: 590px;              
  overflow: hidden;
  margin-bottom: 30px;
  background: var(--card-bg);
  box-shadow: var(--shadow-soft);
}

/* Map background */
.worldmap {
  position: absolute;
  inset: 0;
  background-image: url("../assets/map.avif");
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

/* ✈️ PLANE ANIMATION LAYER */
.plane {
  position: absolute;
  top: 60%;
  left: -150px; /* Start off-screen */
  width: 120px;
  height: 120px;
  background-image: url("data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAEAAAABACAMAAACdt4HsAAAAG1BMVEVHcEyZmZn///+Zmf+ZmZl2dnZ6enoh4Y7Z2dnt7e0IrT3cAAAACXBIWXMAAAsTAAALEwEAmpwYAAABgUlEQVRoge2W227DMAyFj7kAGoAfXnf+TfppcI0l+PazTA/gkGEXUXMaLLq5yRvCNrI6VsgGAaHo9dZYkTfGZNVVRFlEAVYyqS/fWznuaCG2lKT9OAXoJ2Bs8LJIYur2jcWcA6i/K3PaY5E9O+V1YQUAECEV4VpWw2gw2gYdExgkt1aQAFjlGIIKgJESyqCCpt5TRTF+t0NenE2no0RvrKeUjGJPD7W82dManIeZDV4SSQSSHqzTeWYIAvzkdxlIoAgNGdisz8Iky3Uczdlz7YT1DoP70uQgmO6ijLJMEVN6aCMGN28AL5soMgqd7qV3CyMfCVx/gvBy06SnVAk0nnBYnOVrlaRykGGBqBPdZiZsaAJ+lZeI7IuAvV38DSHLVQQRIlnSRrped1IovnHgwlHGawEq+y3OCAXoTr4Wr9PXgCulvRlQCLOwBEmcLkAMPx/+qvOB0fOkH4An1xd6QbaO5wAAAABJRU5ErkJggg==");
  background-size: contain;
  background-repeat: no-repeat;
  z-index: 1; /* Above map, but below text */

  animation: flyPlane 18s linear infinite;
  opacity: 0.85;
}

/* ✈️ FLIGHT PATH (smooth diagonal) */
@keyframes flyPlane {
  0% {
    transform: translateX(0px) translateY(0px) rotate(-5deg);
    opacity: 0;
  }
  10% {
    opacity: 1;
  }
  50% {
    transform: translateX(800px) translateY(-120px) rotate(8deg);
    opacity: 1;
  }
  90% {
    opacity: 1;
  }
  100% {
    transform: translateX(1500px) translateY(-200px) rotate(15deg);
    opacity: 0;
  }
}

</style>
