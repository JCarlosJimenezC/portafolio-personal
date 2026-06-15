<template>
  <!-- Botón hamburguesa (solo móvil) -->
  <button class="menu-toggle" @click="menuAbierto = !menuAbierto" aria-label="Abrir menú">
    <span class="hamburger" :class="{ abierto: menuAbierto }"></span>
  </button>

  <!-- Overlay oscuro al abrir el menú en móvil -->
  <div class="overlay" :class="{ visible: menuAbierto }" @click="menuAbierto = false"></div>

  <!-- Sidebar principal -->
  <aside class="sidebar" :class="{ abierto: menuAbierto }">
    <div class="perfil">
      <img
        class="foto"
        :src="fotoSrc"
        alt="Foto de perfil de Juan Carlos Jiménez"
        width="90"
        height="90"
        decoding="async"
      />
      <h2 class="nombre">Juan Carlos Jiménez</h2>
      <p class="titulo">Informática Empresarial</p>
    </div>

    <hr class="separador" />

    <nav class="nav" aria-label="Navegación principal">
      <a
        v-for="enlace in enlaces"
        :key="enlace.id"
        :href="`#${enlace.id}`"
        class="nav-link"
        :class="{ activo: seccionActiva === enlace.id }"
        @click="menuAbierto = false"
      >
        {{ enlace.texto }}
      </a>
    </nav>

    <div class="redes">
      <a
        href="https://github.com/JCarlosJimenezC"
        target="_blank"
        rel="noopener noreferrer"
        aria-label="GitHub"
        class="red-social"
      >
        <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="currentColor" aria-hidden="true">
          <path d="M12 .297c-6.63 0-12 5.373-12 12 0 5.303 3.438 9.8 8.205 11.385.6.113.82-.258.82-.577 0-.285-.01-1.04-.015-2.04-3.338.724-4.042-1.61-4.042-1.61C4.422 18.07 3.633 17.7 3.633 17.7c-1.087-.744.084-.729.084-.729 1.205.084 1.838 1.236 1.838 1.236 1.07 1.835 2.809 1.305 3.495.998.108-.776.417-1.305.76-1.605-2.665-.3-5.466-1.332-5.466-5.93 0-1.31.465-2.38 1.235-3.22-.135-.303-.54-1.523.105-3.176 0 0 1.005-.322 3.3 1.23.96-.267 1.98-.399 3-.405 1.02.006 2.04.138 3 .405 2.28-1.552 3.285-1.23 3.285-1.23.645 1.653.24 2.873.12 3.176.765.84 1.23 1.91 1.23 3.22 0 4.61-2.805 5.625-5.475 5.92.42.36.81 1.096.81 2.22 0 1.606-.015 2.896-.015 3.286 0 .315.21.69.825.57C20.565 22.092 24 17.592 24 12.297c0-6.627-5.373-12-12-12"/>
        </svg>
      </a>
      <a
        href="http://www.youtube.com/@JcarlosJiménez-h8v"
        target="_blank"
        rel="noopener noreferrer"
        aria-label="YouTube"
        class="red-social"
      >
        <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="currentColor" aria-hidden="true">
          <path d="M23.498 6.186a3.016 3.016 0 0 0-2.122-2.136C19.505 3.545 12 3.545 12 3.545s-7.505 0-9.377.505A3.017 3.017 0 0 0 .502 6.186C0 8.07 0 12 0 12s0 3.93.502 5.814a3.016 3.016 0 0 0 2.122 2.136c1.871.505 9.376.505 9.376.505s7.505 0 9.377-.505a3.015 3.015 0 0 0 2.122-2.136C24 15.93 24 12 24 12s0-3.93-.502-5.814zM9.545 15.568V8.432L15.818 12l-6.273 3.568z"/>
        </svg>
      </a>
    </div>
  </aside>
</template>

<script setup>
import { ref, onMounted, onUnmounted } from 'vue'
import fotoSrc from '@/assets/images/juanky.png'

const menuAbierto = ref(false)
const seccionActiva = ref('about')

const enlaces = [
  { id: 'about',      texto: 'Sobre mí' },
  { id: 'gallery',    texto: 'Proyectos' },
  { id: 'skills',     texto: 'Habilidades' },
  { id: 'educacion',  texto: 'Educación' },
  { id: 'experiencia', texto: 'Experiencia' },
  { id: 'contacto',   texto: 'Contacto' },
]

function actualizarSeccion() {
  const secciones = document.querySelectorAll('section[id]')

  const alFondo = window.scrollY + window.innerHeight >= document.documentElement.scrollHeight - 60
  if (alFondo) {
    const id = secciones[secciones.length - 1]?.id ?? 'about'
    if (id !== seccionActiva.value) {
      seccionActiva.value = id
      history.replaceState(null, '', `#${id}`)
    }
    return
  }

  const punto = window.scrollY + window.innerHeight * 0.4
  let activa = secciones[0]?.id ?? 'about'
  secciones.forEach(sec => {
    if (sec.offsetTop <= punto) activa = sec.id
  })
  if (activa !== seccionActiva.value) {
    seccionActiva.value = activa
    history.replaceState(null, '', `#${activa}`)
  }
}

