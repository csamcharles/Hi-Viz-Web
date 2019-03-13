<template>
  <div>
    <div class="led-row" id="top-row">
      <transition-group name="fade">
        <span v-for="light in lightsArray" v-bind:key="light.index">
          <SingleLight
            class="singleLight"
            v-show="lightsVisibleArray[light.index]"
            v-bind:style="{
                    left: light.xpos + 'px',
                    top: light.ypos + 'px',
                    height: light.height + 'px',
                    '-webkit-transform': `perspective(${light.perspective}px) rotateY(${light.angle}rad)`,
                    transform: `perspective(${light.perspective}px) rotateY(${light.angle}rad)`,
              }"
          />
          <div
            class="led-container"
            @mouseover="toggleLight(light.index)"
            v-bind:style="{
                  left: light.xpos + 'px',
                  top: light.ypos + 'px',
                  height: light.height + 'px',
                  width: light.height + 'px',
            }"
          ></div>
        </span>
      </transition-group>
    </div>
    <div class="led-row" id="bottom-row">
      <SingleLight
        class="singleLight"
        v-for="(light, index) in lightsArray"
        v-bind:key="index"
        v-bind:style="{
                        left: light.xpos + 'px',
                        bottom: light.ypos + 'px',
                        height: light.height + 'px',
                        '-webkit-transform': `perspective(${light.perspective}px) rotateY(${light.angle}rad)`,
                        transform: `perspective(${light.perspective}px) rotateY(${light.angle}rad)`,
                        }"
      />
    </div>
  </div>
</template>

<script>
import SingleLight from "./SingleLight.vue";
import Vue from "vue";

export default {
  name: "lights",
  props: {
    lightContainer: { x: Number, y: Number }
  },

  components: {
    SingleLight
  },

  data: function() {
    return {
      ready: false,
      numLights: 10,
      lensAngle: { x: 133.0, y: 12 },
      lightsVisibleArray: []
    };
  },

  mounted() {
    this.ready = true;
    this.lightsVisibleArray = this.populateLightsVisibleArray();
  },

  methods: {
    populateLightsVisibleArray: function() {
      let array = [];
      for (let i = 0; i < this.numLights; i++) {
        array[i] = false;
      }
      return array;
    },

    toggleLight: function(index) {
      Vue.set(this.lightsVisibleArray, index, !this.lightsVisibleArray[index]);
    },

    checkVisible: function(index) {
      return this.lightsVisibleArray[index];
    },

    toRadians: function(angle) {
      return angle * (Math.PI / 180);
    },

    calculateIncrementalAngle: function(inputAngle) {
      return inputAngle / (this.numLights - 1);
    }
  },

  computed: {
    arcLength: function() {
      return {
        x: 1.26623376623 * this.lightContainer.x,
        y: 1.03 * this.lightContainer.x
      };
    },

    lightsArray: function() {
      let curAngleRad = {
        x: this.toRadians((180 - this.lensAngle.x) / 2),
        y: this.toRadians((180 - this.lensAngle.y) / 2)
      };
      let totalAngleRad = {
        x: this.toRadians(this.lensAngle.x),
        y: this.toRadians(this.lensAngle.y)
      };

      let radius = {
        x: this.arcLength.x / totalAngleRad.x,
        y: this.arcLength.y / totalAngleRad.y
      };

      let lightsArray = [];
      //const lightHeight = 0.2 * this.lightContainer.y;
      const incrementalAngleRad = {
        x: this.calculateIncrementalAngle(totalAngleRad.x),
        y: this.calculateIncrementalAngle(totalAngleRad.y)
      };

      // Calculate all light positions and stretch
      for (let i = 0; i < this.numLights; i++) {
        let adjacent = {
          x: radius.x * Math.cos(curAngleRad.x),
          y: radius.y * Math.sin(curAngleRad.y)
        };

        // Calculate position
        let pos = {
          x: this.lightContainer.x / 2 - adjacent.x,
          y: radius.y - adjacent.y
        };

        // Calculate rotation angle
        let angle = Math.PI / 2 - curAngleRad.x;

        // Increment the angle
        curAngleRad.x += incrementalAngleRad.x;
        curAngleRad.y += incrementalAngleRad.y;

        let height = this.lightContainer.x / 13;
        let width = height;

        // Center LEDs - account for width
        pos.x = pos.x - width / 2;

        lightsArray[i] = {
          index: i,
          show: false,
          xpos: pos.x,
          ypos: pos.y,
          height: height,
          perspective: 0,
          angle: angle
        };
      }
      return lightsArray;
    }
  }
};
</script>

<style scoped>
.led-row {
  position: relative;
  width: 100%;
  height: 20%;
}

.led-container {
  position: absolute;
  border: 2px solid red;
  overflow: visible;
}

#top-row {
  top: 0;
}

#bottom-row {
  bottom: 0;
}

.singleLight {
  position: absolute;
}

.fade-enter-active {
  transition: 0.1s ease-in;
}

.fade-leave-active {
  transition: 1s ease-out;
}

.fade-enter,
.fade-leave-to {
  opacity: 0;
}
</style>