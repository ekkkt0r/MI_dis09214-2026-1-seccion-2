# sesión 10 - 14/04
## Diseño responsivo  
### ¿CÓMO CREAR UN SKETCH QUE SE ADAPTE A DIFERENTES TAMAÑOS DE PANTALLA?  
### Paso 1  
crear un camnvas con dimensiones dinámicas, Para esto usaremos las variables integradas en el sistema: (windowWidth, windowHeight) estas variables leen constantemente el ancho y alto disponibles de la ventana del navegador.  
### Paso 2  
Crear un evento windowResized() Si el usuario estira o encoge la ventana del navegador, el lienzo se adaptará al tamaño de la ventana en tiempo real.  
### Paso 3  
Usar valores relativos  
Ahora p5.js actualiza estas variables width y height automáticamente con el tamaño actual del lienzo. Entonces ahora todos los valores de posición y tamaño que ocupemos los tenemos que pensar en relación proporcional a esas variables.  
### Paso 4  
Incluir un factor de referencia, referencia = min(width, height), Creamos una variable global (referencia) y la asignamos para que calcule el mínimo. Observa el ancho y el alto de la ventana, los compara, y se queda solo con el que sea más pequeño en ese momento.  
### Paso 5  
Usar transalate - push y pop, complejas en cada rect() o ellipse(), usamos translate() para "mover el origen del mundo”. Y siempre utilizando push() y pop() para cada figura o elemento.  

## Agregar imágenes  
Paso 1: subir las imágenes a p5,   
#### paso 1. Hacer clic en la pequeña flecha hacia la derecha (>)
ubicada en la esquina superior izquierda del editor
(debajo del botón de Play). Esto abrirá el menú de
archivos laterales.

2. Hacer clic en el icono de + o el menú desplegable junto a
Files.

3. Seleccionar Upload file (Subir archivo).

4. Arrastrar la imagen o buscarla en el computador.

5. Recomendación: usar nombres de archivo cortos, en
minúsculas y sin espacios. Hacer una carpeta llamada
ASSETS

#### Paso 2: Declarar y precargar la imágen, function preload()  
1. Creamos una variable global al
inicio del código para guardar la
imagen.

2. Usamos la función dentro de function preload() y cargamos la imagen con loadImage()

#### Paso 3: Dibujar y dimensionar el draw  
La imagen se dibuja usando la función image(), Esta función requiere mínimo 3 argumentos, pero acepta 5 si queremos controlar su tamaño de
forma responsiva.  

