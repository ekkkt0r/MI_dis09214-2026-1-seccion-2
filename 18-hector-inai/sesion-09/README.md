# sesión 09 - 14/04
## Estados y camara web  
#### ¿Cómo crear un sketch con estados diferentes?  
1. Crear y definir un estado: Al principio de tu código (fuera de las funciones), debes crear una variable que guarde en qué pantalla nos encontramos. Empezará en 0.
2. Configurar el lienzo (function setup): Creamos el lienzo de fondo donde va a ocurrir toda la magia.
3. Crear la estructura del estado: Aquí usamos un switch Dependiendo del valor de la variable estado, el programa dibujará una cosa u otra.
4. Programar visualmente cada estado: Ahora creamos las funciones que inventamos en el paso anterior. Cada una tendrá un diseño y un color diferente para que se note el cambio.
5. La lógica del cambio y reinicio: Para pasar de un estado a otro y lograr que vuelva al comienzo,
usamos la función mousePressed() Cada vez que hagas un clic, la variable aumentará. Si llega a 3 (después del estado 2), volverá a 0.


1. Interaccíon con el telado: Por barra espaciadora o Enter: Por teclas específicas (ej. Números): Puedes hacer que la tecla 1 te lleve al inicio, la 2 a la experiencia y la 3 al final.
2. Botones reales en la pantalla: En lugar de hacer clic en cualquier parte de la pantalla, puedes crear un botón real de
HTML usando la librería de p5.js. Esto es mucho más profesional para menús.
3. Zonas de clic: Si no quieres usar botones de HTML y prefieres dibujar tus propios botones con rect(), puedes evaluar si el mouse
estaba dentro de esa caja al hacer clic.
4. Interacciones automáticas: Por Tiempo (Temporizador): Ideal para una pantalla de introducción (Splash Screen) que dura 3 segundos y pasa sola al menú.


