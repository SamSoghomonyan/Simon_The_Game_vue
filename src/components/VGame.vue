<template>
  <div class="game-container">
    <div class="game-field">
      <GameButton
        v-for="(value, key) in fields"
        :audio="value.sound"
        :show="value.show"
        :class="`game-button_${key}`"
        @hide="toggleButton(key)"
        @choose="playerChoose(key)"
        :key="key"
        :speed="level"
      />
    </div>

    <div class="game-info">
      <h4>{{ `Раунд: ${round.length}` }}</h4>
      <button @click="newGame" class="start">{{ "Start" }}</button>
      <GameOptions v-model="level" />
    </div>
  </div>
</template>

<script>
import GameButton from "./GameButton.vue";
import GameOptions from "./GameOptions.vue";
export default {
  components: { GameButton, GameOptions },
  data() {
    return {
      status: "disable",
      round: { length: 1, value: [], playerValue: [] },
      level: 1.5,
      fields: {
        red: { show: false, sound: "sounds_1.mp3" },
        blue: { show: false, sound: "sounds_2.mp3" },
        yellow: { show: false, sound: "sounds_3.mp3" },
        green: { show: false, sound: "sounds_4.mp3" },
      },
    };
  },
  watch: {
    status(val) {
      if (val === "run") this.newGame();
      if (val == "next") this.next();
    },
  },
  methods: {
    playerChoose(color) {
      this.round.playerValue.push(color);
      if (this.isFailed()) {
        this.status = "failed";
        this.round.playerValue = [];
        this.round.value = [];
        this.round.length = 0;
      } else if (this.round.value.length === this.round.playerValue.length) {
        this.status = "next";
        this.round.length++;
        this.round.playerValue = [];
      }
    },
    newGame() {
      this.round.value = [];
      this.round.playerValue = [];
      this.round.length = 1;
      this.next();
    },
    isFailed() {
      const length = this.round.playerValue.length;
      return (
        this.round.value[length - 1] !== this.round.playerValue[length - 1]
      );
    },
    async showRound() {
      for (const item of this.round.value) {
        await this.showButton(item);
      }
      this.status = "play";
    },
    async showButton(item) {
      return new Promise((resolve) => {
        setTimeout(() => {
          this.fields[item].show = true;
          resolve();
        }, this.level * 1000);
      });
    },
    next() {
      this.round.value.push(this.randomButton());
      this.showRound();
    },
    toggleButton(key) {
      this.fields[key].show = false;
    },
    randomButton() {
      const fields = Object.keys(this.fields);
      const fieldIndex = Math.floor(Math.random() * (4 - 1 + 1)) + 1;
      console.log(fieldIndex);
      return fields[fieldIndex - 1];
    },
  },
};
</script>

<style lang="scss" scoped>
.game-container {
  display: flex;
  align-items: center;
  justify-content: space-between;
  width: 100%;
  max-width: 700px;
  height: 400px;
  border: 1px solid #aaaaaa;
  border-radius: 16px;
  padding: 24px;
}

.game-field {
  display: grid;
  grid-template-columns: 1fr 1fr;
}

.start {
  background-color: #bbbbbb;
  border: 1px solid #aaaaaa;
  border-radius: 8px;
  padding: 16px;
  width: 100px;
}
.game-button {
  &_red {
    background-color: red;
  }
  &_blue {
    background-color: blue;
  }
  &_yellow {
    background-color: yellow;
  }
  &_green {
    background-color: greenyellow;
  }
}

.game-info {
  min-height: 300px;
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 16px;
}
</style>
