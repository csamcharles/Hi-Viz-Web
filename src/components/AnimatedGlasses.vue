<template>
  <div id="animated-glasses">
    <img id="glasses" src="../assets/final.png">
    <Lights
      id="lights-square"
      :lightContainer="{x: lightSquareWidth, y: lightSquareHeight}"
      :style="{
                width: lightSquareWidth + 'px',
                height: lightSquareHeight + 'px',
                'margin-top': lightSquareTop + 'px',
             }"
    />
  </div>
</template>

<script>
import Lights from "./Lights.vue";
export default {
  name: "animated-glasses",

  components: {
    Lights
  },

  data() {
    return {
      rendered: false
    };
  },

  props: {
    windowWidth: Number
  },

  methods: {},

  mounted() {
    this.rendered = true;
  },

  computed: {
    lightSquareWidth: function() {
      return 0.4865 * this.windowWidth;
    },

    lightSquareHeight: function() {
      return 0.444 * this.lightSquareWidth;
    },

    lightSquareTop: function() {
      return 0.174 * this.windowWidth;
    }
  }
};
</script>

<style scoped>
#animated-glasses {
  width: 100%;
  display: flex;
  justify-content: center;
}

#glasses {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  z-index: 1;
}

#lights-square {
  z-index: 2;
  display: flex;
  flex-direction: column;
  justify-content: space-between;
}
</style>