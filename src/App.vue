<template>
  <header>
    <h1>Minesweeper</h1>
    <h3>Mines left: </h3>
  </header>
  <GameBoard />
</template>

<script>
import GameBoard from "./components/GameBoard";

export default {
  components: {
    GameBoard,
  },
};
</script>

<style>
#app {
  display: grid;
  justify-content: center;
  justify-items: center;
  color: #ea80fc;
  text-align: center;
}
h1 {
  font-size: 4rem;
}
</style>
