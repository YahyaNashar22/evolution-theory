<script setup lang="ts">
import gsap from 'gsap'
import { onMounted, onUnmounted, ref } from 'vue'

const sectionRef = ref<HTMLElement | null>(null)

let context: gsap.Context | null = null

onMounted(() => {
  if (!sectionRef.value) return

  context = gsap.context(() => {
    const timeline = gsap.timeline({
      scrollTrigger: {
        trigger: sectionRef.value,
        start: 'top 70%',
        once: true,
      },
    })

    timeline
      .fromTo(
        '#evolution-text',
        { y: 40, display: 'block', autoAlpha: 0 },
        { y: 0, autoAlpha: 1, duration: 1, ease: 'power3.out' },
      )
      .to('#evolution-text', { duration: 2 })
      .to('#evolution-text', {
        y: -30,
        duration: 0.8,
        display: 'none',
        autoAlpha: 0,
        ease: 'power2.in',
      })
      .fromTo(
        '#mutation-text',
        { y: 40, display: 'none', autoAlpha: 0 },
        { y: 0, display: 'block', duration: 1, ease: 'power3.out', autoAlpha: 1 },
      )
  }, sectionRef.value)
})

onUnmounted(() => {
  context?.revert()
})
</script>

<template>
  <section class="section" id="intro">
    <div class="masked-text-container" ref="sectionRef">
      <h1 class="masked-text" id="evolution-text" ref="firstTextRef">Evolution</h1>
      <h1 class="masked-text" id="mutation-text" ref="secondTextRef">Mutation</h1>
    </div>
  </section>
</template>

<style scoped>
#intro {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;

  background-color: #0c0c0c;
}

.masked-text {
  font-family: Boldonse, sans-serif;
  font-size: 14rem;
  font-weight: bold;
  color: transparent;
  letter-spacing: 1.5rem;
  text-transform: uppercase;

  background-image: url('../assets/backgrounds/human.jpg');
  background-size: 50%;
  background-position: center;
  background-clip: text;
  --webkit-background-clip: text;
  --webkit-text-fill-color: transparent;
  animation: animate-background 5s infinite alternate linear;
}
#evolution-text {
  background-image: url('../assets/backgrounds/human.jpg');
}
#mutation-text {
  background-image: url('../assets/backgrounds/cell-mutation.avif');
}

@keyframes animate-background {
  0% {
    background-position: 0 60%;
  }
  100% {
    background-position: 100% 140%;
  }
}
</style>
