<template>
  <div>
    <div class="row row-cols-1 g-2 justify-content-center">
      <div class="app-list__search col d-flex mb-3">
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
    <div class="row row-cols-1 g-2 justify-content-center">
      <div
        class="app-list__result-filter col d-flex mb-4"
        v-if="genres.size > 0"
      >
        <label for="genre-select">Filter by Genre</label>
        <select
          class="form-select d-flex"
          aria-label="Genre select"
          id="genre-select"
          v-model="genreSelect"
          @change="$emit('pickedGenre', genreSelect)"
        >
          <option selected value>All genres</option>
          <option v-for="(item, index) in genres" :key="index" :value="item[0]">
            {{ item[1] }}
          </option>
        </select>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "AppListFilter",
  data: function () {
    return {
      searchText: "",
      genreSelect: "",
    };
  },
  methods: {},
  props: {
    genres: Map,
  },
};
</script>

<style lang="scss" scoped>
.app-list__search {
  width: 40vw;
}
.app-list__result-filter {
  width: 40vw;
}
</style>
