<template>
  <header>
    <h1>Minesweeper</h1>
    <h2>Mines left: {{ NUM_MINES - numMarked }}</h2>
  </header>
  <GameBoard
    :show="!optionsShow"
    @num-mark-change="changeNumMarks"
    :GAME_SIZE="[GAME_WIDTH, GAME_HEIGHT]"
    :NUM_MINES="NUM_MINES"
  />
  <Options @difficulty-change="respondToDifficultyChange" :show="optionsShow" />
  <footer>
    <button @click="openOptions" id="options-button">Options</button>
  </footer>
</template>

<script>
import GameBoard from "./components/GameBoard";
import Options from "./components/Options";

export default {
  name: "App",
  components: {
    GameBoard,
    Options,
  },
  data() {
    return {
      GAME_WIDTH: 16,
      GAME_HEIGHT: 16,
      NUM_MINES: 40,
      numMarked: 0,
      optionsShow: false,
    };
  },
  methods: {
    changeNumMarks(newNumMarked) {
      this.numMarked = newNumMarked;
    },
    openOptions() {
      const optionsButton = document.querySelector("#options-button");
      if (this.optionsShow === true) {
        this.optionsShow = false;
        optionsButton.textContent = "Options";
      } else if (this.optionsShow === false) {
        this.optionsShow = true;
        optionsButton.textContent = "Return";
      }
    },
    respondToDifficultyChange(newDifficulty) {
      if (newDifficulty === "0") {
        this.GAME_WIDTH = 10;
        this.GAME_HEIGHT = 10;
        this.NUM_MINES = 10;
      } else if (newDifficulty === "1") {
        this.GAME_WIDTH = 16;
        this.GAME_HEIGHT = 16;
        this.NUM_MINES = 40;
      } else if (newDifficulty === "2") {
        this.GAME_WIDTH = 30;
        this.GAME_HEIGHT = 16;
        this.NUM_MINES = 99;
      }
    },
  },
};
</script>

<style>
:root {
  --minepink: #ea80fc;
}
#app {
  display: grid;
  justify-content: center;
  justify-items: center;
  color: var(--minepink);
  text-align: center;
}
h1 {
  font-size: 4rem;
}
#options-button {
  margin-top: 2em;
  font-size: 1.5rem;
  color: var(--minepink);
  background: none;
  border: none;
  cursor: pointer;
  text-decoration: underline;
}
</style>
