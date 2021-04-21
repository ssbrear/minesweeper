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
  name: "Tile",
  props: {
    mine: Boolean,
    index: Number,
    parentMethod: Function,
    end: Boolean,
    clicked: Boolean,
    nearby: Number,
    reset: Number,
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
      if (this.end === true) return; // Doesn't let you keep playing after game is over
      if (this.status === "clicked") return; // Doesn't let you click if you already clicked it -- Limits recursion
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
    end() {
      if (this.mine === true) {
        this.status = "boom";
        console.log("boom");
      }
    },
  },
};
</script>

<style>
.tile {
  background: #373737;
  width: 35px;
  height: 35px;
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
  border: 1px solid #454545;
}
.marked {
  background: var(--minepink);
  border-color: rgb(187, 77, 181);
}
.boom {
  background: url("../../public/images/bomb.png") no-repeat;
  background-size: 100%;
}
</style>
