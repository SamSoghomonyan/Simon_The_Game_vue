<template>
  <div class="game-options">
    <label
      v-for="(levelValue, key) in levels"
      :key="key"
      class="game-options__item"
    >
      <input
        @change="changeHandlers($event)"
        :value="levelValue"
        type="radio"
        id="levels"
        :checked="value === levelValue"
        name="options"
      />{{ key }}
    </label>
  </div>
</template>

<script>
export default {
  props: ["value"],
  data() {
    return { levels: { easy: 1.5, medium: 1, hard: 0.4 } };
  },
  methods: {
    changeHandlers(event) {
      this.$emit("input", Number(event.target.value));
    },
  },
};
</script>

<style lang="scss" scoped>
.game-options {
  display: flex;
  flex-direction: column;
  align-items: center;

  width: 250px;

  padding: 24px;
  border: 1px solid black;
  border-radius: 16px;

  &__item {
    display: flex;
    align-items: center;
    gap: 4px;
    width: 100px;
  }
}
</style>
