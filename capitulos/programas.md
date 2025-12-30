# Programas

Para que una computadora haga algo, tú (o alguien más) debe decirle
exactamente - y en detalle - lo que tiene qué hacer. A la
descripción precisa sobre lo \"que tiene qué hacer una computadora\" se
le llama programa y la programación es la actividad de escribir y
depurar programas.

Desde una perspectiva más abstracta, un programa es simplemente una
lista con instrucciones agrupadas en bloques que una computadora ejecuta
en un orden especifico. Las instrucciones forman parte de lo que se
denomina código fuente. El proceso que traduce el código fuente a
lenguaje maquina es la compilación y al programa terminado se le
denomina programa ejecutable. Un programa ejecutable contiene el código
binario que la computadora puede entender y por supuesto es capaz de
ejecutar.

Este capítulo describe el proceso de compilación detrás de un programa
ejecutable escrito en el lenguaje de programación C++.

## El proceso de compilación

Para crear un programa que pueda ser entendido por una computador es
necesario traducir el código fuente de un programa a código máquina,
usando un programa traductor conocido como **compilador** en un proceso
automatizado llamado **compilación**.

Un compilador es un programa que traduce el código fuente de un programa
de un lenguaje de programación a otro. Por lo general a código máquina.
El proceso de compilación se lleva a cabo en varias etapas:

1.  El programador escribe el **código fuente** usando un editor de
    texto y un lenguaje de programación, como C++.
2.  En el **procesamiento** el analizador sintáctico sustituye cada uno
    de los símbolos y macros definidos por un valor especifico.
3.  El **traductor** traduce el código fuente en código objeto.
    Generalmente este proceso se agrupa en dos tareas: el análisis del
    programa fuente y la síntesis del programa objeto.
4.  El **enlazador** ensambla y vincula el código objeto junto con los
    módulos adicionales (como bibliotecas) para producir un archivo
    ejecutable.
5.  El resultado es un **archivo ejecutable** (o binario) que se puede
    \"ejecutar\" en una computadora.

Los pasos exactos dependen del entorno y del compilador, ya que algunos
compiladores \"traducen\" el código, lo vinculan y producen un archivo
ejecutable en un solo proceso. El termino compilación por tanto incluye
la traducción, el ensamblado y el enlazado.

``` C++
+--------+               +-----------+               +--------+
| Código | Preprocesador |   Código  | Preprocesador | Código |
| fuente |- - - - - - - >| procesado |<- - - - - - --| fuente |
+--------+               +-----+-----+               +--------+
                               |                     
                           Compilador
                               |
                               v
                          +--------+
                          | Código |
                          | objeto |
                          +----+---+
                               |
                           Enlazador
                               |
                               v
                         +-----+------+                   +----------+
                         |   Código   | Sistema Operativo | Programa |
                         | ejecutable |- - - - - - - - - >|en memoria|
                         +-----+------+                   +----------+
```

Por cierto, los archivos binarios que resultan del proceso de
compilación son dependientes de la plataforma en la que se compilan. A
sí que cuando se habla sobre la portabilidad de un programa, esta se
refiere al código fuente, no a los archivos ejecutables.

### El código fuente de un programa

Para empezar a escribir programas es necesario usar un editor de texto
(con resaltado de sintaxis) para escribir el programa fuente y un
compilador como gcc o clang (que son libres), también puedes usar un
entorno de desarrollo integrado (o IDE) que incluya un compilador, un
editor y un depurador. Un IDE ofrece un conjunto de herramientas
destinadas a escribir y modificar código y es útil para detectar y
corregir errores. Si eres curioso puedes encontrar una lista sobre
compiladores y entornos de desarrollo en
<https://es.wikipedia.org/wiki/C%2B%2B>.

Si ya tienes un compilador o un IDE (o ambos) instalado en tu sistema
operativo estas listo para escribir programas ejecutables.

## La función main

El programa más pequeño (y válido) que se puede escribir en C++ es este:

``` C++
// El programa más pequeño (y válido) en C++
int main() { }
```

Este programa define una función llamada `main`.

- Una función en C++ comienza con **el tipo que devuelve la función**,
  seguido por el nombre de la función y un par de paréntesis, seguido
  por llaves de apertura y cierre.
- Dentro de las llaves se define **el cuerpo de la función**. Siempre se
  deben usar llaves aunque el cuerpo de la función este vació.

El valor devuelto por la función `main()` es el valor devuelto por el
programa al sistema. Un valor distinto de cero indica un fallo en la
ejecución. Si no se especifica de forma explicita el valor de retorno,
el compilador devolverá un valor de cero, lo que indica que el programa
se ha ejecutado con éxito.

Aunque este es un programa totalmente valido es inútil. Ya que por lo
general los programas deben realizar alguna tarea, como mostrar en la
consola algún tipo de mensaje o el resultado de un cálculo especifico.

### El primer programa

Por lo general los programas muestran algún tipo de salida. Por ejemplo
un programa que imprime el clásico mensaje: ¡Hola, mundo!.
Tradicionalmente los libros sobre programación comienzan con un ejemplo
que muestra este mensaje.

``` C++
#include <iostream>         // cout

// Programa que imprime ¡Hola, mundo!
int main()
{
    std::cout << "¡Hola, mundo!\n";
    return 0;
}
```

Guárdalo y ponle un nombre, ---`hola.cpp`, por ejemplo---. La extensión
`.cpp`, se usa por convención para nombrar los archivos fuente escritos
en C++.

Esto es lo que hicimos. El programa inicia con una declaración que le
dice al compilador que incluya al archivo `iostream` en el proceso de
compilación.

``` C++
#include <iostream>
```

La **directiva** `#include` le dice al preprocesador que incluya un
archivo de cabecera (o de encabezado). Las directivas comienzan con el
símbolo `#` seguido por la palabra clave `include`. El archivo
`iostream` es una biblioteca estándar de C++ que permite el acceso a los
dispositivos estándar de entrada y salida.

> [!TIP]
> **Las cabeceras estándar**
> 
> C++ contiene muchas bibliotecas de funciones. Cada biblioteca tiene
> asociado un archivo de definición que se denomina cabecera. Para
> utilizar alguna funcionalidad de una biblioteca en un programa (por
> ejemplo, `cout` está definido en el archivo `iostream`), hay que colocar
> al principio del programa una directiva de pre-procesamiento, seguido
> por el nombre de la biblioteca que se quiere usar entre signos \"menor>>
> que\" y \"mayor que\" como: `<iostream>`. Las cabeceras estándar no usan
> extensión.

Después de la línea en blanco se encuentra una línea con un comentario.

``` C++
// Programa que imprime ¡Hola, mundo!
```

Las dos diagonales al inicio, indican que el texto que sigue es un
**comentario**. Los comentarios son opcionales y son ignorados por el
compilador, pero son útiles para que otros programadores (o tú, algún
tiempo después) sepan lo que hace el código.

La línea 4 comienza con la definición de una función de tipo entero
llamada `main`. Esta es la función más importante del programa, ya que
es la encargada de invocar el código cuando el sistema operativo ejecute
el programa.

``` C++
int main() 
{
   // El cuerpo de la función es el código a ejecutar
} 
```

El cuerpo de una función es el código dentro de las llaves. En C++ las
llaves delimitan bloques de declaraciones. Cualquier definición de
función debe usar un par de llaves.

> [!WARNING]
>
>**El nombre de la función main**

