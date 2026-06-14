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

> ⚠️ **Cepos**: hay trampas repartidas por el bosque. Si las pisas te quedas
> **atrapado** unos segundos (forcejea pulsando direcciones para soltarte) — y el
> Verdugo se te echa encima. Pero también puedes **llevarlo a un cepo**: si él lo
> pisa, queda inmovilizado más tiempo y puedes escapar.
>
> 🕗 Al empezar tienes unos segundos de **gracia**: el Verdugo deambula lento y no
> caza hasta que "despierta".

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
- **Sprites pixel-art horneados por código**: cada sprite (jugador, Verdugo, árboles,
  arbustos, llave, tiles de hierba/camino) se dibuja a baja resolución con una paleta
  limitada, se le añade un **contorno automático** de 1px y se escala con
  *nearest-neighbor* (`imageSmoothingEnabled=false`) → estética retro nítida sin
  archivos externos. Personajes **animados por dirección** (abajo/arriba/lado, con
  *flip* horizontal) y **ciclo de caminado** de 2 frames.
- **Iluminación suave** (en vez de niebla cuadriculada): un *mapa de brillo* combina
  oscuridad total, terreno recordado en gris tenue y un halo cálido degradado
  (linterna) que se multiplica sobre la escena. Luces guía sobre llaves/portón.
- **Cámara con zoom** y seguimiento suavizado; movimiento con aceleración/frenado.
- Extras: minimapa-radar (el jefe solo aparece si está muy cerca), partículas,
  *screen shake*, viñeta pulsante y barra de resistencia.

Todo en `index.html` (~700 líneas, comentado en español).