onMounted(() => {
  actualizarSeccion()
  window.addEventListener('scroll', actualizarSeccion, { passive: true })
})

onUnmounted(() => {
  window.removeEventListener('scroll', actualizarSeccion)
})
</script>

<style scoped>
/* ── Mobile-first base ──────────────────────────────── */

.menu-toggle {
  display: flex;
  position: fixed;
  top: 1rem;
  left: 1rem;
  z-index: 200;
  background: var(--azul-noche);
  border: none;
  border-radius: var(--radio);
  padding: 0.5rem;
  cursor: pointer;
  width: 42px;
  height: 42px;
  align-items: center;
  justify-content: center;
}

.hamburger,
.hamburger::before,
.hamburger::after {
  display: block;
  width: 22px;
  height: 2px;
  background-color: var(--crema);
  border-radius: 2px;
  position: relative;
  transition:
    transform var(--transicion),
    opacity var(--transicion),
    background-color var(--transicion);
}

.hamburger::before,
.hamburger::after {
  content: '';
  position: absolute;
  left: 0;
}

.hamburger::before { top: -7px; }
.hamburger::after  { top: 7px; }

.hamburger.abierto { background-color: transparent; }
.hamburger.abierto::before { transform: translateY(7px) rotate(45deg); }
.hamburger.abierto::after  { transform: translateY(-7px) rotate(-45deg); }

.overlay {
  display: block;
  position: fixed;
  inset: 0;
  background: rgba(0, 0, 0, 0.5);
  z-index: 90;
  opacity: 0;
  pointer-events: none;
  transition: opacity var(--transicion);
}

.overlay.visible {
  opacity: 1;
  pointer-events: all;
}

.sidebar {
  position: fixed;
  inset-block: 0;
  inset-inline-start: 0;
  width: 270px;
  height: 100vh;
  background-color: var(--azul-noche);
  color: var(--crema);
  display: flex;
  flex-direction: column;
  padding: 1.75rem var(--espacio-sm) var(--espacio-sm);
  overflow-y: auto;
  z-index: 100;
  transform: translateX(-100%);
  transition: transform var(--transicion);
}

.sidebar.abierto { transform: translateX(0); }

/* ── Perfil ─────────────────────────────────────────── */
.perfil {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 0.5rem;
  text-align: center;
  margin-bottom: 0.4rem;
}

.foto {
  width: 100px;
  height: 100px;
  border-radius: 50%;
  object-fit: cover;
  border: 3px solid var(--teal-medio);
}

.nombre {
  font-size: 1.1rem;
  font-weight: 700;
  color: var(--blanco);
  margin: 0;
}

.titulo {
  font-size: 0.95rem;
  color: var(--verde-suave);
  max-width: none;
}

/* ── Separador ──────────────────────────────────────── */
.separador {
  border: none;
  border-top: 1px solid rgba(255, 255, 255, 0.15);
  margin-block: 0.4rem;
}

/* ── Navegación ─────────────────────────────────────── */
.nav {
  display: flex;
  flex-direction: column;
  gap: 0.3rem;
  margin-block: auto;
}

.nav-link {
  display: block;
  padding: 0.65rem 1rem;
  border-radius: var(--radio);
  color: var(--blanco);
  font-size: 0.95rem;
  font-weight: 500;
  text-align: center;
  text-decoration: none;
  transition:
    background-color var(--transicion),
    color var(--transicion);
}

.nav-link:hover {
  background-color: rgba(0, 200, 81, 0.15);
  color: var(--verde-suave);
  text-decoration: none;
}

.nav-link.activo {
  background-color: var(--teal-medio);
  color: #000;
  font-weight: 700;
}

/* ── Redes sociales ─────────────────────────────────── */
.redes {
  display: flex;
  justify-content: center;
  gap: var(--espacio-sm);
  padding-top: var(--espacio-sm);
  border-top: 1px solid rgba(255, 255, 255, 0.1);
  margin-top: var(--espacio-sm);
}

.red-social {
  color: var(--verde-suave);
  display: flex;
  align-items: center;
  justify-content: center;
  padding: 0.45rem;
  border: 1.5px solid var(--verde-2);
  border-radius: var(--radio);
  transition: color var(--transicion), transform var(--transicion), background-color var(--transicion);
}

.red-social:hover {
  color: var(--verde-suave);
  background-color: rgba(0, 200, 81, 0.15);
  transform: translateY(-2px);
  text-decoration: none;
}

.red-social svg {
  width: 22px;
  height: 22px;
}

/* ── Desktop: sidebar siempre visible ───────────────── */
@media (min-width: 768px) {
  .sidebar {
    transform: translateX(0);
  }

  .menu-toggle {
    display: none;
  }

  .overlay {
    display: none;
  }
}
</style>
