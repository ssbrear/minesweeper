<template>
  <button
    @contextmenu="markTile"
    @click="clickTile"
    class="tile"
    :class="status"
  ></button>
</template>

<script>
export default {
  components: {},
  props: {},
  data() {
    return { status: "hidden" };
  },
  methods: {
    clickTile() {
      this.status = "clicked";
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
</style>
