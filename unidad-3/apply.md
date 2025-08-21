# Unidad 3


## 🛠 Fase: Apply

### Actividad 06 y 07

```
let PASSWORD = ['A', 'B', 'A'];
let key = ['', '', ''];
let keyindex = 0;
let count = 20;
let state = 'CONFIG';
let lastUpdate = 0;
let port;
let connectBtn;
let connectionInitialized = false;

function setup() {
  createCanvas(400, 400);
  textAlign(CENTER, CENTER);
  textSize(32);
  
  port = createSerial();
  connectBtn = createButton("Conectar al micro:bit");
  connectBtn.position(80, 320);
  connectBtn.mousePressed(connectBtnClick);
  
  let btnA = createButton("Botón A");
  btnA.position(80, 350);
  btnA.mousePressed(() => handleButton('A'));
  btnA.style('background-color', '#4CAF50');
  btnA.style('color', 'white');
  
  let btnB = createButton("Botón B");
  btnB.position(160, 350);
  btnB.mousePressed(() => handleButton('B'));
  btnB.style('background-color', '#f44336');
  btnB.style('color', 'white');
  
  let shakeBtn = createButton("Agitar");
  shakeBtn.position(240, 350);
  shakeBtn.mousePressed(() => handleShake());
  shakeBtn.style('background-color', '#2196F3');
  shakeBtn.style('color', 'white');
  
  let resetBtn = createButton("Reiniciar");
  resetBtn.position(320, 350);
  resetBtn.mousePressed(() => handleReset());
  resetBtn.style('background-color', '#FF9800');
  resetBtn.style('color', 'white');
}

function draw() {
  background(220);
  
  if (port.opened() && !connectionInitialized) {
    port.clear();
    connectionInitialized = true;
  }
  
  if (port.available() > 0) {
    let data = port.readUntil('\n');
    if (data.length > 0) {
      processMicrobitData(data);
    }
  }
  
  if (state === 'CONFIG') {
    drawConfigState();
  } else if (state === 'ARMED') {
    drawArmedState();
  } else if (state === 'EXPLODED') {
    drawExplodedState();
  }
  
  drawPasswordInput();
  
  if (!port.opened()) {
    connectBtn.html("Conectar al micro:bit");
    fill(255, 0, 0);
    text("Micro:bit desconectado", width/2, 380);
  } else {
    connectBtn.html("Desconectar");
    fill(0, 255, 0);
    text("Micro:bit conectado", width/2, 380);
  }
}

function drawConfigState() {
  fill(0);
  text("MODO CONFIG", width/2, height/3);
  text(`TIEMPO: ${count}s`, width/2, height/2);
  text("Agita para activar", width/2, height/1.8);
}

function drawArmedState() {
  if (millis() - lastUpdate > 1000) {
    lastUpdate = millis();
    count--;
    
    if (count <= 0) {
      state = 'EXPLODED';
    }
  }
  
  if (count > 10) {
    fill(0, 200, 0);
  } else if (count > 5) {
    fill(255, 165, 0);
  } else {
    fill(255, 0, 0);
  }
  
  text(count, width/2, height/2);
  
  fill(0);
  text("BOMBA ACTIVADA", width/2, height/3);
  text("Ingresa contraseña", width/2, height/1.8);
}

function drawExplodedState() {
  background(255, 0, 0);
  fill(255);
  text("¡EXPLOTÓ!", width/2, height/2);
  text("Toca para reiniciar", width/2, height/1.8);
}

function drawPasswordInput() {
  fill(100);
  rectMode(CENTER);
  rect(width/2, height - 80, 200, 30, 5);
  
  for (let i = 0; i < PASSWORD.length; i++) {
    if (i < keyindex) {
      fill(0, 255, 0);
    } else {
      fill(200);
    }
    ellipse(width/2 - 50 + i * 30, height - 80, 20, 20);
  }
}

function handleButton(btn) {
  if (state === 'CONFIG') {
    if (btn === 'A') {
      count = min(count + 1, 60);
    } else if (btn === 'B') {
      count = max(10, count - 1);
    }
  } else if (state === 'ARMED') {
    key[keyindex] = btn;
    keyindex = (keyindex + 1) % PASSWORD.length;
    
    if (keyindex === 0) {
      let correct = true;
      for (let i = 0; i < PASSWORD.length; i++) {
        if (key[i] !== PASSWORD[i]) {
          correct = false;
          break;
        }
      }
      
      if (correct) {
        state = 'CONFIG';
        count = 20;
        resetKey();
      }
    }
  }
}

function handleShake() {
  if (state === 'CONFIG') {
    state = 'ARMED';
    lastUpdate = millis();
    resetKey();
  }
}

function handleReset() {
  if (state === 'EXPLODED') {
    state = 'CONFIG';
    count = 20;
    resetKey();
  }
}

function resetKey() {
  key = ['', '', ''];
  keyindex = 0;
}

function mousePressed() {
  if (state === 'EXPLODED') {
    handleReset();
  }
}

function connectBtnClick() {
  if (!port.opened()) {
    port.open('MicroPython', 115200);
    connectionInitialized = false;
  } else {
    port.close();
  }
}

function processMicrobitData(data) {
  const parts = data.trim().split(':');
  if (parts.length === 2) {
    const event = parts[0];
    const value = parts[1];
    
    if (event === 'BUTTON') {
      if (value === 'A') {
        handleButton('A');
      } else if (value === 'B') {
        handleButton('B');
      }
    } else if (event === 'GESTURE' && value === 'SHAKE') {
      handleShake();
    } else if (event === 'TOUCH' && value === 'LOGO') {
      handleReset();
    }
  }
}
```
```
from microbit import *
import utime

display.clear()
uart.init(baudrate=115200, bits=8, parity=None, stop=1)

class BombTask:
    def __init__(self):
        self.PASSWORD = ['A','B','A']
        self.key = ['']*len(self.PASSWORD)
        self.keyindex = 0
        self.count = 20
        self.startTime = utime.ticks_ms()
        self.state = 'CONFIG'
        self.last_button_a = False
        self.last_button_b = False
        self.last_shake = False
        self.last_touch = False
        display.clear()
        display.show(self.count, wait=False)

    def update(self):
        self.check_events()
        
        if self.state == 'CONFIG':
            if button_a.was_pressed():
                self.count = min(self.count+1, 60)
                display.show(self.count, wait=False)

            if button_b.was_pressed():
                self.count = max(10, self.count-1)
                display.show(self.count, wait=False)

            if accelerometer.was_gesture('shake'):
                self.startTime = utime.ticks_ms()
                self.state = 'ARMED'
                uart.write("GESTURE:SHAKE\n")

        elif self.state == 'ARMED':
            if utime.ticks_diff(utime.ticks_ms(), self.startTime) > 1000:
                self.startTime = utime.ticks_ms()
                self.count = self.count - 1
                display.show(self.count, wait=False)
                if self.count == 0:
                    display.show(Image.SKULL)
                    self.state = 'EXPLODED'

            if button_a.was_pressed():
                self.key[self.keyindex] = 'A'
                self.keyindex = self.keyindex + 1
                uart.write("BUTTON:A\n")

            if button_b.was_pressed():
                self.key[self.keyindex] = 'B'
                self.keyindex = self.keyindex + 1
                uart.write("BUTTON:B\n")

            if self.keyindex == len(self.key):
                passIsOK = True
                for i in range(len(self.key)):
                    if self.key[i] != self.PASSWORD[i]:
                        passIsOK = False
                        break
                if passIsOK:
                    self.count = 20
                    display.show(self.count, wait=False)
                    self.keyindex = 0
                    self.state = 'CONFIG'
                else:
                    self.keyindex = 0

        elif self.state == 'EXPLODED':
            if pin_logo.is_touched():
                self.count = 20
                display.show(self.count, wait=False)
                self.startTime = utime.ticks_ms()
                self.state = 'CONFIG'
                uart.write("TOUCH:LOGO\n")
    
    def check_events(self):
        current_button_a = button_a.is_pressed()
        current_button_b = button_b.is_pressed()
        current_shake = accelerometer.current_gesture() == 'shake'
        current_touch = pin_logo.is_touched()
        
        if current_button_a and not self.last_button_a:
            uart.write("BUTTON:A\n")
        if current_button_b and not self.last_button_b:
            uart.write("BUTTON:B\n")
        if current_shake and not self.last_shake:
            uart.write("GESTURE:SHAKE\n")
        if current_touch and not self.last_touch:
            uart.write("TOUCH:LOGO\n")
            
        self.last_button_a = current_button_a
        self.last_button_b = current_button_b
        self.last_shake = current_shake
        self.last_touch = current_touch

bombTask = BombTask()

while True:
    bombTask.update()
    sleep(100)
```
[p5.js](https://editor.p5js.org/ValMARR12/sketches/D6BxOoEn_)
