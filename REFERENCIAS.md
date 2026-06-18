# Referencias — IF7102 Multimedios | I Ciclo 2026

Recursos consultados para el aprendizaje de Vue 3 y la construcción del portafolio personal.

**Estudiante:** Juan Carlos Jiménez Castrillo  
**Curso:** IF7102 - Multimedios | I Ciclo 2026 — UCR, Sede de Guanacaste

---

## Nota metodológica

Este proyecto siguió un enfoque de **investigación aplicada**: para cada componente o funcionalidad se identificó primero el concepto técnico a implementar, se consultaron las fuentes oficiales (MDN Web Docs, Vue.js Docs, Vite Docs) y tutoriales especializados, y luego se trasladó ese conocimiento al código. Las secciones siguientes documentan exactamente qué se investigó para implementar cada parte del portafolio.

---

## 1. Fuentes consultadas por componente

### 1.1 `Gallery.vue` — Carrusel infinito

Para implementar el carrusel se investigaron los siguientes conceptos antes de escribir el código:

**CSS `@keyframes` + animación infinita**

MDN Contributors. (2026, 11 de mayo). *animation*. MDN Web Docs.  
https://developer.mozilla.org/en-US/docs/Web/CSS/Reference/Properties/animation

MDN Contributors. (2026, 23 de abril). *@keyframes*. MDN Web Docs.  
https://developer.mozilla.org/en-US/docs/Web/CSS/Reference/At-rules/@keyframes

MDN Contributors. (s.f.). *Using CSS animations*. MDN Web Docs.  
https://developer.mozilla.org/en-US/docs/Web/CSS/Guides/Animations/Using

**`DOMMatrix` — lectura del transform calculado**  
Necesario para detectar la posición actual del carrusel y hacer la transición drag-to-scroll sin saltos visuales.

MDN Contributors. (2025, 27 de octubre). *DOMMatrix*. MDN Web Docs.  
https://developer.mozilla.org/en-US/docs/Web/API/DOMMatrix

**Pointer Events API — arrastre táctil y mouse unificado**

MDN Contributors. (2025, 3 de noviembre). *Pointer events*. MDN Web Docs.  
https://developer.mozilla.org/en-US/docs/Web/API/Pointer_events

MDN Contributors. (2025, 1 de octubre). *Element: setPointerCapture() method*. MDN Web Docs.  
https://developer.mozilla.org/en-US/docs/Web/API/Element/setPointerCapture

**Evento `wheel` con `passive: false`**  
Permite interceptar el scroll del mouse para controlar el carrusel en lugar del scroll de página.

MDN Contributors. (2025, 1 de octubre). *Element: wheel event*. MDN Web Docs.  
https://developer.mozilla.org/en-US/docs/Web/API/Element/wheel_event

MDN Contributors. (2026, mayo). *EventTarget: addEventListener() method*. MDN Web Docs.  
https://developer.mozilla.org/en-US/docs/Web/API/EventTarget/addEventListener

**Reflow forzado — `void element.offsetWidth`**  
Técnica para forzar que el navegador recalcule el layout antes de aplicar una nueva animación CSS (evita que el browser "fusione" los keyframes).

Google for Developers. (2025, 7 de mayo). *Avoid large, complex layouts and layout thrashing*. web.dev.  
https://web.dev/articles/avoid-large-complex-layouts-and-layout-thrashing

Chrome for Developers. (2025, 8 de octubre). *Forced reflow*. Chrome for Developers.  
https://developer.chrome.com/docs/performance/insights/forced-reflow

---

### 1.2 `About.vue` — Sección multimedia

**`HTMLMediaElement` — control de audio/video con JavaScript**  
Necesario para implementar el reproductor de audio personalizado (play/pause, progreso, tiempo).

MDN Contributors. (2026, 30 de marzo). *HTMLMediaElement*. MDN Web Docs.  
https://developer.mozilla.org/en-US/docs/Web/API/HTMLMediaElement

**Evento `error` en `<img>`, `<audio>`, `<video>` — fallbacks multimedia**  
Investigado para mostrar texto alternativo si algún recurso no carga.

