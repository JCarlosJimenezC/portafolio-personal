<template>
  <section id="gallery" class="gallery section">
    <div class="container">
      <h2 class="titulo-seccion">Proyectos</h2>

      <!-- ── Carrusel infinito ── -->
      <div class="carrusel-contenedor" @mouseenter="onHoverEnter" @mouseleave="onHoverLeave">
        <div class="carrusel-pista"
          ref="pista"
          @pointerdown="onPointerDown"
          @pointermove="onPointerMove"
          @pointerup="onPointerUp"
          @pointerleave="onPointerUp"
        >
          <article v-for="proyecto in proyectos" :key="`a-${proyecto.id}`" class="proyecto-card">
            <div class="proyecto-img-wrapper">
              <img :src="getImageUrl(proyecto.imagen)" :alt="proyecto.titulo" class="proyecto-img" loading="lazy" decoding="async" />
              <span class="badge-tipo" :class="proyecto.grupal ? 'badge-grupal' : 'badge-individual'">
                {{ proyecto.grupal ? 'Proyecto grupal' : 'Individual' }}
              </span>
            </div>
            <div class="proyecto-info">
              <h3 class="proyecto-titulo">{{ proyecto.titulo }}</h3>
              <p class="proyecto-desc">{{ proyecto.descripcion }}</p>
              <div class="proyecto-tags">
                <span v-for="tech in proyecto.tecnologias" :key="tech" class="proyecto-tag">{{ tech }}</span>
              </div>
              <div class="proyecto-footer">
                <a :href="proyecto.enlace" target="_blank" rel="noopener noreferrer" class="btn proyecto-btn">Ver proyecto</a>
              </div>
            </div>
          </article>

          <article v-for="proyecto in proyectos" :key="`b-${proyecto.id}`" class="proyecto-card" aria-hidden="true">
            <div class="proyecto-img-wrapper">
              <img :src="getImageUrl(proyecto.imagen)" :alt="proyecto.titulo" class="proyecto-img" loading="lazy" decoding="async" />
              <span class="badge-tipo" :class="proyecto.grupal ? 'badge-grupal' : 'badge-individual'">
                {{ proyecto.grupal ? 'Proyecto grupal' : 'Individual' }}
              </span>
            </div>
            <div class="proyecto-info">
              <h3 class="proyecto-titulo">{{ proyecto.titulo }}</h3>
              <p class="proyecto-desc">{{ proyecto.descripcion }}</p>
              <div class="proyecto-tags">
                <span v-for="tech in proyecto.tecnologias" :key="tech" class="proyecto-tag">{{ tech }}</span>
              </div>
              <div class="proyecto-footer">
                <a :href="proyecto.enlace" target="_blank" rel="noopener noreferrer" class="btn proyecto-btn">Ver proyecto</a>
              </div>
            </div>
          </article>
        </div>
      </div>

      <!-- ── Dots ── -->
      <div class="carrusel-dots">
        <button
          v-for="(proyecto, i) in proyectos"
          :key="proyecto.id"
          class="dot"
          :class="{ activo: indiceActivo === i }"
          @click="seleccionarDot(i)"
          :aria-label="`Ver ${proyecto.titulo}`"
        ></button>
      </div>
    </div>
  </section>
</template>

<script setup>
import { ref, onMounted, onUnmounted } from 'vue'

const props = defineProps({ proyectos: { type: Array, required: true } })

function getImageUrl(nombre) {
  return new URL(`../assets/images/${nombre}`, import.meta.url).href
}

const DURACION = 22
const pista = ref(null)
const indiceActivo = ref(0)

let arrastrando = false
let hovering = false
let startX = 0
let posX = 0
let navTimeout = null
let trackInterval = null

// ── Animación ───────────────────────────────────────────
function translateActual() {
  const m = new DOMMatrix(window.getComputedStyle(pista.value).transform)
  return m.m41
}

function pausarAnim() {
  const x = translateActual()
  pista.value.style.transition = ''
  pista.value.style.animation = 'none'
  pista.value.style.transform = `translateX(${x}px)`
  posX = x
  return x
}

function reanudarAnim(x) {
  if (!pista.value) return
  const mitad = pista.value.scrollWidth / 2
  if (!mitad) {
    pista.value.style.animation = `scroll-infinito ${DURACION}s linear infinite`
    return
  }
  const progreso = (Math.abs(x) % mitad) / mitad
  const delay = -(DURACION * progreso)
  pista.value.style.transform = ''
  pista.value.style.animation = 'none'
  void pista.value.offsetWidth
  pista.value.style.animation = `scroll-infinito ${DURACION}s ${delay}s linear infinite`
  if (hovering) pista.value.style.animationPlayState = 'paused'
}

// ── Hover pause ─────────────────────────────────────────
function onHoverEnter() {
  hovering = true
  if (pista.value) pista.value.style.animationPlayState = 'paused'
}

function onHoverLeave() {
  hovering = false
  if (pista.value) pista.value.style.animationPlayState = 'running'
}

// ── Dots ────────────────────────────────────────────────
function actualizarDot() {
  if (!pista.value) return
  const x = translateActual()
  const ancho = (pista.value.firstElementChild?.offsetWidth ?? 290) + 24
  indiceActivo.value = Math.round(Math.abs(x) / ancho) % props.proyectos.length
}

function seleccionarDot(i) {
  if (!pista.value) return
  clearTimeout(navTimeout)
  const ancho = (pista.value.firstElementChild?.offsetWidth ?? 290) + 24
  const mitad = pista.value.scrollWidth / 2
  pausarAnim()
  let newX = -(i * ancho)
  if (newX < -mitad) newX += mitad
  indiceActivo.value = i
  posX = newX
  pista.value.style.transition = 'transform 0.4s ease'
  pista.value.style.transform = `translateX(${newX}px)`
  navTimeout = setTimeout(() => {
    if (!pista.value) return
    pista.value.style.transition = ''
    reanudarAnim(newX)
  }, 420)
}

