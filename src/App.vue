<template>
  <div class="container">
    <VGame />
  </div>
</template>

<script>
import VGame from "./components/VGame.vue";
export default { components: { VGame } };
</script>

<style lang="scss" scoped>
.container {
  width: 100vw;
  height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;
}
</style>
