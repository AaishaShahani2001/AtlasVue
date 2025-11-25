<template>
  <div class="pagination">
    <button 
      class="pg-btn"
      :disabled="page === 1"
      @click="$emit('change', page - 1)"
    >
      Prev
    </button>

    <span class="page-text">Page {{ page }}</span>

    <button
      class="pg-btn"
      :disabled="page >= totalPages"
      @click="$emit('change', page + 1)"
    >
      Next
    </button>
  </div>
</template>

<script>
export default {
  props: {
    total: Number,
    page: Number,
    perPage: Number
  },
  computed: {
    totalPages() {
      return Math.ceil(this.total / this.perPage);
    }
  }
};
</script>

<style scoped>
.pagination {
  margin-top: 30px;
  display: flex;
  justify-content: center;
  align-items: center;
  gap: 15px;
}

.pg-btn {
  background: #444;
  color: white;
  border: none;
  padding: 8px 14px;
  border-radius: 6px;
  cursor: pointer;
}

.pg-btn:disabled {
  background: #aaa;
  cursor: not-allowed;
}

.page-text {
  font-weight: bold;
}
</style>
