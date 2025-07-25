# Unidad 1

## 🛠 Fase: Apply

### Actividad 5

lo que se realizo para esta actividad fue hacer un codigo que cuando en el micro bit se estuviera pesionando el boton A el rectangulo en la pantalla pasara de verde y rojo cuandoel boton se dejera de precionar, teniendo de imput el botton del microbit y la informacion enviada al computador y de output el cambio de color

### Actividad 6

```
from microbit import *

uart.init(baudrate=115200)
display.show(Image.DUCK)

while True:

    if button_a.was_pressed():
        uart.write('A')
    if button_b.was_pressed():
        uart.write('B')    
    else:
        uart.write('N')

    sleep(100)
```
```
  let port;
  let x =200;
  let connectBtn;
  let connectionInitialized = false;

  function setup() {
    createCanvas(400, 400);
    background(220);
    port = createSerial();
    connectBtn = createButton("Connect to micro:bit");
    connectBtn.position(80, 300);
    connectBtn.mousePressed(connectBtnClick);
  }

  function draw() {
    background(220);

    if (port.opened() && !connectionInitialized) {
      port.clear();
      connectionInitialized = true;
    }

    if (port.availableBytes() > 0) {
      let dataRx = port.read(1);
      if (dataRx == "A") {
       x-=100;
      } 
      if (dataRx == "B") {
       x+=100;
      }
      else if (dataRx == "N") {
      }
    }

    circle(x,150,100);

    if (!port.opened()) {
      connectBtn.html("Connect to micro:bit");
    } else {
      connectBtn.html("Disconnect");
    }
  }

  function connectBtnClick() {
    if (!port.opened()) {
      port.open("MicroPython", 115200);
      connectionInitialized = false;
    } else {
      port.close();
    }
  }
```
[https://editor.p5js.org/ValMARR12/sketches/jykX-xJTp]
