# Unidad 1

## 🔎 Fase: Set + Seek

### Activida 1

#### ¿que es un sitema fisico interactivo?

es un sistema donde se mesclan componentes fisicos con programacion y otros sistemas ya sea por medio de sistemas que registran el movimiento de algo o por otros medios para generar inmersion para las personas

#### ¿como podrias aplicar lo que has visto en tu perfil profecional?

podria usarlo para darle vida mis ilustraciones haciendo algun tipo de accion reconociendo un movimiento

### Actividad 2

#### ¿Qué es el diseño/arte generativo?

es una practica donde se programa una persona da reglas que una persona le da a una maquina instrucciones y reglas y la maquina con la autonomia genera imagenes o patrones y por medio de algun tipo de output plasman lo que generan, por este medio tambien se puede generar graficos a tiempo real que siguen las instrucciones impuestas por el programador como ejemplo podria ser un sestema que detecta las ondas de sonido y las grafique

#### ¿Cómo podrías aplicar lo que has visto en tu perfil profesional?

estos sistemas se pueden aplicar en mi perfil que apartir de colores y formas que me gustan la maquina genere imagenes apartir de mis preferencias y reglas que le ponga a la maquina

### Actividad 3

inputs: el serial por donde pasa la informacion, precionar los botones y agitar del microbit, precionar sende love en el programa
outputs: el cambio de color en la pantalla y el microbit muestra en la luces muestran un corazon
proceso: es lo que promagama impone y aprtir de esas normas registra lo se va haciendo para dar un output

### Actividad 4

```
function setup() {
  createCanvas(400, 400);
  setPositionAndColor();

}

function setPositionAndColor() {
  // Set the position to a random value (within the canvas)
  circleX = random(0, width);
  circleY = random(0, height);
  circleColor = color(random(100, 256), random(100, 256), random(100, 256));
  
  squareX = random(0, width);
  squareY = random(0, height);
  squareColor = color(random(100, 256), random(100, 256), random(100, 256));
  
  triangleX1 = random(0, width);
  triangleY1 = random(0, height);
  triangleX2 = random(0, width);
  triangleY2 = random(0, height);
  triangleX3 = random(0, width);
  triangleY3 = random(0, height);
  triangleColor = color(random(100, 256), random(100, 256), random(100, 256));
}

function draw() {
  background(100);
  
  fill(circleColor);
  circle(circleX, circleY, 100);
  
  fill(squareColor);
  square(squareX, squareY, 100);
  
  fill(triangleColor);
  triangle(triangleX1, triangleY1, triangleX2, triangleY2, triangleX3, triangleY3, 100);
}

function mousePressed() {
  setPositionAndColor();
}

```

<img width="1707" height="980" alt="image" src="https://github.com/user-attachments/assets/bac8007c-3d88-4465-bd64-f5ea382269af" />


[P5js](https://editor.p5js.org/ValMARR12/sketches/wNmGHAFbL)