> Normalmente puedes darle el nombre que quieras a una función, pero
> `main` es un caso especial ---el programa comienza a ejecutarse al
> inicio de esta función---. Esto significa que todo programa ejecutable
> debe tener una función llamada `main()` en algún lugar.

Dentro de las llaves `cout` es el encargado de mostrar un valor literal:
\"el texto entre comillas\", `cout` usa el operador de inserción `<<`
para redirigir el flujo de datos (el mensaje de texto) a la salida
estándar (la consola). Esta es una de las caracteristicas especificas
que distinguen al lenguaje C++, llamada: sobrecarga de operadores.

``` C++
std::cout << "¡Hola, mundo!\n";
```

La palabra clave `std` se usa para acceder a un **espacio de nombres**
llamado `std`, usando el operador de resolución de ámbito, ---los puntos
dobles `::`---.

Todas las clases y funciones del modelo de bibliotecas de C++ se
declaran dentro de lo que se denomina el espacio de nombres `std` (por
estándar). Por lo tanto, para poder acceder a las funcionalidades
definidas dentro de la biblioteca estándar, declaramos con esta
expresión que vamos a utilizar dicho espacio de nombres. Esta línea de
código es muy frecuente en programas que utilizan la biblioteca
estándar. Aunque como veremos un poco más adelante existen otras formas
de usar los espacios de nombres.

La expresión `std::cout << "¡Hola, mundo!\n";` termina con punto y coma.
El punto y coma es el separador de declaraciones y expresiones de C++ y
es obligatorio usarlo al final de una declaración.

``` C++
return 0;
```

La sentencia `return` al final de la función es el valor devuelto por la
función `main`. Un valor de retorno diferente a `0`, indica problemas en
la ejecución de la función. Como dato adicional, la función `main` es el
único requerimiento que necesita un archivo en C++ para ser convertido
en ejecutable. No importa que el cuerpo de la función este vacía.

#### Hola mundo

Históricamente, los libros sobre programación comienzan con un ejemplo
que imprime en la consola la frase: \"Hello World!\". Aunque el programa
no es muy interesante por sí mismo es útil para probar el compilador y
verificar que todo funciona correctamente. Esta tradición fue iniciada a
finales de los años setentas, por un experto en ciencias de la
computación llamado Brian Kernighan quien trabajaba para los
laboratorios Bell. En 1978 Brian Kernighan y Dennis Ritchie, escribieron
el libro: \"El lenguaje de programación C\", uno de los libros más
influyentes sobre programación. En este libro se escribió por primera
vez, el ejemplo que imprime en consola el famoso mensaje:

``` C++
#include <stdio.h>

// Programa en C que muestra el mensaje "hello, world"
main() 
{
    printf("hello, world\n");
}
```

Curiosamente el programa en C, escrito en 1978 aun compila en un
compilador moderno de C++ con una advertencia: \"ISO C++ prohíbe la
declaración de la función `main()` sin un tipo de retorno\". Este
ejemplo muestra uno de los primeros objetivos del lenguaje C++, mantener
compatibilidad retroactiva con C.

Una vez que tenemos escrito un archivo con código fuente, el siguiente
paso es compilarlo.

### Compilando un programa en C++

En algunos lenguajes de programación se puede ejecutar el código fuente
de un programa directamente sin compilarlo. En C++ no. C++ es un
lenguaje compilado. Esto significa que para poder ejecutar un programa
es necesario trasladar las instrucciones a código máquina. Este proceso
de traducción se lleva a cabo por un programa llamado compilador, que
recibe como entrada el código fuente de un programa. Para que este
proceso se lleve a cabo, el código fuente debe estar libre de errores.

Para compilar el código fuente, basta con incluir el nombre del archivo:

``` console
$ c++ hola.cpp 
```

Si no se ha cometido ningún error, como la omisión de un carácter o
escribir algo de forma incorrecta, la compilación se hará sin emitir
ningún mensaje y el compilador creará un archivo ejecutable. Si usas un
entorno de desarrollo, solo necesitas dar clic en el botón de compilar y
el proceso de compilación será automático.

> [!WARNING]
>
> Todos los programas ejecutables deben tener al menos una función llamada
> `main()`. Si intentas compilar un programa sin esta función, el
> compilador genera un error. Hay excepciones, por ejemplo cuando se
> compilan bibliotecas de vínculos dinámicos o bibliotecas estáticas.

El compilador que usamos en este ejemplo es `gcc`, que contiene una
utilidad llamada `c++` (o `g++`). Podemos activar las advertencias
lanzadas por el proceso de compilación usando la opción `Wall`:

``` console
$ c++ -Wall hola.cpp 
```

La opción `-Wall` imprime en consola todas las advertencias emitidas por
el compilador. Estos son algunos indicadores comunes:

- `-Wall` Habilita los mensajes de advertencia.
- `-Wextra` Habilita mensajes de advertencia extra. Puede ser muy
  extenso.
- `-pedantic` Muestra advertencias cuando el código viola el estándar
  elegido.
- `-Wconversion` Habilita las advertencias sobre la conversión
  implícita.
- `-v` Imprime información sobre el proceso de compilación.
- `-std` Usa un estándar especifico. Por ejemplo para usar el 20,
  -std=c++20.

Todos los ejemplos de este capítulo se han compilado usando el
compilador gcc. El proceso de compilación y las opciones pueden ser
diferentes con otros compiladores.

Si no quieres que el programa ejecutable se llame `a` (que es el nombre
usado por defecto), puedes especificar el nombre de salida usando la
opción `-o`, de esta forma:

``` console
$ c++ -Wall -o hola hola.cpp 
```

Esta opción produce un ejecutable llamado `hola`. La extensión depende
de la plataforma, en sistemas \"Windows\" produce un archivo `.exe`,
otros sistemas operativos como \"Linux\" no necesitan una extensión.

Para ejecutar el programa es necesario usar la consola:

``` console
$ ./hola
```

o:

``` console
$ hola.exe
```

Este programa muestra por salida, el clásico mensaje: `¡Hola, mundo!`.

> [!TIP]
> 
> Tradicionalmente en C++ (al igual que C) los archivos de encabezado han
> sido la unica forma de crear y compilar programas. Esto cambio con C++
> 20 que agrega `módulos</capitulos/modulos>`{.interpreted-text
> role="doc"} al lenguaje como una forma alternativa para compilar
> programas.

Aunque el programa puede parecer bastante sencillo para una generación
cada vez más sofisticada de programadores, el ejemplo sigue siendo útil
y emocionante para todo aquel que se inicia por primera vez en la
programación. Hoy como a finales de los años setentas estas son las
primeras palabras \"pronunciadas\" por una máquina y por código que tú
mismo acabas de crear.

### El proceso de enlazado

Un programa por lo general consiste de varias partes separadas y
desarrolladas por diferentes personas. Por ejemplo el programa
`hola.cpp` consiste de código que escribimos y de una biblioteca
estándar llamada `iostream`. Estas partes llamadas *unidades de
traducción* deben ser compiladas y el objeto resultante debe ser
enlazado para crear un archivo ejecutable. El programa que realiza esta
tarea se llama: **enlazador** y al proceso que realiza esta tarea se le
conoce como **enlazado**.

