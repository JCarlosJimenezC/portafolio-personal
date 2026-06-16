<template>
  <section id="about" class="about section">
    <div v-if="personal" class="container about-card" :class="{ 'fade-in': visible }">

      <!-- Foto circular centrada -->
      <div class="foto-ondas" v-if="!imgFallback">
        <div class="onda onda-suave onda-s1"></div>
        <div class="onda onda-suave onda-s2"></div>
        <div class="onda onda-freq onda-f1"></div>
        <div class="onda onda-freq onda-f2"></div>
        <img
          class="about-foto"
          :src="fotoSrc"
          :alt="`Imagen de presentación de ${personal.nombre}`"
          width="150"
          height="150"
          decoding="async"
          @error="imgFallback = true"
        />
      </div>
      <div v-else class="media-fallback foto-fallback">
        <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="currentColor" aria-hidden="true">
          <path d="M12 12c2.7 0 4.8-2.1 4.8-4.8S14.7 2.4 12 2.4 7.2 4.5 7.2 7.2 9.3 12 12 12zm0 2.4c-3.2 0-9.6 1.6-9.6 4.8v2.4h19.2v-2.4c0-3.2-6.4-4.8-9.6-4.8z"/>
        </svg>
        <p>Imagen de presentación de {{ personal.nombre }}</p>
      </div>

      <!-- Nombre y profesión -->
      <h1 class="about-nombre">{{ personal.nombre }}</h1>
      <p class="about-profesion">{{ personal.profesion }}</p>

      <!-- Perfil -->
      <div class="about-perfil-lineas" :class="{ animando: perfilAnimando }">
        <p
          v-for="(linea, i) in perfilLineas"
          :key="i"
          class="perfil-linea"
          :class="i % 2 === 0 ? 'desde-izq' : 'desde-der'"
        >
          <span
            v-for="(palabra, j) in linea.split(' ')"
            :key="j"
            class="perfil-palabra"
          >{{ palabra }}</span>
        </p>
      </div>

      <!-- Audio + Video en fila -->
      <div class="about-media-row">
        <div class="about-audio-col">
          <div class="audio-player">
            <p class="audio-titulo">Autopresentación</p>
            <template v-if="!audioFallback">
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
                @error="audioFallback = true"
              ></audio>
            </template>
            <div v-else class="media-fallback audio-fallback-msg">
              <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="currentColor" aria-hidden="true"><path d="M3 9v6h4l5 5V4L7 9H3zm13.5 3A4.5 4.5 0 0 0 14 7.97v8.05c1.48-.73 2.5-2.25 2.5-4.02zM14 3.23v2.06c2.89.86 5 3.54 5 6.71s-2.11 5.85-5 6.71v2.06c4.01-.91 7-4.49 7-8.77s-2.99-7.86-7-8.77z"/></svg>
              <p>Audio de presentación de {{ personal.nombre }}</p>
            </div>
          </div>
        </div>

        <div class="about-video-col">
          <video
            v-if="!videoFallback"
            class="about-video"
            :src="videoSrc"
            controls
            preload="metadata"
            @error="videoFallback = true"
          >
            Video de presentación de {{ personal.nombre }}
          </video>
          <div v-else class="media-fallback">
            <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="currentColor" aria-hidden="true"><path d="M17 10.5V7a1 1 0 0 0-1-1H4a1 1 0 0 0-1 1v10a1 1 0 0 0 1 1h12a1 1 0 0 0 1-1v-3.5l4 4v-11l-4 4z"/></svg>
            <p>Video de presentación de {{ personal.nombre }}</p>
          </div>
        </div>
      </div>

    </div>
  </section>
</template>

<script setup>
import { ref, onMounted, computed } from 'vue'
import fotoSrc from '@/assets/images/juanky.png'
import audioSrc from '@/assets/audio/juanky-audio.mp3'
import videoSrc from '@/assets/video/juanky-video.mp4'

const props = defineProps({
  personal: { type: Object, required: true }
})

