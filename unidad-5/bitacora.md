
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

Estos "eventos" no son eventos nativos de p5.js sinode la función del micro:bit updateButtonStates().

### 5. Capturas de pantalla de los algunos dibujos que hayas hecho con el sketch. 

<img width="1916" height="948" alt="Captura de pantalla 2025-09-17 162032" src="https://github.com/user-attachments/assets/c4eccaec-f4b3-4164-a65e-d155d0458cca" />

## Actividad 02

### Captura del resultado en texto

<img width="1003" height="565" alt="Captura de pantalla 2025-09-17 162727" src="https://github.com/user-attachments/assets/5480056f-8f5f-40b4-aa52-e7eb11ccf7c1" />

Al ver los datos binarios como texto en la terminal, el resultado se ve como una secuencia de caracteres incomprensibles y símbolos, como ¿?¿..., þÿ o  . Esto pasa porque la terminal esta interpretando los bytes binarios como caracteres ASCII, lo cual no es su función y genera una salida ilegible.

### Captura del resultado en Hex y su relación con el código

<img width="1006" height="712" alt="Captura de pantalla 2025-09-17 163047" src="https://github.com/user-attachments/assets/efc3f163-4702-4e00-9e63-9b2b7e3fe94a" />

Cuando se cambia a la vista hex la terminal muestra los bytes crudos en un formato hexadecimal por cada mensaje enviado, se ven 6 bytes esto esta relacionado con la línea de codigo ```  data = struct.pack('>2h2B', xValue, yValue, int(aState), int(bState)).```

### Ventajas y desventajas de usar un formato binario vs. ASCII

#### Formato Binario

Ventajas:

Usa menos espacio y es más rápido.

Desventajas:

Los datos no son comprensibles sin decodificarlos, lo que dificulta la depuración. <br> -

#### Formato ASCII

ventajas:

Fácil de leer y depurar por humanos directamente en la terminal serial. <br>

desventajas:

La longitud del mensaje cambia según el valor de los datos, lo que puede complicar la lectura

### Captura del experimento con el gesto 'shake' y análisis de los bytes

<img width="994" height="724" alt="Captura de pantalla 2025-09-17 163649" src="https://github.com/user-attachments/assets/e83d61b1-b234-48fe-97c3-189e4dce4bbb" />

Los primeros 4 bytes representan los valores xValue y yValue del acelerómetro.

Los últimos 2 bytes representan el estado de los botones aState y bState.

### Comparación final de los formatos

En este experimento, se observa la diferencia de tamaño y legibilidad entre ambos formatos.

El mensaje binario se ve como 00xx 00yy 0z 0w (donde x, y, z y w son valores hexadecimales). Es corto, fijo y totalmente ilegible.

El mensaje ASCII se ve como 10,20,True,False\n. Es más largo, de tamaño variable, pero es perfectamente legible.

Ventajas/desventajas:

Binario es ideal para sistemas que priorizan la eficiencia y la velocidad, como redes de sensores o dispositivos IoT. Su desventaja es la complejidad de depuración.

ASCII es excelente para la depuración inicial y para proyectos donde la legibilidad y la simplicidad son más importantes que la eficiencia de la transmisión. Su desventaja es que no es tan escalable para grandes cantidades de datos.

## Actividad 03

### Explica por qué en la unidad anterior teníamos que enviar la información delimitada y además marcada con un salto de línea y ahora no es necesario.

En la unidad anterior, se usaba un protocolo de texto ASCII donde los datos (xValue, yValue, aState, bState) tenían una longitud variable. Para que el sketch pudiera saber cuándo terminaba un paquete y comenzaba el siguiente, se necesitaba un delimitador, que en este caso era el salto de línea (\n). Al recibir el \n, p5.js sabía que tenía un mensaje completo para procesar.

En el nuevo protocolo binario, el tamaño del paquete es fijo y conocido de antemano (6 bytes en el primer ejemplo, 8 bytes con el framing). Por lo tanto, el receptor simplemente tiene que leer un número fijo de bytes para obtener un paquete completo, sin necesidad de un delimitador. Esto hace que la comunicación sea más simple y eficiente, ya que no se envían bytes extra.

### Compara el código de la unidad anterior relacionado con la recepción de los datos seriales que ves ahora. ¿Qué cambios observas?

El código de p5.js ha cambiado significativamente en la forma en que lee los datos seriales:

if (port.availableBytes() >= 6): En lugar de esperar un delimitador (\n), el código verifica si hay al menos 6 bytes disponibles en el puerto serial.

let data = port.readBytes(6): Ahora se leen exactamente 6 bytes a la vez, garantizando que se recibe un paquete completo.

Se usa DataView para interpretar los bytes:

view.getInt16(0): Lee 2 bytes a partir del byte 0 como un entero con signo de 16 bits (xValue).

