# Ejemplo Documentación 
![Tinkercad](./img/ArduinoTinkercad.jpg)


## Integrantes 
- Oscar Alonso
- Andrés Anguindegui
- Natalia Diez


## Proyecto: Modificación con interruptor deslizante y números primos. Agregado de motor de corriente continua y sensor de temperatura.
![Tinkercad](./img/Proyecto_2.png)


## Descripción
La primera fase del proyecto consiste en el reemplazo de uno de los botones del primer proyecto, por un interruptor deslizante. Al deslizar el mismo, se representan números primos o números enteros en los display de 7 segmentos.
En la segunda fase del proyecto, se implementan un sistema de refrigeración que incluye un motor de corriente continua, un transistor (NPN), una batería de 9 Voltios y un sensor de temperatura (TMP36). 
Los displays muestran la temperatura a la cual el cooler del CPU se activará, pudiendo esta ser ajustada por los botones de incremento y reducción.
En el momento en que la temperatura está por debajo del valor definido, el motor se apaga.


## Funciones
## encenderSegmento
La función encenderSegmento es una función utilizada para controlar la visualización en un display de 7 segmentos. 
A través de esta función, se pueden encender o apagar segmentos específicos del display, permitiendo mostrar dígitos numéricos.

Parámetros
* "sa" posicion display A.
* "sb" posicion display B.
* "sc" posicion display C.
* "sd" posicion display D.
* "se" posicion display E.
* "sf" posicion display F.
* "sg" posicion display G.

La función toma estos parámetros y utiliza la instrucción digitalWrite para encender o apagar los pines del display de 7 segmentos correspondientes a los segmentos requeridos. Cada segmento está representado por una letra (A, B, C, D, E, F, G), y la función se encarga de establecer el estado de esos segmentos según los valores proporcionados.

~~~ C
void encenderSegmento(int sf, int sa, int sb, int sg, int se, int sd, int sc)
{
  digitalWrite(F, sf);
  digitalWrite(A, sa);
  digitalWrite(B, sb);
  digitalWrite(G, sg);
  digitalWrite(E, se);
  digitalWrite(D, sd);
  digitalWrite(C, sc);
}
~~~

## seleccionarNumero
Esta función toma un número entero como parámetro y configura el estado de los segmentos del display para representar ese número.

Parámetros
* "numero" este parámetro es un número entero que se desea mostrar en el display de 7 segmentos. 
Puede ser un valor del 0 al 9 y se utiliza para seleccionar el número que se mostrará en el display.

La función utiliza una estructura switch para determinar qué configuración de segmentos se debe aplicar para mostrar el número proporcionado.

~~~ C
void seleccionarNumero(int numero)
{
  switch(numero)
  {
    case 0:
        encenderSegmento(1,1,1,0,1,1,1); 
        break;
    case 1: 
        encenderSegmento(0,0,1,0,0,0,1); 
        break;
    case 2: 
        encenderSegmento(0,1,1,1,1,1,0); 
        break;
    case 3: 
        encenderSegmento(0,1,1,1,0,1,1); 
        break;
    case 4: 
        encenderSegmento(1,0,1,1,0,0,1); 
        break;
    case 5: 
        encenderSegmento(1,1,0,1,0,1,1); 
        break;
    case 6: 
        encenderSegmento(1,1,0,1,1,1,1); 
        break;
    case 7: 
      	encenderSegmento(0,1,1,0,0,0,1); 
        break;
    case 8:
        encenderSegmento(1,1,1,1,1,1,1); 
        break;
    case 9: 
        encenderSegmento(1,1,1,1,0,1,1); 
      break;
  }
}
~~~

## encenderDisplay
Esta función establece el encendido de los display, tanto del display de unidad como del de decena.

Parámetros
* "estadoDecena" estado del display de decenas. Puede ser 0 o 1.
* "estadoUnidades" estado del display de unidades. Puede ser 0 o 1.

