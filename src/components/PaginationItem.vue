<template>
  <div class="pagination">
    <div
      class="pagination-item"
      v-for="page in pages"
      :key="page.id"
      @click="pageClick(page)"
      :class="{ active: page === pageNumber }"
    >
      <div>
        {{ page }}
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "pagination-item",
  props: {
    users: {
      type: Array,
      required: true,
    },
    pageNumber: {
      type: Number,
      required: true,
    },
    filteredUserSize: {
      type: Array,
    },
  },
  methods: {
    pageClick(page) {
      this.$emit("page-click", page);
    },
  },
  computed: {
    pages() {
      return Math.ceil(this.filteredUserSize.length / 5);
    },
  },
};
</script>


<style scoped>
.pagination {
  padding: 10px 0;
  width: 100%;
  background: #eceff0;
  display: flex;
  justify-content: center;
  border-bottom-left-radius: 9px;
  border-bottom-right-radius: 9px;
}

.pagination-item {
  cursor: pointer;
  text-decoration: none;
  color: #000;
  float: left;
  padding: 8px 16px;
  margin: 0 4px;
  transition: background-color 0.3s;
}

.active {
  background-color: #0cb4f1;
  color: #fff;
}

.pagination-item:hover {
  background-color: #0cb4f1;
  color: #fff;
}
</style>
