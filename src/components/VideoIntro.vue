<template>
  <section id="video" class="video-intro section">
    <div class="container">
      <h2 class="titulo-seccion">Conóceme</h2>
      <div class="video-wrapper">
        <video
          class="video-player"
          :src="videoSrc"
          controls
          preload="metadata"
        >
          Tu navegador no soporta reproducción de video.
        </video>
      </div>
    </div>
  </section>
</template>

<script setup>
import videoSrc from '@/assets/video/juanky-video.mp4'
</script>

<style scoped>
.video-intro {
  background-color: var(--azul-noche);
}

.titulo-seccion {
  color: var(--crema);
  margin-bottom: var(--espacio-lg);
}

.titulo-seccion::after {
  content: '';
  display: block;
  width: 48px;
  height: 3px;
  background: var(--teal-medio);
  margin-top: 0.5rem;
  border-radius: 2px;
}

.video-wrapper {
  max-width: 800px;
  margin-inline: auto;
  border-radius: var(--radio);
  overflow: hidden;
  box-shadow: var(--sombra-md);
}

.video-player {
  width: 100%;
  display: block;
  background: #000;
}
</style>
