<template>
  <div class="loading">
    <p>Loading...</p>
    <div class="spin"></div>
  </div>
</template>

<script>
export default {
  name: "AppLoading",
};
</script>

<style lang="scss" scoped>
.loading {
  height: calc(100vh - 4rem);
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  color: white;
  p {
    margin-bottom: 2rem;
  }
}
.spin {
  position: relative;
}
@keyframes spinner {
  0% {
    transform: translate3d(-50%, -50%, 0) rotate(0deg);
  }
  100% {
    transform: translate3d(-50%, -50%, 0) rotate(360deg);
  }
}
.spin::before {
  animation: 1.5s linear infinite spinner;
  animation-play-state: inherit;
  border: solid 5px #cfd0d1;
  border-bottom-color: #1c87c9;
  border-radius: 50%;
  content: "";
  height: 40px;
  width: 40px;
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate3d(-50%, -50%, 0);
  will-change: transform;
}
</style>
