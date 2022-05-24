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
      <div class="feature-info p-2">
        <h2 class="feature-title fw-bold mt-5">
          {{ feature.title ? feature.title : feature.name }}
        </h2>

        <p
          class="feature-ogtitle"
          v-if="feature.original_title != feature.title"
        >
          {{ feature.original_title }}
        </p>
        <p class="feature-overview">{{ feature.truncated_overview }}</p>
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
  methods: {
    getYear(date) {
      return date.split("-")[0];
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
  height: 350px;
  width: 228px;
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
    font-size: 1.2rem;
    text-transform: uppercase;
    & ~ * {
      margin-bottom: 0.2rem;
      font-weight: 500;
      font-size: 0.8rem;
    }
  }
  .feature-ogtitle {
    color: $text-lowcontrast-color;
  }
  .feature-overview {
    color: white;
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
