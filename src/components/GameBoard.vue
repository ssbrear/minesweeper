<template>
  <main
    :style="{ gridTemplateColumns: `repeat(${GAME_SIZE[0]}, 1fr)` }"
    @contextmenu="removeContext"
    :class="{ grid: show }"
  >
    <Tile
      @get-index="checkGameState"
      @mark-change="checkNumMarks"
      v-for="i in GAME_SIZE[0] * GAME_SIZE[1]"
      :index="i"
      :key="i"
      :mine="this.mineArray.includes(i)"
      :end="this.end"
      :clicked="clicked[i]"
      :nearby="nearby[i]"
    />
  </main>
</template>

<script>
import Tile from "./Tile";
export default {
  name: "GameBoard",
  components: {
    Tile,
  },
  props: {
    GAME_SIZE: Array,
    NUM_MINES: Number,
    show: Boolean,
  },
  emits: {
    "num-mark-change"(payload) {
      return typeof payload === "number";
    },
  },
  data() {
    return {
      mineArray: [],
      end: false,
      clicked: [],
      nearby: [],
      numMarked: 0,
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
            if (this.checkInvalidTile(x, y, xd, yd)) continue;
            const potentialBombIndex = this.rectToIndex([x + xd, y + yd]);
            if (this.mineArray.includes(potentialBombIndex)) bombCount++;
          }
        }
        if (bombCount === 0) {
          for (let xd = -1; xd <= 1; xd++) {
            for (let yd = -1; yd <= 1; yd++) {
              if (this.checkInvalidTile(x, y, xd, yd)) continue;
              this.clicked[this.rectToIndex([x + xd, y + yd])] = true;
            }
          }
        } else this.nearby[index] = bombCount;
      }
    },
    checkInvalidTile(x, y, xd, yd) {
      if (xd === 0 && yd === 0) return true; // Don't need to check itself
      if ((x === 9 && xd === 1) || (x === 0 && xd === -1)) return true; // Don't check tiles in columns across the board
      if ((y === 9 && yd === 1) || (y === 0 && yd === -1)) return true; // Don't check tiles out of bounds
      return false;
    },
    checkNumMarks(newNumMarked) {
      this.numMarked += newNumMarked;
      this.$emit("num-mark-change", this.numMarked);
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
      const position = Math.floor(
        Math.random() * this.GAME_SIZE[0] * this.GAME_SIZE[1]
      );
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
  padding: 4px;
  display: none;
}
</style>
