<script setup lang="ts">
import { Howl } from 'howler'

import IntroComponent from '@/components/IntroComponent.vue'
import eerieSoundUrl from '@/assets/sounds/eerie.mp3?url'
import PlayButtonComponent from '@/components/PlayButtonComponent.vue'
import EarthComponent from '@/components/EarthComponent.vue'

const bgSound = new Howl({
  src: [eerieSoundUrl],
  loop: true,
})

function playSound() {
  if (!bgSound.playing()) bgSound.play()
}
</script>

<template>
  <PlayButtonComponent :playSound="playSound" />
  <IntroComponent />
  <EarthComponent />
</template>
