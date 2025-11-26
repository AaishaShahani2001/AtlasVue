<template>
  <div class="search-wrapper">
    <div class="search-section">

      <!-- Search -->
      <div class="field">
        <input
          :value="modelValue"
          @input="$emit('update:modelValue', $event.target.value)"
          type="text"
          placeholder="Search for a country..."
          class="input"
        />
      </div>

      <!-- Region filter -->
      <div class="field">
        <select
          :value="region"
          @change="$emit('update:region', $event.target.value)"
          class="input"
        >
          <option value="">Filter by Region</option>
          <option v-for="r in regions" :key="r" :value="r">
            {{ r }}
          </option>
        </select>
      </div>

      <!-- Sort -->
      <div class="field">
        <select
          :value="sort"
          @change="$emit('update:sort', $event.target.value)"
          class="input"
        >
          <option value="">Sort by Population / Name</option>

          <!-- Population -->
          <option value="asc">Population: Low → High</option>
          <option value="desc">Population: High → Low</option>

          <!-- NEW — A–Z & Z–A -->
          <option value="name-asc">Name: A → Z</option>
          <option value="name-desc">Name: Z → A</option>
        </select>
      </div>

    </div>
  </div>
</template>

<script>
export default {
  props: {
    modelValue: String,
    region: String,
    sort: String,
    regions: Array
  },
  emits: ["update:modelValue", "update:region", "update:sort"]
};
</script>

<style scoped>
.search-wrapper {
  margin-bottom: 24px;
}

.search-section {
  display: flex;
  flex-wrap: wrap;
  align-items: center;
  gap: 20px;
  padding: 18px 22px;
  border-radius: 22px;
  background: var(--card-bg);
  box-shadow: var(--shadow-soft);
}

.field {
  flex: 1;
  min-width: 260px;
}

.input {
  width: 100%;
  padding: 12px 16px;
  border-radius: 16px;
  border: 1px solid var(--input-border);
  background: var(--input-bg);
  color: var(--text);
  font-size: 0.93rem;
  box-sizing: border-box;
}

.input::placeholder {
  color: var(--input-placeholder);
}

/* phones: stack vertically */
@media (max-width: 768px) {
  .search-section {
    flex-direction: column;
    align-items: stretch;
  }

  .field {
    min-width: 100%;
  }
}
</style>
