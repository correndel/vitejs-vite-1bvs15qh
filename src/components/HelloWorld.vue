<script setup>
import { ref } from 'vue'

defineProps({
  msg: String,
})

const count = ref(0)

// Import PNG images from src/assets
import bunnyx from '../assets/bunnyx.png'
import caprikid from '../assets/caprikid.png'
import carapace from '../assets/carapace.png'
import catnoir from '../assets/catnoir.png'
import hawkmoth from '../assets/hawkmoth.png'
import kingmonkey from '../assets/kingmonkey.png'
import ladybug from '../assets/ladybug.png'
import masterfu from '../assets/masterfu.png'
import mayura from '../assets/mayura.png'
import minautorox from '../assets/minautorox.png'
import misshound from '../assets/misshound.png'
import pegasus from '../assets/pegasus.png'
import pigella from '../assets/pigella.png'
import polymouse from '../assets/polymouse.png'
import purpletigress from '../assets/purpletigress.png'
import queenbee from '../assets/queenbee.png'
import renarouge from '../assets/renarouge.png'
import roosterbold from '../assets/roosterbold.png'
import ryoko from '../assets/ryoko.png'
import vesperia from '../assets/vesperia.png'
import viperion from '../assets/viperion.png'


const pngImages = [
  { src: bunnyx, alt: 'bunnyx.png' },
  { src: caprikid, alt: 'caprikid.png' },
  { src: carapace, alt: 'carapace.png' },
  { src: catnoir, alt: 'catnoir.png' },
  { src: hawkmoth, alt: 'hawkmoth.png' },
  { src: kingmonkey, alt: 'kingmonkey.png' },
  { src: ladybug, alt: 'ladybug.png' },
  { src: masterfu, alt: 'masterfu.png' },
  { src: mayura, alt: 'mayura.png' },
  { src: minautorox, alt: 'minautorox.png' },
  { src: misshound, alt: 'misshound.png' },
  { src: pegasus, alt: 'pegasus.png' },
  { src: pigella, alt: 'pigella.png' },
  { src: polymouse, alt: 'polymouse.png' },
  { src: purpletigress, alt: 'purpletigress.png' },
  { src: queenbee, alt: 'queenbee.png' },
  { src: renarouge, alt: 'renarouge.png' },
  { src: roosterbold, alt: 'roosterbold.png' },
  { src: ryoko, alt: 'ryoko.png' },
  { src: vesperia, alt: 'vesperia.png' },
  { src: viperion, alt: 'viperion.png' },

]
</script>

<template>
  <h1>{{ msg }}</h1>

  <div class="card">
    <button type="button" @click="count++">count is {{ count }}</button>
    <p>
      Edit
      <code>components/HelloWorld.vue</code> to test HMR
    </p>

    <!-- Buttons for each PNG image -->
    <div v-for="(img, idx) in pngImages" :key="idx" style="margin-top: 8px;">
      <button type="button">
        <img :src="img.src" :alt="img.alt" style="height: 32px; vertical-align: middle;" />
        {{ img.alt }}
      </button>
    </div>
  </div>

  <p>
    Check out
    <a href="https://vuejs.org/guide/quick-start.html#local" target="_blank"
      >create-vue</a
    >, the official Vue + Vite starter
  </p>
  <p>
    Learn more about IDE Support for Vue in the
    <a
      href="https://vuejs.org/guide/scaling-up/tooling.html#ide-support"
      target="_blank"
      >Vue Docs Scaling up Guide</a
    >.
  </p>
  <p class="read-the-docs">Click on the Vite and Vue logos to learn more</p>
</template>

<style scoped>
.read-the-docs {
  color: #888;
}
</style>
