# sesión 04 - 10/04
# sesión 05 - 10/04
# Transformaciones y condicionales {If-else}  

## Transformaciones  
#### Ángulos-angleMode();  
Por defecto p5.js usa RADIANES para medir los ángulos.  
__angleMode(RADIANS):__ Para cambiar a grados se usa esto en el fuction SETUP.
__angleMode(DEGREES):__ Hay que pensarlo como se mueven las agujas del reloj:  
*  TWO_PI - 360°
*  PI - 180°
*  HALF_PI - 90°
*  QUARTER_PI - 90°

#### Rotación-rotate();  
Sirve para rotar elementos.  

Siempre rota alrededor del punto de origen(0,0).  

Se recomienda usar con translate(); y en algunis casos con el rectMode(CENTER);   

__Sintaxis:__ rotate(valor radianes o en grados);  
__Ejemplo:__ rotate(20);  

#### Trasladar-translate();  
Sirve para trasladar el punto de origen (0,0) a otra cordenada de mi canvas:  

__Sintaxis:__ translate(x,y);  
__Ejemplo:__ translate (200,200);  

#### Guardar y restaurar-push(); pop();  
Funciones que trabajan juntas como un "sistema de memoria temporal" para el estilo y las transformaciones del lienzo.  

__Sintaxis:__ push();  
__Sintaxis:__ pop();  

#### Escala-scale();  
La función scale() ajusta la escala del sistema de coordenadas actual por el factor especificado.  

__Sintaxis:__ scale(x,y);  
__Ejemplo:__ scale(2,2);  

## Condicionales  

#### Lógica condicional- Expresión booleana  
Una expresión booleana es cualquier enunciado, dato o instrucción que, al ser evaluado, solo puede arrojar uno de dos valores posibles: verdadero(true) o falso(false).  

__Ejemplo:__ Si algún estudiante apaga por completo las luces de la sala, la profesora debe bailar.  

###### Para construir este tipo de expresiones se utilizan 3 tipos de elementos:  
__Operandos (o Valores):__ Son los datos básicos que se evalúan. Pueden ser:  
*  variables: (como x,y o mouseX, mouseY, etc).
*  Constantes o Literales: Valores fijos como 5, "Hola" o los mismos valores booleanos True y False.

__Operadores de comparación:__ Permiten contrastar dos valores.  
*   == (igual a).
*   != (diferente de).
*   > o < (mayor o menor que).
*   >= o <= (mayor o igual/menor o igual).

__Operadores lógicos:__ Sirven para combinar varias expresiones.  
*  AND (&&): Es verdadero solo si ambas partes son verdaderas.
*  OR (||): Es verdadero si al menos una de las partes es verdadera.
*  NOT(!): Invierte el valor (si era verdadero, pasa a ser falso).

## Sentencia condicional  
#### If - else of - else   
La sentencia if es una estructura especial que existe en casi todos los lenguajes de programación; toma una condición -expresada como un booleano- y ejecuta una pieza de código contenida dentro de las llaves { }.  

If (condición) {ejecuta este código si es true} 
                    = 
Si algún estudiante apaga por completo las luces de la sala, la profesora debe bailar.  

La sentencia if puede complementarse con la sentencia else if, que añade condiciones de prueba completamentarias a la original, y con la sentenci else, que implica todos los casos que no cumplen con la condición original.  

*  If (condición) {ejecuta este código si es true}
*  else if (condición 2) {ejecuta este código si es true}
*  else [ejecuta este código si ambas condiciones son falsas}
                           =
*  Si algún estudiante apaga por completo las luces de la sala, la profesora debe bailar.
*  Si no se cumple lo anterior SI la ayudante se levanta de su silla, la profesora debe cantar.
*  Y si no se cumplen ninguna de las anteriores, los estudiantes guardan silencio.


