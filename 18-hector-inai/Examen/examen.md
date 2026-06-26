
# Desarrollo de identidades de género en ambientes hostiles
**Autor: Héctor Inai Espinoza**

Este proyecto representa en un código de p5.js la dificultad de desarrollar y expresar una identidad de género que se escapa de lo cisgénero y heteronormado.

---

## ¿Qué se ve en la pantalla?

En la pantalla, al entrar, vemos 4 figuras grandes y grises en 4 esquinas del canvas, rodeadas por elipses que cambian de color entre tonos grises, con un texto que dice "abre la boca para que las figuras se expresen". Cuando abres la boca, la pantalla se vuelve progresivamente roja mientras te ves de fondo "gritar", con líneas aleatorias por toda la pantalla, también ojos y bocas apareciendo y desapareciendo aleatoriamente por todo el canvas.
Las figuras también se mueven al abrir la boca, se transforman en una figura diferente mientras se glitchean.

---

## ¿Qué elementos visuales aparecen?

Aparecen elementos como elipses pequeñas en loop y 4 figuras en 4 esquinas del canvas; al abrir la boca aparecen líneas aleatorias e imágenes de ojos y bocas, también un glitch de formas. y el mas importante, el usuario de fondo gritando.

---

## Descripción conceptual

La idea central de este proyecto es representar la opresión que pone la sociedad ante una expresión de género diferente a la común, por lo que al abrir la boca, gestuando el criticar u opinar, la presión encima de las figuras empieza a mostrarse.
el facetrack es el elemento importante en este sistema; al abrir la boca las figuras comienzan a "expresarse" y se revela la respuesta del entorno ante esta acción.

### ¿Cómo se relaciona este sistema con la problemática?
Las elipses en el fondo, en tonos grises, representan a una sociedad heteronormada que rodea a las 4 figuras principales, a las que, al abrir la boca, estamos permitiéndoles que se expresen. Al ellas expresarse, cambian de forma a una que no es su original, pero, junto a este cambio, el entorno de la sociedad comienza a tornarse rojo, con el usuario como principal opresor, lleno de furia y de ojos y bocas que critican, juzgan y oprimen. Los cuerpos principales se glitchean, representando la dificultad de expresarse libremente.

---

## Reglas que gobiernan el sistema

### 1. INPUT
* **Video en tiempo real:** Captura de píxeles de la webcam.
* **Imágenes:** Los archivos `ojo.png` y `boca.png` desde la memoria.
* **Tiempo:** El pulso constante del programa que corre a 50 fotogramas por segundo.
* **Coordenadas espaciales:** El arreglo rostros[0].keypoints entrega las posiciones numéricas (x,y) de los puntos faciales 13 (labio superior) y 14 (labio inferior) del usuario.
* **Presión del teclado:** El estado físico de la barra espaciadora (keyPressed).

### 2. ¿Cómo se procesan y transforman?
* **Variable `ruido`:** Si presionas, el `ruido` sube (hasta 255); si sueltas, baja (hasta 0).
* **calculo de pixeles faciales:** Con un FaceMesh. Se fijan puntos específicos en el rostro (puntos 13 (labio superior) y 14 (labio inferior) del usuario). En píxeles se calcula su distancia; mientras más abierta la boca, el número de píxeles aumenta. Esto afecta en variables y mapeos en el sistema.
* **Map:** Con un número de píxeles mínimo de 5 y máximo de 40 entre los puntos 13 y 14, el "mapeoRuido" transforma estos valores, llevando el mínimo a 0 y el máximo a 5. Para que estos números, ya transformados por map, se sumen a la variable "ruido".
* **La Desaparición:** Existe un map en la variable "opacidadOscura" que toma "ruido" ya transformado y lo utiliza para variar la opacidad de rectángulos que cubren la pantalla y generan este efecto de desaparición progresiva.
* **Elipses de fondo `for`:** El loop procesa las coordenadas `X` e `Y` sumando de 25 en 25 píxeles para multiplicar un solo círculo en una cuadrícula multiplicada y alineada.
* **El Caos `random` y `rotate`:** El sistema muestra ángulos de rotación continuos y genera números al azar para desordenar posiciones, tamaños y colores.

### 3. ¿Qué respuesta visual producen? - OUTPUT
* **Estado 1 (Silencio):** Pantalla gris semitransparente, figuras estáticas y texto instructivo pasivo.
* **Estado 2 (Caos):** Filtro rojo cuya opacidad e intensidad aumentan directamente con el valor de ruido; desaparición gradual de la red gris; parpadeo glitch de color en las figuras geométricas; dibujo continuo de líneas y aparición masiva con transparencia (tint) de imágenes de ojos y bocas.
* **Estado 3 (Censura):** Pantalla completamente negra (background(0)) con texto rojo de gran tamaño que bloquea el resto del programa.

---

## 2. Sistema de interactividad

