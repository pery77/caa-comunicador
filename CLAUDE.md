# Prompt Maestro para Claude: Desarrollo de App de CAA (Comunicación Aumentativa y Alternativa)

## 🤖 Contexto y Rol
Actúa como un Desarrollador Senior de Software y Experto en Accesibilidad UX/UI. Vamos a desarrollar una aplicación de Comunicación Aumentativa y Alternativa (CAA) para una tablet Android.

**El Usuario Final:**
El usuario final es un adulto que ha sufrido un ictus. Presenta hemiplejia (solo puede usar una mano) y afasia/anartria (no puede hablar). **Su visión y comprensión lectora son perfectas**, por lo que la interfaz debe centrarse en la velocidad y eficiencia de escritura de texto, prescindiendo de pictogramas infantiles.

**El Hardware:**
- Pantalla: Tablet Android (pantalla horizontal).
- Controlador: Un mando Bluetooth de una sola mano operado con su mano funcional.
- Entrada (MUY IMPORTANTE): El mando funciona en **Modo Gamepad/Teclado (D-Pad)**. No hay un cursor de ratón libre. Al mover el joystick, el foco (highlight) debe saltar magnéticamente de un elemento de la interfaz a otro de forma estructurada (como navegar por el menú de una consola).

## 🎯 Objetivo de la App
Crear una interfaz de síntesis de voz (Text-to-Speech) que sea extremadamente rápida, requiriendo el **mínimo número de saltos de foco y clics posibles** para generar frases, reduciendo la frustración y la fatiga física.

## 🛠️ Especificaciones Técnicas y Stack
- **Tecnología:** PWA (HTML5/JS) a pantalla completa en Chrome/WebView de Android. *(Decidido en conversación.)*
- **API de Voz:** Uso de la API nativa de Text-to-Speech (TTS) del dispositivo o del navegador (`speechSynthesis`).

## 🧠 Principios de Diseño UX/UI (Accesibilidad Crítica)
1. **Navegación Espacial Precisa:** La interfaz debe estar diseñada para un sistema de cuadrícula (Grid) o nodos. Cada botón debe tener una ruta clara y predecible hacia arriba, abajo, izquierda y derecha.
2. **Cero scroll:** Toda la interfaz principal debe caber en una sola pantalla estática.
3. **Feedback visual extremo:** El elemento seleccionado actualmente (en foco) debe destacar dramáticamente (borde grueso, cambio de color brillante, aumento de tamaño) para que sepa exactamente dónde está el "cursor" sin esfuerzo.
4. **Zonas muertas y debounce:** El sistema debe prever posibles temblores en el joystick, asegurando que un movimiento no salte dos casillas por error.

## ⚙️ Funcionalidades Core (MVP)
Diseña la arquitectura e impleméntala paso a paso según estos requisitos:

### 1. Sistema de Entrada Optimizado para D-Pad
- Implementa una arquitectura de teclado espacialmente eficiente.
- *Reto para Claude:* No uses un teclado QWERTY tradicional lineal, ya que requiere demasiados saltos para ir de la 'A' a la 'P'. Propón un sistema de matriz optimizada, un teclado dividido en cuadrantes, o un teclado predictivo dinámico donde las letras más comunes estén siempre a 1 o 2 saltos de distancia.

### 2. Teclado Predictivo y Autocompletado (Crucial)
- La app debe tener un motor de sugerencia de palabras muy rápido. Al introducir 1 o 2 letras, una gran sección de la pantalla debe mostrar botones gigantes con las palabras más probables.
- Navegar hacia la palabra sugerida debe requerir el menor esfuerzo posible.

### 3. Panel de "Frases Rápidas" (Acceso directo)
- Una sección dedicada a necesidades urgentes (ej. "Sí", "No", "Me duele algo", "Tengo sed").
- Estas opciones deben estar disponibles con movimientos rápidos desde el punto de inicio.

### 4. Controles Principales Permanentes
- Botón de **HABLAR** (Reproduce el TTS y limpia la caja de texto).
- Botón de **BORRAR** (Borra la última letra/palabra).

## 📝 Plan de Trabajo
El proyecto se desarrolla en fases iterativas (no escribir todo el código de golpe):

1. **Fase 1: Propuesta de Interfaz (Wireframe lógico y Navegación).** Layout de pantalla y flujo exacto para escribir con solo movimientos de D-Pad (Arriba, Abajo, Izquierda, Derecha, Confirmar).
2. **Fase 2: Setup y Estructura.** Código base (esqueleto) y conexión con la API de Text-to-Speech.
3. **Fase 3: Implementación de la Navegación Espacial.** Lógica del D-Pad y gestión del foco de UI.
4. **Fase 4: Sistema de Predicción.** Implementación de sugerencias de palabras.

---

## 📌 Estado del Proyecto (actualizar al avanzar)

**Última actualización:** 2026-08-18 (pantalla completa de frases guardadas: favoritas, borrar, paginación)

