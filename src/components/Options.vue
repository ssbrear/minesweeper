<template>
  <section
    :style="{ width: `${width}px`, height: `${height}px` }"
    :class="{ grid: show }"
  >
    <h2>Difficulty</h2>
    <input
      @change="difficultyChange"
      min="0"
      max="2"
      step="1"
      value="1"
      type="range"
    />
    <ul>
      <li>Easy</li>
      <li>Medium</li>
      <li>Hard</li>
    </ul>
  </section>
</template>

<script>
export default {
  name: "Options",
  components: {},
  props: {
    show: Boolean,
  },
  data() {
    return {
      width: 0,
      height: 0,
    };
  },
  methods: {
    difficultyChange(event) {
      const newDifficulty = event.target.value;
      this.$emit("difficulty-change", newDifficulty);
    },
  },
  created() {
    this.width = document.querySelector("main").offsetWidth;
    this.height = document.querySelector("main").offsetHeight;
  },
  updated() {
    this.width = document.querySelector("main").offsetWidth || this.width;
    this.height = document.querySelector("main").offsetHeight || this.height;
  },
};
</script>

<style>
section {
  background: #252525;
  display: none;
  align-items: center;
  justify-content: center;
  grid-template-rows: repeat(4, 1fr);
}
section > h2 {
  margin: 0;
  font-size: 3rem;
}
ul {
  list-style: none;
  padding: 0;
  display: flex;
  flex-flow: row nowrap;
  position: relative;
  bottom: 50px;
}
ul > li {
  margin: 0 1.5em;
}

input[type="range"] {
  justify-self: center;
  -webkit-appearance: none;
  width: 75%;
  height: 25px;
  background: #974aa5;
  outline: none;
  opacity: 0.7;
  -webkit-transition: 0.2s;
  transition: opacity 0.2s;
}

input[type="range"]:hover {
  opacity: 1;
}

input[type="range"]::-webkit-slider-thumb {
  -webkit-appearance: none;
  appearance: none;
  width: 25px;
  height: 25px;
  background: #121212;
  cursor: pointer;
}

input[type="range"]::-moz-range-thumb {
  width: 25px;
  height: 25px;
  background: #121212;
  cursor: pointer;
}
</style>
