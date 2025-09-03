# Evidencias de la unidad 4

## Código

[Enlace a la aplicación a modificar](http://www.generative-gestaltung.de/2/sketches/?01_P/P_2_3_6_01)

Código a modificar:

``` js
var modules;

var tileSize = 30;
var gridResolutionX;
var gridResolutionY;
var tiles = [];

var doDrawGrid = true;
var isDebugMode = false;

function preload() {
  // load SVG modules
  modules = [];

  // METHOD 1: Looping through local files is efficient
  // for (var i = 0; i < 16; i++) {
  //   modules[i] = loadImage('data/' + nf(i, 2) + '.svg');
  // }

  // METHOD 2: Read files one-by-one
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
  // use full window size
  createCanvas(windowWidth, windowHeight);

  cursor(CROSS);
  rectMode(CENTER);
  imageMode(CENTER);
  strokeWeight(0.15);
  textSize(8);
  textAlign(CENTER, CENTER);

  gridResolutionX = round(width / tileSize) + 2;
  gridResolutionY = round(height / tileSize) + 2;

  initTiles();
}

function draw() {
  background(255);

  if (mouseIsPressed) {
    if (mouseButton == LEFT) setTile();
    if (mouseButton == RIGHT) unsetTile();
  }

  if (doDrawGrid) drawGrid();
  drawModules();
}

function initTiles() {
  for (var gridX = 0; gridX < gridResolutionX; gridX++) {
    tiles[gridX] = [];
    for (var gridY = 0; gridY < gridResolutionY; gridY++) {
      tiles[gridX][gridY] = 0;
    }
  }
}

function setTile() {
  // convert mouse position to grid coordinates
  var gridX = floor(mouseX / tileSize) + 1;
  gridX = constrain(gridX, 1, gridResolutionX - 2);
  var gridY = floor(mouseY / tileSize) + 1;
  gridY = constrain(gridY, 1, gridResolutionY - 2);
  tiles[gridX][gridY] = 1;
}

function unsetTile() {
  var gridX = floor(mouseX / tileSize) + 1;
  gridX = constrain(gridX, 1, gridResolutionX - 2);
  var gridY = floor(mouseY / tileSize) + 1;
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
      // use only active tiles
      if (tiles[gridX][gridY] == 1) {
        // check the four neightbours, each can be true or false
        var NORTH = str(tiles[gridX][gridY - 1]);
        var WEST = str(tiles[gridX - 1][gridY]);
        var SOUTH = str(tiles[gridX][gridY + 1]);
        var EAST = str(tiles[gridX + 1][gridY]);

        // create binary result out of it
        var binaryResult = NORTH + WEST + SOUTH + EAST;

        // convert binary string to a decimal value from 0 - 15
        var decimalResult = parseInt(binaryResult, 2);

        var posX = tileSize * gridX - tileSize / 2;
        var posY = tileSize * gridY - tileSize / 2;

        // decimalResult is also the index for the shape array
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

[Enlace a la aplicación modificada](https://editor.p5js.org/ValMARR12/sketches/iPCDeHDDP)

Código modificado:

``` js
var modules;
var tileSize = 30;
var gridResolutionX;
var gridResolutionY;
var tiles = [];
var doDrawGrid = true;
var isDebugMode = false;

// Variables para Microbit
let port;
let connectBtn;
let statusText; // Variable declarada correctamente
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
  // load SVG modules
  modules = [];

  // METHOD 2: Read files one-by-one
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
  // use full window size
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
  
  // Setup WebSerial
  port = createSerial();
  connectBtn = createButton('Connect to micro:bit');
  connectBtn.position(20, height + 20);
  connectBtn.mousePressed(connectBtnClick);
  
  // Create status indicator - CORREGIDO
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
  // Handle button press events
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

  // Handle Microbit connection and data
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

    if (port.availableBytes() > 0) {
      let data = port.readUntil("\n");
      if (data) {
        data = data.trim();
        let values = data.split(",");
        if (values.length == 4) {
          microBitX = int(values[0]);
          microBitY = int(values[1]);
          microBitAState = values[2].toLowerCase() === "true";
          microBitBState = values[3].toLowerCase() === "true";
          updateButtonStates(microBitAState, microBitBState);
        }
      }
    }
  }

  // State management
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
  
  // Show connection status
  fill(0);
  textAlign(LEFT, TOP);
  textSize(12);
  text("Microbit: " + (microBitConnected ? "Connected" : "Disconnected"), 20, 20);
  text("Position: " + microBitX + ", " + microBitY, 20, 40);
  text("Buttons: A=" + microBitAState + " B=" + microBitBState, 20, 60);
  textAlign(CENTER, CENTER);
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
  // Convert accelerometer coordinates to grid coordinates
  var gridX = floor(map(microBitX, -1024, 1024, 0, gridResolutionX - 1));
  gridX = constrain(gridX, 1, gridResolutionX - 2);
  
  var gridY = floor(map(microBitY, -1024, 1024, 0, gridResolutionY - 1));
  gridY = constrain(gridY, 1, gridResolutionY - 2);
  
  tiles[gridX][gridY] = 1;
}

function unsetTileFromMicrobit() {
  // Convert accelerometer coordinates to grid coordinates
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
      // use only active tiles
      if (tiles[gridX][gridY] == 1) {
        // check the four neightbours, each can be true or false
        var NORTH = str(tiles[gridX][gridY - 1]);
        var WEST = str(tiles[gridX - 1][gridY]);
        var SOUTH = str(tiles[gridX][gridY + 1]);
        var EAST = str(tiles[gridX + 1][gridY]);

        // create binary result out of it
        var binaryResult = NORTH + WEST + SOUTH + EAST;

        // convert binary string to a decimal value from 0 - 15
        var decimalResult = parseInt(binaryResult, 2);

        var posX = tileSize * gridX - tileSize / 2;
        var posY = tileSize * gridY - tileSize / 2;

        // decimalResult is also the index for the shape array
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

## Video

[Video demostratativo](https://youtu.be/OxG6gz5GARw)


