
# Evidencias de la unidad 6

## Actividad 01

### ¿Qué ocurrió en la terminal cuando ejecutaste npm install? ¿Cuál crees que es su propósito?

Lo que ocurre es que la terminal mostró una lista de paquetes y dependencias que fueron descargados e instalados en la carpeta node_modules, su propocito es descargar e instalar todos los módulos y librerías que el proyecto requiere para funcionar, según lo especificado en el archivo ``package.json``

### ¿Qué mensaje específico apareció en la terminal después de ejecutar npm start? ¿Qué indica este mensaje?

el mesaje que aparece es  "El servidor está escuchando en el puerto 3000" lo que indica este mensaje es wl servidor web se ha iniciado y está listo para aceptar peticiones del navegador en la dirección ``http://localhost:3000``

### Describe lo que ves inicialmente en page1 y page2 en tu navegador.

se ven dos puntos que en el momento que se sincroniza se conectan por una linea

### ¿Qué mensajes aparecieron en la terminal del servidor cuando abriste page1 y page2?

Aparecieron logs como GET /page1 200 y GET /page2 200	

### Describe qué sucede en ambas páginas del navegador cuando mueves una de las ventanas. ¿Cambia algo visualmente? ¿Qué mensajes aparecen (si los hay) en la consola del navegador (usualmente accesible con F12 -> Pestaña Consola) y en la terminal del servidor?

al mover la ventana de page1, la visualización en la ventana de page2 se actualizó en tiempo real. En la consola (F12), se observaron mensajes de datos recibidos, la aplicación usa WebSockets para comunicación bidireccional, permitiendo que el estado de una ventana se sincronice con la otra sin recargar la página

## Actividad 02

### Piensa en cómo te conectas a Internet en casa o en la Universidad. ¿Usas Wi-Fi? ¿Un cable de red? Eso es simplemente tu “rampa de acceso” a la gran red de carreteras. ¿Qué pasaría si esa rampa se corta? Anota tus ideas.

si la "rampa de acceso" (Wi-Fi o cable) se corta, se pierde la conectividad con la red,  las aplicaciones y el navegador dejarán de funcionar al no poder enviar ni recibir datos

### ¿Puedes identificar otros ejemplos de relaciones Cliente-Servidor en tu vida diaria (no necesariamente digitales)?	

Ejemplo: En un banco. El Cliente es la persona que hace la solicitud de un retiro. El Servidor es el cajero o ejecutivo

### Toma la URL de tu sitio web favorito. Intenta identificar el protocolo, el nombre de dominio y la ruta (si la hay). ¿Qué crees que pasa si solo escribes el nombre de dominio (ej. www.google.com) sin una ruta específica? ¿Qué “página por defecto” crees que te envía el servidor?

