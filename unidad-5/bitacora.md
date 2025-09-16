
# Evidencias de la unidad 5

## Actividad 01


### 1. ¿Cómo se comunican el micro:bit y el sketch de p5.js? ¿Qué datos envía el micro:bit? 

El micro:bit y el sketch de p5.js se comunican a través de una conexión serial usando el protocolo Web Serial API. El micro:bit actúa como un dispositivo emisor, enviando datos de forma continua a la computadora mediante el cable USB. 

### 2. ¿Cuál es la estructura del protocolo ASCII usado? 

Los valores (xValue, yValue, aState, bState) son representados como cadenas de texto ASCII.

Cada valor está separado por una coma (,).

Al final de cada línea de datos se añade un carácter de salto de línea (\n), lo que permite al sketch de p5.js saber cuándo termina un mensaje y cuándo debe comenzar a procesar el siguiente.

### 3. Muestra y explica la parte del código de p5.js donde lee los datos del micro:bit y los transforma en coordenadas de la pantalla.

```
if (port.availableBytes() > 0) {
      let data = port.readUntil("\n");
      if (data) {
        data = data.trim();
        let values = data.split(",");
        if (values.length == 4) {
          microBitX = int(values[0]) + windowWidth / 2;
          microBitY = int(values[1]) + windowHeight / 2;
          microBitAState = values[2].toLowerCase() === "true";
          microBitBState = values[3].toLowerCase() === "true";
          updateButtonStates(microBitAState, microBitBState);
        } else {
          print("No se están recibiendo 4 datos del micro:bit");
        }
      }
    }
```

### 4. ¿Cómo se generan los eventos A pressed y B released que se generan en p5.js a partir de los datos que envía el micro:bit?

