<template>
  <div class="col">
    <div
      class="feature-card text-center"
      :class="{ 'no-image': feature.poster_path === null }"
      :style="{
        backgroundImage: `url(https://image.tmdb.org/t/p/w342${feature.poster_path})`,
      }"
    >
      <div v-if="!feature.poster_path" class="feature-fallback">
        {{ `${feature.title ? feature.title : feature.name}` }}
      </div>
      <div class="feature-info p-4">
        <h2 class="feature-title fw-bold mt-3 mb-0">
          {{ feature.title ? feature.title : feature.name }}
        </h2>

        <div
          class="feature-ogtitle"
          v-if="
            feature.original_title != feature.title ||
            feature.original_name != feature.name
          "
        >
          {{
            feature.original_title
              ? feature.original_title
              : feature.original_name
          }}
        </div>
        <div class="feature-overview mt-3">
          {{ feature.truncated_overview }}
        </div>
        <div class="feature-cast mt-3">
          <strong>Cast: </strong>{{ feature.cast }}
        </div>
        <div class="feature-details mt-3">
          <div>
            <FlagIcons :languageCode="feature.original_language" />
            <span
              class="feature-rating ms-2"
              v-if="feature.stars"
              v-html="feature.stars"
            ></span>
          </div>
          <div class="feature-genres mt-3">
            {{ getGenres(feature.genre_ids) }}
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import FlagIcons from "./FlagIcons.vue";
export default {
  name: "AppMovieCard",
  props: {
    feature: Object,
    genres: Map,
  },
  components: {
    FlagIcons,
  },
  methods: {
    getGenres(genreArray) {
      const genreNames = [];
      genreArray.forEach((element) => {
        genreNames.push(this.genres.get(element));
      });
      return genreNames.join(", ");
    },
  },
};
</script>

<style lang="scss" scoped>
@import "../style/variables.scss";
@import "../style/common.scss";

.feature-card {
  // background-color: $background-secondary;
  background-position: center;
  background-repeat: no-repeat;
  background-size: cover;
  height: 525px;
  width: 342px;
  margin: auto;
  box-shadow: 4px 4px 8px 0px rgba(0, 0, 0, 0.5);
  position: relative;
  &.no-image {
    background-color: $background-secondary;
  }
  &:hover {
    .feature-fallback {
      display: none;
    }
    .feature-info {
      display: block;
      height: 100%;
      background-color: rgba(black, 0.7);
    }
  }
  .feature-fallback {
    position: absolute;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
    font-size: 1.5rem;
    font-weight: 700;
  }
  .feature-info {
    display: none;
  }
  .feature-title {
    font-size: 1.5rem;
    text-transform: uppercase;
    & ~ * {
      margin-bottom: 0.2rem;
      font-weight: 500;
    }
  }
  .feature-cast {
    font-weight: 400;
    font-size: 0.9rem;
  }
  .feature-ogtitle {
    color: $text-lowcontrast-color;
    font-size: 1rem;
  }
  .feature-overview,
  .feature-genres {
    color: white;
    font-size: 1rem;
  }
  .feature-details * {
    vertical-align: middle;
  }
  .feature-rating {
    font-size: 1.4rem;
    color: #f4cd26;
    text-shadow: -1px -1px 0 #000, 1px -1px 0 #000, -1px 1px 0 #000,
      1px 1px 0 #000;
  }
}
</style>