> [!WARNING]
> **Portabilidad**
> 
> Un programa ejecutable se crea para un sistema o hardware en específico.
> No es portable. Es decir no se puede crear un ejecutable en Mac y usarlo
> en un sistema Windows. Cuando se habla sobre portabilidad en C++, esta
> se refiere al código fuente, no al código ejecutable. El código fuente
> puede ser compilado en distintos sistemas operativos, pero el código
> ejecutable y el código objeto solo funcionan en la plataforma en que se
> compilan.

En el enlazado se ensambla y vincula el código objeto junto con las
bibliotecas y archivos separados para producir el código final, un
archivo binario o una biblioteca. El enlazado permite a los
programadores compartir sus bibliotecas y funciones sin tener la
necesidad de liberar su código fuente original.

Si eres curioso, puedes generar el código ensamblador producido por el
compilador usando algo como esto:

``` console
c++ -S hola.cpp  
```

La opción `S` le dice al compilador, que compile el código fuente, pero
que no lo ensamble, ni lo enlace. Esta opción da como resultado un
archivo con extensión `.s` o `.asm`. Puedes abrir el archivo resultante
en un editor de texto y mirar el código (Este es un fragmento).

``` C++
$ cat hola.s|more

    .file   "hola.cpp"
    .section .rdata,"dr"
__ZStL19piecewise_construct:
    .space 1
.lcomm __ZStL8__ioinit,1,1
    .def    ___main;    .scl    2;  .type   32; .endef
LC0:
    .ascii "Hola, mundo\0"
    .text
    .globl  _main
    .def    _main;  .scl    2;  .type   32; .endef
_main:

-- Mas --
```

Si eres curioso, puedes generar el código objeto usando algo como esto:

``` console
c++ -c hola.cpp -o hola.o 
```

La opción `c` le dice al compilador que compile y ensamble el código
fuente, pero no lo enlace. Esta opción da como resultado un archivo
objeto con extensión `.o`. Un archivo objeto es un archivo binario, pero
no es ejecutable. Aunque puede convertirse en ejecutable enlazándolo
directamente:

``` console
c++ hola.o -o hola 
```

Muchos programas y bibliotecas de desarrollo incluyen código objeto
listo para ser enlazado dentro de un programa.

## Errores

Tarde o temprano ocurren errores. Una palabra mal escrita o una
condición mal ejecutada pueden ocasionar que un programa no compile o
que no funcione según lo esperado. En la primera, el compilador puede
ayudarte, en la segunda solo la experiencia.

### Errores de sintaxis

Modifica el ejemplo anterior e introduce algunos errores a \"propósito\"
y observa como el compilador se \"queja\" por errores de sintaxis o por
errores de tipo.

``` C++
#include <iostream>

// Un programa con "errores" de sintaxis    
int main() 
{
    std::cout << "¡Hola, mundo!\n"
    return 0;
}
```

En este ejemplo se ha omitido el punto y coma al final de la palabra
\"Hola, mundo\". Al compilar el programa se genera un error como el
siguiente:

``` C++
$ c++ -Wall -o error error.cpp

error.cpp: En la función ‘int main()’:
error.cpp:6:33: error: expected ‘;’ before ‘return’
    6 |    std::cout << "¡Hola, mundo\n!"
      |                                  ^
      |                                  ;
    7 |    return O;
      |    ~~~~~~  
```

En este caso el compilador marca la función, la línea y el *token* que
genera el error de sintaxis. Un error de sintaxis se refiere a una
palabra clave mal escrito o a la falta de algún signo de puntuación. El
caso más común es olvidar poner el punto y coma al final de una
declaración.

Existen otros errores que no se aprecian a simple vista. Como en este
ejemplo:

``` C++
#include <iostream>

// Error: cout no fue declarado en este ámbito
int main() 
{
    cout << "¡Hola, mundo!\n";
    return 0;
}
```

En este ejemplo el compilador se queja de que \'cout\' no fue declarado
en este ámbito.

``` C++
$ c++ -Wall -o error2 error2.cpp

error2.cpp: En la función ‘int main()’:
error2.cpp:6:4: error: ‘cout’ was not declared in this scope; 
                                    did you mean ‘std::cout’?
    6 |    cout << "¡Hola, mundo!\n";
      |    ^~~~
      |    std::cout
En el fichero incluido desde error2.cpp:1:
```

Un error que sucede con frecuencia por olvidar declarar el espacio de
nombres al que pertenece un objeto, en este caso `cout` pertenece al
espacio de nombres `std`.

``` C++
// cout es un objeto global declarado en el espacio de nombres std
std::cout << "Hola, mundo";
```

Cada vez que modifiques el código fuente de un programa es necesario
compilar de nuevo el código para que el programa ejecutable refleje los
cambios y se corrijan los errores.

> [!TIP]
> 
> **¿Que es un ámbito?**
> 
> En C++ los nombres usados como identificadores únicamente pueden ser
> usados en ciertas regiones de un programa. Esta área se llama ámbito. El
> ámbito determina \"el tiempo de vida\" de un identificador (o nombre).
> Por ejemplo si un nombre se declara dentro de una función, solo se puede
> usar dentro de la función, es decir su ámbito es local, por otro lado;
> si se declara fuera de una función entonces su ámbito es global respecto
> a la función. Por lo general, las llaves definen el alcance léxico de un
> nombre usado como identificador.

En lugar de usar un espacio de nombres directamente se puede usar la
palabra clave `using` para introducir el espacio de nombres en el ámbito
actual, de esta forma.

``` C++
#include <iostream>          // cout
using namespace std;         // Un espacio de nombres global

// Usa el espacio de nombres std de forma global
int main()
{
    cout << "¡Hola, mundo!\n";
    return 0;
}
```

Esto mejora la legibilidad del programa, sin embargo esta práctica es
poco recomendada porque aglutina el espacio de nombres del programa. Ya
que la mayoría de las utilidades de C++ están definidas dentro de este
espacio de nombres global. Una alternativa es usar los espacios de
nombres de forma local. Por ejemplo:

``` C++
#include <iostream>            // cout

// Usando el espacio de nombres std de forma local
int main()
{
    using namespace std;     
    cout << "¡Hola, mundo!\n";

    return 0;
}
```

Existe otra forma de usar objetos declarados en un espacio de nombres y
es usando la palabra clave `using` para declarar una función en
específico:

``` C++
#include <iostream>        // cout, cin
using std::cout;
using std::cin; 

// Usando using para acceder a un objeto
int main()
{
    cout << "¡Hola, mundo!\n";
    cin.get();            // Mantiene la consola abierta
    return 0;
}
```

En este caso se usa la palabra clave `using` y el operador de resolución
de alcance para declarar al inicio del programa que queremos usar las
funciones `cout` y `cin` en el ámbito actual. Por cierto, el ejemplo usa
al final una línea como esta: `cin.get()`, que simplemente mantiene la
consola abierta, a la espera de que se presione cualquier otra tecla. La
función `cin` es la contraparte de `cout` que lee de la entrada
estandar, al igual que `cout` es un objeto que tiene funciones miembro.

> [!TIP]
> 
> **¿Que es un espacio de nombres?**
> 
> Un espacio de nombres es parte de un programa en el cual ciertos nombres
> o identificadores son reconocidos. El caso más común es el espacio de
> nombres `std`. Varias utilidades de la librería `iostream` están
> declarados dentro de este espacio de nombres como `cout`, `cin`, `endl`
> etc.


Un espacio de nombres funciona como un prefijo para las variables,
funciones o clases declaradas en su interior, de modo que para acceder a
una de esas \"entidades\" se tiene que usar un especificador de ámbito
`::`, para activar el espacio de nombres correcto. Los espacios de
nombres se crean para evitar colisiones entre nombres comunes
proporcionados por diferentes proyectos o bibliotecas.

