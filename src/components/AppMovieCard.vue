<template>
  <div class="col">
    <div class="feature-card p-2 text-center">
      <img class="img-fluid" alt="" />
      <h2 class="feature-title fw-bold mt-3">{{ feature.title }}</h2>

      <p>{{ feature.original_title }}</p>
      <div>
        {{ feature.original_language }} &centerdot;
        {{ feature.vote_average }} &star;
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "AppMovieCard",
  props: {
    feature: Object,
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
}
</style>
