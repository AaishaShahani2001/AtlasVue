<template>
  <div class="search-section">
    <!-- Search -->
    <input
      :value="modelValue"
      @input="$emit('update:modelValue', $event.target.value)"
      type="text"
      placeholder="Search for a country..."
      class="search-input"
    />

    <!-- Region Filter -->
    <select
      :value="region"
      @change="$emit('update:region', $event.target.value)"
      class="dropdown"
    >
      <option value="">Filter by Region</option>
      <option v-for="r in regions" :key="r" :value="r">
        {{ r }}
      </option>
    </select>

    <!-- Sort -->
    <select
      :value="sort"
      @change="$emit('update:sort', $event.target.value)"
      class="dropdown"
    >
      <option value="">Sort by Population</option>
      <option value="asc">Low → High</option>
      <option value="desc">High → Low</option>
    </select>
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
.search-section {
  display: flex;
  gap: 15px;
  margin-bottom: 25px;
  flex-wrap: wrap;
}

.search-input {
  padding: 10px 15px;
  width: 250px;
  border-radius: 8px;
  border: 1px solid #ccc;
}

.dropdown {
  padding: 10px;
  border-radius: 8px;
  border: 1px solid #ccc;
}
</style>