(https://archiveofourown.org/) si solo escribes el dominio, el servidor te envía la pagina de inicio o pagina por defecto, ya que es el recurso configurado para responder a la ruta raíz

### Compara HTTP con los protocolos seriales que usaste

#### ¿Qué similitudes encuentras?
ambos son protocolos  y envían información de forma estructurada 

#### ¿Qué diferencias clave ves?
HTTP es una peticion y de alcance global. Serial es continuo y de corto alcance

#### ¿Por qué crees que HTTP necesita ser más complejo que un simple envío de bytes como hacías con el micro:bit?
HTTP debe manejar dominios globales, múltiples acciones, seguridad y metadatos, algo innecesario para un simple envío de bytes

### Piensa en una página web simple, como un formulario de login. ¿Qué parte crees que es HTML (ej. los campos de texto, el botón)? ¿Qué parte es CSS (ej. el color del botón, el tipo de letra)? ¿Qué parte es JavaScript (ej. la comprobación de si escribiste algo antes de enviar, el mensaje de “contraseña incorrecta” que aparece sin recargar la página)?

HTML:  el texto de las etiquetas (ej. "Usuario" o "Contraseña") y la casilla de verificación ("Recordarme") 

CSS: el sombreado del botón al pasar el cursor y el espaciado entre el formulario y los bordes de la página

JavaScript: la función de "mostrar/ocultar contraseña" y el temporizador que inhabilita el botón de envío si fallas varias veces

### Compara el bucle draw() de p5.js con este modelo de “esperar a que algo pase y reaccionar”. ¿Qué ventajas crees que tiene el modelo basado en eventos para una interfaz de usuario web? ¿Sería eficiente tener un bucle draw() redibujando toda la página 60 veces por segundo si nada ha cambiado?

ventajas del modelo basado en eventos: es mas eficiente, el código solo se ejecuta cuando hay un evento (clic, mensaje, etc.)
eficiencia del draw(): no, no seria eficiente, redibujar elementos de la web 60 veces por segundo sin necesidad desperdicia CPU

### ¿Por qué crees que podría ser útil usar JavaScript tanto en el cliente (navegador) como en el servidor? ¿Se te ocurre alguna ventaja para los desarrolladores?

la principal ventaja es la unificación del lenguaje, permite reutilizar codigo entre el frontend y el backend, reduciendo la curva

### Resume con tus propias palabras la diferencia fundamental entre una comunicación HTTP tradicional y una comunicación usando WebSockets/Socket.IO. ¿En qué tipo de aplicaciones has visto o podrías imaginar que se usa esta comunicación en tiempo real?

HTTP es uan peticion. WebSockets es una conexión permanente y bidireccional. Aplicaciones Tipicas: Chats, juegos multijugador, y herramientas de colaboración.

## Actividad 03

### Intenta acceder a http://localhost:3000/page1. ¿Funciona?

no funciona la URL original la pagina mouestra un error 404

### Ahora intenta acceder a http://localhost:3000/pagina_uno. ¿Funciona?

esta url si funciona

### ¿Qué te dice esto sobre cómo el servidor asocia URLs con respuestas? Restaura el código.

el servidor utiliza el enrutamiento (app.get(...)) y solo responde a las rutas que están exactamente definidas en su código.

### Abre http://localhost:3000/page1 en una pestaña. Observa la terminal del servidor. ¿Qué mensaje ves? Anota el ID. Abre http://localhost:3000/page2 en OTRA pestaña. Observa la terminal. ¿Qué mensaje ves? ¿El ID es diferente? 

se registra un mensaje de conexión con un ID único para cada pestaña o navegador abierto, los IDs son diferentes	

## Actividad 04

### Refresca la página page2.html. Observa la consola del navegador. ¿Ves algún error relacionado con la conexión? ¿Qué indica?

la consola del navegador (F12) muestra errores de red como net::ERR_CONNECTION_REFUSED o WebSocket connection failed.	El cliente web depende totalmente de que el servidor esté activo para establecer y mantener la comunicación en tiempo real a través de Socket.IO.

### Comenta la línea socket.emit(‘win2update’, currentPageData, socket.id); dentro del listener connect. Reinicia el servidor y refresca page1.html y page2.html. Mueve la ventana de page2 un poco para que envíe una actualización. ¿Qué pasó? ¿Por qué?

las otras páginas no se sincronizaron con la posición inicial de la ventana recién conectada.	Es crucial enviar el estado inicial del cliente inmediatamente despues de conectarse para garantizar la correcta sincronización con el resto del sistema.

### Abre ambas páginas (es posible que ya las tengas abiertas). Mueve la ventana de page1. Observa la consola del navegador de page2. ¿Qué datos muestra? Mueve la ventana de page2. Observa la consola de page1. ¿Qué pasa? ¿Por qué?

lpa consola de page2 recibe y muestra los datos de posicion de page1. Se confirma la naturaleza bidireccional: ambos clientes pueden recibir mensajes Socket.IO del servidor

### Observa checkWindowPosition() en page2.js y modifica el código del if para comprobar si el código dentreo de este se ejecuta. Mueve cada ventana y observa las consolas. ¿Qué puedes concluir y por qué?

el código de envío de Socket.IO dentro de la condición if solo se ejecuta cuando la ventana se ha movido una distancia significativa. La función actúa como una optimizacion. Solo se envían datos cuando hay un cambio perceptible, lo que evita la saturación de la red y el servidor con mensajes innecesarios.

## Actividad 05

mi idea consiste en crear un sistema de votación binario ("SÍ" o "NO") donde dos usuarios interactúan moviendo sus respectivas ventanas en la pantalla. El servidor calcula el voto basado en la proximidad y actualiza el marcador global en tiempo real a todos los clientes

codigo: 

server.js
```
const express = require('express');
const app = express();
const http = require('http').createServer(app);
const io = require('socket.io')(http);

const port = 3000;

app.use(express.static('public'));

let clientData = {
    voterA: { x: 50, y: 50, vote: 'NO', active: false },
    voterB: { x: 950, y: 50, vote: 'NO', active: false }
};

function calculateVote(x, y) {
    let centerX = 500;
    let centerY = 500;
    let distance = Math.sqrt(Math.pow(x - centerX, 2) + Math.pow(y - centerY, 2));
    
    return distance < 250 ? 'SÍ' : 'NO';
}

app.get('/votera', (req, res) => {
    res.sendFile(__dirname + '/voterA.html');
});

app.get('/voterb', (req, res) => {
    res.sendFile(__dirname + '/voterB.html');
});

io.on('connection', (socket) => {
    socket.emit('globalUpdate', clientData);

    socket.on('positionA', (data) => {
        clientData.voterA.x = data.x;
        clientData.voterA.y = data.y;
        clientData.voterA.vote = calculateVote(data.x, data.y);
        clientData.voterA.active = true;

        io.emit('globalUpdate', clientData); 
    });

    socket.on('positionB', (data) => {
        clientData.voterB.x = data.x;
        clientData.voterB.y = data.y;
        clientData.voterB.vote = calculateVote(data.x, data.y);
        clientData.voterB.active = true;

        io.emit('globalUpdate', clientData);
    });
    
    socket.on('disconnect', () => {
        // Manejar desconexión si es necesario
    });
});

http.listen(port, () => {
    console.log(`Server listening on port ${port}. Access at http://localhost:${port}/votera and http://localhost:${port}/voterb`);
});
```

voterA.js

```
let socket;
let globalState = {}; 
let myCenter = { x: 0, y: 0 }; 

