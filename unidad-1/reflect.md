# Unidad 1

## 🤔 Fase: Reflect

### Actividad 07

#### Parte 1: recuperación de conocimiento (Retrieval Practice)

1. la capacidad de que puede recivir y procesar informacion que probiene de un estimulo no virtual (ya sea precionando un boton o registrando movimiento), que es capas de procesar esta informacion para usarla en el funcionamiento del programa y  que puede mostrar ya sea por un medio visual el hehco que reconoce estos estimulos y siguiendo las normas puestas por el codificador puede exitosamente cumplir con la tarea impuesta
   
2.  el imput fue haber precionado los botones en el microbit y el cable que envia la informacion a la computadora, el proceso es como el programa procesa que boton esta siendo precionado en el dspositivo para proceder a ver que funcion tiene que hacer y el output seria ver como en la pantalla dependiendo del boton precionado el circulo se mueve o hacia la izquierda o a la derecha
   
3.  la funcion ```uart.write('A')``` en el microbit es para que el microbit registre y pase la informacion a la computadora princial de las veces que el boton a fue o esta siendo precionado y para que este estimulo sea convertido a una funcion en nuestro programa en p5.js habria que poner esta funcion:
   
.  ``` if (port.availableBytes() > 0) {
       let dataRx = port.read(1);
       if (dataRx == "A") ```

4. la dferencia entre arte/diseño tradicional y arte/diseño generativo es quien es el que esta haciendo el procedo de arte/diseño, en tradiconal es directamente la persona quien esta creando la piesa mientras que en el generativo quie esta creando/generando la piesa es directamente la computadora siguiendo las normas impuestas por el codicador
   
5. primero que nada el microbit lo que habria que poner es algo mu similar como cuando se hace lo del boton pero en ves de poner ```button_b.is_pressed():``` se pondria la siguiente linea ```accelerometer.was_gesture('shake'): uart.write('C') sleep(500)``` y en p5.js se pondria ``` if (port.availableBytes() > 0) {let dataRx = port.read(1);if (dataRx == "C") {circleColor = color(random(100, 256), random(100, 256), random(100, 256)); }```
  
####  Parte 2: reflexión sobre tu proceso (Metacognición)

1. 
