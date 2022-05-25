<template>
  <AppLoading v-if="loading" />
  <div v-else class="app-list__container container">
    <AppListFilter
      @clickedSearch="clickedSearch($event)"
      @pickedGenre="updateFilter('genre', $event)"
      :genres="genresInList"
    />
    <div class="row row-cols-1 row-cols-md-2 row-cols-xl-3 g-2">
      <AppMovieCard
        v-for="(item, index) in enrichedList"
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
      filter: {
        genre: "",
      },
      genres: new Map(),
      casts: new Map(),
      api_key: "e09aa4b6cedf910831b4920c78e04281",
    };
  },
  methods: {
    updateFilter(key, filter) {
      this.filter[key] = filter;
    },
    getGenresfromApi(url) {
      return axios
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
    castSearch(type, id) {
      return axios.get(`https://api.themoviedb.org/3/${type}/${id}/credits`, {
        params: {
          api_key: this.api_key,
          lang: "en-US",
        },
      });
    },
    titleSearch(url, searchTerm) {
      return axios.get(url, {
        params: {
          api_key: this.api_key,
          lang: "en-US",
          page: 1,
          query: searchTerm,
        },
      });
    },
    clickedSearch(searchTerm) {
      if (searchTerm.length > 0) {
        this.performCombinedSearch(searchTerm);
      }
    },
    performCombinedSearch(searchTerm) {
      this.loading = true;
      this.movieList = [];
      const castRequests = [];

      const movieReq = this.titleSearch(
        "https://api.themoviedb.org/3/search/movie",
        searchTerm,
        "movie",
        this.movieList
      );
      const tvReq = this.titleSearch(
        "https://api.themoviedb.org/3/search/tv",
        searchTerm,
        "tv",
        this.movieList
      );
      axios
        .all([movieReq, tvReq])
        // cerco di spiegare cosa fa il seguente codice:
        // il .all() qui sopra restituisce un array con 2 oggetti, ciascuno è una risposta di axios alla rispettiva request
        // come argomenti della arrow function del .then, potremmo scrivere semplicemente (resp) e vedremmo questo array con due oggetti.
        // per farlo, usiamo l'Object destructuring per ottenere i due array:
        // .then(([{ data: movieResp }, { data: tvResp }]) => {
        //   console.log(movieResp);
        //   console.log(tvResp);
        // });
        //
        // il codice qui sopra rende disponibile, dentro l'arrow function, due variabili "movieResp" e "tvResp",
        // il valore delle quali viene assegnato, tramite Object Destructuring, prendendolo dalla key "data" di ciascuna response di Axios
        // però noi vogliamo ottenere i due array che stanno dentro .data.results di ciascuna response, non semplicemente il .data
        // quindi dobbiamo usare la sintassi dell'Object destructuring per gli oggetti annidati:
        //const myObject = {
        //   props: {
        //     match : 'Some value'
        //   }
        // };
        //const {
        //   props : {
        //     match
        //   },
        // } = myObject;
        // console.log(match); // prints: 'Some value'
        // Pagine utili:
        // https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Destructuring_assignment#nested_object_and_array_destructuring
        // https://itnext.io/using-es6-to-destructure-nested-objects-in-javascript-avoid-undefined-errors-that-break-your-code-612ae67913e9
        .then(
          ([
            {
              data: { results: movieResp },
            },
            {
              data: { results: tvResp },
            },
          ]) => {
            movieResp.forEach((el) => {
              el.type = "movie";
              castRequests.push(this.castSearch(el.type, el.id));
              this.movieList.push(el);
            });
            tvResp.forEach((el) => {
              el.type = "tv";
              castRequests.push(this.castSearch(el.type, el.id));
              this.movieList.push(el);
            });
            //abbiamo pushato tutti i risultati in un array comune dove i titoli hanno il type "movie" o "tv"
            //abbiamo anche creato tante requests axios (non ancora partite) quanti sono i titoli
            // console.log(castRequests);
            //facciamo partire tutte le richieste del cast insieme e salviamo il cast restituito dentro una mappa
            axios.all(castRequests).then((resp) => {
              resp.forEach((castResp) => {
                // console.log(castResp.data);
                this.casts.set(castResp.data.id, castResp.data.cast);
                // console.log(castResp.id, castResp.cast);
              });
              // abbiamo ufficialmente ricevuto risposta (se dio vuole) a tutte le richieste. Possiamo disattivare il flag "loading"
              this.loading = false;
            });
          }
        );
    },
  },
  computed: {
    enrichedList: function () {
      this.movieList.forEach((element) => {
        element.stars = "&starf;".repeat(Math.ceil(element.vote_average / 2));
        element.truncated_overview =
          element.overview.length < 180
            ? element.overview
            : element.overview.slice(0, 180) + "...";
        const featureCast = [];
        this.casts.get(element.id).forEach((castMember) => {
          featureCast.push(castMember.name);
        });
        element.cast = featureCast.slice(0, 5).join(", ");
      });
      const filteredFeatures = this.movieList.filter((item) => {
        console.log(this.filter.genre);
        return (
          item.genre_ids.includes(this.filter.genre) || this.filter.genre === ""
        );
      });

      filteredFeatures.sort((a, b) => {
        return b.popularity - a.popularity;
      });
      return filteredFeatures;
    },
    genresInList: function () {
      const genresInList = new Map();
      this.enrichedList.forEach((element) => {
        element.genre_ids.forEach((genre) => {
          if (!genresInList.has(genre)) {
            genresInList.set(genre, this.genres.get(genre));
          }
        });
      });
      return genresInList;
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
