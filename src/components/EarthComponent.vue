<script setup lang="ts">
import { GLTFModel, OrbitControls } from '@tresjs/cientos'
import { TresCanvas } from '@tresjs/core'
import { Vector3 } from 'three'

const cameraPosition = new Vector3(170, 170, 170)
const directionalLightPosition = new Vector3(150, 150, 150)
</script>

<template>
  <section class="earth-section">
    <TresCanvas clear-color="#00A0FA">
      <TresPerspectiveCamera :position="cameraPosition" :look-at="[0, 0, 0]" />

      <TresAmbientLight :intensity="1" />
      <TresDirectionalLight :position="directionalLightPosition" />

      <Suspense>
        <GLTFModel path="/models/earth/scene.gltf" />
      </Suspense>
      <OrbitControls :enable-zoom="false" :enable-pan="false" />
    </TresCanvas>
  </section>
</template>

<style scoped>
.earth-section {
  width: 100%;
  height: 100vh;
  scroll-snap-align: start;
}
</style>
