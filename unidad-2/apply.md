# Unidad 2


## 🛠 Fase: Apply

### Actividad 04

#### Evento: Pulsar UP (botón A)

Estado de origen: Configuración

Acción: Aumenta el tiempo en 1 segundo, hasta un máximo de 60.

destino: Configuración

 #### Evento: Pulsar DOWN (botón B)

Estado de origen: Configuración

Acción: Disminuye el tiempo en 1 segundo, hasta un mínimo de 10.

destino: Configuración

#### Evento: Shake (ARMED)

Estado de origen: Configuración

Acción: Almacena el tiempo configurado, inicia la cuenta regresiva y cambia al estado Armado.

destino: Armado

#### Evento: Tiempo = 0

Estado de origen: Armado

Acción: Reproduce un sonido de "explosión" y cambia al estado Explosión.

destino: Explosión

#### Evento: Pulsar Touch

Estado de origen: Armado o Explosión

Acción: Detiene cualquier sonido y resetea el sistema al modo de configuración.

destino: Configuración

### Actividad 05

```
from microbit import *
import utime as time

ESTADO_CONFIGURACION = 0
ESTADO_ARMADO = 1
ESTADO_EXPLOSION = 2

TIEMPO_INICIAL = 20
TIEMPO_MIN = 10
TIEMPO_MAX = 60
INTERVALO_ACTUALIZACION = 1000

estado_actual = ESTADO_CONFIGURACION
tiempo_bomba = TIEMPO_INICIAL
tiempo_restante = 0
tiempo_anterior_ms = 0

def cambiar_a_configuracion():
    global estado_actual, tiempo_bomba, tiempo_restante
    estado_actual = ESTADO_CONFIGURACION
    display.scroll(str(tiempo_bomba), delay=100, wait=False)
    pin0.write_analog(0)

cambiar_a_configuracion()

while True:
    if estado_actual == ESTADO_CONFIGURACION:
        if button_a.was_pressed():
            tiempo_bomba = min(tiempo_bomba + 1, TIEMPO_MAX)
            display.show(str(tiempo_bomba))
        
        if button_b.was_pressed():
            tiempo_bomba = max(tiempo_bomba - 1, TIEMPO_MIN)
            display.show(str(tiempo_bomba))
            
        if accelerometer.was_gesture("shake"):
            tiempo_restante = tiempo_bomba
            estado_actual = ESTADO_ARMADO
            tiempo_anterior_ms = time.ticks_ms()
            display.scroll("BOMBA ARMADA!", delay=90, wait=True)
            display.show(str(tiempo_restante))
            pin0.write_analog(500)
            sleep(50)
            pin0.write_analog(0)

    elif estado_actual == ESTADO_ARMADO:
        tiempo_actual_ms = time.ticks_ms()
        if time.ticks_diff(tiempo_actual_ms, tiempo_anterior_ms) >= INTERVALO_ACTUALIZACION:
            tiempo_restante -= 1
            display.show(str(tiempo_restante))
            tiempo_anterior_ms = tiempo_actual_ms
            
            if tiempo_restante <= 0:
                estado_actual = ESTADO_EXPLOSION
        
        if pin_logo.is_touched():
            cambiar_a_configuracion()
    
    elif estado_actual == ESTADO_EXPLOSION:
        for _ in range(5):
            pin0.write_analog(1023)
            sleep(100)
            pin0.write_analog(0)
            sleep(100)
        
        if pin_logo.is_touched():
            cambiar_a_configuracion()

    sleep(10)
```

#### Vectores de prueba

**1. Pruebas del estado Configuración**

1	Iniciar el micro:bit	

*La pantalla muestra```20```*

2	Presionar el botón A (UP)	     

*La pantalla muestra```21```*

3	Presionar el botón A repetidamente hasta llegar a 60	   

*La pantalla muestra```60``` Presionar de nuevo no cambia el valor.*

4	Presionar el botón B (DOWN)	             

*La pantalla muestra```59```*

5	Presionar el botón B repetidamente hasta llegar a 10	    

*La pantalla muestra```10``` Presionar de nuevo no cambia el valor*

**2. Pruebas del estado Armado**

1	En estado de Configuración, presionar botón B dos veces

*a pantalla muestra```18```*

2	Hacer el gesto de Shake (ARMED)	     

*La pantalla se desplaza mostrando "BOMBA ARMADA!" y luego comienza la cuenta regresiva desde```18``` Se escucha un pitido corto*

3	Esperar 5 segundos	   

*La pantalla muestra```13```*

4	Presionar el botón de touch	La cuenta regresiva se detiene  

*La pantalla muestra```18``` (o el último valor configurado) y el sistema vuelve al modo de configuración*

**3. Pruebas del estado Explosión**

1	En estado de Configuración, presionar botón B hasta que el tiempo sea 10.	

*La pantalla muestra ```10```*

2	Hacer el gesto de Shake (ARMED)

*La pantalla se desplaza mostrando "BOMBA ARMADA!" y luego comienza la cuenta regresiva desde ```10```*

3	Esperar 10 segundos	

*La pantalla muestra ```0```, se reproduce un sonido de explosión (pitidos rápidos)*

4	Presionar el botón de touch	

*El sonido de explosión se detiene y la pantalla muestra ```10```, volviendo al modo de configuración*
