
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

## Actividad 02

### Captura del resultado en texto

Al ver los datos binarios como texto en la terminal, el resultado se ve como una secuencia de caracteres incomprensibles y símbolos, como ¿?¿..., þÿ o  . Esto pasa porque la terminal esta interpretando los bytes binarios como caracteres ASCII, lo cual no es su función y genera una salida ilegible.

### Captura del resultado en Hex y su relación con el código

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

El framing como solución
Con la implementación del framing, el código de micro:bit y p5.js cambia para asegurar una comunicación robusta:

Micro:bit:

Ahora envía un paquete de 8 bytes (b'\xAA' + data + bytes([checksum])).

El primer byte es un header (0xAA) que marca el inicio de cada paquete.

El último byte es un checksum, que sirve para verificar la integridad de los datos.

p5.js:

El código no lee los bytes de 6 en 6, sino que los va almacenando en un buffer (serialBuffer).

Utiliza un bucle while para buscar el byte de inicio (0xAA). Si el primer byte del buffer no es 0xAA, lo descarta y avanza.

Una vez que encuentra el header, verifica que haya un paquete completo (8 bytes) disponible.

Calcula el checksum de los datos recibidos y lo compara con el checksum enviado. Si no coinciden, descarta el paquete para evitar procesar datos corruptos.

Al ejecutar el código final, verás en la consola de p5.js que los datos se imprimen de forma consistente y correcta: microBitX: 500 microBitY: 524 microBitAState: true microBitBState: false. Esto confirma que el framing y el checksum solucionaron los problemas de sincronización e integridad de los datos.