La práctica más común y la más recomendada es usar el operador de
resolución de alcance, cuando se usan espacios de nombre, de esta forma:
`std::cout`.

## Interactuando con el usuario

El programa \"Hola mundo\" muestra un mensaje en la consola. Esa es su
salida. No lee, ni espera datos del usuario. Los programas reales
tienden a producir resultados basados en los datos que reciben como
entrada en lugar de hacer la misma cosa cada vez que se ejecuta el
programa. Por esta razón los programas reales involucran el uso de datos
y esperan la interacción del usuario.

Por ejemplo, podemos crear un programa que interactúe con el usuario de
esta forma:

``` C++
#include <iostream>                // cout
#include <string>                  // string

// Programa que obtiene datos del usuario usando la entrada estándar
int main()
{
    std::string nombre;
    std::cout << "¿Cuál es tu nombre?: ";
    std::cin  >> nombre;

    std::cout << "Hola: " << nombre << '\n';
    std::cout << "Tu nombre tiene: " << nombre.size() << " letras\n";

    return EXIT_SUCCESS;          // El programa termino con éxito
}
```

La lógica del programa se lleva a cabo dentro de la función `main()`. La
función `main()` le pide al usuario su nombre y luego lo concatena con
una frase predefinida para mostrar un mensaje completo.

Esto es lo que hicimos:

- El programa inicia incluyendo dos bibliotecas:

  ``` C++
  #include <iostream>
  #include <string>
  ```

  La biblioteca `iostream` define los objetos `cin` y `cout` que se
  encargan de recoger y mostrar los datos obtenidos del usuario usando
  la consola. La biblioteca `string` es una clase que proporciona un
  tipo de dato llamado `string`. Un `string` es simplemente una cadena
  de texto.

- El código dentro de la función `main` define una variable vacía
  llamada `nombre`. El tipo `string` se usa para trabajar con \"cadenas
  de texto\". y pertenece al espacio de nombres `std`.

  ``` C++
  std::string nombre;
  ```

- El objeto `cout` se usa para mostrar texto en la consola. En este caso
  le pregunta al usuario su nombre y el objeto `cin` lee el texto
  introducido por el usuario en la consola, usando el operador de
  extracción `>>`.

  ``` C++
  std::cout << "¿Cuál es tu nombre? ";      
  std::cin >> nombre;
  ```

- Los objetos `string` tienen funciónes como `size()` o `length()` que
  devuelven el número de caracteres que contiene la cadena. El carácter
  `\n` al final, inserta un salto de línea.

  ``` C++
  std::cout << "Tu nombre tiene: " << nombre.size() << " letras\n";
  ```

Por cierto, la salida de `cout` se puede encadenar, usando el operador
de inserción `<<` para mostrar varias líneas, siempre y cuando no se use
el punto y coma para terminar la declaración.

Para terminar el programa de forma exitosa, la función `main` devuelve
`EXIT_SUCCESS` un valor constante que equivale a 0. Para terminar un
programa con un tipo de error o una falla se usa el valor `EXIT_FAILURE`
que equivale a 1.

Ahora podemos compilar el código fuente de esta forma:

``` console
c++ -Wall -o nombre nombre.cpp 
```

y jugar con la salida del programa. No es mucho, pero el programa ahora
puede leer y escribir en la salida estándar (la consola).

### Entrada de datos

El objeto `cin` es el encargado de recoger el flujo de datos de la
entrada estándar, `cin` usa el operador de extracción de datos `>>`.
Puedes pensar en `cin` como en la contraparte de `cout`. El operador de
inserción `<<` y el operador de extracción `>>` de datos son ejemplos de
la sobrecarga de operadores (operadores que pueden hacer varias tareas).

Usando `cin` se puede extraer más de un dato de una misma línea. Por
ejemplo.

``` C++
#include <iostream>                 // cout
#include <string>                   // string

// Programa que extrae varios datos de la entrada estándar
int main()
{
    std::string nombre;
    std::string apellido;    
    std::cout << "¿Cuál es tu nombre y tu primer apellido?: ";
    std::cin  >> nombre >> apellido;

    std::cout << "Hola: " << nombre + " " + apellido << '\n';
    std::cout << "Tu nombre completo tiene: " 
              << nombre.size() + apellido.size() 
              << " letras" << std::endl;

    return EXIT_SUCCESS;           // El programa termina con exito
}
```

Esta vez el programa le pregunta al usuario su nombre y su apellido.
Luego los concatena, y muestra el resultado en la consola. Los objetos
`string` se pueden concatenar usando el operador `+`.

Al final, se usa el manipulador `endl`. Este manipulador de flujo se
encarga de insertar un salto de línea y se encarga de vaciar el *búfer*
de la consola. El objeto `cin` usa como separador el espacio en blanco,
así que cada vez que encuentra un espacio en blanco, toma la palabra y
la agrega a la variable previamente definida.

#### La función getline

Para leer una línea completa se puede usar la función `getline()`. A
diferencia de `cin` puede leer una línea de texto completa.

``` C++
#include <iostream>           // cout
#include <string>             // string

// Programa que lee una línea completa usando getline
int main()
{ 
    std::cout << "¿Cuál es tu nombre? ";
    std::string nombre;    
    std::getline(std::cin, nombre);

    std::cout << "Escribe un mensaje: ";     
    std::string mensaje;    
    std::getline(std::cin, mensaje);

    std::cout << "¿Cuál es tu edad? ";
    int edad;    
    std::cin >> edad;

    std::cout << "Mi nombre es: " << nombre << "\nTengo: " 
              << edad << " años\n" << mensaje << '\n';

    return EXIT_SUCCESS;
}
```

En este ejemplo en lugar de usar `cin`, se usa `getline`. La función
`getline()` es una utilidad incorporada en el lenguaje que se utiliza
para leer una línea de texto completa de la entrada estándar. La función
espera tres argumentos:

``` C++
std::getline(std::cin, nombre, '\n');
```

un flujo de entrada (tomado de `cin`), una cadena de texto (la variable
que definimos para guardar el texto) y opcionalmente un separador usado
como límite de la cadena, su valor predeterminado es el salto de línea:
`\n`.

El siguiente ejemplo inicia un REPL interactivo que espera la entrada
del usuario para imprimirla usando un bucle `while`.

``` C++
#include <iostream>             // cout
#include <string>               // string 

// Un REPL interactivo 
int main()
{
    std::cout << "REPL interactivo\n";
    std::cout << "Escribe expresiones como:\n";
    std::cout << "Hola mundo\n";
    std::cout << "Escribe 'salir' para terminar.\n";

    std::string linea;
    while (true) {
        std::cout << "<<< ";
        std::getline(std::cin, linea);
        if (linea == "salir") {
            break;
        }

       // Imprime la lí­nea introducida 
       std::cout << linea << std::endl;
    }

    return EXIT_SUCCESS;
}
```

En este caso, cada que el usuario introduce una línea de texto, el
programa la replica usando la función `getline` para leer `cout` para
escribir. Si el usuario introduce la palabra `salir` el programa
termina.

En programación, REPL significa Leer, Evaluar, Imprimir y Bucle
(Read-Eval-Print Loop). Un repl es un entorno de programación
interactivo donde se pueden ingresar comandos, el sistema los evalúa,
muestra el resultado y luego repite el proceso, esperando más comandos.