const perfilLineas = computed(() => {
  if (!props.personal?.perfil) return []
  return props.personal.perfil.split(/(?<=[.!?])\s+/).filter(Boolean)
})


const visible = ref(false)
const perfilAnimando = ref(false)
const imgFallback = ref(false)
const videoFallback = ref(false)
const audioFallback = ref(false)
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
  const section = document.querySelector('#about')

  function activarPerfil() {
    perfilAnimando.value = false
    requestAnimationFrame(() => { perfilAnimando.value = true })
  }

  const observer = new IntersectionObserver(
    ([entry]) => {
      if (entry.isIntersecting) {
        visible.value = true
        activarPerfil()
      } else {
        perfilAnimando.value = false
      }
    },
    { threshold: 0.15 }
  )

  if (section) {
    observer.observe(section)
    // Trigger on load if already in viewport
    if (section.getBoundingClientRect().top < window.innerHeight) {
      visible.value = true
      activarPerfil()
    }
  }
})
</script>

<style scoped>
.about {
  background-color: var(--crema);
}

/* ── Tarjeta centrada ────────────────────────────────── */
.about-card {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: var(--espacio-md);
  max-width: 720px;
  margin: 0 auto;
  opacity: 0;
  transform: translateY(24px);
  transition: opacity 0.65s ease, transform 0.65s ease;
}

.about-card.fade-in {
  opacity: 1;
  transform: translateY(0);
}

/* ── Foto ────────────────────────────────────────────── */
.foto-ondas {
  position: relative;
  display: inline-flex;
  justify-content: center;
  align-items: center;
}

.onda {
  position: absolute;
  inset: 0;
  border-radius: 50%;
  opacity: 0;
  pointer-events: none;
}

/* Anillos suaves */
.onda-suave {
  border: 2px solid var(--verde-2);
  animation: onda-circulo 2.6s ease-out infinite;
}
.onda-s1 { animation-delay: 0s; }
.onda-s2 { animation-delay: 1.3s; }