MDN Contributors. (2025, 3 de noviembre). *HTMLElement: error event*. MDN Web Docs.  
https://developer.mozilla.org/en-US/docs/Web/API/HTMLElement/error_event

MDN Contributors. (2025, 25 de septiembre). *HTMLMediaElement: error event*. MDN Web Docs.  
https://developer.mozilla.org/en-US/docs/Web/API/HTMLMediaElement/error_event

**Intersection Observer API — animación al entrar en viewport**  
Permite activar las animaciones de texto y fade-in solo cuando la sección es visible.

MDN Contributors. (2025, abril). *Intersection Observer API*. MDN Web Docs.  
https://developer.mozilla.org/en-US/docs/Web/API/Intersection_Observer_API

MDN Contributors. (2025, 26 de febrero). *IntersectionObserver: IntersectionObserver() constructor*. MDN Web Docs.  
https://developer.mozilla.org/en-US/docs/Web/API/IntersectionObserver/IntersectionObserver

**`repeating-conic-gradient()` — ondas circulares animadas alrededor de la foto**

MDN Contributors. (2025, 30 de octubre). *repeating-conic-gradient()*. MDN Web Docs.  
https://developer.mozilla.org/en-US/docs/Web/CSS/gradient/repeating-conic-gradient

**CSS `mask` / `-webkit-mask`**  
Usado junto con `repeating-conic-gradient` para crear el efecto de anillo segmentado.

MDN Contributors. (2026, 20 de abril). *mask*. MDN Web Docs.  
https://developer.mozilla.org/en-US/docs/Web/CSS/Reference/Properties/mask

MDN Contributors. (2025, 13 de agosto). *CSS mask properties*. MDN Web Docs.  
https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_masking/Mask_properties

**`aspect-ratio` CSS**

MDN Contributors. (2026, 20 de abril). *aspect-ratio*. MDN Web Docs.  
https://developer.mozilla.org/en-US/docs/Web/CSS/Reference/Properties/aspect-ratio

---

### 1.3 `App.vue` — Carga centralizada de datos

**`fetch()` + `async/await`**  
Investigado para cargar `portafolio.json` una sola vez y distribuir los datos por props a los componentes hijos.

MDN Contributors. (2025, 20 de agosto). *Using the Fetch API*. MDN Web Docs.  
https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API/Using_Fetch

MDN Contributors. (2026, 16 de diciembre). *Window: fetch() method*. MDN Web Docs.  
https://developer.mozilla.org/en-US/docs/Web/API/Window/fetch

**`import.meta.env.BASE_URL` en Vite**  
Necesario para que la ruta al JSON funcione tanto en desarrollo local como desplegado en GitHub Pages.

Vite. (s.f.). *Env Variables and Modes*. Vite.  
https://vite.dev/guide/env-and-mode

**`new URL(url, import.meta.url)` — resolución dinámica de assets**  
Investigado para referenciar imágenes, audio y video por nombre de archivo leído desde el JSON, sin rutas hardcodeadas.

Vite. (s.f.). *Static Asset Handling — new URL(url, import.meta.url)*. Vite.  
https://vitejs.dev/guide/assets.html#new-url-url-import-meta-url

---

### 1.4 Vue 3 — Composition API (global a todos los componentes)

**`ref()`, `reactive()`, `onMounted()`, `onUnmounted()`**

Vue.js. (s.f.). *Reactivity Fundamentals*. Vue.js Guide.  
https://vuejs.org/guide/essentials/reactivity-fundamentals.html

Vue.js. (s.f.). *Composition API FAQ*. Vue.js Guide.  
https://vuejs.org/guide/extras/composition-api-faq.html

**`computed()`**  
Usado en `Header.vue` y `About.vue` para derivar URLs de assets reactivamente desde props.

Vue.js. (s.f.). *Computed Properties*. Vue.js Guide.  
https://vuejs.org/guide/essentials/computed.html

**`defineProps()`**  
Investigado para implementar el patrón Single Source of Truth: App.vue pasa datos a los hijos via props.

Vue.js. (s.f.). *Props*. Vue.js Guide.  
https://vuejs.org/guide/components/props.html

**`v-for`, `v-if`**

Vue.js. (s.f.). *List Rendering*. Vue.js Guide.  
https://vuejs.org/guide/essentials/list