## ¿Qué son las funciones?

Todo programa en C++ tiene al menos una función, tal es el caso de la
función `main()`, pero hasta los programas más triviales definen
funciones adicionales. Una función es un **bloque de código con un
nombre** que se puede invocar o ejecutar desde un lugar diferente al que
fue declarada una o varias veces. Las funciones no solo realizan
cálculos también devuelven valores. Los valores se pueden guardar o se
pueden usar como entrada para funciones y clases.

### Usando funciónes

El siguiente ejemplo muestra un algoritmo en pseudo-código que calcula
el tiempo de vida que le queda a una persona.

``` C++
Obtener edad;

si la edad es menor de 85
    resultado = 72 - (edad * 80 / 100);
si no 
    resultado = 22 - (edad * 20 / 100);
```

El algoritmo establece que la esperanza de vida de una persona es de 72
años, menos el `80%` de su edad actual, siempre y cuando tenga menos de
85 años. Si tiene más de 85 años, puede contar con vivir 22 años más,
menos el `20%` de su edad actual. La formula usa un método heurístico
que arroja una estimación razonable de la esperanza de vida y se basa en
datos estadísticos obtenidos por el gobierno de Estados Unidos. El
algoritmo esta basado en \"La vida es Matemática\" de John Allen Paulos.
Por ejemplo, si una persona tiene 60 años, una 20 y otra 90. Haciendo
las substituciones tenemos:

``` C++
72 - (60 * 80 / 100) = 24 años
72 - (20 * 80 / 100) = 56 años
22 - (90 * 20 / 100) =  4 años 
```

Una persona de 60 años tiene una esperanza de vida de 24 años, una de 20
le quedan 56 y una de 90 aún puede contar con vivir 4 años más. Podemos
convertir el pseudo-código en una función substituyendo los valores por
variables:

``` C++
// Función que calcula el tiempo de vida que le queda a una persona.
unsigned int esperanza_de_vida(unsigned int edad)
{
    // Si la edad es menor de 85 (ejecuta el bloque y devuelve el resultado)
    if (edad < 85) {
        return 72 - (edad * 80 / 100);
    }        
    // Si no, ejecuta esta línea
    return 22 - (edad * 20 / 100);
}
```

En este ejemplo, el nombre de la función es `esperanza_de_vida`.

- La función devuelve como resultado un tipo de dato llamado
  `unsigned int` (Un entero sin signo o positivo, que equivale a
  `std::size_t`).
- La función acepta como entrada (dentro de los paréntesis) un entero
  sin signo llamado: `edad`. Este valor es el argumento de la función.
- Dentro del cuerpo de la función (el código entre llaves `{ }`), se
  realizan los cálculos. Si la edad de la persona es menor de 85 años se
  ejecuta el contenido del bloque `if`, si no; la ejecución sigue el
  flujo normal y ejecuta la última línea.
- La sentencia `return` devuelve un valor al programa que la invoca y
  termina la ejecución de la función, no importa sin aún quedan
  declaraciones. Una sentencia `return` puede aparecer varias veces en
  una función.

Para usar la función, basta con **invocarla** desde la función `main()`
con algún parámetro (del mismo tipo).

``` C++
#include <iostream>                             // cout

// Calcula el tiempo de vida que le queda a una persona
unsigned int esperanza_de_vida(unsigned int edad)
{
    if (edad < 85) {
        return 72 - (edad * 80 / 100);
    }

    return 22 - (edad * 20 / 100);
}

// Programa que devuelve el tiempo de vida que le queda a una persona 
int main()
{
    std::cout << esperanza_de_vida(60) << '\n'   // 24
              << esperanza_de_vida(20) << '\n'   // 56
              << esperanza_de_vida(90) << '\n';  //  4

    return EXIT_SUCCESS;
}
```

En este ejemplo, el valor que devuelve la función `esperanza_de_vida()`
es un tipo entero (positivo) que es enviado a la salida estándar usando
el operador de salida `<<`. El operador de salida permite encadenar
varios flujos, mientras no encuentre el punto y coma. El carácter `'\n'`
agrega un salto de línea.

### Declaración y definición de funciones

Para invocar una función es necesario que la función este declarada o
definida antes de usar el nombre de la función de lo contrario se genera
un error en tiempo de compilación. Una **declaración** proporciona
información al compilador sobre las características de un tipo o una
función pero no define su código. Una **definición** por otra parte,
implementa el código, variables, o algún tipo de código.

``` C++
#include <iostream>                                // cout

// Calcula el tiempo de vida que le queda a una persona (Declaración)
unsigned int esperanza_de_vida(unsigned int edad);

// Devuelve el tiempo de vida que le queda a una persona.
int main ()
{
    int edad{0};
    std::cout << "Introduce tu edad: ";
    std::cin >> edad;  
    std::cout << "Tu esperanza de vida de vida es de "
              << esperanza_de_vida(edad) << " años\n";

    return EXIT_SUCCESS;
}

// Calcula el tiempo de vida que le queda a una persona (Definición)
unsigned int esperanza_de_vida(unsigned int edad)
{
    if (edad < 85){
        return 72 - (edad * 80 / 100);
    }

    return 22 - (edad * 20 / 100);
}
```

Una declaración de función debe terminar con punto y coma y solo puede
aparecer antes de que se llame a la función.

``` C++
// Prototipo de función
unsigned int esperanza_de_vida(unsigned int edad);
```

A la declaración de función también se le conoce como el **prototipo**
de función. Un prototipo es un modelo limitado de una entidad más
completa que se define más adelante. En el caso de las funciones, el
cuerpo de la función es la entidad completa. Cuando se define una
función se coloca el cuerpo de la función entre llaves.

### Comprobando la entrada del usuario

Aunque el programa funciona a primera vista tiene un error grave. No
maneja los casos extremos. Qué pasa si se introduce un valor negativo o
un valor muy grande, después de todo una persona solo puede tener una
edad que este dentro de cierto rango. Por ejemplo entre 0 y 100 años
(quizás 110). Cuando se esperan datos de la entrada estándar, la regla
común dicta \"Nunca confíes en los datos obtenidos del usuario.\"

``` C++
#include <iostream>             // cout

// Calcula el tiempo de vida que le queda a una persona
unsigned int esperanza_de_vida(unsigned int edad);

// Calcula el tiempo de vida que le queda a una persona.
int main ()
{
    int edad{0};              // Espera un tipo int
    std::cout << "Introduce tu edad: ";
    std::cin >> edad;

    // Si el valor es menor que 0 o mayor que 110 el programa termina
    if (edad < 0 || edad > 110) {
        std::cout << "Edad no válida";    
        return EXIT_FAILURE;  // (1) El programa termina prematuramente 
    }

    std::cout << "Tu esperanza de vida de vida es de "
              << esperanza_de_vida(edad) << " años\n";

    return EXIT_SUCCESS;     // (0) El programa termina de forma exitosa
}

// Calcula el tiempo de vida que le queda a una persona 
unsigned int esperanza_de_vida(unsigned int edad)
{
    if (edad < 85){
        return 72 - (edad * 80 / 100);
    }

    return 22 - (edad * 20 / 100);
}
```

En este ejemplo, se agrega una condición `if` a la función `main()` para
asegurar que el programa solo acepte valores entre 1 y 110 (años), de
esta forma si el usuario introduce un valor negativo o un valor muy
grande el programa termina mostrando un mensaje.

