<template>
  <header>
    <h1>Minesweeper</h1>
    <h2>Mines left: {{ NUM_MINES - numMarked }}</h2>
  </header>
  <GameBoard
    @num-mark-change="changeNumMarks"
    :GAME_SIZE="GAME_SIZE"
    :NUM_MINES="NUM_MINES"
  />
  <footer><button id="options-button">Options</button></footer>
</template>

<script>
import GameBoard from "./components/GameBoard";

export default {
  components: {
    GameBoard,
  },
  data() {
    return {
      GAME_SIZE: 100,
      NUM_MINES: 10,
      numMarked: 0,
    };
  },
  methods: {
    changeNumMarks(newNumMarked) {
      this.numMarked = newNumMarked;
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
