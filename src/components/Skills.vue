<template>
  <section id="skills" class="skills section">
    <div class="container">
      <h2 class="titulo-seccion">Habilidades</h2>

      <div class="skills-bloque">
        <h3 class="skills-subtitulo">Técnicas</h3>
        <div class="categorias-grid">
          <div
            v-for="cat in habilidades.tecnicas"
            :key="cat.categoria"
            class="categoria"
          >
            <p class="categoria-nombre">{{ cat.categoria }}</p>
            <div class="tags">
              <span
                v-for="(tech, i) in cat.tecnologias"
                :key="tech"
                class="tag"
                :style="{ animationDelay: `${i * 55}ms` }"
              >{{ tech }}</span>
            </div>
          </div>
        </div>
      </div>

      <div class="skills-bloque">
        <h3 class="skills-subtitulo">Blandas</h3>
        <div class="tags">
          <span
            v-for="(habilidad, i) in habilidades.blandas"
            :key="habilidad"
            class="tag tag--blanda"
            :style="{ animationDelay: `${i * 80}ms` }"
          >{{ habilidad }}</span>
        </div>
      </div>
    </div>
  </section>
</template>

<script setup>
defineProps({
  habilidades: { type: Object, required: true }
})
</script>

<style scoped>
.skills {
  background-color: var(--gris-claro);
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

.skills-bloque {
  margin-bottom: var(--espacio-lg);
}

.skills-subtitulo {
  color: var(--azul-noche);
  font-size: 1.05rem;
  margin-bottom: var(--espacio-sm);
}

.categorias-grid {
  display: grid;
  grid-template-columns: 1fr;
  gap: var(--espacio-sm);
}

.categoria {
  background: #fff;
  border-radius: var(--radio);
  padding: var(--espacio-sm);
  box-shadow: var(--sombra-sm);
}

.categoria-nombre {
  font-weight: 600;
  color: var(--azul-noche);
  font-size: 0.82rem;
  letter-spacing: 0.04em;
  text-transform: uppercase;
  margin-bottom: 0.5rem;
  max-width: none;
}

.tags {
  display: flex;
  flex-wrap: wrap;
  gap: 0.4rem;
}

.tag {
  display: inline-block;
  padding: 0.3rem 0.7rem;
  border-radius: 999px;
  font-size: 0.8rem;
  font-weight: 500;
  background-color: var(--teal-medio);
  color: #fff;
  cursor: default;
  animation: aparecerTag 0.4s ease both;
  transition: transform var(--transicion), background-color var(--transicion);
}

.tag:hover {
  transform: scale(1.1);
  background-color: var(--azul-noche);
}

.tag--blanda {
  background-color: var(--azul-noche);
}

.tag--blanda:hover {
  background-color: var(--teal-medio);
}

@keyframes aparecerTag {
  from { opacity: 0; transform: scale(0.85) translateY(6px); }
  to   { opacity: 1; transform: scale(1) translateY(0); }
}

@media (min-width: 480px) {
  .categorias-grid { grid-template-columns: repeat(2, 1fr); }
}

@media (min-width: 900px) {
  .categorias-grid { grid-template-columns: repeat(3, 1fr); }
}

@media (min-width: 1100px) {
  .categorias-grid { grid-template-columns: repeat(4, 1fr); }
}
</style>
