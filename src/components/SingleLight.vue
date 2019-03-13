<template>
    <svg class="led" viewBox="0 0 1 1">
      <defs>
        <radialGradient id="grad1" cx="50%" cy="50%" r="50%" fx="50%" fy="50%">
          <stop class="stop-inner" offset="10%" style="stop-opacity:1"></stop>
          <stop class="stop-inner" offset="20%" style="stop-opacity:.3"></stop>
          <stop class="stop-inner" offset="50%" style="stop-opacity:.1"></stop>
          <stop class="stop-outer" offset="100%" style="stop-opacity:0"></stop>
        </radialGradient>
      </defs>
      <circle cx="50%" cy="50%" r="120%" fill="url(#grad1)"></circle>
    </svg>
</template>

<script>
export default {
  name: "single-light",

};
</script>

<style scoped>
@keyframes rainbow {
  0% {
    stop-color: hsl(0, 100%, 50%);
  }
  12% {
    stop-color: hsl(31, 100%, 50%);
  }
  25% {
    stop-color: hsl(63, 100%, 50%);
  }
  37% {
    stop-color: hsl(95, 100%, 50%);
  }
  50% {
    stop-color: hsl(127, 100%, 50%);
  }
  62% {
    stop-color: hsl(159, 100%, 50%);
  }
  75% {
    stop-color: hsl(191, 100%, 50%);
  }
  87% {
    stop-color: hsl(223, 100%, 60%);
  }
  100% {
    stop-color: hsl(255, 100%, 60%);
  }
}

.stop-inner {
  animation: rainbow 10s infinite alternate;
}

.led {
  overflow: visible;
}


</style>