Esta función muestra los números en los displays de 7 segmentos. Al controlar cuál de los dos displays está encendido en un momento dado, permite la visualización de números de dos dígitos.

~~~ C
void encenderDisplay(int estadoDecena, int estadoUnidades)
{
  digitalWrite(displayDecena, estadoDecena);
  digitalWrite(displayUnidad, estadoUnidades);
}
~~~

## mostrarNumeroEnDisplay
Esta función se encarga de mostrar un número en los display de 7 segmentos.

Parámetros
* "numero" número entero que se desea mostrar en el display de 7 segmentos. Debe ser un número entre 0 y 99.

Divide el número en sus dígitos de decena y unidad, y luego activa los segmentos correspondientes en el display para representar estos dígitos en orden.

# multiplexación

## calcularPrimoSiguiente
Función que calcula el siguiente número primo al proporcionado en el parámetro.

Parámetros
* "nuevoNumero" número entero que representa el número partir del cual se busca el siguiente número primo.

Return 
* devuelve un número entero que representa el siguiente número primo mayor o igual al valor proporcionado en nuevoNumero.

Calcula y devuelve el siguiente número primo mayor que el valor de entrada nuevoNumero. Comienza evaluando nuevoNumero y, si no es primo, busca el próximo número primo en la secuencia incrementando nuevoNumero hasta encontrarlo. Una vez que lo encuentra, lo retorna.

~~~ C
int calcularPrimoSiguiente(int nuevoNumero){
  int divisores = 0;
  //bucle de validacion de numero primo
  for (int i=nuevoNumero-1; i<nuevoNumero; i++){
    //calcular cantidad de divisores
    for (int j=2; j<nuevoNumero; j++){
      if (nuevoNumero % j == 0){
        divisores += 1;
      }
    }
    //verificacion de numero primo
    if (divisores != 0){
      nuevoNumero = nuevoNumero + 1;
      divisores = 0;
    }
  }
  return nuevoNumero;
}
~~~

## calcularPrimoAnterior
Función que calcula el anterior número primo al proporcionado en el parámetro.

Parámetros
* "nuevoNumero" número entero que representa el número partir del cual se busca el siguiente número primo.

Return 
* devuelve un número entero que representa el número primo anterior al valor proporcionado en nuevoNumero.

Calcula y devuelve el anterior número primo menor que el valor de entrada nuevoNumero. Comienza evaluando nuevoNumero y, si no es primo, busca el posterior número primo en la secuencia reduciendo nuevoNumero hasta encontrarlo. Una vez que lo encuentra, lo retorna.

~~~ C
int calcularPrimoAnterior(int nuevoNumero){
  divisores = 0;
  //bucle de validacion de numero primo
  for (int i = nuevoNumero+1; i>nuevoNumero; i--){
    //calcular cantidad de divisores
    for (int j=2; j<nuevoNumero; j++){
      if (nuevoNumero % j == 0){
        divisores += 1;
      }
    }
    //verificacion de numero primo
    if (divisores != 0){
      nuevoNumero = nuevoNumero - 1;
      divisores = 0;
    }
  }
  return nuevoNumero;
}
~~~

## :robot: Link al proyecto
- [proyecto](https://www.tinkercad.com/things/ehpzdGsuO3Q?sharecode=D0ccmFK8HFq4Cpc5kR4ZHy3QSPFQ4Mxe5icBIgs1Tp0)

---
### Fuentes
- [Consejos para documentar](https://www.sohamkamani.com/how-to-write-good-documentation/#architecture-documentation).

- [Lenguaje Markdown](https://markdown.es/sintaxis-markdown/#linkauto).

- [Markdown Cheatsheet](https://github.com/adam-p/markdown-here/wiki/Markdown-Cheatsheet).

- [Tutorial](https://www.youtube.com/watch?v=oxaH9CFpeEE).

- [Emojis](https://gist.github.com/rxaviers/7360908).

---






