# Comunicador CAA

Aplicación de Comunicación Aumentativa y Alternativa (CAA) para tablet Android, diseñada para un adulto con hemiplejia y anartria tras un ictus. Escritura rápida de texto + síntesis de voz (TTS), controlada íntegramente con un mando Bluetooth de una mano (D-Pad, sin ratón).

Ver [CLAUDE.md](CLAUDE.md) para la especificación completa, los principios de accesibilidad y el estado actual del proyecto.

## Stack

- **PWA — HTML5/JS autocontenido**, sin dependencias ni build. Un solo `index.html` para que copiarlo a la tablet sea trivial.
- **TTS:** API nativa del navegador (`speechSynthesis`), voces en español del dispositivo.
- **Entrada:** eventos de teclado + API Gamepad (el mando puede aparecer por cualquiera de las dos vías según su modo).

## Cómo ejecutar

**Opción rápida:** doble clic en `index.html` (funciona con `file://`).

**Con VSCode:** instala la extensión recomendada *Live Server* y pulsa "Go Live". Útil para probar en la tablet por red local: abre `http://<ip-del-pc>:5500` en Chrome de la tablet (mismo WiFi).

## Estado actual

La app está en **modo diagnóstico**: registra en pantalla todo lo que emite el mando (teclado y gamepad) para identificar los códigos reales de cada botón antes de implementar la navegación espacial (Fase 3).

### Cómo hacer el diagnóstico en la tablet

1. Pasa `index.html` a la tablet y ábrelo con Chrome (o usa Live Server por WiFi).
2. Empareja el mando por Bluetooth.
3. Mueve el joystick en las 4 direcciones y pulsa cada botón, de uno en uno.
4. Anota qué aparece para cada control (azul = teclado, naranja = gamepad).
5. Si no aparece nada o sale un puntero de ratón, el mando está en el modo equivocado: prueba sus otros modos de arranque (combinaciones tipo mantener un botón al encender).

## Estructura

```
caa-comunicador/
├── index.html    # La app completa (autocontenida)
├── CLAUDE.md     # Especificación del proyecto + estado por fases
└── .vscode/      # Config del editor y extensiones recomendadas
```
