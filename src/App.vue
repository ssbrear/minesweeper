<template>
  <header>
    <h1>Minesweeper</h1>
    <h3>Mines left:</h3>
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
</style>
