<template>
  <button
    @click="handleClick"
    :class="{ active: show }"
    class="game__button"
  ></button>
</template>

<script>
export default {
  props: ["audio", "show", "speed"],
  data() {
    return { voice: new Audio(this.audio || null) };
  },
  watch: {
    show() {
      if (this.show) {
        this.voice.playbackRate = Math.min(1 / this.speed, 1);
        this.voice.play();
        setTimeout(() => {
          this.$emit("hide");
        }, 300);
      }
    },
  },
  methods: {
    handleClick() {
      this.voice.play();
      this.$emit("choose");
    },
  },
};
</script>

<style lang="scss" scoped>
.game__button {
  border-radius: 160px;
  background-color: unset;
  border: 1px solid black;
  width: 150px;
  height: 150px;
}
.active,
.game__button:active {
  box-shadow: inset 0 0 10px 3px #222222;
}
</style>
