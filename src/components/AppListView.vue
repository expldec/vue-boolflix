<template>
  <AppLoading v-if="loading" />
  <div v-else class="app-list__container container">
    <AppListFilter @clickedSearch="performCombinedSearch($event)" />
    <div class="row row-cols-2 row-cols-md-4 g-2">
      <AppMovieCard
        v-for="(item, index) in sortedList"
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
    performSearch(url, searchTerm) {
      axios
        .get(url, {
          params: {
            api_key: "e09aa4b6cedf910831b4920c78e04281",
            lang: "en-US",
            page: 1,
            query: searchTerm,
          },
        })
        .then((resp) => {
          this.movieList = this.movieList.concat(resp.data.results);
        });
    },
    performCombinedSearch(searchTerm) {
      this.loading = true;
      this.performSearch(
        "https://api.themoviedb.org/3/search/movie",
        searchTerm
      );
      this.performSearch("https://api.themoviedb.org/3/search/tv", searchTerm);
      console.log(this.movieList);
      setTimeout(() => {
        this.loading = false;
      }, 500);
    },
  },
  computed: {
    sortedList: function () {
      const sortedFeatures = this.movieList.slice(0).sort((a, b) => {
        return b.vote_average - a.vote_average;
      });
      return sortedFeatures;
    },
  },
  created() {},
};
</script>

<style lang="scss" scoped>
.app-list__container {
  padding-top: 2rem;
  color: white;
}
</style>
