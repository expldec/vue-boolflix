<template>
  <AppLoading v-if="loading" />
  <div v-else class="app-list__container container">
    <AppListFilter @clickedSearch="performCombinedSearch($event)" />
    <div class="row row-cols-1 row-cols-md-2 row-cols-xl-3 g-2">
      <AppMovieCard
        v-for="(item, index) in sortedList"
        :key="index"
        :feature="item"
        :genres="genres"
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
      genres: new Map(),
      api_key: "e09aa4b6cedf910831b4920c78e04281",
    };
  },
  methods: {
    performSearch(url, searchTerm, type, destination) {
      axios
        .get(url, {
          params: {
            api_key: this.api_key,
            lang: "en-US",
            page: 1,
            query: searchTerm,
          },
        })
        .then((resp) => {
          resp.data.results.forEach((el) => {
            el.type = type;
            destination.push(el);
          });
        });
    },
    getGenresfromApi(url) {
      axios
        .get(url, {
          params: {
            api_key: this.api_key,
            lang: "en-US",
          },
        })
        .then((resp) => {
          resp.data.genres.forEach((genre) => {
            this.genres.set(genre.id, genre.name);
          });
        });
    },
    performCombinedSearch(searchTerm) {
      this.loading = true;
      this.movieList = [];
      this.performSearch(
        "https://api.themoviedb.org/3/search/movie",
        searchTerm,
        "movie",
        this.movieList
      );
      this.performSearch(
        "https://api.themoviedb.org/3/search/tv",
        searchTerm,
        "tv",
        this.movieList
      );
      setTimeout(() => {
        this.loading = false;
      }, 500);
    },
  },
  computed: {
    sortedList: function () {
      const sortedFeatures = this.movieList.slice(0).sort((a, b) => {
        return b.popularity - a.popularity;
      });
      sortedFeatures.forEach((element) => {
        element.vote_average = "&starf;".repeat(
          Math.ceil(element.vote_average / 2)
        );
        element.truncated_overview =
          element.overview.length < 160
            ? element.overview
            : element.overview.slice(0, 160) + "...";
      });
      return sortedFeatures;
    },
  },
  created() {
    this.getGenresfromApi("https://api.themoviedb.org/3/genre/movie/list");
    this.getGenresfromApi("https://api.themoviedb.org/3/genre/tv/list");
  },
};
</script>

<style lang="scss" scoped>
.app-list__container {
  padding-top: 2rem;
  color: white;
}
</style>