Vue.js. (s.f.). *Conditional Rendering*. Vue.js Guide.  
https://vuejs.org/guide/essentials/conditional

**`<script setup>`**

Vue.js. (s.f.). *`<script setup>`*. Vue.js API Reference.  
https://vuejs.org/api/sfc-script-setup

**`:style` binding con template literal**  
Necesario en Skills.vue para aplicar posicionamiento calculado matemáticamente con `Math.cos`/`Math.sin`.

Vue.js. (s.f.). *Class and Style Bindings*. Vue.js Guide.  
https://vuejs.org/guide/essentials/class-and-style.html

---

### 1.5 CSS Global

**Variables CSS (`--nombre`)**

MDN Contributors. (2026, 2 de febrero). *Custom properties (--\*): CSS variables*. MDN Web Docs.  
https://developer.mozilla.org/en-US/docs/Web/CSS/Reference/Properties/--*

MDN Contributors. (2025, 16 de diciembre). *Using CSS custom properties (variables)*. MDN Web Docs.  
https://developer.mozilla.org/en-US/docs/Web/CSS/Guides/Cascading_variables/Using_custom_properties

**Mobile-first con `@media (min-width)`**

MDN Contributors. (2025, 19 de diciembre). *Responsive web design*. MDN Web Docs.  
https://developer.mozilla.org/en-US/docs/Learn_web_development/Core/CSS_layout/Responsive_Design

MDN Contributors. (s.f.). *Media query fundamentals*. MDN Web Docs.  
https://developer.mozilla.org/en-US/docs/Learn_web_development/Core/CSS_layout/Media_queries

**`display: flex` / `display: grid`**

MDN Contributors. (2026, 20 de abril). *display*. MDN Web Docs.  
https://developer.mozilla.org/en-US/docs/Web/CSS/Reference/Properties/display

MDN Contributors. (2025, 5 de diciembre). *Basic concepts of flexbox*. MDN Web Docs.  
https://developer.mozilla.org/en-US/docs/Web/CSS/Guides/Flexible_box_layout/Basic_concepts

MDN Contributors. (s.f.). *CSS grid layout*. MDN Web Docs.  
https://developer.mozilla.org/en-US/docs/Web/CSS/Guides/Grid_layout

---

### 1.6 `Skills.vue` — Visualización circular con SVG y matemáticas

**SVG inline — `<svg>`, `<line>`, `viewBox`, `preserveAspectRatio`**

MDN Contributors. (2025, 6 de junio). *`<line>`*. MDN Web Docs.  
https://developer.mozilla.org/en-US/docs/Web/SVG/Reference/Element/line

MDN Contributors. (2026, 7 de abril). *viewBox*. MDN Web Docs.  
https://developer.mozilla.org/en-US/docs/Web/SVG/Reference/Attribute/viewBox

MDN Contributors. (2026, 7 de abril). *preserveAspectRatio*. MDN Web Docs.  
https://developer.mozilla.org/en-US/docs/Web/SVG/Reference/Attribute/preserveAspectRatio

**Trigonometría con `Math.cos` / `Math.sin` — distribución circular**  
Investigado para calcular las coordenadas (x, y) de cada habilidad en el anillo orbital.

MDN Contributors. (2025, 29 de diciembre). *Math.cos()*. MDN Web Docs.  
https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Math/cos

MDN Contributors. (2025, 29 de diciembre). *Math.sin()*. MDN Web Docs.  
https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Math/sin

**`Array.from({ length: n }, ...)`**

MDN Contributors. (2025, 10 de julio). *Array.from()*. MDN Web Docs.  
https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/from

**Animaciones CSS — órbita y contra-rotación**  
El truco principal de Skills: la órbita gira en un sentido y cada ícono contra-gira en el sentido opuesto para que el texto quede siempre legible (patrón `rotate(a) translate(r) rotate(-a)`).

MDN Contributors. (2026, 11 de mayo). *animation*. MDN Web Docs.  
https://developer.mozilla.org/en-US/docs/Web/CSS/Reference/Properties/animation

MDN Contributors. (2026, 18 de abril). *rotate() — CSS transform function*. MDN Web Docs.  
https://developer.mozilla.org/en-US/docs/Web/CSS/Reference/Values/transform-function/rotate

