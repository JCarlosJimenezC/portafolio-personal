<template>
  <section id="skills" class="skills section">
    <div class="container">
      <h2 class="titulo-seccion">Habilidades</h2>
      <div class="bosque">

        <!-- Árbol 1: Técnicas -->
        <div class="arbol">
          <div class="copa">
            <!-- Tronco fijo (no gira) -->
            <svg class="svg-tronco" viewBox="0 0 100 100" preserveAspectRatio="none" aria-hidden="true">
              <line x1="50" y1="100" x2="50" y2="50" class="linea-tronco"/>
            </svg>

            <!-- Copa giratoria -->
            <div class="orbita">
              <svg class="svg-ramas" viewBox="0 0 100 100" preserveAspectRatio="none" aria-hidden="true">
                <line v-for="(pos, i) in posiciones" :key="i"
                      x1="50" y1="50" :x2="pos.left" :y2="pos.top"
                      class="linea-rama"/>
              </svg>
              <div
                v-for="(cat, i) in habilidades.tecnicas"
                :key="cat.categoria"
                class="hoja-ancla"
                :style="`left:${posiciones[i].left}%;top:${posiciones[i].top}%`"
              >
                <div class="hoja" :title="cat.tecnologias.join(', ')">
                  <span class="hoja-nombre">{{ cat.categoria }}</span>
                  <span class="hoja-techs">{{ cat.tecnologias.slice(0, 3).join(' · ') }}{{ cat.tecnologias.length > 3 ? '…' : '' }}</span>
                </div>
              </div>
            </div>

            <!-- Hojas cayendo -->
            <div v-for="(h, i) in hojasCayendo" :key="`t${i}`"
                 class="hoja-cae" :class="`cae-${h.v}`"
                 :style="`left:${h.x}%;animation-duration:${h.dur}s;animation-delay:${h.del}s`"
                 aria-hidden="true"></div>

            <span class="copa-titulo">Técnicas</span>
          </div>
          <div class="tronco"></div>
        </div>

        <!-- Árbol 2: Blandas -->
        <div class="arbol">
          <div class="copa">
            <svg class="svg-tronco" viewBox="0 0 100 100" preserveAspectRatio="none" aria-hidden="true">
              <line x1="50" y1="100" x2="50" y2="50" class="linea-tronco"/>
            </svg>

            <div class="orbita">
              <svg class="svg-ramas" viewBox="0 0 100 100" preserveAspectRatio="none" aria-hidden="true">
                <line v-for="(pos, i) in posiciones" :key="i"
                      x1="50" y1="50" :x2="pos.left" :y2="pos.top"
                      class="linea-rama"/>
              </svg>
              <div
                v-for="(hab, i) in habilidades.blandas"
                :key="hab"
                class="hoja-ancla"
                :style="`left:${posiciones[i].left}%;top:${posiciones[i].top}%`"
              >
                <div class="hoja">
                  <span class="hoja-nombre">{{ hab }}</span>
                </div>
              </div>
            </div>

            <!-- Hojas cayendo -->
            <div v-for="(h, i) in hojasCayendo" :key="`b${i}`"
                 class="hoja-cae" :class="`cae-${h.v}`"
                 :style="`left:${h.x}%;animation-duration:${h.dur}s;animation-delay:${h.del}s`"
                 aria-hidden="true"></div>

            <span class="copa-titulo">Blandas</span>
          </div>
          <div class="tronco"></div>
        </div>

      </div>
    </div>
  </section>
</template>

<script setup>
import { computed } from 'vue'

const props = defineProps({ habilidades: { type: Object, required: true } })

const COPA_W = 300
const COPA_H = 280
const R      = 100

const hojasCayendo = [
  { x: 28, dur: 5.2, del: 0.0, v: 'a' },
  { x: 55, dur: 4.6, del: 1.3, v: 'b' },
  { x: 72, dur: 6.1, del: 0.6, v: 'c' },
  { x: 40, dur: 5.7, del: 2.5, v: 'a' },
  { x: 18, dur: 4.3, del: 3.2, v: 'b' },
  { x: 65, dur: 5.4, del: 1.9, v: 'c' },
  { x: 48, dur: 6.8, del: 4.1, v: 'a' },
]

const posiciones = computed(() => {
  const n = Math.max(
    props.habilidades.tecnicas.length,
    props.habilidades.blandas.length
  )
  return Array.from({ length: n }, (_, i) => {
    const angulo = -Math.PI / 2 + (2 * Math.PI * i) / n
    return {
      left: +(50 + (R * Math.cos(angulo)) / COPA_W * 100).toFixed(1),
      top:  +(50 + (R * Math.sin(angulo)) / COPA_H * 100).toFixed(1),
    }
  })
})
</script>

