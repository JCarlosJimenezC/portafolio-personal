<template>
  <section id="experiencia" class="experiencia section">
    <div class="container">
      <h2 class="titulo-seccion">Experiencia</h2>
      <div class="timeline">
        <ExperienceCard
          v-for="exp in experiencia"
          :key="exp.id"
          :empresa="exp.empresa"
          :rol="exp.puesto"
          :periodo="`${exp.periodo} · ${exp.duracion}`"
          :modalidad="exp.modalidad"
          :descripcion="exp.tareas"
        />
      </div>
    </div>
  </section>
</template>

<script setup>
import { ref, onMounted } from 'vue'
import ExperienceCard from './ExperienceCard.vue'

const experiencia = ref([])

onMounted(async () => {
  try {
    const res = await fetch(`${import.meta.env.BASE_URL}data/portafolio.json`)
    const datos = await res.json()
    experiencia.value = datos.experiencia
  } catch (err) {
    console.error('Error cargando experiencia:', err)
  }
})
</script>

<style scoped>
.experiencia {
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

.timeline {
  position: relative;
  padding-left: 2.5rem;
}

.timeline::before {
  content: '';
  position: absolute;
  left: 0.75rem;
  top: 0.4rem;
  bottom: 0.4rem;
  width: 2px;
  background: var(--teal-medio);
  opacity: 0.35;
  border-radius: 1px;
}
</style>
