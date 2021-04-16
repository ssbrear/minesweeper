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
  data() {
    return { status: "hidden" };
  },
  methods: {
    clickTile() {
      this.status = "clicked";
    },
    markTile() {
      this.status = "marked";
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
</style>