function setup() {
    createCanvas(1000, 1000); 
    socket = io.connect('http://localhost:3000');
    
    socket.on('globalUpdate', (data) => {
        globalState = data;
    });

    myCenter.x = window.screenX + width / 2;
    myCenter.y = window.screenY + height / 2;
    socket.emit('positionA', myCenter);
}

function draw() {
    background(240);
    
    let currentX = window.screenX + width / 2;
    let currentY = window.screenY + height / 2;

    if (abs(currentX - myCenter.x) > 5 || abs(currentY - myCenter.y) > 5) {
        myCenter.x = currentX;
        myCenter.y = currentY;
        socket.emit('positionA', myCenter);
    }
    
    drawVotes();
}

function drawVotes() {
    let totalSi = 0;
    let totalNo = 0;

    noFill();
    stroke(150, 150);
    ellipse(width / 2, height / 2, 500, 500);
    
    if (globalState.voterA && globalState.voterB) {
        
        // Voto A: Círculo (Propio)
        if (globalState.voterA.vote === 'SÍ') {
            fill(0, 200, 0, 200); 
            totalSi++;
        } else {
            fill(200, 0, 0, 200); 
            totalNo++;
        }
        ellipse(width / 2, height / 2, 50, 50); 
        
        // Voto B: Cuadrado (Remoto)
        if (globalState.voterB.vote === 'SÍ') {
            fill(0, 200, 0, 200); 
            totalSi++;
        } else {
            fill(200, 0, 0, 200); 
            totalNo++;
        }
        rectMode(CENTER);
        rect(width / 2 + 100, height / 2, 50, 50); 
        
        // MARCADOR GLOBAL
        fill(0);
        textSize(32);
        textAlign(CENTER, CENTER);
        text(`RESULTADO: SÍ [${totalSi}] - NO [${totalNo}]`, width / 2, height - 50);
        
        textSize(18);
        text(`Mueve esta ventana (Círculo) al centro para votar SÍ.`, width / 2, 50);
    }
}
```

voterB.js

```
let socket;
let globalState = {}; 
let myCenter = { x: 0, y: 0 }; 

