<template>
  <header>
    <h1>Minesweeper</h1>
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
}
h1 {
  color: #ea80fc;
  text-align: center;
  font-size: 4rem;
}
</style>
