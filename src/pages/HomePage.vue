<script setup lang="ts">
import { ref } from 'vue'
import { Howl } from 'howler'
import { gsap } from 'gsap'

import IntroComponent from '@/components/IntroComponent.vue'
import PlayButtonComponent from '@/components/PlayButtonComponent.vue'
import EarthComponent from '@/components/EarthComponent.vue'
import eerieSoundUrl from '@/assets/sounds/eerie_bg.mp3?url'
import EvolutionDefinitionComponent from '@/components/EvolutionDefinitionComponent.vue'

const bgSound = new Howl({
  src: [eerieSoundUrl],
  loop: true,
  volume: 0,
})

const isMounted = ref<boolean>(false)

function onEnter(el: Element, done: () => void): void {
  gsap.fromTo(el, { opacity: 0, y: -80 }, { opacity: 1, y: 0, duration: 1.2, onComplete: done })
}

function onLeave(el: Element, done: () => void): void {
  gsap.to(el, { opacity: 0, y: 20, duration: 0.6, onComplete: done })
}

function playSound() {
  if (!bgSound.playing()) {
    const bgSoundId = bgSound.play()
    bgSound.fade(0, 1, 4000, bgSoundId)
  }
  if (isMounted.value) isMounted.value = false
}
</script>

<template>
  <Transition appear mode="out-in" :css="false" @enter="onEnter" @leave="onLeave">
    <div :key="isMounted ? 'play' : 'content'">
      <PlayButtonComponent :playSound="playSound" v-if="isMounted" />
      <div v-else>
        <IntroComponent />
        <EvolutionDefinitionComponent />
        <EarthComponent />
      </div>
    </div>
  </Transition>
</template>