view.getInt16(2): Lee 2 bytes a partir del byte 2 como un entero con signo de 16 bits (yValue).

view.getUint8(4): Lee 1 byte a partir del byte 4 como un entero sin signo de 8 bits (aState). Se compara si es 1 para obtener el estado booleano.

view.getUint8(5): Lee 1 byte a partir del byte 5 como un entero sin signo de 8 bits (bState). Se compara si es 1 para obtener el estado booleano.

### ¿Qué ves en la consola? ¿Por qué crees que se produce este error?

El problema es de la sincronización Cuando se ejecuta el código y se observa la consola,  se notara que los datos a veces se desalinean. Por ejemplo, en el resultado que muestras, aparecen valores extraños como 92 o 222 al inicio de las líneas, o valores incorrectos como microBitY: 513 o microBitX: 3073.

Este error se produce porque no hay un mecanismo de sincronización. El receptor asume que el primer byte que recibe es el inicio de un nuevo paquete. Sin embargo, si la comunicación se interrumpe o el programa de p5.js se inicia a mitad de una transmisión, el port.readBytes(6) podría leer los 6 bytes a partir del segundo, tercero, o cuarto byte del paquete real, lo que causaría que los datos se interpreten de forma incorrecta.

### ¿Qué cambios tienen los programas y ¿Qué puedes observar en la consola del editor de p5.js?

El framing como solución Con la implementación del framing, el código de micro:bit y p5.js cambia para asegurar una comunicación

Micro:bit:

Ahora envía un paquete de 8 bytes (b'\xAA' + data + bytes([checksum])).

El primer byte es un header (0xAA) que marca el inicio de cada paquete.

El último byte es un checksum, que sirve para verificar la integridad de los datos.

p5.js:

El código no lee los bytes de 6 en 6, sino que los va almacenando en un buffer (serialBuffer).

Utiliza un bucle while para buscar el byte de inicio (0xAA). Si el primer byte del buffer no es 0xAA, lo descarta y avanza.

Una vez que encuentra el header, verifica que haya un paquete completo (8 bytes) disponible.

Calcula el checksum de los datos recibidos y lo compara con el checksum enviado. Si no coinciden, descarta el paquete para evitar procesar datos corruptos.

Al ejecutar el código final se vera en la consola que los datos se imprimen de forma consistente y correcta: microBitX: 500 microBitY: 524 microBitAState: true microBitBState: false. Esto confirma que el framing y el checksum solucionaron los problemas de sincronización e integridad de los datos.

## Actividad 04

### Proceso de construcción de la aplicación con protocolo binario

Adapte el código de p5.js para leer y procesar el nuevo paquete binario.

A diferencia del protocolo ASCII, que usaba ```port.readUntil("\n")``` , el nuevo código utiliza un buffer para acumular los bytes recibidos.

Un bucle while busca el encabezado 0xAA en el buffer. Si no se encuentra, los bytes se descartan para resincronizar la lectura.

Una vez que se encuentra un paquete completo (8 bytes), se desempaqueta usando DataView y los valores se extraen según su tipo y posición.

Prueba: Depuración del desempaquetado (error de sincronización)
Al principio, intenté leer un paquete de 8 bytes directamente sin un mecanismo de sincronización.

Error: Los valores recibidos eran a menudo incorrectos o se mostraban datos basura en la consola, como microBitX: 3073.

Causa: La comunicación serial no garantiza que la lectura comience al inicio de un paquete. Si se perdía un byte al principio, todo el paquete se desplazaba, y los valores del acelerómetro se interpretaban incorrectamente.

Solucion:
Implementé un algoritmo de framing en la función readSerialData(). Este algoritmo busca el byte de inicio (0xAA) en el buffer y descarta cualquier dato antes de él. Una vez que lo encuentra, lee los 8 bytes completos. Esto aseguró que cada lectura comenzara en el lugar correcto. Adicionalmente, se agregó la verificación del checksum para descartar paquetes que pudieran llegar corruptos.

### experimentación en la bitácora

#### Comprobación de la sincronización

Verifico que el framing funcionara correctamente y eliminara los errores de lectura.

Para eso Ejecuté la aplicación con el micro:bit enviando los valores binarios estáticos (xValue = 500, etc.) y observé la consola.

Resultado: Los valores impresos en la consola (microBitX: 500, microBitY: 524, etc.) eran siempre consistentes. No hubo errores de lectura o valores incorrectos.

Conclusion: La estrategia de framing solucionó con éxito el problema de sincronización y validó la integridad de los datos.

#### Reintegración de la lógica de los botones

Aseguro que los estados de los botones del micro:bit se leyeran correctamente y afectaran la cuadrícula de dibujo. Sustitui los valores estáticos del micro:bit por los valores dinámicos del acelerómetro y los botones. Presioné los botones A y B mientras movía el micro:bit.

