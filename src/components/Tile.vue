<template>
  <button
    type="button"
    tabindex="-1"
    @contextmenu="markTile"
    @click="clickTile"
    class="tile"
    :class="status"
  >
    {{ nearby }}
  </button>
</template>

<script>
export default {
  components: {},
  props: {
    mine: Boolean,
    index: Number,
    parentMethod: Function,
    end: Boolean,
    clicked: Boolean,
    nearby: Number,
  },
  emits: {
    "get-index"(payload) {
      return typeof payload === "number";
    },
    "mark-change": null,
  },
  data() {
    return { status: "hidden" };
  },
  methods: {
    clickTile() {
      if (this.end) return; // Doesn't let you keep playing after game is over
      this.$emit("get-index", this.index); // Sends index to board to compare with game state
      if (this.mine) this.status = "boom";
      else {
        this.status = "clicked";
      }
    },
    markTile(e) {
      e.preventDefault();
      if (this.status === "marked") {
        this.status = "hidden";
        this.$emit("mark-change", -1);
      } else if (this.status === "hidden") {
        this.status = "marked";
        this.$emit("mark-change", 1);
      }
    },
  },
  watch: {
    clicked() {
      this.clickTile();
    },
  },
};
</script>

<style>
.tile {
  background: #373737;
  width: 50px;
  height: 50px;
  margin: 0.2em;
  cursor: pointer;
  outline: none;
  font-size: 1.25rem;
  color: var(--minepink);
}
.hidden:hover {
  background: #494949;
}
.clicked {
  border: none;
  background: #343434;
}
.marked {
  background: var(--minepink);
  border-color: rgb(187, 77, 181);
}
.boom {
  background: url("https://tinyurl.com/5aak3rjr") no-repeat;
  background-size: 100%;
}
</style>