// ── Drag ────────────────────────────────────────────────
function onPointerDown(e) {
  if (e.target.closest('a, button')) return
  arrastrando = true
  startX = e.clientX
  posX = pausarAnim()
  pista.value.setPointerCapture(e.pointerId)
  pista.value.style.cursor = 'grabbing'
}

function onPointerMove(e) {
  if (!arrastrando) return
  const delta = e.clientX - startX
  const mitad = pista.value.scrollWidth / 2
  let newX = posX + delta
  if (newX > 0) newX -= mitad
  if (newX < -mitad) newX += mitad
  pista.value.style.transform = `translateX(${newX}px)`
  posX = newX
  startX = e.clientX
}

function onPointerUp() {
  if (!arrastrando) return
  arrastrando = false
  pista.value.style.cursor = 'grab'
  reanudarAnim(posX)
}

onMounted(() => { trackInterval = setInterval(actualizarDot, 400) })
onUnmounted(() => { clearInterval(trackInterval); clearTimeout(navTimeout) })
</script>

<style>
/* Debe ser global — Vue 3 mangling scoped @keyframes y JS no puede referenciarlos */
@keyframes scroll-infinito {
  from { transform: translateX(0); }
  to   { transform: translateX(-50%); }
}
</style>

<style scoped>
.gallery { background-color: var(--crema); }

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

/* ── Carrusel ────────────────────────────────────────── */
.carrusel-contenedor {
  overflow: hidden;
  padding-block: var(--espacio-sm);
}

.carrusel-pista {
  display: flex;
  gap: 1.5rem;
  width: max-content;
  animation: scroll-infinito 22s linear infinite;
  cursor: grab;
  user-select: none;
}


/* ── Card ────────────────────────────────────────────── */
.proyecto-card {
  flex: 0 0 290px;
  background: var(--azul-noche);
  border-radius: var(--radio);
  overflow: hidden;
  display: flex;
  flex-direction: column;
  transition: transform var(--transicion), box-shadow var(--transicion);
}
.proyecto-card:hover {
  transform: translateY(-5px);
  box-shadow: var(--sombra-md);
}

.proyecto-img-wrapper {
  position: relative;
  width: 100%;
  aspect-ratio: 2 / 1;
  overflow: hidden;
  background: rgba(0,0,0,0.3);
}
.proyecto-img {
  width: 100%; height: 100%;
  object-fit: cover;
  transition: transform 0.4s ease;
}
.proyecto-card:hover .proyecto-img { transform: scale(1.05); }

.badge-tipo {
  position: absolute;
  top: 0.5rem;
  left: 0.5rem;
  font-size: 0.65rem;
  padding: 0.2rem 0.55rem;
  border-radius: 999px;
  pointer-events: none;
}
.badge-grupal    { background: rgba(0,0,0,.55); border: 1px solid rgba(0,200,81,.6); color: var(--verde-2); }
.badge-individual { background: rgba(0,0,0,.55); border: 1px solid rgba(255,255,255,.3); color: rgba(255,255,255,.7); }

.proyecto-info {
  padding: 0.75rem var(--espacio-sm);
  display: flex;
  flex-direction: column;
  gap: 0.55rem;
  flex: 1;
}
.proyecto-titulo {
  color: var(--blanco);
  font-size: 0.95rem;
  text-align: center;
  display: -webkit-box;
  -webkit-line-clamp: 2;
  -webkit-box-orient: vertical;
  overflow: hidden;
  min-height: 2.5rem;
}
.proyecto-desc {
  color: var(--gris-claro);
  font-size: 0.75rem;
  line-height: 1.5;
  text-align: justify;
  display: -webkit-box;
  -webkit-line-clamp: 4;
  -webkit-box-orient: vertical;
  overflow: hidden;
  min-height: 4.5rem;
}

.proyecto-tags { display: flex; flex-wrap: wrap; gap: 0.2rem; min-height: 2.8rem; align-content: flex-start; margin-top: 0.4rem; justify-content: center; }
.proyecto-tag {
  font-size: 0.6rem;
  padding: 0.1rem 0.35rem;
  border-radius: 999px;
  background: rgba(0,200,81,.08);
  color: var(--verde-2);
  border: 1px solid rgba(0,200,81,.3);
}

.proyecto-footer { margin-top: auto; padding-top: 0.4rem; display: flex; justify-content: center; }
.proyecto-btn {
  background-color: transparent;
  border: 2px solid var(--verde-2);
  color: var(--verde-2);
  font-size: 0.78rem;
  padding: 0.35rem 0.85rem;
}
.proyecto-btn:hover {
  background-color: rgba(0,200,81,.12);
  color: var(--verde-2);
  transform: none;
  box-shadow: none;
  text-decoration: none;
}

/* ── Dots ────────────────────────────────────────────── */
.carrusel-dots {
  display: flex;
  justify-content: center;
  gap: 0.5rem;
  margin-top: var(--espacio-sm);
}
.dot {
  width: 8px; height: 8px;
  border-radius: 50%;
  background: rgba(255,255,255,.2);
  border: none;
  cursor: pointer;
  padding: 0;
  transition: background-color var(--transicion), transform var(--transicion);
}
.dot.activo { background: var(--verde-2); transform: scale(1.35); }
.dot:hover  { background: rgba(0,200,81,.5); }
</style>
