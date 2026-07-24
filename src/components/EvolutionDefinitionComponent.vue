<script setup lang="ts">
import { gsap } from 'gsap'
import { SplitText } from 'gsap/SplitText'

const splitText = SplitText.create('.text', { type: 'words' })

gsap.from(splitText, {
  x: 150,
  opacity: 0,
  duration: 0.7,
  ease: 'power4',
  stagger: 0.04,
})
</script>

<template>
  <section class="section">
    <div></div>
    <div class="text-container">
      <h1>Evolution Definition</h1>
      <p class="text">This is a text that will be broken and animated and then replaced.</p>
    </div>
  </section>
</template>

<style scoped>
.text {
  background-color: red;
  color: '#ffffff';
  font-size: 3rem;
}
</style>
