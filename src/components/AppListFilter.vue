<template>
  <div class="row row-cols-1 row-cols-md-2 g-2 justify-content-center">
    <div class="d-flex mb-3">
      <input
        type="text"
        class="form-control"
        id="search-text"
        placeholder="Search Movies or TV Shows"
        v-model="searchText"
        @keyup.enter="$emit('clickedSearch', searchText)"
      />
      <button
        type="button"
        class="btn btn-primary ms-2"
        @click="$emit('clickedSearch', searchText)"
      >
        Search
      </button>
    </div>
  </div>
</template>

<script>
export default {
  name: "AppListFilter",
  data: function () {
    return {
      searchText: "",
    };
  },
  methods: {},
  props: {},
};
</script>

<style></style>
