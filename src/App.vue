<script setup>
import { ref, onMounted } from 'vue'
import TerminalScreen from './components/TerminalScreen.vue'
import MainDashboard from './components/MainDashboard.vue'

const isTerminalDone = ref(false)
const audio = ref(null)

onMounted(() => {
  // Preload audio
  audio.value = new Audio('/src/assets/levelup.mp3')
  audio.value.load()
})

const handleTerminalFinished = () => {
  isTerminalDone.value = true
}

const playLevelUp = () => {
  if (audio.value) {
    audio.value.currentTime = 0
    audio.value.play().catch(e => console.warn('Audio play blocked:', e))
  }
}
</script>

<template>
  <div class="app-container bg-dark text-white">
    <transition name="fade" mode="out-in">
      <TerminalScreen 
        v-if="!isTerminalDone" 
        @finished="handleTerminalFinished" 
      />
      <MainDashboard 
        v-else 
        @play-audio="playLevelUp" 
      />
    </transition>
  </div>
</template>

<style>
body {
  margin: 0;
  padding: 0;
  background-color: #121212;
  font-family: 'Montserrat', sans-serif;
  overflow-x: hidden;
}

.app-container {
  min-height: 100vh;
  display: flex;
  flex-direction: column;
}

/* Transição suave de Fade */
.fade-enter-active,
.fade-leave-active {
  transition: opacity 1s ease;
}

.fade-enter-from,
.fade-leave-to {
  opacity: 0;
}

/* Estilo Monospace para o Terminal */
.font-mono {
  font-family: 'Fira Code', 'Courier New', monospace;
}
</style>
