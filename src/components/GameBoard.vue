<template>
  <main>
    <Tile v-for="i in GAME_SIZE" :key="i" />
  </main>
</template>

<script>
import Tile from "./Tile";

export default {
  components: {
    Tile,
  },
  data() {
    return { GAME_SIZE: 100 };
  },
};
</script>

<style>
main {
  background: #252525;
  display: grid;
  grid-template-columns: repeat(10, 1fr);
  padding: 0.2em;
}
</style>
