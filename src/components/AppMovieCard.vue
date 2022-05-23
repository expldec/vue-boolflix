<template>
  <div class="col">
    <div class="feature-card p-2 text-center">
      <img
        class="img-fluid"
        :src="`https://image.tmdb.org/t/p/w342${feature.poster_path}`"
        alt=""
      />
      <h2 class="feature-title fw-bold mt-3">
        {{ feature.title ? feature.title : feature.name }}
      </h2>

      <p class="feature-ogtitle">{{ feature.original_title }}</p>
      <div class="feature-details">
        <FlagIcons :languageCode="feature.original_language" />
        <span
          class="feature-rating ms-2"
          v-if="feature.vote_average"
          v-html="feature.vote_average"
        ></span>
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
  },
  components: {
    FlagIcons,
  },
};
</script>

<style lang="scss" scoped>
@import "../style/variables.scss";
@import "../style/common.scss";

.feature-card {
  background-color: $background-secondary;
  height: 100%;
  .feature-title {
    font-size: 1.2rem;
    text-transform: uppercase;
    & ~ * {
      color: $text-lowcontrast-color;
      margin-bottom: 0.2rem;
      font-weight: 500;
      font-size: 0.8rem;
    }
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