> [!WARNING]
> 
> **Comparación de tipos enteros**
> 
> La razón por la que el programa espera un tipo `int` y no un tipo
> `unsigned int` (o positivo) tiene que ver con la comprobación, un tipo
> `unsigned int` no se puede comparar con valores negativos, porque el
> valor mínimo que puede almacenar es 0 y cuando se le asigna un valor
> negativo se produce un desbordamiento.

El uso de valores constantes como `EXIT_SUCCESS` y `EXIT_FAILURE`
describen lo que pasa en el programa. Si el usuario introduce datos
incorrectos el programa termina prematuramente, de lo contario el
programa termina de forma exitosa. Estos valores equivalen a 0 y 1
respectivamente. Estos nombres están definidos en el archivo de cabecera
`<cstdlib>`, y son importados por la cabecera `<iostream>`.

## Argumentos para la ejecución de un programa

La función `main()` puede usarse de dos formas. Como lo hemos venido
haciendo hasta ahora, sin argumentos:

``` C++
// La función main no espera argumentos
int main();
```

y con argumentos:

``` C++
// La función main espera al menos un argumento
int main(int argc, char *argv[]);
```

La función `main()` puede recibir argumentos a través de la línea de
comandos. El lenguaje define `argc` y `argv` como los nombres que toman
los argumentos que recibe:

1.  `argc` Es un numero entero que contiene el total de argumentos del
    arreglo llamado `argv`. El parámetro `argc` siempre es mayor o igual
    que 1.
2.  `argv` Es un arreglo de tipo `char` (una cadena de caracteres que
    termina con un carácter nulo) y representa los argumentos de la
    línea de comandos especificados por el usuario del programa.

Por convención, `argv[0]` es el comando con el que se invoca el programa
y `argv[1]` es el primer argumento pasado a través de la línea de
comandos. El último argumento es `argv[argc-1]` y `argv[argc]` siempre
es un valor nulo.

El siguiente programa imprime los dos argumentos pasados a través de la
línea de comandos.

``` C++
#include <iostream>                               // cout

// Como pasar argumentos a la función main
int main(int argc, char *argv[])
{
    // argc indica el número de argumentos pasados a la función
    if (argc != 3) {
        std::cerr << "¡Introduce dos argumentos!" << '\n';
        return EXIT_FAILURE;  
    }

    // El primer valor de argv es el nombre y la ruta del ejecutable
    std::cout << "Programa .........: " << argv[0] << '\n'
              // El segundo valor de argv es el primer argumento
              << "Argumento 1.......: " << argv[1] << '\n'
              // El tercer valor de argv es el segundo argumento
              << "Argumento 2.......: " << argv[2] << '\n';

    return EXIT_SUCCESS;
}
```

El primer argumento llamado `argc` indica la cantidad de parámetros
recibidos por la función `main`, mientras que la cadenas de caracteres
`argv[i]` contienen los parámetros introducidos. Los argumentos se pasan
desde la línea de comandos como si fueran una lista de caracteres, al
estilo C. Los elementos del arreglo se acceden mediante un índice que
inicia con el número 0.

Suponiendo que el programa compilado se llame `args` entonces:

``` Console
$ ./args 100 200
```

o:

``` Console
$ args.exe 100 200
```

imprimirá:

    Programa .........: args
    Argumento 1.......: 100
    Argumento 2.......: 200

Si no se introducen argumentos, entonces el programa muestra el mensaje
de error: \"¡Introduce dos argumentos!\".

> [!TIP]
>
> **El argumento argc**
> 
> El argumento `argc` es una cadena de caracteres que siempre es igual o
> mayor a uno, ya que `argv[0]` contiene la llamada al ejecutable (el
> nombre y la ruta). Y los parámetros que recibe la función comienzan a
> partir de `argv[1]` y siempre son en total `argc - 1`.

Los nombres `argc` y `argv` son solo convenciones prácticas, puedes
asignarles el nombre que quieras (aunque no es recomendable hacerlo).

### Paso de argumentos a programas

Veamos como pasarle argumentos al programa que creamos en el ejemplo
anterior. Esta es la versión modificada que muestra como pasar datos
desde la línea de comandos a un programa.

``` C++
#include <cstdlib>              // EXIT_SUCCESS, EXIT_FAILURE 
#include <print>                // println

// Devuelve el tiempo de vida que le queda a una persona
unsigned int esperanza_de_vida(unsigned int edad);

// Programa que calcula el tiempo de vida que le queda a una persona
// Compilar con -std=c++23 (gcc 14.1 soporta print)
int main (int argc, char *argv[])
{
    // Si no hay argumentos, el programa termina
    if (argc == 1) {
        std::print("¡Introduce tu edad!");
        return EXIT_FAILURE;
    }

    // Convierte el valor recibido en un tipo entero
    int edad = atoi(argv[1]);

    // Si la edad esta fuera del rango el programa termina
    if (edad < 0 || edad > 110) {
        std::print("Edad fuera de rango (0-110)");
        return EXIT_FAILURE;
    }

    // Imprime los resultados
    std::println("Esperanza de vida {} años.", esperanza_de_vida(edad));

    return EXIT_SUCCESS;
}

// Devuelve el tiempo de vida que le queda a una persona
unsigned int esperanza_de_vida(unsigned int edad)
{
    if (edad < 85){
        return 72 - (edad * 80 / 100);
    }
    return 22 - (edad * 20 / 100);
}
```

Esto es lo que hicimos. La función `main()` inicia comprobando el número
de argumentos pasados a través de la línea de comandos. Si no hay
argumentos el programa termina mostrando un mensaje de error.

``` C++
if (argc == 1) {
    std::print("¡Introduce tu edad!");
    return EXIT_FAILURE;
}
```

Si el usuario introduce un argumento usa la función `atoi()`. La función
`atoi()` convierte el valor recibido (el valor del arreglo) en un número
entero.

``` C++
int edad = atoi(argv[1]);
```

La función `main()` usa una condición extra para evitar calcular edades
fuera del rango 0-110 y para comprobar que los valores recibidos sean
valores numéricos. Si el valor no es un número positivo o un número
menor que 110, el programa termina mostrando un mensaje de error.

> ``` C++
> if (edad < 0 || edad > 110) {
>     std::print("Edad fuera de rango (0-110)");
>     return EXIT_FAILURE;
> }
> ```

Una vez comprobado el rango de edad, el programa imprime el resultado
usando la función `println()`.

``` C++
std::println("Esperanza de vida {} años", esperanza_de_vida(edad));
```

Las funciones `print()` y `println()` están definidas en el archivo de
cabecera `<print>` y fueron agregadas a C++ 23, como apoyo para las
bibliotecas de salida. Estas funciones se usan para enviar texto
formateado a la salida estándar.

Para compilar el programa es necesario usar la opción \"-std=c++23\":

``` Console
$ c++ esperanza.cpp - Wall -o esperanza -std=c++23
```

Y esta es la forma en que se pasan argumentos mediante la línea de
comandos al programa:

``` C++
$ ./esperanza 20 
```

o

``` C++
$ esperanza.exe 20 
```

y esta es la salida: \"Esperanza de vida: 56 años.\"

### La función print()

Tradicionalmente, C++ usa `cout` para escribir flujos de caracteres a la
salida estándar y usa el operador de inserción `<<` para manipular el
flujo de salida usando manipuladores (como `endl`). Sin embargo C++ 23
agrega una nueva función al lenguaje llamada `print()`. Esta función (al
igual que `cout`) se usa para imprimir cadenas de texto en la salida
estándar:

