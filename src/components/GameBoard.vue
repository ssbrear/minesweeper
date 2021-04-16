<template>
  <main @contextmenu="removeContext">
    <Tile
      @get-index="checkForGameEnd"
      v-for="i in GAME_SIZE"
      :index="i"
      :key="i"
      :mine="this.mineArray.includes(i)"
    />
  </main>
</template>

<script>
import Tile from "./Tile";
export default {
  components: {
    Tile,
  },
  data() {
    return { GAME_SIZE: 100, NUM_MINES: 10, mineArray: [] };
  },
  methods: {
    removeContext(e) {
      e.preventDefault();
    },
    checkForGameEnd(index) {
      if (this.mineArray.includes(index)) {
        console.log("END");
      }
    },
  },
  // Not sure if "mounted()" is the best point in the lifecycle,
  // but this was chosen for the reason that it would not cause
  // a re-render since the component has NOT rendered yet.
  // There are other lifecycle hooks before the component renders,
  // so out of those, picking "mounted()" was arbitrary.
  mounted() {
    while (this.mineArray.length < this.NUM_MINES) {
      const position = Math.floor(Math.random() * this.GAME_SIZE);
      if (
        !this.mineArray.some(
          (existingPosition) => existingPosition === position
        )
      )
        this.mineArray.push(position);
    }
    console.log(this.mineArray);
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