MDN Contributors. (2026, 20 de abril). *transform-origin*. MDN Web Docs.  
https://developer.mozilla.org/en-US/docs/Web/CSS/Reference/Properties/transform-origin

MDN Contributors. (2026, 11 de junio). *transform*. MDN Web Docs.  
https://developer.mozilla.org/en-US/docs/Web/CSS/Reference/Properties/transform

Boyd, W. (2024, 16 de septiembre). *Making Orbit Animations with CSS Custom Properties*. Coder's Block.  
https://codersblock.com/blog/making-orbit-animations-with-css-custom-properties/

**CSS Posicionamiento — `position: absolute` + `inset: 0` + `translate(-50%, -50%)`**

MDN Contributors. (2026, 7 de junio). *position*. MDN Web Docs.  
https://developer.mozilla.org/en-US/docs/Web/CSS/Reference/Properties/position

MDN Contributors. (2026, 20 de abril). *inset*. MDN Web Docs.  
https://developer.mozilla.org/en-US/docs/Web/CSS/Reference/Properties/inset

MDN Contributors. (2026, 18 de abril). *translate() — CSS transform function*. MDN Web Docs.  
https://developer.mozilla.org/en-US/docs/Web/CSS/Reference/Values/transform-function/translate

---

## 2. Tutoriales y cursos

- GOGODEV. (2023, 13 de febrero). *Curso de VUE 3 - Profesional* [Video]. YouTube. https://www.youtube.com/watch?v=LAozf_wDejU
- Gutiérrez, I. [Bluuweb]. (2022, 23 de agosto). *Fundamentos de Vue 3 con Composition API: Aprende a crear aplicaciones web modernas y escalables* [Video]. YouTube. https://www.youtube.com/watch?v=PL-aTHv2L3E
- AMazaing Code. (2025, 17 de marzo). *Vue - Curso COMPLETO desde 0: Paso a paso y con ejemplos* [Video]. YouTube. https://www.youtube.com/watch?v=KdfrY2GYuTo
- Pelling, S. [The Net Ninja]. (2020, 2 de diciembre). *Vue JS 3 tutorial for beginners #1 - Introduction* [Video]. YouTube. https://www.youtube.com/watch?v=YrxBCBibVo0
- Traversy, B. [Traversy Media]. (2024, 1 de julio). *Vue.js crash course 2024* [Video]. YouTube. https://www.youtube.com/watch?v=VeNfHj6MhgA
- Lenguaje JS. (s.f.). *Composition API (Vue 3)*. Recuperado el 11 de junio de 2026, de https://lenguajejs.com/vuejs/componentes/composition-api/
- Vue School. (s.f.). *Vue.js fundamentals with the Composition API* [Curso]. Recuperado el 11 de junio de 2026, de https://vueschool.io/courses/vue-js-fundamentals-with-the-composition-api
- Platzi. (s.f.). *¿Qué es la Composition API en Vue 3?* Recuperado el 11 de junio de 2026, de https://platzi.com/blog/vue-composition-api/

---

## 3. Estándares de desarrollo

- Manz.dev. (s.f.). *Bootcamp de desarrollo web moderno*. Documentos de referencia del curso IF7102 consultados para buenas prácticas en HTML semántico, CSS moderno con variables, JavaScript ESM y diseño mobile-first.

---

## 4. Proyectos académicos exhibidos

