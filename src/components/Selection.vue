<script setup>
import { ref } from 'vue'

// Reactive state
const selectedRoles = ref([])
const showModal = ref(false)
const countdown = ref(15 * 60)
const isPaused = ref(false)
const bgVolume = ref(0.5)
const roleVolume = ref(0.8)
const currentRole = ref(null)

// Audio and timer control
let countdownInterval = null
let bgMusic = null
let roleAudioQueue = []
let currentRoleAudio = null
let roleAudioIndex = 0

// Role selection
function toggleRole(role) {
  const index = selectedRoles.value.indexOf(role)
  index === -1
    ? selectedRoles.value.push(role)
    : selectedRoles.value.splice(index, 1)
}

function isSelected(role) {
  return selectedRoles.value.includes(role)
}

// Timer formatting
function formatTime(seconds) {
  const mins = Math.floor(seconds / 60)
  const secs = seconds % 60
  return `${String(mins).padStart(2, '0')}:${String(secs).padStart(2, '0')}`
}

// Countdown lifecycle
function startCountdown() {
  countdown.value = 15 * 60
  showModal.value = true
  isPaused.value = false

  playBackgroundMusic()
  playRoleAudioSequence()

  countdownInterval = setInterval(() => {
    if (!isPaused.value && countdown.value > 0) {
      countdown.value--
    }
    if (countdown.value <= 0) {
      clearInterval(countdownInterval)
      closeModal()
    }
  }, 1000)
}

function togglePause() {
  isPaused.value = !isPaused.value
  if (isPaused.value) {
    bgMusic?.pause()
    currentRoleAudio?.pause()
  } else {
    bgMusic?.play()
    currentRoleAudio?.play()
  }
}

function closeModal() {
  clearInterval(countdownInterval)
  stopAllAudio()
  showModal.value = false
}

// Audio playback
function playBackgroundMusic() {
  bgMusic = new Audio('onum_bg_music_standard.mp3')
  bgMusic.loop = true
  bgMusic.volume = bgVolume.value
  bgMusic.play()
}

function playRoleAudioSequence() {
  roleAudioQueue = selectedRoles.value.map(role => {
    const audio = new Audio(`audio/${role.toLowerCase().replace(/\s+/g, '_')}.mp3`)
    audio.volume = roleVolume.value
    return { role, audio }
  })
  roleAudioIndex = 0
  playNextRoleAudio()
}

function playNextRoleAudio() {
  if (roleAudioIndex >= roleAudioQueue.length) return

  const { role, audio } = roleAudioQueue[roleAudioIndex]
  currentRole.value = role
  currentRoleAudio = audio
  currentRoleAudio.play()
  currentRoleAudio.onended = () => {
    roleAudioIndex++
    playNextRoleAudio()
  }
}

function stopAllAudio() {
  bgMusic?.pause()
  bgMusic = null

  currentRoleAudio?.pause()
  currentRoleAudio = null

  roleAudioQueue = []
  roleAudioIndex = 0
  currentRole.value = null
}

// Image imports
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
  <main>
    <h1>Select Roles</h1>

    <!-- Role Selection Grid -->
    <section class="container">
      <div class="grid">
        <div
          v-for="(img, idx) in pngImages"
          :key="idx"
          class="role-button"
          :class="{ selected: isSelected(img.alt), deselected: !isSelected(img.alt) }"
          @click="toggleRole(img.alt)"
        >
          <span class="label">{{ img.alt }}</span>
          <img :src="img.src" :alt="img.alt" />
        </div>
      </div>
    </section>

    <!-- Selected Roles Summary -->
    <section class="selected-list">
      <h2>Selected Roles:</h2>
      <p>{{ selectedRoles.join(', ') || 'None selected yet.' }}</p>
    </section>

    <!-- Play Button -->
    <section class="play-button-wrapper">
      <button class="play-button" @click="startCountdown">
        Play ({{ selectedRoles.length }})
      </button>
    </section>

    <!-- Countdown Modal -->
    <div v-if="showModal" class="modal-overlay">
      <button class="close-button" @click="closeModal" aria-label="Close modal">×</button>

      <div class="modal-content">
        <!-- Role Image -->
        <img
          v-if="currentRole"
          :src="pngImages.find(img => img.alt === currentRole)?.src"
          :alt="currentRole"
          class="role-image"
        />

        <!-- Countdown Timer -->
        <h1 class="timer">{{ formatTime(countdown) }}</h1>

        <!-- Volume Controls -->
        <div class="volume-controls">
          <label>
            Background Volume:
            <input
              type="range"
              min="0"
              max="1"
              step="0.01"
              v-model="bgVolume"
              @input="bgMusic && (bgMusic.volume = bgVolume)"
            />
          </label>
          <label>
            Role Audio Volume:
            <input
              type="range"
              min="0"
              max="1"
              step="0.01"
              v-model="roleVolume"
              @input="currentRoleAudio && (currentRoleAudio.volume = roleVolume)"
            />
          </label>
        </div>

        <!-- Pause/Resume Button -->
        <button class="pause-button" @click="togglePause">
          {{ isPaused ? 'Resume' : 'Pause' }}
        </button>

        <!-- Audio Info -->
        <p class="audio-info">
          Now playing: {{ currentRole ? currentRole.toLowerCase().replace(/\s+/g, '_') + '.mp3' : 'None' }}
        </p>
      </div>
    </div>
  </main>
</template>