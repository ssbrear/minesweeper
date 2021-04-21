<template>
  <header>
    <h1>Minesweeper</h1>
    <h2>Mines left: {{ NUM_MINES - numMarked }}</h2>
  </header>
  <GameBoard
    :key="gameboardKey"
    :show="!optionsShow"
    @num-mark-change="changeNumMarks"
    :GAME_SIZE="[GAME_WIDTH, GAME_HEIGHT]"
    :NUM_MINES="NUM_MINES"
  />
  <Options @difficulty-change="respondToDifficultyChange" :show="optionsShow" />
  <footer>
    <button @click="openOptions" id="options-button">Options</button>
    <button @click="resetGame" id="reset-button">Reset</button>
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
      // Reset will be a binary state that updates components when it changes
      // Similar to D-flip-flop
      gameboardKey: 0,
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
        optionsButton.textContent = "Board";
      }
    },
    respondToDifficultyChange(newDifficulty) {
      this.gameboardKey++;
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
    resetGame() {
      this.numMarked = 0;
      // This is forcing a re-render on the gameboard, and therefore the tile children.
      // This is definitely not the best solution, forcing a re-render should never be the solution
      // The alternative seems to be to create a new boolean prop for the gameboard that I can toggle from the app.
      // I could create a watch on the gameboard and manually reset all the data variables whenever this boolean changes.
      // Would I then not be forcing a re-render on all of the tile children then? There must be a way to avoid this.
      this.gameboardKey++;
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
#options-button,
#reset-button {
  margin-top: 2em;
  font-size: 1.5rem;
  color: var(--minepink);
  background: none;
  border: none;
  cursor: pointer;
  text-decoration: underline;
}
</style>
