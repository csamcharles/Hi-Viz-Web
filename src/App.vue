<template>
  <div id="app">
    <AnimatedGlasses v-bind:windowWidth="windowWidth" v-bind:windowHeight="windowHeight"/>
  </div>
</template>

<script>
import AnimatedGlasses from "./components/AnimatedGlasses";

export default {
  name: "app",
  components: {
    AnimatedGlasses
  },

  data: function() {
    return {
      windowWidth: 0,
      windowHeight: 0
    };
  },

  created() {
    window.addEventListener("resize", this.handleResize);
    this.handleResize();
  },
  destroyed() {
    window.removeEventListener("resize", this.handleResize);
  },

  methods: {
    handleResize: function() {
      this.windowHeight = window.innerHeight;
      this.windowWidth = window.innerWidth;
    }
  }
};
</script>

<style>
* {
  margin: 0;
}

#app {
  display: flex;
  height: 100vh;
  background-color: #0f0f0f;
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 0;
  padding: 0;
}
</style>