- ✅ **Fase 1 completada** — Diseño acordado:
  - Matriz de letras 6×5 organizada por frecuencia del español, con la **E en el centro** (letras frecuentes a 0–2 saltos). El foco **no vuelve al centro** tras cada letra (foco persistente).
  - Layout: caja de texto arriba (no enfocable) + HABLAR · fila de predicciones · matriz de letras (centro-izquierda) · frases rápidas (columna derecha) · barra de acciones abajo (espacio, borrar letra/palabra/todo).
  - Los **botones físicos extra del mando** se mapean como atajos de coste cero (confirmar, borrar, aceptar predicción nº 1, espacio).
  - El layout de teclado será **configurable** (matriz de frecuencia vs QWERTY compacto), por si el usuario conserva memoria muscular del QWERTY.
- ✅ **Fase 2 completada** — `index.html` autocontenido con:
  - Modo diagnóstico de entrada: registra eventos de teclado (`key`/`code`/`keyCode`) y de la API Gamepad (botones y ejes sondeados por frame), con compresión de repeticiones ×N.
  - TTS conectado vía `speechSynthesis` con detección de voces en español (prefiere `es-ES`).
- ✅ **Fase 3 implementada (pendiente de validar en la tablet)** — `index.html` es ya el comunicador real; el diagnóstico se conserva en `diagnostico.html`.
  - ⚠️ **Los códigos del mando varían entre PC y Android** (confirmado en pruebas): en Android la cruceta llega además como **botones 12-15** (mapeo "standard"), lo que hacía que mover el joystick también pulsara casillas. Corregido: botones 12-15 = movimiento, excluidos de confirmar. El usuario desarrolla en PC por comodidad; validar cada cambio de entrada también en la tablet. Pendiente verificar voces TTS de Android y layout real antes de la entrega.
  - **Datos reales del mando (reportados por el usuario):** joystick como ejes analógicos 0/1 **y duplicado como hat (cruceta) en el eje 9** (reposo=3.29, arriba=−1.00, codificación estándar Android en pasos de 2/7). Un botón físico emite **ráfagas de varios botones a la vez** (p. ej. 1+7+9; también se vieron 0 y 10) y en algún modo una tecla **F19**.
  - **Capa de entrada unificada:** todo converge en `pedirMovimiento()` / `pedirConfirmar()` con dedupe — ráfaga de botones <250 ms = 1 pulsación; movimiento duplicado analógico/hat <90 ms = 1 salto. **Cualquier botón del mando = confirmar** (no hace falta distinguir gatillo/botón frontal para la v1). Histéresis en zona muerta (0.6 activar / 0.35 soltar), autorepetición al mantener (400 ms + 190 ms), solo direcciones puras (sin diagonales).
  - **Navegación espacial geométrica:** el foco salta al elemento más cercano en la dirección pedida (penalizando desvío lateral), sin envolver bordes. Foco inicial en la E. Estilo de foco dramático (amarillo, escala, glow).
  - Matriz 6×5 por frecuencia implementada, frases rápidas que hablan al instante, barra de acciones (espacio/borrar letra/palabra/todo), HABLAR limpia la caja. Desbloqueo de TTS con el primer gesto (Chrome lo exige).
- ✅ **Fase 4 implementada** — Motor de predicción offline embebido en `index.html`:
  - Diccionario de ~500 palabras ordenado por frecuencia **conversacional** (función + vida diaria + salud/cuidados, no corpus de prensa). Emparejamiento **sin tildes** (la matriz no tiene acentos: "medico" → sugiere "médico") preservando la ñ.
  - **Aprendizaje personal:** cada palabra dicha con HABLAR suma uso en `localStorage` (clave `caa_usos`) y sube de rango; palabras desconocidas (nombres propios…) se incorporan a las sugerencias. Con la caja vacía se sugieren las más usadas por el usuario + arranques ("quiero", "necesito"…).
  - Aceptar una predicción reemplaza la palabra parcial e inserta espacio. Casillas vacías no reciben foco; si el foco se queda en una casilla que se vacía, vuelve a la E.
  - Verificado con test en Node (stubs de DOM): sintaxis, normalización y ranking.

