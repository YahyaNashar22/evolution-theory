<script setup lang="ts">
import EarthComponent from './components/EarthComponent.vue'
import IntroComponent from './components/IntroComponent.vue'
</script>

<template>
  <EarthComponent />
  <IntroComponent />
</template>

<style>
html {
  scroll-behavior: smooth;
  scroll-snap-type: y mandatory;
}

html,
body,
#app {
  margin: 0;
  min-height: 100%;
}
</style>
