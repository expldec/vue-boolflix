<template>
  <AppLoading v-if="loading" />
  <div v-else class="app-list__container container">
    <AppListFilter @clickedSearch="performSearch($event)" />
    <div class="row row-cols-2 row-cols-md-4 g-2">
      <AppMovieCard
        v-for="(item, index) in movieList"
        :key="index"
        :feature="item"
      />
    </div>
  </div>
</template>

<script>
import AppMovieCard from "./AppMovieCard.vue";
import AppLoading from "./AppLoading.vue";
import AppListFilter from "./AppListFilter.vue";
import axios from "axios";

export default {
  name: "AppListView",
  components: {
    AppMovieCard,
    AppLoading,
    AppListFilter,
  },
  data() {
    return {
      movieList: [],
      loading: false,
      filter: {
        genre: "",
        artist: "",
      },
    };
  },
  methods: {
    performSearch(searchTerm) {
      this.loading = true;
      axios
        .get("https://api.themoviedb.org/3/search/movie", {
          params: {
            api_key: "e09aa4b6cedf910831b4920c78e04281",
            lang: "en-US",
            page: 1,
            query: searchTerm,
          },
        })
        .then((resp) => {
          this.movieList = resp.data.results;
          setTimeout(() => {
            this.loading = false;
          }, 500);
        });
    },
  },
  computed: {},
  created() {},
};
</script>

<style lang="scss" scoped>
.app-list__container {
  padding-top: 2rem;
  color: white;
}
</style>