- ✅ **Mejoras post-MVP implementadas:**
  - **REPETIR** (🔁 junto a HABLAR): vuelve a decir la última locución.
  - **Pantalla de frases guardadas** (sustituye a la columna alternadora frases↔recientes, que se quedaba pequeña): botón "🕒 GUARDADAS" arriba de la columna derecha abre una pantalla completa (`#pantalla-frases`; `#app` se oculta con `body.en-frases`, y `esEnfocable` filtra por `offsetParent` para que el foco no salte a la pantalla oculta). Muestra **favoritas primero** (clave `caa_favoritas`, nunca caducan) y luego las últimas frases dichas (`caa_historial`, ampliado de 12 a 60), **paginadas de 10 en 10** (◀ 1/3 ▶; flechas `.deshabilitado` no enfocables). Barra superior con VOLVER (también Escape en PC) y **selector de modo**: pulsar una frase la habla / la marca-desmarca ⭐ / la borra según el modo activo — así cada frase es un solo botón sin submenús, y la lista entera se tiñe (ámbar/rojo) para anticipar el efecto. Siempre se entra en modo hablar y con el foco en la primera frase; al volver, el foco regresa a la E. **La columna derecha muestra las 6 primeras favoritas como accesos rápidos** (el usuario elige sus frases marcándolas ⭐); solo si no hay ninguna se ofrecen las frases fijas de urgencia (`FRASES_RAPIDAS`). Verificado con test en Node (stubs de DOM): apertura, modos, borrado, paginación, recolocación de foco y tope del historial.
  - **Predicción de palabra siguiente:** bigramas personales (`caa_pares`) aprendidos de lo que dice el usuario; con la caja en inicio de palabra se sugiere lo que suele seguir a la palabra anterior, y durante la escritura los seguidores habituales reciben gran bono de ranking. Todo con test en Node.
  - **Grupos visuales en la matriz:** vocales con tinte cálido (localizables de un vistazo), puntuación atenuada, K/W/X apagadas. Decisión de diseño: NO colorear más grupos de consonantes (más de 2-3 categorías de color = ruido visual que ralentiza la búsqueda; las consonantes frecuentes ya están codificadas por su posición central). El resaltado de foco usa `!important` para dominar sobre cualquier tinte.
  - Descartado por hardware: "gatillo = aceptar predicción" (el gatillo ES el botón 0 en este mando VR de una mano). Descartado de momento: frases editables por el cuidador.
- ✅ **PWA implementada** — `manifest.webmanifest` (pantalla completa, orientación horizontal forzada al instalar, iconos 192/512 generados por script), `sw.js` (red-primero con caché de respaldo → offline total), Wake Lock para mantener la pantalla encendida mientras se usa. **Ojo:** instalación PWA, service worker y Wake Lock requieren HTTPS — no funcionan sobre el `http://IP:8000` de desarrollo (el código lo detecta y no molesta). **Pendiente de decidir el despliegue final:** lo natural es GitHub Pages (gratis, HTTPS, y actualizar = `git push`); el repo aún no tiene commits ni remoto.

**Flujo de desarrollo:** se edita en PC y se prueba en la tablet vía servidor local en la misma Wi-Fi: `npx live-server --port=8000 --no-browser` (auto-recarga) o `python -m http.server 8000`, abriendo `http://<IP-del-PC>:8000` en la tablet. Para depurar en la tablet: USB + `chrome://inspect` desde el PC.

**Hardware confirmado:** mando Bluetooth tipo "VR remote" de una mano (joystick de pulgar, botón frontal, gatillo inferior). Ojo: estos mandos tienen varios modos de arranque (juego/ratón/música) que emiten códigos distintos; el modo válido es el que emita flechas de teclado o ejes de gamepad. **Atajos de botones implementados:** botón 0 (y cualquier otro no mapeado) = confirmar; grupo {1,5,7,9} = **borrar letra** sin mover el foco (también Backspace en teclado); es un único botón físico cuya ráfaga varía por SO: 1+7+9 en Windows, 1+5 en Android. La cruceta (12-15) sigue siendo movimiento. Si en Android los índices difieren, comprobar con `diagnostico.html`. Futuros atajos (aceptar predicción nº 1…) seguirán este mismo patrón de grupos.
- **Botones extra:** `BOTONES_HABLAR = {8}` (HABLAR directo; índice confirmado por el usuario en la tablet con el indicador "últ:"; el log `[entrada] mando: botón N` / `[entrada] tecla: …` sigue activo por consola, y además la esquina de estado muestra la última entrada EN PANTALLA — "últ: b1+b7+b9", ráfagas <300 ms encadenadas — porque el usuario no siempre puede usar `chrome://inspect`; retirar ambos cuando esté confirmado). La esquina de estado muestra también la **`VERSION`** (constante al inicio del script — ⚠️ **subirla en cada despliegue**) para comprobar de un vistazo qué versión corre la tablet. `BOTONES_ESPACIO` queda vacío: no hay botón físico libre. **Uno de los botones del mando es el selector de modo del firmware** (juego/ratón/música): cambia de modo sí o sí, la web no puede bloquearlo ni aprovecharlo — el usuario debe evitarlo y volver a pulsarlo si activa el ratón por accidente. Helper `pedirAccionGlobal(accion)` conectado para futuros atajos.
- **Pantalla completa:** el `display: fullscreen` del manifest solo aplica con la PWA **instalada** (menú Chrome → "Añadir a pantalla de inicio"/"Instalar aplicación"). Como respaldo, en pestaña normal el primer gesto llama a `requestFullscreen()` (solo en https; `ponerPantallaCompleta()`).