<style scoped>
.skills {
  background-color: var(--crema);
  overflow-x: hidden;
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

/* ── Bosque ──────────────────────────────────────────── */
.bosque {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: var(--espacio-xl);
}

@media (min-width: 768px) {
  .bosque {
    flex-direction: row;
    justify-content: center;
    align-items: flex-end;
    gap: 4rem;
  }
}

/* ── Árbol ───────────────────────────────────────────── */
.arbol {
  display: flex;
  flex-direction: column;
  align-items: center;
}

/* ── Copa ────────────────────────────────────────────── */
.copa {
  position: relative;
  width: 300px;
  height: 280px;
  overflow: visible;
}

/* ── SVG tronco (fijo) ───────────────────────────────── */
.svg-tronco {
  position: absolute;
  inset: 0;
  width: 100%;
  height: 100%;
  pointer-events: none;
  z-index: 0;
  overflow: visible;
}

.linea-tronco {
  stroke: var(--verde-3);
  stroke-width: 2.5;
  stroke-opacity: 0.6;
  fill: none;
}

/* ── Órbita (todo gira) ──────────────────────────────── */
.orbita {
  position: absolute;
  inset: 0;
  transform-origin: center;    /* 50% 50% — coincide con el radio calculado */
  animation: girar 40s linear infinite;
}

/* ── SVG ramas (giran con la órbita) ────────────────── */
.svg-ramas {
  position: absolute;
  inset: 0;
  width: 100%;
  height: 100%;
  pointer-events: none;
  overflow: visible;
}

.linea-rama {
  stroke: var(--verde-3);
  stroke-width: 1.2;
  stroke-opacity: 0.4;
  fill: none;
}

/* ── Ancla de posición (no tiene transform propio) ───── */
.hoja-ancla {
  position: absolute;
  transform: translate(-50%, -50%);
}

/* ── Hoja: contra-gira para mantener texto derecho ───── */
.hoja {
  width: 90px;
  height: 90px;
  border-radius: 50%;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  text-align: center;
  gap: 0.12rem;
  padding: 0.5rem;
  overflow: hidden;
  background: var(--azul-noche);
  border: 1.5px solid rgba(0, 200, 81, 0.4);
  cursor: default;
  z-index: 1;
  animation: contra-girar 40s linear infinite;
  transition: border-color var(--transicion), box-shadow var(--transicion);
}

.hoja:hover {
  border-color: var(--verde-2);
  box-shadow: 0 0 18px rgba(0, 200, 81, 0.5);
}

.hoja-nombre {
  color: var(--verde-2);
  font-size: 0.55rem;
  font-weight: 700;
  line-height: 1.2;
}

.hoja-techs {
  color: rgba(255, 255, 255, 0.6);
  font-size: 0.47rem;
  line-height: 1.3;
}

/* ── Título de copa ──────────────────────────────────── */
.copa-titulo {
  position: absolute;
  left: 50%;
  top: 50%;
  transform: translate(-50%, -50%);
  color: #ffffff;
  font-size: 0.82rem;
  font-weight: 700;
  letter-spacing: 0.14em;
  text-transform: uppercase;
  pointer-events: none;
  z-index: 2;
  white-space: nowrap;
  text-shadow: 0 0 12px rgba(0, 0, 0, 0.8);
}

/* ── Tronco ──────────────────────────────────────────── */
.tronco {
  width: 12px;
  height: 55px;
  background: linear-gradient(to bottom, var(--verde-3), rgba(0, 130, 40, 0.3));
  border-radius: 6px;
}

/* ── Hojas cayendo ───────────────────────────────────── */
.hoja-cae {
  position: absolute;
  top: 10%;
  width: 8px;
  height: 11px;
  background: var(--verde-2);
  border-radius: 50% 10% 50% 10%;
  opacity: 0;
  pointer-events: none;
  z-index: 4;
}

.cae-a { animation: caer-a linear infinite; }
.cae-b { animation: caer-b linear infinite; }
.cae-c { animation: caer-c linear infinite; }

@keyframes caer-a {
  0%   { transform: translate(0,   0)    rotate(0deg);   opacity: 0;    }
  12%  { opacity: 0.8; }
  85%  { opacity: 0.4; }
  100% { transform: translate(22px, 300px) rotate(480deg); opacity: 0; }
}
@keyframes caer-b {
  0%   { transform: translate(0,   0)     rotate(30deg);  opacity: 0;   }
  12%  { opacity: 0.8; }
  85%  { opacity: 0.4; }
  100% { transform: translate(-18px, 300px) rotate(-420deg); opacity: 0; }
}
@keyframes caer-c {
  0%   { transform: translate(0,   0)    rotate(-20deg); opacity: 0;    }
  12%  { opacity: 0.8; }
  85%  { opacity: 0.4; }
  100% { transform: translate(10px, 300px) rotate(540deg);  opacity: 0; }
}

/* ── Órbita / contra-rotación ────────────────────────── */
@keyframes girar {
  from { transform: rotate(0deg); }
  to   { transform: rotate(360deg); }
}

@keyframes contra-girar {
  from { transform: rotate(0deg); }
  to   { transform: rotate(-360deg); }
}
</style>
