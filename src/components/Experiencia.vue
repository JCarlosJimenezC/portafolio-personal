<template>
  <section id="experiencia" class="experiencia section">
    <div class="container">
      <h2 class="titulo-seccion">Experiencia</h2>
      <div class="timeline">
        <ExperienceCard
          v-for="exp in props.experiencia"
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
import ExperienceCard from './ExperienceCard.vue'

const props = defineProps({ experiencia: { type: Array, required: true } })
</script>

<style scoped>
.experiencia {
  background-color: var(--crema);
}

.titulo-seccion {
  color: var(--verde-2);
  margin-bottom: var(--espacio-lg);
}

.titulo-seccion::after {
  content: '';
  display: block;
  width: 48px;
  height: 3px;
  background: var(--verde-2);
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
