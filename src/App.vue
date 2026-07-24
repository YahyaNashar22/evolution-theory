<script setup lang="ts">
import gsap from 'gsap'
import { TextPlugin } from 'gsap/all'
import { ScrollTrigger } from 'gsap/ScrollTrigger'
import { SplitText } from 'gsap/SplitText'
import { RouterView } from 'vue-router'

gsap.registerPlugin(TextPlugin, ScrollTrigger, SplitText)
</script>

<template>
  <RouterView />
</template>

<style>
@import url('https://fonts.googleapis.com/css2?family=Boldonse&display=swap');
html {
  scroll-behavior: smooth;
  scroll-snap-type: y mandatory;
}

html,
body,
#app {
  margin: 0;
  min-height: 100%;
  background-color: #000;
}
.section {
  height: 100dvh;
  width: 100%;
  scroll-snap-align: start;
}
</style>