Resultado: Al presionar el botón A, se activaban los mosaicos en la cuadrícula de p5.js en la posición correspondiente al movimiento del micro:bit. Al presionar el botón B, se desactivaban. La respuesta fue instantánea y precisa.

Conclusion: El protocolo binario y el esquema de framing no solo mejoraron el rendimiento, sino que mantuvieron la funcionalidad original de la aplicación de manera robusta.

<img width="1919" height="940" alt="image" src="https://github.com/user-attachments/assets/1e2d8506-62d6-41ae-8aca-ce8a8203de62" />

```
// Variables para el buffer y los módulos de dibujo
let serialBuffer = [];
let modules;
let tileSize = 30;
let gridResolutionX;
let gridResolutionY;
let tiles = [];
let doDrawGrid = true;
let isDebugMode = false;

// Variables para Microbit
let port;
let connectBtn;
let statusText;
let connectionInitialized = false;
let microBitConnected = false;
let microBitX = 0;
let microBitY = 0;
let microBitAState = false;
let microBitBState = false;
let prevMicroBitAState = false;
let prevMicroBitBState = false;

const STATES = {
  WAIT_MICROBIT_CONNECTION: "WAIT_MICROBIT_CONNECTION",
  RUNNING: "RUNNING",
};
let appState = STATES.WAIT_MICROBIT_CONNECTION;

function preload() {
  modules = [];
  modules[0] = loadImage('data/00.svg');
  modules[1] = loadImage('data/01.svg');
  modules[2] = loadImage('data/02.svg');
  modules[3] = loadImage('data/03.svg');
  modules[4] = loadImage('data/04.svg');
  modules[5] = loadImage('data/05.svg');
  modules[6] = loadImage('data/06.svg');
  modules[7] = loadImage('data/07.svg');
  modules[8] = loadImage('data/08.svg');
  modules[9] = loadImage('data/09.svg');
  modules[10] = loadImage('data/10.svg');
  modules[11] = loadImage('data/11.svg');
  modules[12] = loadImage('data/12.svg');
  modules[13] = loadImage('data/13.svg');
  modules[14] = loadImage('data/14.svg');
  modules[15] = loadImage('data/15.svg');
}

function setup() {
  createCanvas(windowWidth, windowHeight - 100);
  cursor(CROSS);
  rectMode(CENTER);
  imageMode(CENTER);
  strokeWeight(0.15);
  textSize(8);
  textAlign(CENTER, CENTER);
  gridResolutionX = round(width / tileSize) + 2;
  gridResolutionY = round(height / tileSize) + 2;
  initTiles();
  port = createSerial();
  connectBtn = createButton('Connect to micro:bit');
  connectBtn.position(20, height + 20);
  connectBtn.mousePressed(connectBtnClick);
  statusText = createElement('span', 'Disconnected');
  statusText.position(connectBtn.x + connectBtn.width + 20, height + 25);
  statusText.style('padding', '5px 10px');
  statusText.style('background-color', '#FFBABA');
  statusText.style('color', '#D8000C');
  statusText.style('border-radius', '3px');
}

function connectBtnClick() {
  if (!port.opened()) {
    port.open('MicroPython', 115200);
    connectionInitialized = false;
  } else {
    port.close();
    microBitConnected = false;
    statusText.html('Disconnected');
    statusText.style('background-color', '#FFBABA');
    statusText.style('color', '#D8000C');
  }
}

function updateButtonStates(newAState, newBState) {
  if (newAState === true && prevMicroBitAState === false) {
    setTileFromMicrobit();
  }
  if (newBState === true && prevMicroBitBState === false) {
    unsetTileFromMicrobit();
  }
  prevMicroBitAState = newAState;
  prevMicroBitBState = newBState;
}

function draw() {
  background(255);
  if (!port.opened()) {
    connectBtn.html("Connect to micro:bit");
    microBitConnected = false;
  } else {
    microBitConnected = true;
    connectBtn.html("Disconnect");
    if (port.opened() && !connectionInitialized) {
      port.clear();
      connectionInitialized = true;
    }
    readSerialData();
  }

  switch (appState) {
    case STATES.WAIT_MICROBIT_CONNECTION:
      if (microBitConnected === true) {
        print("Microbit ready to draw");
        appState = STATES.RUNNING;
        statusText.html('Connected');
        statusText.style('background-color', '#DFF2BF');
        statusText.style('color', '#4F8A10');
      }
      break;

    case STATES.RUNNING:
      if (microBitConnected === false) {
        print("Waiting microbit connection");
        appState = STATES.WAIT_MICROBIT_CONNECTION;
        statusText.html('Disconnected');
        statusText.style('background-color', '#FFBABA');
        statusText.style('color', '#D8000C');
      }
      break;
  }

  if (doDrawGrid) drawGrid();
  drawModules();
  fill(0);
  textAlign(LEFT, TOP);
  textSize(12);
  text("Microbit: " + (microBitConnected ? "Connected" : "Disconnected"), 20, 20);
  text("Position: " + microBitX + ", " + microBitY, 20, 40);
  text("Buttons: A=" + microBitAState + " B=" + microBitBState, 20, 60);
  textAlign(CENTER, CENTER);
}

function readSerialData() {
  let available = port.availableBytes();
  if (available > 0) {
    let newData = port.readBytes(available);
    serialBuffer = serialBuffer.concat(newData);
  }
  while (serialBuffer.length >= 8) {
    if (serialBuffer[0] !== 0xaa) {
      serialBuffer.shift();
      continue;
    }
    if (serialBuffer.length < 8) break;
    let packet = serialBuffer.slice(0, 8);
    serialBuffer.splice(0, 8);
    let dataBytes = packet.slice(1, 7);
    let receivedChecksum = packet[7];
    let computedChecksum = dataBytes.reduce((acc, val) => acc + val, 0) % 256;
    if (computedChecksum !== receivedChecksum) {
      console.log("Checksum error in packet");
      continue;
    }
    let buffer = new Uint8Array(dataBytes).buffer;
    let view = new DataView(buffer);
    microBitX = view.getInt16(0);
    microBitY = view.getInt16(2);
    microBitAState = view.getUint8(4) === 1;
    microBitBState = view.getUint8(5) === 1;
    updateButtonStates(microBitAState, microBitBState);
    // Log para verificar la decodificación
    console.log(
      `X: ${microBitX}, Y: ${microBitY}, A: ${microBitAState}, B: ${microBitBState}`
    );
  }
}

function initTiles() {
  for (var gridX = 0; gridX < gridResolutionX; gridX++) {
    tiles[gridX] = [];
    for (var gridY = 0; gridY < gridResolutionY; gridY++) {
      tiles[gridX][gridY] = 0;
    }
  }
}

function setTileFromMicrobit() {
  var gridX = floor(map(microBitX, -1024, 1024, 0, gridResolutionX - 1));
  gridX = constrain(gridX, 1, gridResolutionX - 2);
  var gridY = floor(map(microBitY, -1024, 1024, 0, gridResolutionY - 1));
  gridY = constrain(gridY, 1, gridResolutionY - 2);
  tiles[gridX][gridY] = 1;
}

function unsetTileFromMicrobit() {
  var gridX = floor(map(microBitX, -1024, 1024, 0, gridResolutionX - 1));
  gridX = constrain(gridX, 1, gridResolutionX - 2);
  var gridY = floor(map(microBitY, -1024, 1024, 0, gridResolutionY - 1));
  gridY = constrain(gridY, 1, gridResolutionY - 2);
  tiles[gridX][gridY] = 0;
}

function drawGrid() {
  for (var gridX = 0; gridX < gridResolutionX; gridX++) {
    for (var gridY = 0; gridY < gridResolutionY; gridY++) {
      var posX = tileSize * gridX - tileSize / 2;
      var posY = tileSize * gridY - tileSize / 2;
      fill(255);
      if (isDebugMode) {
        if (tiles[gridX][gridY] == 1) fill(220);
      }
      rect(posX, posY, tileSize, tileSize);
    }
  }
}

function drawModules() {
  for (var gridX = 0; gridX < gridResolutionX - 1; gridX++) {
    for (var gridY = 0; gridY < gridResolutionY - 1; gridY++) {
      if (tiles[gridX][gridY] == 1) {
        var NORTH = str(tiles[gridX][gridY - 1]);
        var WEST = str(tiles[gridX - 1][gridY]);
        var SOUTH = str(tiles[gridX][gridY + 1]);
        var EAST = str(tiles[gridX + 1][gridY]);
        var binaryResult = NORTH + WEST + SOUTH + EAST;
        var decimalResult = parseInt(binaryResult, 2);
        var posX = tileSize * gridX - tileSize / 2;
        var posY = tileSize * gridY - tileSize / 2;
        image(modules[decimalResult], posX, posY, tileSize, tileSize);
        if (isDebugMode) {
          fill(150);
          text(decimalResult + '\n' + binaryResult, posX, posY);
        }
      }
    }
  }
}

function keyPressed() {
  if (key == 's' || key == 'S') saveCanvas(gd.timestamp(), 'png');
  if (keyCode == DELETE || keyCode == BACKSPACE) initTiles();
  if (key == 'g' || key == 'G') doDrawGrid = !doDrawGrid;
  if (key == 'd' || key == 'D') isDebugMode = !isDebugMode;
}

```
