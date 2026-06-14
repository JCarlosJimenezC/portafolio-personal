<template>
  <section id="about" class="about section">
    <div v-if="personal" class="container about-grid" :class="{ 'fade-in': visible }">
      <div class="about-foto-col">
        <img
          class="about-foto"
          :src="fotoSrc"
          :alt="`Foto de ${personal.nombre}`"
          width="280"
          height="280"
          decoding="async"
        />
      </div>

      <div class="about-texto-col">
        <h1 class="about-nombre">{{ personal.nombre }}</h1>
        <p class="about-profesion">{{ personal.profesion }}</p>
        <p class="about-perfil">{{ personal.perfil }}</p>

        <div class="audio-player">
          <p class="audio-titulo">Autopresentación</p>
          <div class="audio-controles">
            <button
              class="audio-btn"
              @click="toggleAudio"
              :aria-label="reproduciendo ? 'Pausar' : 'Reproducir'"
            >
              <svg v-if="!reproduciendo" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="currentColor" aria-hidden="true">
                <path d="M8 5v14l11-7z"/>
              </svg>
              <svg v-else xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="currentColor" aria-hidden="true">
                <path d="M6 19h4V5H6v14zm8-14v14h4V5h-4z"/>
              </svg>
            </button>

            <div class="audio-progreso-wrapper" @click="buscarEnAudio" role="progressbar" :aria-valuenow="Math.round(progreso)">
              <div class="audio-progreso-barra">
                <div class="audio-progreso-fill" :style="{ width: progreso + '%' }"></div>
              </div>
            </div>

            <span class="audio-tiempo">{{ tiempoActual }} / {{ duracionTotal }}</span>
          </div>

          <audio
            ref="audioEl"
            :src="audioSrc"
            preload="metadata"
            @timeupdate="actualizarTiempo"
            @loadedmetadata="cargarDuracion"
            @ended="reproduciendo = false"
          ></audio>
        </div>
      </div>
    </div>
  </section>
</template>

<script setup>
import { ref, onMounted } from 'vue'
import fotoSrc from '@/assets/images/juanky.png'
import audioSrc from '@/assets/audio/juanky-audio.mp3'

defineProps({
  personal: { type: Object, required: true }
})

const visible = ref(false)
const audioEl = ref(null)
const reproduciendo = ref(false)
const progreso = ref(0)
const tiempoActual = ref('0:00')
const duracionTotal = ref('0:00')

function formatearTiempo(segundos) {
  const m = Math.floor(segundos / 60)
  const s = Math.floor(segundos % 60).toString().padStart(2, '0')
  return `${m}:${s}`
}

function toggleAudio() {
  if (!audioEl.value) return
  if (reproduciendo.value) {
    audioEl.value.pause()
    reproduciendo.value = false
  } else {
    audioEl.value.play()
    reproduciendo.value = true
  }
}

function actualizarTiempo() {
  const { currentTime, duration } = audioEl.value
  progreso.value = duration ? (currentTime / duration) * 100 : 0
  tiempoActual.value = formatearTiempo(currentTime)
}

function cargarDuracion() {
  duracionTotal.value = formatearTiempo(audioEl.value.duration)
}

function buscarEnAudio(e) {
  if (!audioEl.value?.duration) return
  const rect = e.currentTarget.getBoundingClientRect()
  const porcentaje = (e.clientX - rect.left) / rect.width
  audioEl.value.currentTime = audioEl.value.duration * porcentaje
}

onMounted(() => {
  requestAnimationFrame(() => { visible.value = true })
})
</script>

<style scoped>
.about {
  background-color: var(--crema);
}

.about-grid {
  display: grid;
  grid-template-columns: 1fr;
  gap: var(--espacio-lg);
  align-items: center;
  opacity: 0;
  transform: translateY(24px);
  transition: opacity 0.65s ease, transform 0.65s ease;
}

.about-grid.fade-in {
  opacity: 1;
  transform: translateY(0);
}

.about-foto-col {
  display: flex;
  justify-content: center;
}

.about-foto {
  width: 200px;
  height: 200px;
  border-radius: 50%;
  object-fit: cover;
  border: 4px solid var(--teal-medio);
  box-shadow: var(--sombra-md);
}

.about-texto-col {
  display: flex;
  flex-direction: column;
  gap: var(--espacio-sm);
}

.about-nombre {
  color: var(--azul-noche);
}

.about-profesion {
  color: var(--teal-medio);
  font-weight: 500;
  font-size: 1.1rem;
  max-width: none;
}

.about-perfil {
  color: var(--azul-noche);
  line-height: 1.75;
}

.audio-player {
  background-color: var(--azul-noche);
  border-radius: var(--radio);
  padding: var(--espacio-sm) var(--espacio-md);
  margin-top: var(--espacio-xs);
}

.audio-titulo {
  color: var(--verde-suave);
  font-size: 0.8rem;
  font-weight: 600;
  letter-spacing: 0.05em;
  text-transform: uppercase;
  margin-bottom: var(--espacio-xs);
  max-width: none;
}

.audio-controles {
  display: flex;
  align-items: center;
  gap: var(--espacio-sm);
}

.audio-btn {
  background: var(--teal-medio);
  border: none;
  border-radius: 50%;
  width: 40px;
  height: 40px;
  display: flex;
  align-items: center;
  justify-content: center;
  cursor: pointer;
  flex-shrink: 0;
  color: #fff;
  transition: background-color var(--transicion), transform var(--transicion);
}

.audio-btn:hover {
  background: var(--verde-suave);
  transform: scale(1.08);
}

.audio-btn svg {
  width: 20px;
  height: 20px;
}

.audio-progreso-wrapper {
  flex: 1;
  cursor: pointer;
  padding-block: 0.6rem;
}

.audio-progreso-barra {
  height: 4px;
  background-color: rgba(255, 255, 255, 0.15);
  border-radius: 2px;
  overflow: hidden;
}

.audio-progreso-fill {
  height: 100%;
  background-color: var(--teal-medio);
  border-radius: 2px;
  transition: width 0.1s linear;
}

.audio-tiempo {
  color: var(--gris-claro);
  font-size: 0.75rem;
  white-space: nowrap;
  font-variant-numeric: tabular-nums;
}

@media (min-width: 768px) {
  .about-grid {
    grid-template-columns: 260px 1fr;
  }

  .about-foto {
    width: 240px;
    height: 240px;
  }
}
</style>