/* Anillos tipo frecuencia: segmentados + rotan */
.onda-freq {
  background: repeating-conic-gradient(
    from 0deg,
    var(--verde-2) 0deg 5deg,
    transparent 5deg 16deg
  );
  -webkit-mask: radial-gradient(farthest-side, transparent calc(100% - 3px), #fff calc(100% - 3px));
  mask: radial-gradient(farthest-side, transparent calc(100% - 3px), #fff calc(100% - 3px));
  animation: onda-frecuencia 2.6s ease-out infinite;
}
.onda-f1 { animation-delay: 0.65s; }
.onda-f2 { animation-delay: 1.95s; }

@keyframes onda-circulo {
  0%   { transform: scale(1);   opacity: 0.7; }
  100% { transform: scale(1.75); opacity: 0; }
}

@keyframes onda-frecuencia {
  0%   { transform: scale(1)    rotate(0deg);   opacity: 0.7; }
  100% { transform: scale(1.75) rotate(135deg); opacity: 0; }
}

.about-foto {
  width: 140px;
  height: 140px;
  border-radius: 50%;
  object-fit: cover;
  border: 3px solid var(--verde-2);
  box-shadow: 0 0 20px rgba(0, 200, 81, 0.5);
}

.foto-fallback {
  width: 140px;
  height: 140px;
  border-radius: 50%;
  border: 2px dashed rgba(0, 200, 81, 0.35);
}

/* ── Nombre y profesión ──────────────────────────────── */
.about-nombre {
  color: var(--neon-cyan);
  text-align: center;
  margin: 0;
}

.about-profesion {
  color: var(--teal-medio);
  font-size: 1rem;
  font-style: italic;
  text-align: center;
  margin: 0;
  max-width: none;
}

/* ── Perfil ──────────────────────────────────────────── */
.about-perfil-lineas {
  display: flex;
  flex-direction: column;
  gap: 0.6rem;
  width: 100%;
}

@keyframes deslizarIzq {
  from { opacity: 0; transform: translateX(-32px); }
  to   { opacity: 1; transform: translateX(0); }
}

@keyframes deslizarDer {
  from { opacity: 0; transform: translateX(32px); }
  to   { opacity: 1; transform: translateX(0); }
}

.perfil-linea {
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
  gap: 0.3em;
  font-size: 0.95rem;
  line-height: 1.75;
  max-width: none;
  opacity: 0;
  animation-duration: 0.55s;
  animation-fill-mode: forwards;
  animation-timing-function: ease-out;
}

.perfil-palabra {
  color: rgba(255, 255, 255, 0.9);
  cursor: default;
  transition: color var(--transicion);
}

.perfil-palabra:hover { color: var(--verde-2); }

.about-perfil-lineas.animando .perfil-linea.desde-izq { animation-name: deslizarIzq; }
.about-perfil-lineas.animando .perfil-linea.desde-der { animation-name: deslizarDer; }

.about-perfil-lineas.animando .perfil-linea:nth-child(1) { animation-delay: 0.1s; }
.about-perfil-lineas.animando .perfil-linea:nth-child(2) { animation-delay: 0.35s; }
.about-perfil-lineas.animando .perfil-linea:nth-child(3) { animation-delay: 0.6s; }
.about-perfil-lineas.animando .perfil-linea:nth-child(4) { animation-delay: 0.85s; }

/* ── Fila media ──────────────────────────────────────── */
.about-media-row {
  display: flex;
  flex-direction: column;
  gap: var(--espacio-md);
  width: 100%;
}

.about-audio-col {
  display: flex;
  flex-direction: column;
}

.about-video-col {
  display: flex;
  flex-direction: column;
}

/* ── Video ───────────────────────────────────────────── */
.about-video {
  width: 100%;
  display: block;
  border-radius: var(--radio);
  box-shadow: var(--sombra-md);
  background: #000;
  object-fit: contain;
  transition: transform 0.3s ease, box-shadow 0.3s ease;
}

.about-video-col:hover .about-video {
  transform: translateY(-6px);
  box-shadow: 0 10px 32px rgba(0, 200, 81, 0.45);
}

/* ── Audio ───────────────────────────────────────────── */
.audio-player {
  background-color: var(--azul-noche);
  border-radius: var(--radio);
  padding: var(--espacio-sm) var(--espacio-md);
  box-shadow: var(--sombra-md);
  flex: 1;
  display: flex;
  flex-direction: column;
  justify-content: center;
  gap: var(--espacio-xs);
  transition: transform 0.3s ease, box-shadow 0.3s ease;
}

.about-audio-col:hover .audio-player {
  transform: translateY(-6px);
  box-shadow: 0 10px 32px rgba(0, 200, 81, 0.45);
}

.audio-titulo {
  color: var(--verde-suave);
  font-size: 0.8rem;
  font-weight: 600;
  letter-spacing: 0.05em;
  text-transform: uppercase;
  text-align: center;
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

.audio-btn svg { width: 20px; height: 20px; }

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
  font-size: 0.8rem;
  white-space: nowrap;
  font-variant-numeric: tabular-nums;
}

/* ── Fallbacks ───────────────────────────────────────── */
.media-fallback {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  gap: 0.6rem;
  padding: var(--espacio-md);
  border: 2px dashed rgba(0, 200, 81, 0.35);
  border-radius: var(--radio);
  color: var(--gris-claro);
  text-align: center;
  min-height: 100px;
}

.media-fallback svg { width: 32px; height: 32px; color: var(--verde-2); opacity: 0.6; }
.media-fallback p   { font-size: 0.85rem; color: var(--gris-claro); max-width: none; }

.audio-fallback-msg { min-height: auto; padding: var(--espacio-xs) var(--espacio-sm); }

/* ── Desktop: audio y video en fila ─────────────────── */
@media (min-width: 768px) {
  .about-media-row {
    flex-direction: row;
    align-items: stretch;
  }

  .about-audio-col { flex: 1; }
  .about-video-col { flex: 1; }

  .audio-player { height: 100%; }
}
</style>
