<template>
  <section id="educacion" class="educacion section">
    <div class="container">
      <h2 class="titulo-seccion">Educación</h2>
      <div class="timeline">
        <article
          v-for="(item, i) in educacion"
          :key="item.id"
          class="edu-item"
          :ref="el => { if (el) itemRefs[i] = el }"
        >
          <div class="edu-dot"></div>
          <div class="edu-contenido">
            <span class="edu-estado" :class="{ activo: item.estado === 'En curso' }">
              {{ item.estado }}
            </span>
            <h3 class="edu-titulo">{{ item.titulo }}</h3>
            <p class="edu-institucion">{{ item.institucion }}</p>
            <p class="edu-periodo">{{ item.periodo }}</p>
          </div>
        </article>
      </div>
    </div>
  </section>
</template>

<script setup>
import { ref, onMounted, onUnmounted } from 'vue'

const educacion = ref([])
const itemRefs = ref([])
let observer = null

onMounted(async () => {
  try {
    const res = await fetch(`${import.meta.env.BASE_URL}data/portafolio.json`)
    const datos = await res.json()
    educacion.value = datos.educacion
  } catch (err) {
    console.error('Error cargando educación:', err)
  }

  observer = new IntersectionObserver(
    (entries) => {
      entries.forEach((entry) => {
        if (entry.isIntersecting) {
          entry.target.classList.add('visible')
          observer?.unobserve(entry.target)
        }
      })
    },
    { threshold: 0.2 }
  )

  setTimeout(() => {
    itemRefs.value.forEach((el) => el && observer?.observe(el))
  }, 50)
})

onUnmounted(() => observer?.disconnect())
</script>

<style scoped>
.educacion {
  background-color: var(--crema);
}

.titulo-seccion {
  color: var(--azul-noche);
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
  display: flex;
  flex-direction: column;
  gap: var(--espacio-md);
}

.timeline::before {
  content: '';
  position: absolute;
  left: 0.75rem;
  top: 0.4rem;
  bottom: 0.4rem;
  width: 2px;
  background: var(--teal-medio);
  opacity: 0.4;
  border-radius: 1px;
}

.edu-item {
  position: relative;
  opacity: 0;
  transform: translateY(16px);
  transition: opacity 0.5s ease, transform 0.5s ease;
}

.edu-item.visible {
  opacity: 1;
  transform: translateY(0);
}

.edu-dot {
  position: absolute;
  left: -2rem;
  top: 0.4rem;
  width: 14px;
  height: 14px;
  border-radius: 50%;
  background-color: var(--teal-medio);
  border: 2px solid var(--crema);
  box-shadow: 0 0 0 3px rgba(90, 138, 146, 0.3);
  transform: translateX(-0.4375rem);
}

.edu-contenido {
  display: flex;
  flex-direction: column;
  gap: 0.25rem;
}

.edu-estado {
  display: inline-block;
  font-size: 0.72rem;
  font-weight: 600;
  padding: 0.15rem 0.6rem;
  border-radius: 999px;
  background-color: var(--gris-claro);
  color: var(--azul-noche);
  width: fit-content;
}

.edu-estado.activo {
  background-color: var(--teal-medio);
  color: #fff;
}

.edu-titulo {
  color: var(--azul-noche);
  font-size: 1rem;
  margin-top: 0.25rem;
}

.edu-institucion {
  color: var(--teal-medio);
  font-weight: 500;
  font-size: 0.9rem;
  max-width: none;
}

.edu-periodo {
  color: rgba(26, 42, 58, 0.55);
  font-size: 0.8rem;
  max-width: none;
}
</style>
