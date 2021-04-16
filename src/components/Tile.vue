<template>
  <button
    tabindex="-1"
    @contextmenu="markTile"
    @click="clickTile"
    class="tile"
    :class="status"
  ></button>
</template>

<script>
export default {
  components: {},
  props: { mine: Boolean, index: Number, parentMethod: Function, end: Boolean },
  emits: {
    "get-index"(payload) {
      return typeof payload === "number";
    },
  },
  data() {
    return { status: "hidden" };
  },
  methods: {
    clickTile() {
      console.log(this.end);
      if (this.end) return;
      this.$emit("get-index", this.index);
      if (!this.mine) this.status = "clicked";
      else this.status = "boom";
    },
    markTile(e) {
      e.preventDefault();
      if (this.status === "marked") this.status = "hidden";
      else if (this.status === "hidden") this.status = "marked";
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
