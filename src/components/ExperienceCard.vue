<template>
  <article class="exp-card" ref="cardEl">
    <div class="exp-dot"></div>
    <div class="exp-contenido">
      <div class="exp-header">
        <h3 class="exp-empresa">{{ empresa }}</h3>
        <span class="exp-modalidad">{{ modalidad }}</span>
      </div>
      <p class="exp-rol">{{ rol }}</p>
      <p class="exp-periodo">{{ periodo }}</p>
      <ul class="exp-tareas">
        <li v-for="(tarea, i) in descripcion" :key="i">{{ tarea }}</li>
      </ul>
    </div>
  </article>
</template>

<script setup>
import { ref, onMounted, onUnmounted } from 'vue'

defineProps({
  empresa:    { type: String, required: true },
  rol:        { type: String, required: true },
  periodo:    { type: String, required: true },
  modalidad:  { type: String, default: '' },
  descripcion:{ type: Array,  required: true },
})

const cardEl = ref(null)
let observer = null

onMounted(() => {
  observer = new IntersectionObserver(
    ([entry]) => {
      if (entry.isIntersecting) {
        cardEl.value?.classList.add('visible')
        observer?.disconnect()
      }
    },
    { threshold: 0.15 }
  )
  if (cardEl.value) observer.observe(cardEl.value)
})

onUnmounted(() => observer?.disconnect())
</script>

<style scoped>
.exp-card {
  position: relative;
  padding-bottom: var(--espacio-lg);
  opacity: 0;
  transform: translateX(-18px);
  transition: opacity 0.5s ease, transform 0.5s ease;
}

.exp-card.visible {
  opacity: 1;
  transform: translateX(0);
}

.exp-dot {
  position: absolute;
  left: -2rem;
  top: 0.35rem;
  width: 14px;
  height: 14px;
  border-radius: 50%;
  background-color: var(--teal-medio);
  border: 2px solid var(--azul-noche);
  box-shadow: 0 0 0 3px rgba(90, 138, 146, 0.3);
  transform: translateX(-0.4375rem);
}

.exp-contenido {
  background-color: rgba(255, 255, 255, 0.05);
  border: 1px solid rgba(255, 255, 255, 0.1);
  border-radius: var(--radio);
  padding: var(--espacio-sm) var(--espacio-md);
}

.exp-header {
  display: flex;
  align-items: baseline;
  gap: var(--espacio-sm);
  flex-wrap: wrap;
  margin-bottom: 0.25rem;
}

.exp-empresa {
  color: var(--crema);
  font-size: 1.05rem;
}

.exp-modalidad {
  font-size: 0.72rem;
  background: rgba(90, 138, 146, 0.2);
  color: var(--teal-medio);
  padding: 0.15rem 0.5rem;
  border-radius: 999px;
  border: 1px solid rgba(90, 138, 146, 0.35);
}

.exp-rol {
  color: var(--teal-medio);
  font-weight: 600;
  font-size: 0.95rem;
  max-width: none;
}

.exp-periodo {
  color: var(--verde-suave);
  font-size: 0.8rem;
  margin-top: 0.2rem;
  max-width: none;
}

.exp-tareas {
  margin-top: var(--espacio-sm);
  display: flex;
  flex-direction: column;
  gap: 0.3rem;
  list-style: disc;
  padding-left: var(--espacio-sm);
}

.exp-tareas li {
  color: var(--gris-claro);
  font-size: 0.88rem;
  line-height: 1.55;
}
</style>
