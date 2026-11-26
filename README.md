# AR B-24D — A-Frame + MindAR + MediaPipe

Realidad aumentada web: al apuntar la cámara a `markers/marcador.png` aparece el B-24D volando encima,
y se puede controlar con gestos de las manos (MediaPipe Hand Landmarker).

## Estructura
```
index.html              Escena, interfaz y panel de ayuda
js/app.js               Botones, avisos, foto, teclado, cámara en alta resolución
js/componentes.js       Componentes A-Frame: seguimiento suavizado, piloto (animaciones),
                        manipulador (rotar/escalar), partículas
js/manos.js             MediaPipe: detección de manos, gestos e interacciones
js/sonido.js            Motor y efectos sintetizados con Web Audio
markers/marcador.png    Marcador a imprimir / mostrar
markers/marcador.mind   Marcador compilado para MindAR
markers/pato.*          Marcador anterior (pato)
models/b24d.glb         Modelo 3D
```

## Probar
```bash
cd ar-avion
python3 -m http.server 8000
```
Abrir http://localhost:8000. En el celular la cámara exige **HTTPS**
(GitHub Pages / Netlify, o un túnel como `ngrok http 8000`).

## Gestos (interacción directa en 3D)
| Gesto | Acción |
|---|---|
| ☝️ Tocar el avión con la punta del índice | Tonel + confeti |
| 🤏 Pellizcar el avión y moverlo | Lo arrastras en 3D (también hacia/desde la cámara); al soltar vuelve |
| ✋ Palma abierta 0,7 s | El avión vuela frente a tu palma y la sigue; ciérrala y vuelve |
| 🤏🤏 Pinza con las dos manos | Escalar (separar/juntar) |

Botones: 🌀 modo · 🚀 turbo · 💨 estela · ↺ tamaño · 📏 calibrar · 🖐 manos · 🔇 sonido · 📷 foto · ⇆ espejo
Teclado: M modo · T turbo · E estela · R tonel · Espacio pausa · F foto · C calibrar · H ver mano 3D · O oclusión

## Profundidad con una sola webcam
- La palma es casi rígida: su tamaño en pantalla indica su distancia (`distancia = focal × tamañoReal / tamañoEnPantalla`).
- **Calibración 📏**: mano plana sobre el marcador 1,5 s. En ese momento la mano está a la distancia del marcador
  (que MindAR conoce), así se aprende el tamaño de tu palma. Se guarda en el navegador.
- MediaPipe da la profundidad relativa de cada dedo; con eso se reconstruye la mano 3D en el espacio del avión.
- Tocar/agarrar exige estar a la profundidad del avión. El dedo índice se colorea:
  🟢 a su profundidad · 🔵 detrás · 🟠 delante.
- **Oclusión**: una mano 3D invisible escribe en el buffer de profundidad, así la mano tapa el avión cuando está delante
  (tecla H para verla).

## Seguimiento
- MindAR con poco suavizado (responde rápido a la inclinación).
- `seguir-marcador` aplica un filtro adaptativo: quieto = estable, movimiento rápido = sigue sin retraso.
- Si el marcador se pierde (p. ej. lo tapa una mano) mantiene el avión 1,5 s.
- Ajustes en `index.html`: `seguir-marcador="suavidad: 10; mantener: 1500"`.

## Cambiar el marcador
Compila la imagen en https://hiukim.github.io/mind-ar-js-doc/tools/compile,
reemplaza `markers/marcador.mind` y sube el `?v=` en `index.html` para que el navegador no use el anterior.

---

## Ficha para el Taller de Diseño de Interfaces

**Unidad 3 · Interfaces Gráficas · Módulo de realidad aumentada** · Taller de Diseño de Interfaces, Ingeniería en Diseño UAI · 2026 · Prof. Jorge Forero
Autor: Luis-Felipe Benitez · Ficha: https://portafolio.insta3d.cl/ar-avion/ · Experiencia: https://portafolio.insta3d.cl/ar-avion/app/

### Objetivos
- Anclar un objeto 3D (un bombardero B-24D) a una imagen impresa con realidad aumentada en el navegador.
- Manipularlo con las manos desnudas: tocarlo, agarrarlo, hacerlo seguir la palma y escalarlo con dos manos.
- Resolver la profundidad de la mano con una sola webcam, para que tocar el avión se sienta físico.

### Descripción y público
Una lámina impresa se convierte en una pista: al apuntar la cámara aparece el avión volando encima, con motor y efectos sonoros sintetizados, y responde a los gestos. Pensado para exhibiciones y aulas: cualquier persona con un celular o un computador con cámara puede interactuar sin instrucciones previas.

### Tecnologías
| Tecnología | Uso |
|---|---|
| A-Frame 1.5 | Escena 3D y componentes propios (seguimiento suavizado, piloto, manipulador, partículas) |
| MindAR 1.2.5 (image tracking) | Seguimiento del marcador impreso |
| MediaPipe Hand Landmarker | Detección de manos y gestos; reconstrucción 3D de la mano y oclusión |
| Web Audio | Motor del avión y efectos sintetizados, sin archivos de audio |

### Proceso e iteraciones
1. **Marcador de imagen (NFT):** primera prueba con descriptores de imagen (`markers/images.fset`, `.fset3`, `.iset`), el formato de AR.js.
2. **MindAR con un marcador de pato** (`markers/pato.*`): seguimiento más estable en el navegador.
3. **Marcador propio** (`markers/marcador.png`) con más detalle, para que no se pierda al inclinarlo.
4. **Gestos:** de botones en pantalla a tocar, pellizcar y seguir la palma.
5. **Profundidad y oclusión:** calibración con la palma sobre el marcador y una mano 3D invisible que tapa el avión.

> 📌 **Por aportar:** capturas de cada etapa, foto del marcador impreso en uso y notas de pruebas con usuarios (carpeta `proceso/`). Confirmar la licencia y el autor del modelo 3D del B-24D.

### Video demostrativo
> 📌 **Por aportar:** video tocando y moviendo el avión con las manos, subido a YouTube y enlazado aquí.
