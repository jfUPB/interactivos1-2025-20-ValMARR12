# Unidad 2

## 🔎 Fase: Set + Seek

### Actividad 01

#### 1. Describe detalladamente cómo funciona este ejemplo.

   es un bucle de una secuencia de leds que se encienden y se apagan dependiendo del tiempo determinado, utilizando estados y herramintas de tiempo para seguir la secuencia impuesta

#### 2. ¿Cuáles son los estados en el programa?

   este programa tiene dos estados, un estado de espera llamado ```innit``` que solo se ejecuta cuando el programa se inicializa para determinar la hora actual y otro estado llamado ```WaitTimeout``` que cumple la funcion de contabilizar el tiempo que pasa para encender o apagr un led

#### 3. ¿Cuáles son los eventos/inputs en el programa?

   el imput usado en este programa es el transcurso del tiempo que calcula la diferencia de tiempo entre la hora actual y la última vez que se reinició el temporizador

#### 4. ¿Cuáles son las acciones en el programa?

   Establecer la hora de inicio, Configurar el estado de brillo del píxel, Cambiar el estado de brillo, Cambiar el estado de la máquina de estados

```
from microbit import *
import utime

class Pixel:
    def __init__(self,pixelX,pixelY,initState,interval):
        self.state = "Init"
        self.startTime = 0
        self.interval = interval
        self.pixelX = pixelX
        self.pixelY = pixelY
        self.pixelState = initState

    def update(self):

        if self.state == "Init":
            self.startTime = utime.ticks_ms()
            self.state = "WaitTimeout"
            display.set_pixel(self.pixelX,self.pixelY,self.pixelState)

        elif self.state == "WaitTimeout":
            if utime.ticks_diff(utime.ticks_ms(),self.startTime) > self.interval:
                self.startTime = utime.ticks_ms()
                if self.pixelState == 9:
                    self.pixelState = 0
                else:
                    self.pixelState = 9
                display.set_pixel(self.pixelX,self.pixelY,self.pixelState)

pixel1 = Pixel(0,0,0,1000)
pixel2 = Pixel(4,4,0,500)

while True:
    pixel1.update()
    pixel2.update()
```
