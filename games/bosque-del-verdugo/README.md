# 🪓 El Bosque del Verdugo

Juego de terror **2D top-down** hecho en un **único archivo HTML** sin dependencias
(Canvas 2D + Web Audio API). Te despiertas perdido en un bosque infinito mientras
**El Verdugo** —una mole con máscara de hockey y un martillo gigante— te persigue
entre los árboles.

## 🎮 Cómo jugar

Abre `index.html` en cualquier navegador moderno (Chrome, Firefox, Edge, Safari).
Recomendado con **auriculares** 🎧: la música reacciona a la cercanía del jefe.

| Acción | Teclas |
| --- | --- |
| Moverse | `W A S D` o flechas |
| Esprintar (gasta resistencia) | `Shift` |
| Recoger llave / abrir portón | `E` o `Espacio` |
| Esconderse | Métete en un **arbusto** y quédate quieto |

También funciona en **móvil/táctil** (joystick virtual + botón USAR).

## 🎯 Objetivo

1. Explora el bosque (el mapa se revela con **niebla de guerra** según avanzas).
2. Reúne las **3 llaves** ocultas en zonas alejadas.
3. Llega al **Portón de Salida** y escapa. Salir es lo difícil: cada llave que
   recoges **enfurece al Verdugo y lo hace más rápido**.

## 🧠 Diseño técnico (lo interesante)

- **IA del jefe — máquina de estados** `PATROL → HUNT → SEARCH`:
  - Visión realista = **rango + cono de visión (FOV) + línea de visión** (Bresenham,
    los árboles bloquean).
  - Esconderse en arbustos reduce drásticamente el rango de detección; correr lo
    aumenta (haces ruido).
  - Al perderte de vista va a tu **último avistamiento** y patrulla la zona antes
    de rendirse.
- **Pathfinding A\*** sobre la grilla de celdas caminables, con re-cálculo periódico
  y sin cortar esquinas. Heap binario propio para rendimiento.
- **Generación procedural del bosque**: cúmulos de árboles por caminatas aleatorias,
  arbustos-escondite, llaves repartidas por cuadrantes y un portón en un borde
  aleatorio. **Conectividad garantizada** vía flood-fill + tunelado en L.
- **Música procedural de suspenso (Web Audio API)**: un valor de *peligro* `(0..1)`
  derivado de la distancia/estado del jefe controla en tiempo real:
  - **Latidos de corazón** cuyo tempo se acelera de ~48 a ~165 BPM.
  - Un **drone** grave y **cuerdas disonantes** (2ª menor + tritono) que entran con
    el terror.
  - Ambiente de viento que baja cuando sube la tensión.
  - *Stingers* al ser descubierto y golpe de martillo al morir.
- **Niebla de guerra** con tres niveles: inexplorado (negro), recordado (oscuro) y
  visible (gradiente suave alrededor del jugador).
- Extras: minimapa-radar (el jefe solo aparece si está muy cerca), linterna en cono,
  partículas, *screen shake*, viñeta pulsante y barra de resistencia.

Todo en `index.html` (~700 líneas, comentado en español).