``` C++
#include <print>                                // print

// Imprime una cadena de texto
int main()
{
   std::print("¡Hola mundo!");                  // ¡Hola mundo! 
   return EXIT_SUCCESS;
}
```

Este programa muestra por salida el clásico programa \"¡Hola mundo!\".

En el fondo la función `print()` es simplemente un atajo que usa la
biblioteca `<format>` para imprimir cadenas de texto con formato.

``` C++
#include <print>                                 // print

// Imprime una cadena de texto usando marcadores de posición
int main()
{
   std::print("C Mas {}\n", "Moderno");          // C Mas Moderno
   return EXIT_SUCCESS;
}
```

La función `print()` soporta argumentos posicionales y argumentos por
indice:

``` C++
#include <print>                                  // print

// Imprime una cadena de texto
int main()
{
   std::print("C {0} {1}\n",  "Mas", "Moderno");  // C Más Moderno
   return EXIT_SUCCESS;                             
}
```

La función `println` agrega un salto de línea al final de la cadena de
texto.

## ¿Qué son las clases?

Los programas tienden a crecer y a volverse complejos con el tiempo. Una
de las formas de manejar la complejidad, es usando clases. Una clase es
una **estructura de datos** que define las propiedades (variables) y los
comportamientos (funciones) de un tipo de objeto. En C++, una clase
representa un tipo y cada tipo mantiene su propia representación que le
dice al compilador como almacenar los valores y las operaciones
disponibles para manipular esos valores.

### Creando una clase

Una clase encapsula funciones y variables en una misma entidad. Las
funciones pueden usar las variables declaradas dentro de la clase. Por
ejemplo, siguiendo con la función `esperanza_de_vida`, creada en la
sección anterior, podemos hacer que el programa no solo calcule el
tiempo de vida que le queda a una persona, sino que también nos diga el
año en que esto ocurrirá. Para hacer esto necesitamos agregar otra
función que devuelva el año actual usando la biblioteca `<chrono>`.

``` C++
    #include <chrono>         // year_month_day, floor, system_clock
    #include <iostream>       // cout

    // Clase que calcula el tiempo de vida que le queda a una persona
    class Esperanza {
    public:
        // El constructor de la clase. 
        Esperanza(int edad_actual) : edad{edad_actual} { }

        // Función que calcula la esperanza de vida. Devuelve un tipo double.
        double vida()  
        {
            if (edad < 85) {
                return 72.0 - (edad * 80.0) / 100.0;
            }

            return 22.0 - (edad * 20.0) / 100.0;
        }

        // Función que devuelve el año (un tipo entero) en que estadísticamente 
        // morirá una persona. Usa la biblioteca chrono para calcular la fecha.
        std::size_t muerte() 
        {
            using namespace std::chrono;
            year_month_day fecha = floor<days>(system_clock::now());

           // Al año actual le suma la esperanza de vida
           return static_cast<int>(fecha.year()) + vida();
        }

    private:
        int edad{0};      // Almacena la edad como un tipo entero 
    };

    // Usando una clase
    int main()
    {
        Esperanza esperanza{24};
        std::cout << esperanza.vida()   << '\n'     // 52.8
                  << esperanza.muerte() << '\n';    // 2076

        return EXIT_SUCCESS;
    }   
```

Para crear una clase se usa la palabra reservada `class` seguido del
nombre de la clase y llaves. Es un error de sintaxis omitir el punto y
coma final.

``` C++
class Esperanza {    
    // Datos y funciones mienbro
};
```

Dentro de las llaves se declaran los niveles de acceso y en cada nivel
de acceso se declaran los miembros que puede contener la clase. Los
miembros de una clase pueden ser funciones y variables:

``` C++
class Esperanza {
public:
    double vida();        // Una función mienbro
    std::size_t muerte(); // Una función mienbro 
private:    
    int edad{0};          // Los datos de la clase
};
```

Los niveles de acceso de una clase separan los datos que son privados y
las funciones públicas que permiten interactuar con la clase. Los
miembros públicos definen la interfaz de la clase mientras que los
miembros privados solo pueden ser accedidos por las funciones miembro
definidas dentro de la clase. En C++ a los valores de una clase se les
llama datos mienbro y a las operaciones funciones mienbro.

El programa usa la biblioteca `<chrono>` para calcular una fecha y
obtener el año actual. La biblioteca `chrono` contiene funciones para
manipular puntos en el tiempo y está definido en el espacio de nombres
`std::chrono`.

``` C++
#include <chrono>                     // year_month_day, system_clock 
#include <iostream>                   // cout

// Usa la biblioteca chrono's para calcular la fecha actual
int main()
{
    using namespace std::chrono;     // Declara un espacio de nombres
    year_month_day fecha = floor<days>(system_clock::now());

    // Imprime el día, el mes y el año actual
    std::cout << fecha.day() << " de " << fecha.month() 
              << " del " << fecha.year() << '\n';

    return EXIT_SUCCESS;
}
```

En este ejemplo, para facilitar el uso de la biblioteca `chrono` se
declara un espacio de nombres anidado dentro de la función `main()` para
no tener que escribir:

``` C++
// Devuelve el año, mes y el día actual
std::chrono::year_month_day fecha = 
   std::chrono::floor<std::chrono::days>(std::chrono::system_clock::now());
```

Esta es una de esas ocasiones en que declarar un espacio de nombres
ayuda a escribir menos código y vuelve el código fuente más legible. Si
queremos usar `print` en lugar de `cout` podemos escribir:

``` C++
std::print("{} de {} del {}", fecha.day(), fecha.month(), fecha.year());  
```

La función `print` al igual que `cout` sabe cómo formatear fechas y
puntos en el tiempo. Solo hay que compilar el programa con
`-std::c++23`.

### Instancias de clase

Para usar una clase es necesario crear una instancia para poder acceder
a los miembros públicos de la clase. Por ejemplo a la función `vida()`:

``` C++
Esperanza esperanza{24}; // El constructor espera un argumento de tipo int
esperanza.vida();        // Devuelve un tipo double: 52.8
```

o la función `muerte()`:

``` C++
esperanza.muerte();     // Devuelve un tipo unsigned int: 2076
```

Dentro de una clase, una función mienbro es como una función ordinaria.
Con la diferencia de que el ámbito de la función mienbro se limita al
bloque de la clase que la contiene y la notación de punto proporciona
acceso a los miembros públicos declarados dentro de la clase.

### Manejo de errores

Las clases también pueden manejar errores. El mecanismo proporcionado
por el lenguaje para este propósito son las **excepciones**. Por ejemplo
si queremos que la clase valide la entrada de datos, podemos hacer que
el constructor de la clase lance una excepción si los datos de entrada
no están en el rango de valores permitidos, en este caso entre 0 y 110
años.

``` C++
#include <stdexcept>     // invalid_argument

// Clase que calcula el tiempo de vida que le queda a una persona.
class Esperanza {
public:
    // El constructor de la clase.
    Esperanza(int actual) : edad(actual)
    {
        // Lanza una excepción si la edad es menor que 0 o mayor que 110.
        if (edad < 0 || edad > 110) {
            throw std::invalid_argument("Edad fuera de rango (0-110)");
        }
    }

    // Las funciones de la clase

private:
    // Los miembros de datos privados
};
```

