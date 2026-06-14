<template>
  <section id="gallery" class="gallery section">
    <div class="container">
      <h2 class="titulo-seccion">Proyectos</h2>
      <div class="proyectos-grid">
        <article
          v-for="proyecto in proyectos"
          :key="proyecto.id"
          class="proyecto-card"
        >
          <div class="proyecto-img-wrapper">
            <img
              :src="getImageUrl(proyecto.imagen)"
              :alt="proyecto.titulo"
              class="proyecto-img"
              loading="lazy"
              decoding="async"
            />
          </div>
          <div class="proyecto-info">
            <h3 class="proyecto-titulo">{{ proyecto.titulo }}</h3>
            <p class="proyecto-desc">{{ proyecto.descripcion }}</p>
            <div class="proyecto-tags">
              <span
                v-for="tech in proyecto.tecnologias"
                :key="tech"
                class="proyecto-tag"
              >{{ tech }}</span>
            </div>
            <a
              :href="proyecto.enlace"
              target="_blank"
              rel="noopener noreferrer"
              class="btn proyecto-btn"
            >Ver proyecto</a>
          </div>
        </article>
      </div>
    </div>
  </section>
</template>

<script setup>
defineProps({
  proyectos: { type: Array, required: true }
})

function getImageUrl(nombre) {
  return new URL(`../assets/images/${nombre}`, import.meta.url).href
}
</script>

<style scoped>
.gallery {
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

/* Mobile-first: columna única */
.proyectos-grid {
  display: grid;
  grid-template-columns: 1fr;
  gap: var(--espacio-lg);
}

.proyecto-card {
  background: rgba(255, 255, 255, 0.04);
  border: 1px solid rgba(255, 255, 255, 0.1);
  border-radius: var(--radio);
  overflow: hidden;
  transition: transform var(--transicion), box-shadow var(--transicion);
}

.proyecto-card:hover {
  transform: translateY(-4px);
  box-shadow: var(--sombra-md);
}

.proyecto-img-wrapper {
  width: 100%;
  aspect-ratio: 16 / 9;
  overflow: hidden;
  background-color: rgba(0, 0, 0, 0.3);
}

.proyecto-img {
  width: 100%;
  height: 100%;
  object-fit: cover;
  transition: transform 0.4s ease;
}

.proyecto-card:hover .proyecto-img {
  transform: scale(1.04);
}

.proyecto-info {
  padding: var(--espacio-sm) var(--espacio-md) var(--espacio-md);
  display: flex;
  flex-direction: column;
  gap: var(--espacio-xs);
}

.proyecto-titulo {
  color: var(--crema);
  font-size: 1.1rem;
}

.proyecto-desc {
  color: var(--gris-claro);
  font-size: 0.9rem;
  line-height: 1.6;
}

.proyecto-tags {
  display: flex;
  flex-wrap: wrap;
  gap: 0.35rem;
  margin-top: 0.25rem;
}

.proyecto-tag {
  font-size: 0.75rem;
  padding: 0.2rem 0.6rem;
  border-radius: 999px;
  background: rgba(90, 138, 146, 0.2);
  color: var(--teal-medio);
  border: 1px solid rgba(90, 138, 146, 0.35);
}

.proyecto-btn {
  margin-top: var(--espacio-xs);
  align-self: flex-start;
}

/* 2 columnas en desktop */
@media (min-width: 768px) {
  .proyectos-grid {
    grid-template-columns: repeat(2, 1fr);
  }
}
</style>