| # | Proyecto | Tipo | Curso | Profesor encargado | Repositorio |
|---|---|---|---|---|---|
| 1 | Guía Turística Multimedia Costa Rica | Grupal | IF-7102 Multimedios · I Ciclo 2026 · UCR | Lic. Iván Alonso Chavarría Cubero | https://github.com/JCarlosJimenezC/proyecto-guia-turistica |
| 2 | Sistema de Gestión de Restaurante | Grupal | IF-4101 Lenguajes para Aplicaciones Comerciales · I Ciclo 2026 · UCR | Luis Fernando Charpentier González | https://git.ucr.ac.cr/JUAN.JIMENEZCASTRILLO/gestorderestaurante |
| 3 | Aurora Boutique — Tienda Femenina | Grupal | IF-5100 Administración de Bases de Datos · I Ciclo 2026 · UCR | MSc. Andrés Alberto Cortés Fuentes | https://github.com/TatianaJSM/BoutiqueAurora |
| 4 | El Impostor Ambiental | Individual | — | — | https://github.com/JCarlosJimenezC/Video-Juego---Impostor-Ambiental |
| 5 | Sistema de Declaraciones Juradas UCR | Grupal | IF6100 Análisis & Diseño de Sistemas · I Ciclo 2026 · UCR | Lic. Iván Alonso Chavarría Cubero | https://github.com/1KATZUNO/Declaracion_Jurada |
| 6 | Reciclaje Sámara | Individual | — | — | https://github.com/JCarlosJimenezC/Reciclaje-Samara |
| 7 | Sexy Mango Challenge | Individual | — | — | https://github.com/JCarlosJimenezC/SexyMango |

---

### 4.1 Integrantes por proyecto

#### 1. Guía Turística Multimedia Costa Rica

**Profesor encargado:** Lic. Iván Alonso Chavarría Cubero

| Carné | Estudiante |
|---|---|
| C33980 | Jiménez Castrillo Juan Carlos |
| C36462 | Ramírez Sandoval Demian Josué |
| C37730 | Sosa Rodríguez Kener Josué |
| C35880 | Parra Aguirre Maylo |
| C35652 | Obando González Joshua |

#### 2. Sistema de Gestión de Restaurante

**Profesor encargado:** Luis Fernando Charpentier González

| Carné | Estudiante |
|---|---|
| C33980 | Jiménez Castrillo Juan Carlos |
| C33417 | González Quirós Eddy Josué |
| C37730 | Sosa Rodríguez Kener Josué |
| C35880 | Parra Aguirre Maylo |

#### 3. Aurora Boutique — Tienda Femenina

**Profesor encargado:** MSc. Andrés Alberto Cortés Fuentes

| Carné | Estudiante |
|---|---|
| C34080 | Jiménez Solís Tatiana María |
| C33631 | Guzmán Gómez Manuel Esteban |
| C33980 | Jiménez Castrillo Juan Carlos |
| C34386 | López Reyes Alison Sofía |
| C33417 | González Quirós Eddy Josué |
| C34387 | López Reyes Keity Valeria |

#### 4. El Impostor Ambiental *(individual)*

| Carné | Estudiante |
|---|---|
| C33980 | Jiménez Castrillo Juan Carlos |

#### 5. Sistema de Declaraciones Juradas UCR

**Profesor encargado:** Lic. Iván Alonso Chavarría Cubero

**Grupo 5 — grupo del autor**

| Carné | Estudiante | Correo |
|---|---|---|
| C33980 | Jiménez Castrillo Juan Carlos | juan.jimenezcastrillo@ucr.ac.cr |
| C33417 | González Quirós Eddy Josué | eddy.gonzalezquiros@ucr.ac.cr |
| C35652 | Obando González Joshua David | joshua.obandogonzalez@ucr.ac.cr |
| C35880 | Parra Aguirre Maylo Daring | maylo.parra@ucr.ac.cr |
| C37730 | Sosa Rodríguez Kener Josué | kener.sosa@ucr.ac.cr |
| C36462 | Ramírez Sandoval Demian | demian.ramirez@ucr.ac.cr |

**Grupo 6**

| Carné | Estudiante | Correo |
|---|---|---|
| C01729 | Carrillo Ponce Austin Antonio | austin.carrillo@ucr.ac.cr |
| C35538 | Narváez Umaña Ashley Dariana | ashley.narvaez@ucr.ac.cr |
| C35697 | Oporta Campos Chrisley | chrisley.oporta@ucr.ac.cr |
| C31386 | Caballero Jarquín Nathalie Tamara | nathalie.caballero@ucr.ac.cr |
| B56394 | Ruíz Pérez Oscar | oscar.ruizperez@ucr.ac.cr |

**Grupo 7**

