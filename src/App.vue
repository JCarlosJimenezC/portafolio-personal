<template>
  <AppHeader v-if="portfolio" :personal="portfolio.personal" />
  <main class="main-content">
    <div v-if="portfolio">
      <About       :personal="portfolio.personal" />
      <Gallery     :proyectos="portfolio.proyectos" />
      <Skills      :habilidades="portfolio.habilidades" />
      <Education   :educacion="portfolio.educacion" />
      <Experiencia :experiencia="portfolio.experiencia" />
      <Contact     :contacto="portfolio.personal.contacto" />
    </div>
    <div v-else class="loading" aria-live="polite">Cargando...</div>
  </main>
</template>

<script setup>
import { ref, onMounted } from 'vue'
import AppHeader   from './components/Header.vue'
import About       from './components/About.vue'
import Gallery     from './components/Gallery.vue'
import Skills      from './components/Skills.vue'
import Education   from './components/Education.vue'
import Experiencia from './components/Experiencia.vue'
import Contact     from './components/Contact.vue'

const portfolio = ref(null)

onMounted(async () => {
  try {
    const res = await fetch(`${import.meta.env.BASE_URL}data/portafolio.json`)
    portfolio.value = await res.json()
  } catch (err) {
    console.error('Error cargando portfolio:', err)
  }
})
</script>

<style>
.main-content {
  padding-top: 3.5rem;
}

.loading {
  display: flex;
  justify-content: center;
  align-items: center;
  min-height: 100vh;
  color: var(--teal-medio);
  font-size: 1rem;
}

@media (min-width: 768px) {
  .main-content {
    margin-left: 250px;
    padding-top: 0;
  }
}
</style>