El usuario no interactúa con un mouse o un botón físico para activar la obra, sino con su propia expresión facial.
El sistema rompe el bucle interactivo lineal mediante la automatización de estados: el usuario inicia una acción (hablar), lo que altera una variable numérica de manera invisible (ruido). Cuando esa variable cruza límites críticos (0, 15, 255), el software toma decisiones autónomas cambiando el valor de la variable estado.
Finalmente, para salir del bucle de censura del Estado 3, se introduce una regla de interactividad explícita mediante periféricos (keyPressed de la barra espaciadora), lo que reinicia los contadores de memoria de la computadora (ruido = 0; estado = 1;) devolviendo el sistema a su equilibrio inicial.

![Diagrama de flujo](diagrama1.png)

---
# Codigo P5.js

```
let ruido = 0;
let rotacion = 0;
let ojo, boca;

// Variables para la camara y IA
let capturaCamara;
let faceMesh;
let rostros = [];
let bocaAbierta = false;

let estado = 1;

let sociedadArray = []; //array creado para las elipses grises

class sociedad {  //class que les da las caracteristicas y posición a las elipses grises
  constructor(x, y) {
    this.x = x;
    this.y = y;
    this.tamano = 15;
  }

  mostrar() {
    let personasGrises = random(255);
    fill(personasGrises, 255 - ruido);
    noStroke();
    ellipse(this.x, this.y, this.tamano);
  }
}

function preload() {  //función para cargar fotos
  ojo = loadImage("ojo.png");
  boca = loadImage("boca.png");
}

function setup() {
  createCanvas(1920, 1080);
  frameRate(50);

  capturaCamara = createCapture(VIDEO); //activa la cámara
  capturaCamara.size(640, 480);
  capturaCamara.hide(); //oculta la camara html que por defecto aparece

  faceMesh = ml5.faceMesh({ maxFaces: 1, refineLandmarks: true }, listo); //Activa el motor de seguimiento facial de la biblioteca ml5.js. define el máximo de rostros a reconocer. Activa máxima presición en reconocimiento facial y crea callback para iniciar todo cuando el facemesh este completamente cargado.

  for (let x = 0; x <= width; x += 25) {
    for (let y = 0; y <= height; y += 25) {
      sociedadArray.push(new sociedad(x, y)); // repite y da posición a las elipses creadas en el class y las guarda en el array
    }
  }
}

function listo() {
  faceMesh.detectStart(capturaCamara, gotFaces); //función que le da el OK al facemesh para que se ejecute cuando este completamente cargado
}

function windowResized() { //función que permite que el canvas se adapte a la ventana
  resizeCanvas(windowWidth, windowHeight);
}

function draw() {
  analizarRostro();

  if (estado === 1) { //declara cada estado a su respectiva función que ejecute los necesario en cada estado
    estadoSilencio();
  } else if (estado === 2) {
    estadoCaos();
  } else {
    estadoCensura();
  }
}

function analizarRostro() {
  if (rostros.length > 0) { //enumera al primer rostro que aparezca, como el máximo de rostros es 1, el unico que aparezca sera enumerado como "0" para reconocerlo.
    let puntos = rostros[0].keypoints; //pone puntos de rastreo facial por todo el rostro del usuario "0"
    let labioSuperior = puntos[13]; //define el punto central del labio superior
    let labioInferior = puntos[14]; //define el punto central del labio inferior

    if (labioSuperior && labioInferior) { //el doble && permite que ambas variables solo se ejecuten cuando hayan datos disponibles, en este caso, que esten visibles los labios, y asi no se buguee el sistema
      let distanciaBoca = dist( //la función dist permite calcular la distancia entre puntos 13 y 14 en X y Y
        labioSuperior.x,
        labioSuperior.y,
        labioInferior.x,
        labioInferior.y
      );

      let mapeoRuido = map(distanciaBoca, 5, 40, 0, 5); //map que transforma la distancia de la boca en numeros del 0 al 5
      mapeoRuido = constrain(mapeoRuido, 0, 5); //limita que a pesar de que el usuario abra mucho la boca, el número maximo no pase del 5, o que el minimo no baje infinitamente

      if (distanciaBoca > 15) { // si la distancia de la boca es mayor o igual a 15, la variable bocaAbierta es verdadera
        bocaAbierta = true;
        ruido += mapeoRuido; //si es verdadera, el ruido se suma con los resultantes de "mapeoRuido"
        if (estado === 1) estado = 2; //si esto sucede, el estado 1 pasa directamente al estado 2
      } else {
        bocaAbierta = false; //si bocaAbierta es falso
        ruido -= 4; //ruido comienza a restarse de a -4 hasta llegar a 0
        if (estado === 2) estado = 1; //al cerrar la boca ya estando en el estado 2, los números de ruido comienzan a bajar, por lo que esta linea la hace volver al estado 1
      }
    }
  }
}

function estadoSilencio() { //definir estado 1, el estado del silencio
  ruido = max(ruido, 0); // la funcion "max" tambien funciona para dar limites, pero solo al extremo inferior, por lo que permite al maximo seguir subiendo libremente, pero no bajar de 0

  image(capturaCamara, 0, 0, width, height); //volvemos a capturar la Webcam para ocuparla en este estado

  let opacidadOscura = map(ruido, 0, 50, 255, 180); //let que define el juego de opacidad en la pantalla 1. el map utiliza la variable "ruido" como valor, que transforma el minimo a la maxima opacidad, y el maximo a una opacidad media
  fill(28, 28, 28, opacidadOscura); //llena la pantalla con un color gris y su opacidad depende del let "opacidadOscura"
  rect(0, 0, width, height);

  dibujarSociedad();

  noStroke();
  fill(35);
  rect(width * 0.2, height * 0.2, 80, 80);
  circle(width * 0.8, height * 0.2, 80);
  triangle(
    width * 0.2,
    height * 0.7,
    width * 0.18,
    height * 0.8,
    width * 0.22,
    height * 0.8
  );
  ellipse(width * 0.8, height * 0.7, 70, 100);

  dibujarTexto("abre la boca para que las figuras se expresen");
}

function estadoCaos() { //definir estado 2, el estado del caos
  ruido = constrain(ruido, 0, 255); // limita que ruido no baje mas que 0 ni suba mas a 255

  image(capturaCamara, 0, 0, width, height); //captura de nuevo la imagen de Webcam para utilizarla en este estado

  fill(28 + ruido, 28, 28, ruido); //rectangulo gris que se desvanece dependiendo de ruido
  rect(0, 0, width, height);

  dibujarSociedad();
  dibujarFotosAleatorias(); //declara estas dos variables dentro del estado dos para que se dibujen mientras el sistema este dentro de este estado.

  fill(random(255), random(255), random(255), 220);
  noStroke();
  rect(width * 0.8 + random(-15, 15), height * 0.7 + random(-15, 15), 100, 120);
  circle(width * 0.2 + random(-15, 15), height * 0.7 + random(-15, 15), 90);
  triangle(
    width * 0.8 + random(-15, 15),
    height * 0.2 + random(-15, 15),
    width * 0.78,
    height * 0.3,
    width * 0.82,
    height * 0.3
  );
  ellipse(
    width * 0.2 + random(-15, 15),
    width * 0.2 + random(-15, 15),
    70,
    100
  ); //dibuja las figuras aleatorias para crear el glitch

  dibujarTexto("juzgándolas... ¿Por qué lo haces?");

  if (ruido >= 255) { //si la variable ruido llega a ser igual o mayor a 255, automaticamente pasa del estado 2 al estado 3
    estado = 3;
  }
}
function estadoCensura() { //definir estado 3, el estado de la censura
  background(0);
  fill(255, 0, 0);
  textSize(50);
  textAlign(CENTER, CENTER);
  text(
    "CALLATE! dejalas vivir en paz.\nPresiona 'Espacio' para reiniciar",
    width / 2,
    height / 2
  );
}

function dibujarSociedad() { //función que declara el dibujo de la sociedad fuera de todo estado, para que se pueda declarar facilmente dentro de cada estado en el que sea necesario
  for (let i = 0; i < sociedadArray.length; i++) {
    sociedadArray[i].mostrar();
  }
}

function Glitch() { //función que declara el glitch fuera de todo estado, para que se pueda declarar facilmente dentro de cada estado en el que sea necesario
  let r, g, b;
  r = random(50);
  g = random(50);
  b = random(255);
  push();
  rotacion += 1;
  rotate(rotacion);
  stroke(r, g, b);
  line(0, random(0, height), width, random(0, height));
  line(0, random(0, height), random(0, width), random(0, height));
  pop();
}

function dibujarFotosAleatorias() {//función que declara el glitch fuera de todo estado, para que se pueda declarar facilmente dentro de cada estado en el que sea necesario
  tint(255, ruido); //ayuda a la aparición progresiva de las imagenes
  let d = random(100); //variable que arroja numeros aleatorios de 0 al 100
  let tam = random(40, 300); //variable que entrega tamaño aleatorio entre 40 y 300 a las imagenes
  if (d < 50) { //si d arroja un numero menor o igual a 50 se dibujara la imagen de "ojo"
    image(ojo, random(0, width - tam), random(0, height - tam), tam, tam);
  } else { //si d arroja un numero mayor o igual a 50 se dibujara la imagen "boca"
    image(boca, random(0, width - tam), random(0, height - tam), tam, tam);
  }
  noTint();//cierra el filtro de tint para que no afecte a otras cosas
}

function dibujarTexto(textitos) { //da caracteristicas a los textos
  fill(255, 255 - ruido);
  textSize(30);
  textAlign(CENTER);
  noStroke();
  text(textitos, width / 2, height * 0.9);
}

function keyPressed() { //función que declara que al apretar tecla espacio, estando Solo en el estado 3, se vuelve al estado 1, el ruido vuelve a 0 y bocaAbierta se hace falso.
  if (key === " " && estado === 3) {
    estado = 1;
    ruido = 0;
    bocaAbierta = false;
  }
}
function gotFaces(results) { //función que recibe los datos de los puntos del rostro, los almacena en la variable "rostros" para que se puedan usar en el codigo.
  rostros = results; //variable rostros es igual a los resultados de identificación de puntos del facemesh
}

```
---

[Link P5.js]([https://editor.p5js.org/ekkkt0r/sketches/mfg8rmYoM](https://editor.p5js.org/ekkkt0r/sketches/QgtesbCIl))