En este ejemlo, el objeto lanzado por el constructor de la clase es un
tipo `invalid_argument` (argumento no válido) declarado en el archivo de
cabecera `<stdexcept>`.

Una excepción es un objeto que representa el problema que se ha
producido y que se lanza desde la parte del código donde se detecta el
error a otra parte del código que se encarga de manejarlo. Este lugar es
por lo general un bloque `try`. Esta es la sintaxis:

``` C++
// Si ocurre una excepción el bloque try lo atrapa y muestra un mensaje.
try {
    Esperanza esperanza{edad};
    std::cout << esperanza.vida() << '\n';
} catch (const std::invalid_argument& ex) {
    std::cerr << ex.what() << std::endl;
}  
```

En este caso el bloque `try` captura la excepción y el bloque `catch`
maneja el error. El control de excepciones se basa en tres palabras
clave:

1.  Cuando un programa encuentra un problema lanza una excepción usando
    la palabra clave `throw`.
2.  El bloque `try` identifica el bloque de código para el cual se
    activarán las excepciones. Debe ir seguido de uno o más bloques de
    captura.
3.  Un bloque de captura `catch` se agrega a la sección del programa
    donde se gestiona el error o se maneja el problema.

Las excepciones no previenen de errores pero ayudan en la gestión, el
manejo y el control. La idea detrás de las excepciones es sencilla. Si
una función encuentra un error que no puede manejar no debe devolver
normalmente un resultado, en su lugar; debe lanzar una señal indicando
qué algo salió mal.

### Declaración de clases

Al igual que las funciones una clase se puede declarar y se puede
definir. Para \"declarar\" una clase se utiliza el nombre de la clase y
las llaves. Dentro de la clase se declaran los datos y las funciones
mienbro. Como es usual con la declaraciones de funciones, el cuerpo de
la funciones mienbro se omite y la clase termina con punto y coma.

``` C++
#ifndef ESPERANZA_H_
#define ESPERANZA_H_ 
#include <chrono>                   // system_clock 
#include <stdexcept>                // invalid_argument

/*
* Clase que calcula el tiempo de vida que le queda a una persona.
*
* La clase usa un método heurístico que arroja una estimación 
* razonable de la esperanza de vida. Este método establece que 
* una persona puede vivir 72 años menos el 80% de su edad actual,
* si tiene menos de 85 años. Si tiene más de 85 años, puede contar 
* con vivir 22 años más, menos el 20% de su edad actual. El algoritmo 
* esta basado en "La vida es Matemática" de John Allen Paulos.
*/ 
class Esperanza {
public:
    // El constructor de la clase
    Esperanza(int edad_actual);

    // Función que calcula la esperanza de vida de una persona.
    double vida() const noexcept;

    // Función que devuelve el año en que morirá una persona
    std::size_t muerte() const noexcept;
private:
    int edad{0};
};

#endif // esperanza.h
```

Por convensión las declaraciones se colocan en archivos con extensión
`.h` Por ejemplo \"esperanza.h\". Cada archivo de cabecera tiene un
nombre único que incluye las llamadas *guardas*. Las guardas son macros
que agregan la cabecera al proceso de compilación, para asegurar que no
se haya agregado antes. Por que es un error de compilación agregar dos
veces el mismo nombre.

``` C++
#ifndef NOMBRE_H_
#define NOMBRE_H_
    // El código de la clase
#endif 
```

Usualmente, el nombre del archivo se basa en el nombre de la clase, solo
que en lugar de usar el punto para denotar la extensión, se usa el guion
bajo y el nombre siempre va en mayusculas.

> [!TIP]
>
>**Funciones que no levantan excepciones**
> 
> Las funciones que no levantan excepciones se declaran con la palabra
> clave `noexcept` (antes de las llaves). Esto le permite al compilador
> realizar algunas optimizaciones.

La declaración de funciones mienbro dentro de una clase, le dice al
compilador que existe una clase de un tipo específico, qué contiene
datos y funciones. También le dice que tan grande es la clase, para que
el compilador reserve el espacio necesario para crear objetos de ese
tipo. Hay que recordar que para usar un nombre este debe estar declarado
antes de su usarlo y cualquier nombre declarado debe estar definido en
alguna parte.

### Definición de clases

Podemos definir una función mienbro dentro y fuera de una clase. Cuando
definimos una función miembro fuera del cuerpo de la clase, la
definición debe corresponder con la declaración. Es decir, el tipo, la
lista de parámetros, y el valor devuelto deben corresponder con la
declaración colocada en el cuerpo de la clase. Las definiciones por lo
general se escriben en un archivo con extensión `cpp`. Por ejemplo:
`esperanza.cpp`:

``` C++
#include "esperanza.h"

// El constructor de la clase.
Esperanza::Esperanza(int actual) : edad(actual)
{
    // Lanza una excepción si la edad es menor que 0 o mayor que 110 años
    if (edad < 0 || edad > 110) {
        throw std::invalid_argument("Edad fuera de rango (0-110)");
    }
}

// Función que calcula estadísticamente la esperanza de vida de una persona
double Esperanza::vida() const noexcept
{
    if (edad < 85) {
        return 72.0 - (edad * 80.0) / 100.0;
    }
    return 22.0 - (edad * 20.0) / 100.0;
}

// Función que devuelve el año en que estadísticamente morirá una persona
std::size_t Esperanza::muerte() const noexcept
{
    using namespace std::chrono;
    year_month_day fecha = floor<days>(system_clock::now());
    return static_cast<int>(fecha.year()) + vida();
}
```

Los nombres de las funciones `Esperanza::vida` y `Esperanza::muerte`
usan el operador de resolución de alcance `::` para decirle al
compilador que estamos definiendo una función mienbro que está declarada
en el ámbito de una clase llamada `Esperanza`. Una vez que el compilador
ve el nombre de la función, el resto del código se interpreta como si la
función estuviera dentro de la clase.

### Usando un archivo de cabecera

El archivo que usa la clase debe importar el archivo con extensión
\".h\" al inicio del programa. Por ejemplo:

``` C++
#include <iostream>                         // cout
#include "esperanza.h"                      // Esperanza 

// Programa que calcula la esperanza de vida de una persona
int main()
{    
    std::cout << "+===========================================+\n"
              << "| Programa que calcula la esperanza de vida |\n"
              << "+===========================================+\n";
    int edad{0};              
    std::cout << " Introduce tu edad  : ";
    std::cin >> edad;

    // Si ocurre una excepción el bloque try lo atrapa, muestra 
    // un mensaje de error y termina la ejecución del programa.
    try {
        Esperanza esperanza{edad};
        double vida = esperanza.vida();
        std::cout << " Edad actual        : " << edad << " años\n"
                  << " Esperanza de vida  : " << vida << " años\n"
                  << " Fecha de muerte    : " << esperanza.muerte() << '\n'
                  << " Edad aproximada    : " << vida + edad << " años\n";
    } catch (const std::invalid_argument& ex) {
        std::cerr << ex.what() << std::endl; 
        return EXIT_FAILURE;            
    }    

    return EXIT_SUCCESS;
}
```

Para compilar el programa es necesario agregar los archivos con
extensión `.cpp` al proceso de compilación.

``` C++
$ c++ -Wall esperanza.cpp main.cpp -o esperanza -std=c++23
```

A grandes razgos esta es la forma en que se escribe y compila un
programa usando clases y funciones en C++. En el siguiente capítulo se
describe la estructura léxica del lenguaje.