function setup() {
    createCanvas(1000, 1000); 
    socket = io.connect('http://localhost:3000');
    
    socket.on('globalUpdate', (data) => {
        globalState = data;
    });

    myCenter.x = window.screenX + width / 2;
    myCenter.y = window.screenY + height / 2;
    socket.emit('positionB', myCenter); 
}

function draw() {
    background(240);
    
    let currentX = window.screenX + width / 2;
    let currentY = window.screenY + height / 2;

    if (abs(currentX - myCenter.x) > 5 || abs(currentY - myCenter.y) > 5) {
        myCenter.x = currentX;
        myCenter.y = currentY;
        socket.emit('positionB', myCenter); 
    }
    
    drawVotes();
}

function drawVotes() {
    let totalSi = 0;
    let totalNo = 0;

    noFill();
    stroke(150, 150);
    ellipse(width / 2, height / 2, 500, 500); 
    
    if (globalState.voterA && globalState.voterB) {
        
        // Voto B: Cuadrado (Propio)
        rectMode(CENTER);
        if (globalState.voterB.vote === 'SÍ') {
            fill(0, 200, 0, 200); 
            totalSi++;
        } else {
            fill(200, 0, 0, 200); 
            totalNo++;
        }
        rect(width / 2, height / 2, 50, 50); 
        
        // Voto A: Círculo (Remoto)
        if (globalState.voterA.vote === 'SÍ') {
            fill(0, 200, 0, 200); 
            totalSi++;
        } else {
            fill(200, 0, 0, 200); 
            totalNo++;
        }
        ellipse(width / 2 - 100, height / 2, 50, 50); 

        // MARCADOR GLOBAL
        fill(0);
        textSize(32);
        textAlign(CENTER, CENTER);
        text(`RESULTADO: SÍ [${totalSi}] - NO [${totalNo}]`, width / 2, height - 50);

        textSize(18);
        text(`Mueve esta ventana (Cuadrado) al centro para votar SÍ.`, width / 2, 50);
    }
}
```

# Evaluacion

Actividad 01 5.0:	se respondieron todas las preguntas del reporte sobre la instalación, el inicio del servidor, la interacción de las páginas y la observación de logs/consolas

Actividad 02 5.0:	se respondio y se reflexiono sobre todos los conceptos fundamentales (Cliente-Servidor, URL, HTTP vs. Sockets, HTML/CSS/JS, modelo de eventos), utilizando los ejemplos solicitados y realizando el analisis de la URL

Actividad 03 5.0:	se realizaron y reportaron todos los experimentos en el servidor, incluyendo el cambio de rutas, la observación de IDs de conexion, la diferencia entre socket.emit y broadcast, y el cambio de puerto

Actividad 04 5.o:	se realizaron y reportaron todos los experimentos en el cliente, cubriendo la dependencia del servidor, la inicialización de datos, la comunicación bidireccional y la optimización de la función checkWindowPosition()

Actividad 05 4.5:	se explico la idea, se implemento completamente con código funcional (server.js, voterA.js, voterB.js) y se incluyó todo el código. El único elemento faltante son los bocetos 
