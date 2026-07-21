<script setup lang="ts">
import { OrbitControls, useGLTF } from '@tresjs/cientos'
import { TresCanvas } from '@tresjs/core'
import { PerspectiveCamera, Vector3, type Group } from 'three'
import gsap from 'gsap'
import { nextTick, ref, watch } from 'vue'
import { ScrollTrigger } from 'gsap/ScrollTrigger'

const cameraPosition = new Vector3(170, 170, 170)
const directionalLightPosition = new Vector3(150, 150, 150)

const earthGroup = ref<Group | null>(null)
const entranceGroup = ref<Group | null>(null)
const camera = ref<PerspectiveCamera | null>(null)
const { state: earthModel } = useGLTF('/models/earth/scene.gltf')

function animateEarth() {
  if (!earthGroup.value || !entranceGroup.value || !camera.value) return

  const rotatingGroup = earthGroup.value
  const group = entranceGroup.value
  const currentCamera = camera.value

  // Keep a constant base rotation running beneath the entrance animation.
  rotatingGroup.rotation.set(0, 0, 0)
  gsap.to(rotatingGroup.rotation, {
    y: Math.PI * 2,
    duration: 20,
    ease: 'none',
    repeat: -1,
  })

  // Initial model state
  group.scale.set(0.01, 0.01, 0.01)
  group.rotation.set(0, -Math.PI, 0)

  // Initial camera state
  currentCamera.position.set(260, 260, 260)
  currentCamera.lookAt(0, 0, 0)

  const timeline = gsap.timeline({
    defaults: {
      ease: 'power3.out',
    },
    scrollTrigger: {
      trigger: '.earth-section',
      start: 'top 80%',
    },
  })

  timeline
    .to('.earth-section', { filter: 'blur(0px)', duration: 2, ease: 'power1.in' }, '<')
    .to(
      group.scale,
      {
        x: 1,
        y: 1,
        z: 1,
        duration: 2,
      },
      '<',
    )
    .to(group.rotation, { y: 0, duration: 2.5 }, '<')
    .to(
      currentCamera.position,
      {
        x: cameraPosition.x,
        y: cameraPosition.y,
        z: cameraPosition.z,
        duration: 2.5,
        onUpdate: () => {
          currentCamera.lookAt(0, 0, 0)
        },
      },
      '<',
    )

  ScrollTrigger.refresh()
}

watch(
  earthModel,
  async (model) => {
    if (!model) return

    await nextTick()
    animateEarth()
  },
  { flush: 'post' },
)
</script>

<template>
  <section class="earth-section section">
    <TresCanvas clear-color="#00000000">
      <TresPerspectiveCamera ref="camera" :position="cameraPosition" :look-at="[0, 0, 0]" />

      <TresAmbientLight :intensity="0.4" />
      <TresDirectionalLight :position="directionalLightPosition" />

      <TresGroup ref="earthGroup">
        <TresGroup ref="entranceGroup">
          <primitive v-if="earthModel" :object="earthModel.scene" />
        </TresGroup>
      </TresGroup>
      <OrbitControls :enable-zoom="false" :enable-pan="false" />
    </TresCanvas>
  </section>
</template>

<style scoped>
.earth-section {
  filter: blur(4px);
  background-image: url('../assets/backgrounds/stars_bg.jpg');
  background-size: cover;
  background-position: center;
}
</style>
