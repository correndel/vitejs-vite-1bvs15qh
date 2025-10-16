<script setup>
import { ref } from 'vue'

const selectedRoles = ref([])
const showModal = ref(false)
const countdown = ref(15 * 60) // 15 minutes in seconds
const isPaused = ref(false)
let countdownInterval = null

function toggleRole(role) {
  const index = selectedRoles.value.indexOf(role)
  if (index === -1) {
    selectedRoles.value.push(role)
  } else {
    selectedRoles.value.splice(index, 1)
  }
}

function isSelected(role) {
  return selectedRoles.value.includes(role)
}

function formatTime(seconds) {
  const mins = Math.floor(seconds / 60)
  const secs = seconds % 60
  return `${String(mins).padStart(2, '0')}:${String(secs).padStart(2, '0')}`
}

function startCountdown() {
  countdown.value = 15 * 60
  showModal.value = true
  isPaused.value = false

  countdownInterval = setInterval(() => {
    if (!isPaused.value && countdown.value > 0) {
      countdown.value--
    }
    if (countdown.value <= 0) {
      clearInterval(countdownInterval)
      showModal.value = false
    }
  }, 1000)
}

function togglePause() {
  isPaused.value = !isPaused.value
}

function closeModal() {
  clearInterval(countdownInterval)
  showModal.value = false
}

function addRole(role) {
  if (!selectedRoles.value.includes(role)) {
    selectedRoles.value.push(role)
  }
}
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
  { src: bunnyx, alt: 'Bunnyx' },
  { src: caprikid, alt: 'Caprikid' },
  { src: carapace, alt: 'Carapace' },
  { src: catnoir, alt: 'Cat Noir' },
  { src: hawkmoth, alt: 'Hawk Moth' },
  { src: kingmonkey, alt: 'King Monkey' },
  { src: ladybug, alt: 'Ladybug' },
  { src: masterfu, alt: 'Master Fu' },
  { src: mayura, alt: 'Mayura' },
  { src: minautorox, alt: 'Minautorox' },
  { src: misshound, alt: 'Miss Hound' },
  { src: pegasus, alt: 'Pegasus' },
  { src: pigella, alt: 'Pigella' },
  { src: polymouse, alt: 'Polymouse' },
  { src: purpletigress, alt: 'Purple Tigress' },
  { src: queenbee, alt: 'Queen Bee' },
  { src: renarouge, alt: 'Rena Rouge' },
  { src: roosterbold, alt: 'Rooster Bold' },
  { src: ryoko, alt: 'Ryoko' },
  { src: vesperia, alt: 'Vesperia' },
  { src: viperion, alt: 'Viperion' },

]
</script>

<template>
  <h1>One Night Ultimate Miraculous</h1>
  <div class="container">
    <div class="grid">
      <div
        class="role-button"
        v-for="(img, idx) in pngImages"
        :key="idx"
        :class="{ selected: isSelected(img.alt), deselected: !isSelected(img.alt) }"
        @click="toggleRole(img.alt)"
      >
        <span class="label">{{ img.alt }}</span>
        <img :src="img.src" :alt="img.alt" />
      </div>
    </div>
  </div>

  <div class="selected-list">
    <h2>Selected Roles:</h2>
    <p>{{ selectedRoles.join(', ') || 'None selected yet.' }}</p>
  </div>

  <div class="play-button-wrapper">
    <button class="play-button" @click="startCountdown">
      Play ({{ selectedRoles.length }})
    </button>
  </div>

  <!-- Modal -->
  <div v-if="showModal" class="modal-overlay">
    <button class="close-button" @click="closeModal">×</button>
    <div class="modal-content">
      <h1>{{ formatTime(countdown) }}</h1>
      <button class="pause-button" @click="togglePause">
        {{ isPaused ? 'Resume' : 'Pause' }}
      </button>
    </div>
  </div>
</template>