| Carné | Estudiante | Correo |
|---|---|---|
| C24594 | Martínez Martínez Emiliano | emiliano.martinez@ucr.ac.cr |
| C24318 | López Alvarado Israel | israel.lopezalvarado@ucr.ac.cr |
| C28846 | Calderón Espinoza Bryan Josué | brayan.calderonespinoza@ucr.ac.cr |
| C09149 | Ortiz Martínez Dany Josué | danny.ortizmartinez@ucr.ac.cr |
| C31592 | Campos Mora César Alberto | cesar.camposmora@ucr.ac.cr |

**Grupo 8**

| Carné | Estudiante | Correo |
|---|---|---|
| C27912 | Umaña Guevara Alexander | alexander.umanaguevara@ucr.ac.cr |
| C18966 | Chavarría Duarte Eli | eli.chavarria@ucr.ac.cr |
| C10098 | Aguilar Alvarado Esteban | esteban.aguilaralvarado@ucr.ac.cr |
| C16913 | Rojas Zúñiga Bryan | bryan.rojaszuniga@ucr.ac.cr |

**Grupo 9**

| Carné | Estudiante | Correo |
|---|---|---|
| C23740 | Hernández González Rafael David | rafael.hernandez@ucr.ac.cr |
| C36641 | Rodríguez Badilla Ángeles | angeles.rodriguez@ucr.ac.cr |
| C07126 | Salazár Hernández Gerald | gerald.salazarhernandez@ucr.ac.cr |
| C10487 | Arauz Villanueva Clivian | clivian.arauz@ucr.ac.cr |
| C34106 | Juárez González Elfred Adal | elfred.juarez@ucr.ac.cr |

#### 6. Reciclaje Sámara *(individual)*

| Carné | Estudiante | Correo |
|---|---|---|
| C33980 | Jiménez Castrillo Juan Carlos | juan.jimenezcastrillo@ucr.ac.cr |

#### 7. Sexy Mango Challenge *(individual)*

| Carné | Estudiante | Correo |
|---|---|---|
| C33980 | Jiménez Castrillo Juan Carlos | juan.jimenezcastrillo@ucr.ac.cr |

---

## 5. Recursos multimedia — Licencias

| Recurso | Archivo | Origen | Licencia |
|---|---|---|---|
| Fotografía de perfil | `juanky.png` | Fotografía personal del estudiante | Autoría propia |
| Audio de autopresentación | `juanky-audio.mp3` | Grabación de voz realizada por el estudiante | Autoría propia |
| Video de introducción | `juanky-video.mp4` | Video grabado por el estudiante | Autoría propia |
| Imagen proyecto 1 — Guía Turística | `Guia-Turistica.png` | Captura de pantalla de proyecto propio | Autoría propia |
| Imagen proyecto 2 — Gestor Restaurante | `Gestor-Restaurante.png` | Captura de pantalla de proyecto propio | Autoría propia |
| Imagen proyecto 3 — Aurora Boutique | `aurora-boutique.jpg` | Captura de pantalla de proyecto propio | Autoría propia |
| Imagen proyecto 4 — El Impostor Ambiental | `El_Impostor_Ambiental.png` | Captura de pantalla de proyecto propio | Autoría propia |
| Imagen proyecto 5 — Declaraciones Juradas | `Declaracion-jurada.png` | Captura de pantalla de proyecto propio | Autoría propia |
| Imagen proyecto 6 — Reciclaje Sámara | `Reciclaje-Samara.png` | Captura de pantalla de proyecto propio | Autoría propia |
| Imagen proyecto 7 — Sexy Mango Challenge | `Sexy-Mango.png` | Captura de pantalla de proyecto propio | Autoría propia |
| Tipografía Inter | `Inter.var.woff2` | Rasmus Andersson / Google Fonts | SIL Open Font License 1.1 |

---

## 6. Herramientas de IA utilizadas

- **Claude (Anthropic) — claude.ai:**
  - Apoyo en la comprensión de la Composition API de Vue 3
  - Debugging de errores de configuración de Vite (alias `@`, rutas de assets)
  - Consultas sobre CSS Grid, Flexbox e Intersection Observer API
  - Revisión de estructura de componentes y props
  - *Uso:* Consultas conceptuales y resolución de errores puntuales. Todo el código fue revisado, comprendido y adaptado por mi (Juan C. Jiménez C.).
