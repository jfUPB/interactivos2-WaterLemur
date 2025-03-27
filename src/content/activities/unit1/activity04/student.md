#### Slime Molds

##### Descripción del código:

Primero en la funcion setup se crea el canvas de fondo y se asignan unas variables iniciales, un loop crea los "mold", luego en la funcion draw se va actualizando la posicion de las trayectorias. Luego se calcula por donde a pasado y se decide si se genera una nueva trayectoria.

##### Modificación del código:

Al reducir el valor de la variable num, el numero de lineas (mold) se reduce considerablemente, afectando la imagen final.
Al cambiar el color de fondo tambien se altera la imagen final, en algunos casos no se distingue por ser del mismo color.

#### Reflexión:
El codigo es como usar pintura en un lienzo, cualquier linea que se le agregue o modifique, va a modificar el resultado final de la obra de arte.

#### Text in flowers

##### Descripción del código:

Primero hay unas variables con texto y formato de texto, se precarga las fuentes, el setup crea el canvas y los colores de la paleta
La funcion draw crea unas nubes utilizando las paletas de color seleccionadas

##### Modificación del código:

Al cambiar la variable dummytext se modifica el texto que muestra la aplicacion y las flores de fondo solo cubren hasta donde llegan estas letras.
Al reducir la cantidad de paletas disponibles, se reduce la cantidad de colores que van a tomar las flores, al igual que si se le agregan variables a la paleta, esta va a quedar mas enriquesida y mostrar mas colores.

#### Reflexión:

Al crear estos codigos, se debe tener muy presente que nivel de editabilidad puede tener, inclusive se pueden crear "features" que le den control al usuario sin comprometer la integridad de la obra que se desea realizar.
