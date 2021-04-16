<template>
  <main @contextmenu="removeContext">
    <Tile
      @get-index="checkGameState"
      v-for="i in GAME_SIZE"
      :index="i"
      :key="i"
      :mine="this.mineArray.includes(i)"
      :end="this.end"
      :clicked="clicked[i]"
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
    return {
      GAME_SIZE: 100,
      NUM_MINES: 10,
      mineArray: [],
      end: false,
      clicked: [],
    };
  },
  methods: {
    removeContext(e) {
      e.preventDefault();
    },
    // NOTE: there is no default 'event' parameter for custom emits
    // checkForGameEnd(e, index) results in index being undefined
    checkGameState(index) {
      this.clicked[index] = true;
      if (this.mineArray.includes(index)) {
        this.end = true;
      } else {
        let [x, y] = this.indexToRect(index);
        let bombCount = 0;
        for (let xd = -1; xd <= 1; xd++) {
          for (let yd = -1; yd <= 1; yd++) {
            if (xd === 0 && yd === 0) continue; // Don't need tile to check itself
            const potentialBombIndex = this.rectToIndex([x + xd, y + yd]);
            if (this.mineArray.includes(potentialBombIndex)) bombCount++;
          }
        }
        console.log(bombCount);
        if (bombCount === 0) {
          for (let xd = -1; xd <= 1; xd++) {
            for (let yd = -1; yd <= 1; yd++) {
              if (xd === 0 && yd === 0) continue; // Don't need tile to check itself
              this.clicked[this.rectToIndex([x + xd, y + yd])] = true;
            }
          }
        }
      }
    },
    // Convert index to rectangular coordinates (0, 0) is top left, (9, 9) is bottom right
    indexToRect(index) {
      const x = index % 10 === 0 ? 9 : (index % 10) - 1;
      const y = Math.floor(Math.abs(index - 1) / 10);
      return [x, y];
    },
    rectToIndex([x, y]) {
      const index = y * 10 + x + 1;
      return index;
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
