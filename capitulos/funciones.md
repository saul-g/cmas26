# Funciones

::: {.meta title="Funciones" capitulo="" description="Como usar funciones." keywords="Funciones, prototipos de funciones"}
:::

A medida que los programas crecen se vuelven grandes y complejos. Una
forma de manejar la complejidad es dividir los bloques de código en
partes pequeñas, manejables e independientes. En C++ esto se hace usando
funciones. Las funciones permiten organizar el código en bloques
reutilizables que ejecutan una tarea específica, lo que facilita la
modularidad y mejora la legibilidad de un programa.

Dividir un programa en funciones ayuda a reducir la complejidad de un
problema, reduciendo la redundancia y favoreciendo la reutilización de
código. Además las funciones permiten estructurar los cálculos y
operaciones de manera lógica y eficiente lo que ayuda a reducir el
tamaño de un programa.

Las funciones pueden devolver un valor después de su ejecución y se
pueden usar en expresiones como si fueran variables, también pueden
modificar variables a través de argumentos, sin devolver valores
directamente, produciendo lo que se conoce como efectos secundarios.

Este capítulo describe la forma de declarar, definir e invocar funciones
en C++.

## El origen de las funciones

Las funciones en programación surgieron como una evolución natural de la
necesidad de estructurar el código y reutilizar fragmentos de lógica en
programas de software. Uno de los primeros lenguajes en incorporar
funciones fue Fortran, creado por John Backus en la década de 1950.
Posteriormente, Dennis Ritchie introdujo el lenguaje C en los años 70,
donde las funciones jugaron un papel clave en la modularidad del código.
Más adelante, Stroustrup expandió el concepto a C++, agregando
programación orientada a objetos y permitiendo la sobrecarga de
funciones.

### La evolución de las funciones en C++

La evolución de las funciones en C++ ha sido bastante compleja, desde
sus orígenes en C hasta las características avanzadas que contiene el
lenguaje hoy en día. Aquí hay un vistazo general:

1\. **Funciones en C (la base de C++)**: en los primeros días, el
lenguaje C proporcionaba funciones como bloques reutilizables de código.
Se utilizaban principalmente para modularizar programas y evitar la
repetición de código.

2\. **Funciones en C++ (Programación orientada a objetos)**: cuando se
introdujo C++, las funciones adquirieron más poder con la capacidad de
pertenecer a clases como funciones (o mienbros). Esto permitió
encapsular el comportamiento dentro de los objetos.

3\. **Sobrecarga de funciones**: En C++, se introdujo la sobrecarga de
funciones, lo que permitió definir múltiples versiones de la misma
función con diferentes parámetros.

4\. **Funciones inline**: para mejorar el rendimiento, C++ incorporó las
funciones `inline`, que evitaban la sobrecarga de llamadas a funciones
en tiempo de ejecución al insertar su código directamente donde se
invocaban.

5\. **Funciones de plantilla**: la capacidad de crear funciones
genéricas usando plantillas fue un gran avance, permitiendo escribir
código reutilizable y flexible.

6\. **Funciones lambda (C++11 y versiones posteriores)**: con C++11,
llegaron las funciones lambda, que permitieron definir funciones
anónimas en línea, mejorando la flexibilidad y reduciendo la necesidad
de escribir funciones auxiliares.

7\. **Funciones de alto nivel (C++17 y C++20)**: se introdujeron mejoras
como `std::invoke`, `std::apply` y otras técnicas modernas que
facilitaron el trabajo con funciones, junto con el refinamiento de
funciones lambda y capacidades de metaprogramación.

8\. **Funciones abreviadas** son funciones que usan la palabra clave
`auto` para que el compilador deduzca los parámetros de la función.

C++ ha expandido la funcionalidad y la eficiencia de las funciones
tomadas de C, para que sean más expresivas y poderosas.

### ¿Que son las funciones?

Una función es un bloque de código con un nombre que se puede invocar (o
ejecutar) desde un lugar diferente al que fue declarada una o más veces.

``` C++
#include <iostream>            // cout

// Función que multiplica dos números
int multiplicar(int a, int b) 
{
    return a * b;
}

// Usando una función
int main() 
{
    int num = 9;
    int limite = 10;

    for (int i = 0; i <= limite; ++i) {
        std::cout << num << " * " << i << " = " 
                  << multiplicar(num, i) << '\n'; 
    } 

    return EXIT_SUCCESS;
}
```

La función `multiplicar()` recibe dos valores de tipo entero, los
multiplica y devuelve el resultado. En este caso la función encapsula
una operación Matématica específica para tipos enteros.

En C++, una función se define especificando su tipo de retorno, nombre y
parámetros de entrada (si los tiene). Luego, dentro de la función, se
ejecuta el conjunto de instrucciones que realiza la acción deseada. Al
final, la función puede devolver un valor al código que la llamó.

## Declaración de funciones

Antes de podes usar una función esta deve haber sido declarada en alguna
parte. En C++ la declaración de función comienza por el tipo de valor
devuelto, el nombre de la función y la lista de parámetros (opcionales).
Ademas de tener una declaración, una función deve tener una definición
que incluya el cuerpo de la función. A la declaración de función se le
conoce como prototipo de función.

### Prototipo de función

Un **prototipo** es un modelo limitado de una entidad completa que se
define más adelante. En el caso de las funciones, el cuerpo de la
función (el código entre llaves) es la entidad completa y la declaración
de función es el prototipo.

``` C++
Nombre de la función
      |
      v                    
int tribonacci(int limite);  // Prototipo de función 
^                   ^           
|                   |           
|           Lista de parámetros     
Tipo devuelto       
```

Dónde:

1.  **El tipo de valor devuelto** especifica el tipo del valor que
    devuelve la función o `void`, si no devuelve un valor.
2.  **El nombre de la función** se usa para invocar la función. Solo
    puede comenzar con una letra o un guion bajo y no puede contener
    espacios en blanco.
3.  **La lista de parámetros** es una lista delimitada por paréntesis y
    separada por comas, con cero o más parámetros (formales) que
    especifican el tipo y el nombre, mediante el cual se puede acceder
    al valor del parámetro dentro del cuerpo de la función.

Además de especificar un nombre, un conjunto de argumentos y el tipo de
valor devuelto, una declaración de función puede contener un
especificador, un declarador o un modificador de acceso:

- `inline` le indica al compilador que reemplace todas las llamadas a la
  función por el código de la función.
- `const` indica que la función devuelve un valor que no se puede
  modificar.
- `constexpr` indica que el valor devuelto por la función es un valor
  constante que se puede calcular (posiblemente) en tiempo de
  compilación.
- `consteval` indica que el valor devuelto por la función es un valor
  constante que se puede calcular únicamente en tiempo de compilación.
- `noexcept` expresión que especifica que la función no puede lanzar
  excepciones.
- Un especificador de enlace como `static` o `extern`.
- `noreturn`, indica que la función no devuelve el flujo de ejecución
  usando el mecanismo normal de llamadas.

Los nombres para funciones son a menudo palabras o frases que comienzan
o usan verbos como: `sumar`, `restar`, `factorizar`, etc. Una convención
muy común es comenzar el nombre de una función con una letra minúscula.
Cuando un nombre incluye varias palabras, se pueden separar las palabras
usando el guion bajo como `suma_total()`. Otra convención muy popular es
comenzar todas las palabras después de la primera palabra con una letra
mayúscula `sumaTotal()` (Notación camello). Cualquier identificador
valido puede usarse como nombre de una función.

Una declaración de función debe terminar con punto y coma, y solo puede
aparecer antes de que se llame a la función. Una función solo puede ser
invocada si ha sido definida. La definición de función debe aparecer
solo una vez en el programa según **la regla de definición única**.

## Definición de funciones

Una definición de función se compone de una declaración, más el
**cuerpo** de la función. El cuerpo de una función es el código entre
llaves.

``` C++
Nombre de la función
        |
        v                    
int tribonacci(int limite) { // El cuerpo de la función }
 ^                   ^           
 |                   |           
 |                Parámetro     
Tipo devuelto       
```

Cuando se define una función se coloca el cuerpo de la función entre
llaves. Dentro de las llaves se pueden incluir expresiones,
declaraciones y sentencias, como parte del cuerpo. Estas instrucciones
se ejecutan cada vez que la función es invocada. La única restricción es
que no se puede definir otra función dentro del cuerpo de una función.

El siguiente ejemplo muestra como declarar, definir e invocar una
función:

``` C++
#include <iostream>                                // cout

// Calcula un número de tribonacci (declaración de función)
int tribonacci(int limite);

// Imprime los primeros 20 numeros de la serie de tribonacci
int main()
{
    const int limite{20};

    // Invoca la función "tribonacci()" veinte veces.
    for (int num = 0; num < limite; ++num) {
        std::cout << tribonacci(num) << ' ';
    }

    return EXIT_SUCCESS;
}

// Calcula un número de tribonacci (definición de función)
int tribonacci(int limite)
{
    if (limite <= 1) {
        return 0;
    }

    // Los números de tribonacci son una secuencia lineal de
    // orden 3 con valores iniciales de (0, 0, 1), inspirados en
    // los números de fibonacci (de orden 2, con valores (0, 1)).
    // Cada número es la suma de los tres anteriores.
    int a = 0;
    int b = 0;
    int c = 1;
    for (int i = 3; i <= limite; ++i) {
        const int temp = a + b + c;
        a = b;
        b = c;
        c = temp;
    }

    return c;
}
```

En este ejemplo, se declara el prototipo de la función al principio del
programa. Pero, por lo general los prototipos de función se declaran en
archivos de cabecera, para poder usarlos en otros programas.

``` C++
// Una declaración de función incluye el tipo de datos que
// acepta la función, así como el tipo de datos que devuelve.
int tribonacci(int limite); 
```

Los nombres de los parámetros en la declaración de función son usados
únicamente con propósitos de auto documentación, y pueden omitirse:

``` C++
// El nombre del parámetro es opcional
int tribonacci(int); 
```

La declaración de función es necesaria, porque una función solo se puede
usar si ha sido declarada (o definida) antes de usarse. El prototipo de
función le indica al compilador el tipo de dato devuelto por la función,
el número de parámetros que espera recibir, los tipos de los parámetros,
y el orden en que espera recibir los parámetros. El compilador utiliza
el prototipo de la función para verificar (y comprobar) las llamadas. Si
los tipos no coinciden, genera un error en tiempo de compilación.

:::: tip
::: title
Tip
:::

**Funciones libres**

En C++, a diferencia de otros lenguajes una función pueden definirse en
el espacio de nombres global. Estas funciones se denominan **funciones**
**libres** o funciones no miembro. A las funciones definidas en el
ámbito de una clase se les denomina **funciones miembro**.

Las funciones libres son por lo general funciones globales y se pueden
usar en cualquier parte, una vez que se han declarado, sin embargo cada
declaración debe tener una definición, de lo contrario el compilador
genera un error al tratar de enlazarla.
::::

A diferencia de una declaración, una definición le dice al compilador
que reserve memoria para guardar un objeto y genere el código de la
función. La definición incluye el código entre llaves, y como cualquier
bloque de código puede contener declaraciones de variables, expresiones
y sentencias de control.

``` C++
// El cuerpo de una función es un bloque de código
int tribonacci(int limite) 
{   
    if (limite <= 1) {
        return 0;
    }

    // Variables locales
    int a = 0;
    int b = 0;
    int c = 1;

    // Suma las variables y luego intercambia los valores
    for (auto i = 3; i <= limite; ++i) {
        const int temp = a + b + c;
        a = b;
        b = c;
        c = temp;
    }

    // La sentencia return, devuelve el resultado de la evaluación
    return c;
}
```

El cuerpo de una función es un bloque de código, en el que se pueden
usar expresiones, declaraciones y sentencias de control. A las variables
declaradas dentro del cuerpo de una función se les denomina **variables
locales**. Estas variables salen de su ámbito cuando finaliza la
ejecución de la función, por esta razón también se les denomina
variables automáticas.

``` C++
// num es el argumento de la función
tribonacci(num);
```

Lo único que se necesita para invocar una función es un nombre, seguido
por los paréntesis que incluyen el argumento que espera la función. La
sintaxis es muy parecida a la usada en declaración de expresiones. Cada
vez que se ejecuta la función, el programa transfiere el flujo de
ejecución a la función, que ejecuta el código entre llaves. Una vez que
el flujo alcanza el final de la función o evalúa una sentencia `return`
termina la ejecución de la función, aun cuando la función tenga más
instrucciones.

### Conversión de argumentos

Cualquier llamada de función que no coincida con el prototipo de la
función genera un error de sintaxis. También se genera un error, si el
prototipo de la función y la definición no coinciden (en tipo y en
número de argumentos).

``` C++
tribonacci("10");    // Error: Conversión no valida 'const char*' a 'int'
tribonacci(10, 3);   // Error: Muchos argumentos para la función
```

El compilador comprueba el tipo de valor devuelto contra el valor
declarado por la función, si el valor devuelto no es del mismo tipo o no
puede ser convertido (implícitamente) al valor declarado, el compilador
genera un error en tiempo de compilación.

``` C++
// El compilador convierte 10.5 en 10
tribonacci(10.5);   // Bien: 81
```

Una característica de las funciones es la coerción o conversión de
argumentos. Esta característica obliga a que un argumento tenga el tipo
especificado por la declaración del parámetro. Por ejemplo, un programa
puede llamar a una función con un argumento de tipo `double`, aun cuando
el prototipo de la función especifique un argumento de tipo `int`. La
función de todas formas trabaja correctamente porque el compilador
convierte el tipo `double` a `int` según las reglas de conversión de
argumentos. Estas reglas indican cómo se realizan las conversiones entre
los tipos estandar. Un tipo `int`, por ejemplo se puede convertir en un
tipo `double` sin modificar su valor. Sin embargo, un tipo `double`
pierde precisión cuando se convierte a entero.

### Definición y declaración

Para no declarar y definir una función por separado, se puede usar la
definición de función directamente:

``` c++
#include <cmath>                 // sqrt
#include <iostream>              // cout

// Devuelve la magnitud de un vector
double mag(double x, double y)
{
    return std::sqrt(x * x + y * y);
}

// Calcula la magnitud de un vector
int main()
{
    std::cout << mag(200, 100);   // 223.607

    return EXIT_SUCCESS;
}
```

Una forma sencilla de insertar una función en un programa directamente
es eliminando la declaración y poniendo solo la definición de función al
inicio del programa, como en el ejemplo anterior. Este acercamiento es
simple y es útil en programas cortos, ya que elimina la necesidad de
usar la declaración de función, pero es poco flexible en otros
contextos. ¿Qué pasa si el programa usa funciones que dependen a su vez
de otras funciones?, ¿Como las ordenamos? ¿Cómo las usamos en otro
programas?

Por otra parte, muchos programadores prefieren colocar la función
`main()` al inicio, ya que es en donde comienza la ejecución del
programa. A sí que generalmente es buena idea colocar (y usar) las
declaraciones al inicio del programa o mejor aún, en un archivo de
cabecera, como la función `sqrt` declarada en el archivo de cabecera
`cmath`. La declaración de funciones en archivos de cabecera le permite
a una función usarse en cualquier parte de un programa.

:::: warning
::: title
Warning
:::

Si se define una función antes de invocarla, entonces la definición de
función también sirve como el prototipo de la función. Si se invoca una
función sin definirla, aunque la función tenga un prototipo se produce
un error de compilación.
::::

Separar la definición y la declaración en C++ es una práctica de
programación que previene de errores comunes como usar una función que
no ha sido definida.

### La sentencia return

La sentencia `return` se usa dentro de una función para devolver un
valor. Por lo general es la última instrucción dentro del cuerpo de una
función, sin embargo se puede evaluar en cualquier parte. Inclusive se
pueden usar varias sentencias `return`:

``` C++
#include <iostream>

// Devuelve el valor que es mayor
int max(int a , int b) 
{
    if (a > b) {
        return a;
    }

    return b;
}
```

Cuando el flujo de ejecución evalúa una instrucción `return` la función
termina y devuelve un valor.

``` C++
int num = max(100, 200);      // 200
```

La sentencia `return` hace dos cosas:

1.  Devuelve el valor a quien la invoca. Por esta razón se puede guardar
    el valor devuelto por la función en una variable.
2.  Termina inmediatamente la ejecución de la función.

Al igual que cualquier otra estructura de control, una sentencia
`return` puede aparecer varias veces en el cuerpo de una función.

:::: tip
::: title
Tip
:::

Si una función no devuelve un valor se puede usar la sentencia `return`
sin parámetros para terminar la ejecución de la función.
::::

Una sentencia `return` es una de las varias formas de salir de una
función, estas son otras:

- Ejecutar todas las instrucciones contenidas dentro del cuerpo de una
  función, es decir, llegar al final de la función.
- Lanzar una excepción en caso de error.
- Terminar la función porque el código lanzo una excepción y no se pudo
  atrapar localmente en una función `noexcept`.
- Directa o indirectamente invocando una función del sistema que no
  regresa el flujo de control como `exit()`. Una función que no devuelve
  normalmente el flujo de control a través de un valor de retorno deve
  ser marcada como `noreturn`.

Una función no puede devolver otra función o un arreglo, sin embargo;
puede devolver punteros a estos tipos, o puede usar una expresión
lambda, que genera un objeto tipo función. Excepto en estos casos, una
función puede devolver un valor de cualquier tipo que esté en el mismo
ámbito, sin embargo cuando es declarada como `void` la función no puede
devolver ningún valor.

### Funciones void

Cuando una función no devuelve un valor se deve declarar como `void`.

``` C++
// Imprime la cadena de texto pasada como argumento
void ver(std::string_view texto) 
{
    std::cout << texto;
}
```

Un tipo `void` no es un valor en sí mismo, solo representa un valor
vacío. Por lo general, las funciones que no devuelven valores no usan
una sentencia `return`, aunque se puede usar para finalizar el flujo de
ejecución, sin devolver un valor.

``` C++
// Imprime la cadena de texto pasada como argumento
void ver(std::string_view texto) 
{
    // Si el texto está vacío termina usando return (sin valores). 
    if (texto.size() == 0) {
        return;
    }    

    std::cout << texto;
}
```

Es un error usar la sentencia `return` para devolver un valor en una
función declarada como `void`, igual que no devolver un valor en una
función que declara un tipo de valor devuelto.

``` C++
int  f1() { }             // Error: la función devuelve un tipo int
void f2() { }             // Bien 
int  f3() { return 1; }   // Bien
void f4() { return 1; }   // Error: la función void devuelve un valor
int  f5() { return; }     // Error: la función int no devuelve un valor
void f6() { return; }     // Bien: return no devuelve un valor
```

A las funciones que no devuelven valores se les denomina procedimientos
en algunos lenguajes de programación. Por lo general una función `void`
solo es útil si puede producir efectos secundarios, como modificar otro
valor o mostrar un mensaje en la consola.

### Invocación de funciones

Las funciones son como variables globales. Una vez que se declara o se
define una función, el nombre de la función se puede usar en cualquier
lugar que sea visible.

``` C++
#include <iostream>                               // cout

// Calcula un valor entre dos valores con un incremento especifico.
// El monto es el parámetro para interpolar entre los dos valores
// donde 0.0 es igual al primero valor y 1.0 es igual al segundo. 
// El valor predeterminado es 0.5
double lerp(double inicio, double fin, double monto = 0.5) noexcept;

// Calcula la interpolación lineal entre dos valores
int main()
{
    double a{20.0};
    double b{80.0};

    // La funciones se pueden usar para declarar variables
    double c = lerp(a, b, 0.1); 
    std::cout << c << std::endl;                   // 26

    // Las funciones se pueden usar en estructuras de control
    if (lerp(a, b) == 50.0) { 
        std::cout << 50.0 << std::endl;            // 50  
    }

    // Las funciones se pueden usar en expresiones
    double d = lerp(a, b, 0.5) + lerp(a, b, 0.8);   // 50 + 68 = 118  
    std::cout << d << std::endl;  

    return EXIT_SUCCESS;
}

// Calcula la interpolación lineal de dos valores
double lerp(double inicio, double fin, double monto) noexcept
{
    return monto * (fin - inicio) + inicio;
}
```

A diferencia de las variables, un nombre de función se deve usar en
conjunto con el operador de invocación de funciones (los paréntesis) y
se deben incluir opcionalmente los argumentos que acepta la función. Si
la función no acepta argumentos se deben usar los paréntesis vacíos.

:::: tip
::: title
Tip
:::

Para ejecutar una función es necesario invocar el nombre de la función
seguido del operador de invocación de funciones (los paréntesis), y
colocar opcionalmente los argumentos que acepta la función separados por
comas.
::::

Las funciones en C++ son parametrizadas. Una declaración de función
puede incluir una lista de identificadores locales conocidos como
parámetros, que funcionan como lo hacen las variables locales en un
bloque de código. La invocación de la función provee los valores para
los parámetros y los paréntesis sirven para pasar los argumentos a la
función.

### Argumentos predeterminados

Una función puede usar argumentos predeterminados:

``` C++
#include <iostream>            // cout
#include <string>              // string
#include <string_view>         // string_view

// Cifra una cadena de texto usando el cifrado Cesar. Usa rot 13 como 
// clave inicial. Si dígitos es true, también cifra los números.  
std::string cifrar_cesar(std::string_view texto_plano, 
    int clave = 13, bool digitos = true);

// Descifra una cadena de texto cifrada con el cifrado de Cesar.
std::string descifrar_cesar(std::string_view texto_encriptado, 
    int clave = 13, bool digitos = true);

// Cifra y descifra una cadena de texto usando el cifrado César
int main()
{
    // Usa los valores predeterminados
    std::string texto_plano{"La clave del texto es 13"}; 
    auto texto_encriptado = cifrar_cesar(texto_plano);
    std::cout << texto_encriptado << std::endl;
    std::cout << cifrar_cesar(texto_encriptado) << std::endl;    

    // Usa dos argumentos
    texto_encriptado = cifrar_cesar(texto_plano, 5);
    std::cout << texto_encriptado << std::endl;
    std::cout << descifrar_cesar(texto_encriptado, 5) << std::endl; 

    // Usa tres argumentos 
    texto_encriptado = cifrar_cesar(texto_plano, 23, false);
    std::cout << texto_encriptado << std::endl;
    std::cout << descifrar_cesar(texto_encriptado, 23, false) << std::endl; 

    return EXIT_SUCCESS;
}

std::string cifrar_cesar(std::string_view texto_plano, 
    int clave, bool digitos)
{
    std::string salida;
    for (char letra : texto_plano) {
        if (std::isalpha(letra)) {
            char c = std::isupper(letra) ? 'A' : 'a';
            letra = ((letra + clave - c) % 26) + c;
        }

        // Encripta los digitos usando rot5
        if (digitos) {
            if (std::isdigit(letra)) {
                int root5 = (letra < '5') ? 5 : -5;
                letra = ((letra + root5) - letra) % 26 + letra;
            }
        }
        salida += letra;
    }

    return salida;
}

std::string descifrar_cesar(std::string_view texto_encriptado, 
    int clave, bool digitos)
{
    return cifrar_cesar(texto_encriptado, (26 - clave) % 26, digitos);
}
```

Una forma sencilla de proveer valores predeterminados para los
parámetros de una función, es proporcionar valores que actúen como
argumentos inicializadores en caso de que alguno de los parámetros de
función no sea pasado formalmente. Esto se puede hacer en la declaración
o definición de función, pero no en ambos:

``` C++
// La función requiere un valor obligatorio y dos valores opcionales.
std::string cifrar_cesar(std::string_view texto_plano, 
    int clave = 13, bool digitos = true);
```

Los argumentos predeterminados solo puede aparecer a la derecha (o ser
los últimos) en la lista de parámetros de función. Si se omite un
argumento en la llamada, la función usa el valor predeterminado para el
argumento.

:::: tip
::: title
Tip
:::

**La clase string_view**

Se puede pasar una cadena de texto como `const string&` a una función
que únicamente requiere acceso de lectura, sin embargo a partir de C++17
se puede usar una vista de tipo `string_view` para acceder a los
elementos de una cadena de texto o de un objeto `string`. A diferencia
de la clase `string`, un objeto `string_view` es un objeto liviano que
no almacena ningún elemento, solo proporciona una vista que se puede
usar para acceder y modificar los elementos de una secuencia de
caracteres.
::::

La función puede llamarse de esta forma:

``` C++
// La función puede aceptar 1, 2 y 3 argumentos
cifrar_cesar(texto_plano);
cifrar_cesar(texto_plano, 5);
cifrar_cesar(texto_plano, 16, false);
```

Cuando una función usa parámetros predeterminados, se pueden omitir los
argumentos cuando se invoca la función, o se pueden especificar otros
valores de forma explícita. Los argumentos predefinidos son usados para
suplir los argumentos no pasados en la invocación de la función.

### Sobrecarga de funciones

Es posible definir varias funciones con el mismo nombre, siempre y
cuando tengan diferentes tipos de parámetros (en número y tipo). A este
peculiar comportamiento el estandar lo llama sobrecarga.

Por ejemplo estas funciones tienen el mismo nombre, sin embargo aceptan
y devuelven tipos diferentes.

``` C++
#include <iostream>                        // cout

// Prototipos de función definidos en el espacio de nombre math 
namespace math {

    // Devuelve el número mayor de dos enteros
    int max(int num1, int num2);

    // Devuelve el número mayor de dos decimales
    double max(double num1, double num2);

    // Devuelve el número mayor de tres decimales
    double max(double num1, double num2, double num3);
}

// Sobrecarga de funciones
int main()
{
    std::cout << "El valor mayor de 10 y 20 es: " 
              // Invoca la función max que acepta dos enteros.
               << math::max(10, 20) << '\n'
              // Invoca la función max que acepta dos decimales.
              << "El valor mayor de 1.5 y 2.5 es: " 
              << math::max(1.5, 2.5) << '\n'
              // Invoca la función max que acepta tres decimales.
              << "El valor mayor de 1.5, 5.1, 3.0 es: " 
              << math::max(1.5, 5.1, 3.0) << '\n';

    return EXIT_SUCCESS;
}

// Devuelve el número mayor de dos enteros
int math::max(int num1, int num2)
{
    if (num1 > num2) {
        return num1;
    }

    return num2;
}

// Devuelve el número mayor de dos decimales
double math::max(double num1, double num2)
{
    if (num1 > num2) {
        return num1;
    }

    return num2;
}

// Devuelve el número mayor de tres decimales
double math::max(double num1, double num2, double num3) 
{
    double tmp = (num1 > num2) ? num1 : num2;
    return (tmp > num3) ? tmp : num3;
}
```

La sobrecarga se utiliza para crear funciones con el mismo nombre.

``` C++
max(10, 20);          // Invoca la función max que acepta dos enteros.
max(1.5, 2.5);        // Invoca la función max que acepta dos decimales.
max(1.5, 5.1, 3.0);   // Invoca la función max que acepta tres decimales.
```

En este caso, las funciones sobrecargadas realizan tareas similares sin
embargo operan en diferentes tipos de datos.

Si la llamada de función no coincide en tipo o en número de argumentos
se genera un error:

``` C++
// Error: conversión no valida 'const char*' a 'int'
math::max("uno", "dos"); 
```

La idea de la sobrecarga es invocar a la función que coincida con el
número de argumentos (en número y tipo), o generar un error en tiempo de
compilación si ninguna función coincide con la llamada.

### Restricciones al usar sobrecarga

La única restricción que impone la sobrecarga de funciones es que cada
función con el mismo nombre deve aceptar un tipo diferente, o un número
diferente de argumentos. El estandar considera las siguientes
características al aplicar sobrecarga:

- El número de argumentos.
- El tipo de los argumentos.
- El uso de los puntos suspensivos.
- Calificadores como `const` o `volatile`.
- Calificadores de referencia.

Para encontrar la versión adecuada de un conjunto de funciones, el
compilador busca una coincidencia entre el tipo de los argumentos y los
parámetros de las función usando las siguientes reglas (y otras más
complejas):

- **Coincidencia exacta:** si existe una coincidencia exacta entre los
  tipos de los parámetros de la función que llama a una función
  sobrecargada, se utiliza la función sobrecargada.
- **Coincidencia usando promociones:** si no existe una coincidencia
  exacta, pero se puede producir una conversión de un tipo a un tipo
  superior (tal como un parámetro `int` a `long`, o `float` a `double`)
  se utiliza la función seleccionada.
- **Coincidencia usando conversiones estandar** también se puede
  producir una coincidencia de tipos, realizando conversiones (definidas
  por el compilador o por el usuario) entre los tipos.
- **Coincidencia usando puntos suspensivos:** si una función
  sobrecargada se define con un número variable de parámetros (usando
  los puntos suspensivos `...`), se puede utilizar como función
  sobrecargada.

Si el compilador no encuentra coincidencias, rechaza la llamada a la
función por considerarla \"ambigua\". La resolución del orden de
sobrecarga es independiente del orden en que se declaren las funciones,
y no toma en cuenta el valor devuelto.

``` C++
// Error llamada a sobrecarga 'max(int, double)' es ambigua
max(10, 9.0);
```

Los argumentos predeterminados no se consideran parte del tipo de
función. Por lo tanto, no se utilizan en la selección de funciones
sobrecargadas.

## Pasando argumentos a una función

Las formas más comunes de pasar argumentos a una función son el paso por
valor y el paso por referencia. Cuando se pasa un argumento por valor,
la función crea una copia del valor y lo pasa (usando la pila de
llamadas) a la función que la invoca. Las modificaciones que hace la
función en la copia del valor, no afectan al valor de la variable
(original) que hizo la llamada.

Mediante el paso por referencia, la función que hace la llamada accede
directamente a los datos almacenados en la memoria, esto le permite
modificar los datos originales. Otra forma de pasar argumentos a una
función es usando apuntadores o punteros. Los punteros proporcionan una
forma alternativa de usar el paso por referencia para modificar de forma
indirecta objetos almacenados en memoria.

### Argumentos por valor

La semántica para el paso de argumentos es idéntica a la semántica de
inicialización (inicialización por copia, para ser exactos). Cuando una
función recibe un argumento, compara el tipo del argumento con el tipo
del parámetro que espera la función, y si corresponden los tipos de
conversión definidos por el usuario con los tipos de conversión estándar
se invoca la función. A menos que un argumento formal sea una
referencia, se pasa una copia del argumento real a la función.

``` C++
#include <algorithm>   // sort
#include <iostream>    // cout
#include <stdexcept>   // domain_error
#include <vector>      // vector
#include <tuple>       // tuple

// Devuelve el valor mínimo, medio y máximo de un vector.
// La función copia el vector que recibe como argumento.
// Lanza una excepción si el vector está vacío.
std::tuple<double, double, double> media(std::vector<double> rango);

// Calcula el valor mínimo, medio y máximo de un vector pasado por valor.
int main()
{
    std::vector<double> rango{20.0, 50.0, 40.0, 30.1, 40.2, 10.3, 12.5};

    // Desestructura la tupla en variables independientes
    auto [minimo, medio, maximo] = media(rango);
    std::cout << "Valor mínimo: " << mínimo << '\n'
              << "Valor medio:  " << medio  << '\n'
              << "Valor máximo: " << máximo << '\n';

    return EXIT_SUCCESS;
}

// Devuelve el valor mínimo, medio y máximo de un vector.
std::tuple<double, double, double> media(std::vector<double> rango)
{
    std::size_t len = rango.size();

    // Si el vector está vacío lanza una excepción
    if (len == 0) {
        throw std::domain_error("El vector está vacío.");
    }

    // Ordena el vector.
    std::ranges::sort(rango);
    std::size_t mitad = len / 2;

    double minimo = rango[0]; 
    double maximo = rango[len-1];
    double medio  = (len % 2 == 0) ?
                    (rango[mitad] + rango[mitad - 1]) / 2 : rango[mitad];

    // Devuelve una tupla con tres valores.
    return std::make_tuple(minimo, medio, maximo);      
}
```

Cuando se llama a una función o se invoca usando los paréntesis. La
función almacena localmente los argumentos formales que recibe. Y cada
parámetro de la función es inicializado con el argumento
correspondiente. La función `media()` es un ejemplo que muestra el paso
de argumentos por valor. Con el paso por valor, la función recibe una
copia del vector original.

``` C++
// La función usa el paso por valor (o copia)
media(std::vector<double> rango)
```

En este caso, aunque el objeto es grande, el paso por valor es necesario
por que el cuerpo de la función modifica el vector que recibe, ya que
realiza un ordenamiento usando `sort`. La función `sort` es un algoritmo
genérico que ordena los elementos de un contendor usando rangos.

``` C++
// sort ordena los elementos del contenedor de menor a mayor.
std::ranges::sort(rango);
```

La función `sort` ordena los elementos del contenedor que recibe como
argumento, sin embargo el parámetro original no sufre ningún cambio
porque la función opera en una copia del vector, no en el vector
original. Por esta razón se usa el paso por valor.

La función `make_tuple()` devuelve una tupla. Una tupla es una
estructura de datos que puede contener elementos de diferente tipo:

``` C++
// Devuelve una tupla con tres valores.
return std::make_tuple(minimo, medio, maximo); 
```

Una función solo puede devolver un resultado, pero usando una estructura
de datos como una tupla, o una estructura, una función puede devolver
varios tipos de datos. Cualquier objeto que contenga miembros de datos
puede ser desestructurado en identificadores únicos usando vinculos (o
enlaces) estructurados.

``` C++
// Desestructura el valor devuelto por la función
auto [minimo, medio, maximo] = media(lista);;

// Y los usa como variables
std::cout << "Valor mínimo: " << mínimo << '\n'
          << "Valor medio:  " << medio  << '\n'
          << "Valor máximo: " << máximo << '\n';
```

Los enlaces estructurados crean variables independientes usando vínculos
anónimos que hacen referencia a los miembros del objeto.

### Parámetros y argumentos

Los términos **parámetros** y **argumentos** se usan comúnmente de forma
intercambiable para nombrar los valores que acepta y recibe una función.
Sin embargo el contexto en el que se usan es el que usualmente ayuda a
distinguir su significado. La siguiente es una regla general para
distinguirlos:

Los **parámetros** se usan para definir una función. En el ejemplo
siguiente: `a` y `b` son los \"parámetros formales\" que acepta la
función sumar:

``` C++
// a y b son parámetros
int sumar(int a, int b) 
{
    return a + b; 
};
```

Los **argumentos** se usan para invocar una función. En el ejemplo
siguiente, los valores `10` y `20` son los \"argumentos reales\" que
recibe la función cuando es invocada.

``` C++
// 10 y 20 son argumentos
sumar(10, 20);   
```

Las funciones usan los valores de los argumentos para calcular y
opcionalmente devolver un valor y usan los parámetros para inferir el
tipo o tipos que usa el código de la función.

El paso por valor evita que los valores pasados como argumentos
modifiquen objetos fuera de una función y previene de los efectos
secundarios que ocurren cuando se modifica un objeto fuera de su ámbito.
Este es el comportamiento predeterminado para todos los tipos del
lenguaje, excepto para los arreglos, los cuales son en realidad punteros
(al primer elemento del arreglo).

### Paso por referencia

Los parámetros pasados por referencia pueden ser modificados por la
función que los recibe, mientras que los parámetros pasados por valor no
pueden ser modificados. Esto significa que una función por valor puede
manipular los parámetros, pero no puede reflejar los cambios en los
parámetros originales, pero una función que recibe una referencia, si
puede modificar los parámetros originales.

El ejemplo siguiente muestra la semántica del paso por referencia:

``` C++
#include <vector>                 // vector
#include <iostream>               // cout

// Una matriz es un vector de vectores
using Matriz = std::vector<std::vector<int>>;

// Rota la matriz que recibe como referencia 90 grados 
// Asume que las filas y columnas tienen el mismo tamaño.
void rotar(Matriz& matriz);

// Imprime el contenido de la matriz
void print(const Matriz& matriz);

// Rota una matriz cuatro veces
int main() 
{
    Matriz matriz {
        {1, 2, 3, 4},
        {0, 0, 0, 0},
        {0, 0, 0, 0},
        {0, 0, 0, 0},
    }; 

    // Rota la matriz 4 veces y muestra cada rotación
    for (std::size_t i = 0; i < 4; ++i) {
        rotar(matriz);
        print(matriz);
    }
}

// Rota la matriz que recibe como referencia 90 grados
void rotar(Matriz& matriz) 
{ 
    // Copia la matriz pasada como referencia
    Matriz copia = matriz;  
    std::size_t len = matriz.size();

    // Intercambia el contenido de las dos matrices
    for (std::size_t y = 0; y < len; ++y) {
        for (std::size_t x = 0; x < len; ++x) {
            copia[y][x] = matriz[(len - 1) - x][y];
        }
    }

    // Copia la matriz antes de que se destruya.
    matriz = copia;
}

// Imprime el contenido de la matriz
void print(const Matriz& matriz) 
{
    for (const auto& filas : matriz) {
        for (const auto& num : filas ) {
            std::cout << ' ' << num << ' ';
        }
        std::cout << '\n';
    }
    std::cout << '\n';    
}
```

Un parámetro por referencia es otro nombre (o alias) para un argumento
en la llamada a una función. Para indicar que un parámetro de función se
pasa por referencia, simplemente hay que colocar el signo `&` después
del tipo del parámetro en el prototipo de la función.

``` C++
// La función rotar recibe una referencia
void rotar(Matriz& matriz);
```

Esto se lee de derecha a izquierda, y significa que `matriz` es una
referencia a un objeto `Matriz` (un vector de vectores). En la llamada a
la función, simplemente hay que colocar el nombre de una variable para
pasarla por referencia.

``` C++
rotar(matriz);
```

Cuando se usa el valor dentro del cuerpo de la función, el parámetro en
realidad se refiere al objeto original en la función que hizo la
llamada, y el valor de este objeto se puede modificar. El paso por
referencia se deve usar con precaución, ya que la función que se invoca
puede cambiar y eventualmente corromper los datos de la función que hace
la llamada.

### Referencias constantes

Las funciones pueden modificar argumentos pasados por referencia, esto
produce por supuesto, los conocidos efectos secundarios. Y siempre que
sea posible es mejor evitarlos. Por otra parte, es más eficiente pasar
un objeto grande por referencia que por valor. En este caso se puede
declarar la referencia como constante, para indicarle al compilador que
no queremos modificar el valor al que apunta la referencia.

``` C++
// Pasa un valor por referencia constante. 
// El compilador asume que no queremos modificar el objeto referenciado.
void print(const Matriz& matriz);
```

El paso por referencia es bueno por cuestiones de rendimiento, ya que
elimina la sobrecarga asociada a la copia de (grandes cantidades) de
datos que usa el paso por valor. Y las referencias declaradas como
`const` son útiles porque la función no puede modificar el valor
original pasada como referencia.

La ausencia de la palabra clave `const` en una declaración de tipo
referencia es tomada por el compilador como un sentencia para modificar
el valor de la variable.

``` C++
// Pasa el valor por referencia
// El compilador asume que queremos modificar el objeto referenciado. 
void rotar(Matriz& matriz);
```

Muchas veces no es sencillo determinar cuándo usar el paso por valor y
cuando usar el paso por referencia. Estas son algunas recomendaciones
que pueden ser útiles:

1.  Usa el paso por valor cuando trabajes con objetos pequeños, es más
    rápido.
2.  Usa el paso por referencia constante para pasar valores (grandes)
    que no necesiten modificarse.
3.  Devuelve un resultado usando la sentencia `return` en lugar de
    modificar un argumento pasado como referencia.
4.  Como último recurso usa el paso por referencia cuando necesites
    modificar un objeto (sin moverlo).

Si se declaran parámetros por referencia, no se pueden usar valores
literales, ya que las referencias apuntan a objetos (variables o
funciones con nombre) residentes en memoria; por otra parte, si un
parámetro es declarado por valor, el mismo valor puede pasarse como una
constante literal o como una variable.

### Devolviendo referencias

Una función puede devolver un valor pasado como referencia.

``` C++
#include <iostream>

// Convierte un número decimal a binario usando desplazamiento de bits. 
std::ostream& binario(std::ostream& os, uint32_t num);

// Convierte un número decimal a binario.
int main()
{
    uint32_t num;
    std::cout << "Introduce un número: "; 
    std::cin >> num;
    std::cout << "Numero en binario:   ";

    // Un flujo de salida se puede (y se deve) pasar por referencia
    binario(std::cout, num);        
    return EXIT_SUCCESS;
}

// Convierte un número decimal a binario y lo envía a la salida estandar. 
std::ostream& binario(std::ostream& os, uint32_t num)
{
    if (num >= 2 ) {
        binario(os, num >> 1 );  
    }

    return os << (num & 1);
}
```

Algunos objetos como los flujos de entrada o de salida solo se pueden
pasar como referencias. Para que una función pueda devolver una
referencia es necesario adjuntarle el símbolo `&`:

``` C++
// Acepta y devuelve una referencia a un objeto ostream.
std::ostream& binario(std::ostream& os, uint32_t num);
```

La función `cout` devuelve una referencia a un objeto de tipo `ostream`
(el flujo de salida global).

``` C++
// Un flujo de salida se puede pasar como una referencia
binario(std::cout, num);
```

Una función puede devolver una referencia a cualquier objeto declarado
de forma global, sin embargo, si se intenta devolver el valor de un
objeto o variable local declarada dentro del cuerpo de una función se
genera un error, porque una vez que termina la ejecución de la función,
las variables locales se destruyen. Por ejemplo **esto es un error**:

``` C++
// Error: La función intenta devolver una referencia a una variable local.
std::string& nombre() 
{
    std::string tmp{"Error"};
    return tmp;
} 
```

En este caso el compilador lanza una advertencia, por intentar devolver
una referencia a una variable local. El comportamiento es indefinido
porque la variable que intenta devolver la función no existe después de
la ejecución de la función. Esto se puede corregir usando una variable
estática:

``` C++
// Bien: La función devuelve una referencia a una variable estática.
std::string& nombre() 
{
    static std::string tmp{"Bien"};
    return tmp;
}

std::cout << nombre() << std::endl;  
```

Las variables estáticas se tratan como si fueran variables globales.
Estas variables se almacenan de forma permanente en la memoria de la
computadora mientras el programa se ejecuta.

Cuando se usan referencias, el nombre de la función se puede usar de
forma directa para modificar el valor del objeto que se usa como
referencia. Por ejemplo, esto es válido:

``` C++
// Una función que devuelve una referencia puede modificar 
// el objeto referenciado, colocando el nombre de la función 
// del lado izquierdo del operador de asignación.
nombre() = "Algún otro nombre";

// El valor al que se refiere la función ha cambiado.
std::cout << nombre() << std::endl;    // "Algún otro nombre"
```

Cuando una función devuelve un tipo de referencia, la función se
convierte en una expresión con valor-l. Las referencias son valores-l,
esto significa que tienen una dirección a la que se puede acceder, para
consultar, asignar y modificar el valor al que hace referencia. Aunque
la sintaxis no es obvia, es legal.

Para prevenir la modificación se puede declarar tanto la función como
los parámetros de la función con el calificador `const`.

``` C++
// Bien: La función devuelve una referencia de tipo const.
const std::string& nombre() 
{   
    // El valor de "tmp" no se puede modificar
    const static std::string tmp{"imagen"};
    return tmp;
}

// Error de compilación: 
// No se puede usar el operador de asignación "=" en tipos const
nombre() = "texto";
```

En este caso cualquier intento por modificar los valores de la función
resultan en un error de compilación.

#### Variables locales estáticas

Después que una función completa su ejecución todas las variables
locales son destruidas. Es decir salen de su ámbito. Estas variables son
conocidas como variables automáticas. Sin embargo en algunas ocasiones
es necesario retener un valor almacenado en una variable local para
usarlo en la próxima llamada de función. En C++, esto se puede hacer
usando variables locales estáticas. Estas variables se declararan con la
palabra clave `static`.

``` C++
#include <iostream>                  // cout
#include <string>                    // string

// Genera un nombre con un valor incrementable
std::string generar_nombre(const std::string& nombre);

// Usando variables locales estáticas
int main()
{
    std::cout << generar_nombre("imagen") <<  '\n'    // imagen_1
              << generar_nombre("imagen") <<  '\n';   // imagen_2

    return EXIT_SUCCESS;
}

std::string generar_nombre(const std::string& nombre)
{
    std::string tmp = nombre + "_";    
    static int secuencia = 0;         // Una contador estático
    ++secuencia;                      // Incrementa el contador
    tmp += std::to_string(secuencia); // Convierte el contador a texto

    return tmp;                       // Devuelve el nuevo nombre 
}
```

En el ejemplo, cada vez que se invoca la función se incrementa el
contador declarado como `static`.

``` C++
static int secuencia = 0;      
```

La inicialización de las variables estáticas sucede la primera vez que
se invoca la función. Las llamadas sucesivas pueden usar y cambiar el
valor inicializado por la llamada anterior.

:::: note
::: title
Note
:::

Las variables que usan almacenamiento estático existen a partir del
punto en el que el programa empieza a ejecutarse y dejan de existir
cuando el programa termina. El almacenamiento de una variable estática
se asigna cuando el programa empieza su ejecución y se libera cuando el
programa termina.
::::

A diferencia de las variables automáticas, las variables locales
declaradas como `static` retienen sus valores cuando la función devuelve
el control al código que la invocó. La próxima vez que se hace una
llamada a la función, las variables estáticas contienen los valores que
tenían cuando la función se ejecutó por última vez. Todas las variables
numéricas declaradas como estáticas se inicializan a cero si no son
inicializadas con un valor. Una buena práctica de programación es
inicializar todas las variables estáticas de forma explícita.

### Paso por puntero

El paso de parámetros por punteros es bastante parecido al paso de
parámetros por referencia, salvo que el proceso de los datos dentro de
la función es un poco diferente. Por ejemplo:

``` C++
#include <iostream>                              // cout

// Intercambia el valor de dos variables de tipo entero.
void swap(int* a, int* b);

// Invierte un arreglo usando el indice de los elementos de inicio y fin.
void invertir(int* arreglo, int inicio, int final);

// Imprime el contenido de un arreglo.
void print(const int* arreglo, std::size_t len);

// Invierte el contenido de un arreglo usando punteros
int main()
{
    int arreglo [] {1, 2, 3, 4 , 5, 6, 7, 8, 9};

    print(arreglo, 9);
    invertir(arreglo, 0, 8);
    print(arreglo, 9);

    return EXIT_SUCCESS;
}

// Intercambia el valor de dos variables de tipo entero
void swap(int* a, int* b)
{
    int temp = *a;
    *a = *b;
    b = &temp;
}

// Invierte un arreglo 
void invertir(int* arreglo, int inicio, int final)
{
    while (inicio < final) {
        swap(&arreglo[inicio], &arreglo[final]);
        ++inicio;
        --final;
    }
}

// Imprime el contenido de un arreglo
void print(const int* arreglo, std::size_t len)
{
    for (std::size_t i = 0; i < len; ++i) {
        std::cout << arreglo[i] << ' ';
    }
    std::cout << '\n';
}
```

En este caso, las funciones `swap()` e `invertir()` aceptan punteros
como parámetros. La función `swap()` espera dos punteros de tipo entero,
mientras que la función `invertir()` espera un arreglo. La sintaxis para
declarar punteros requiere del operador de indirección `*`.

``` C++
// La función espera dos punteros
void swap(int* a, int* b);
```

Los punteros son variables que almacenan valores enteros. Lo interesante
sobre los punteros es que los valores que almacenan son direcciones de
memoria de otras variables (u objetos).

La diferencia entre referencias y punteros radica en la sintaxis usada
para acceder al valor apuntado. Ya que un puntero no puede acceder de
forma directa al valor al que apunta.

``` C++
// Dentro de una función, las referencias "&" pueden
// acceder directamente a los valores referenciados.  
void swap(int& a, int& b)
{
    int temp = a;
    a = b;
    b = temp;
}

// Un puntero necesita usar el operador de indirección "*" y el
// operador de referencia "&" para acceder a los valores apuntados. 
void swap(int* a, int* b)
{
    int temp = *a;
    *a = *b;
    b = &temp;
}
```

Para acceder al valor al que apunta un puntero es necesario
**desreferenciar** el puntero, usando el operador de indirección `*`
antes del nombre.:

``` C++
int temp = *a;       // *a es un valor entero (no un puntero).
```

Al igual que las referencias, un puntero es usado para acceder de forma
indirecta a otro objeto. Pero a diferencia de las referencias, un
puntero es un objeto por derecho propio.

``` C++
*a = *b;            // asigna el valor apuntado por "b" en "a"
```

Para que un puntero apunte a un objeto es necesario tomar la dirección
del otro objeto:

``` C++
*b = &temp;         //  b almacena la dirección de "temp"
```

Para tomar la dirección de otro objeto se usa el operador de referencia
`&`, a la izquierda del nombre del objeto. Como los punteros son
objetos, podemos emplearlos en asignaciones y operaciones, pero hay que
diferenciar claramente entre el puntero como dato (o dirección) y el
valor al que apunta. Por ejemplo:

La operación:

``` C++
a = b;
```

hace que `a` y `b` apunten a la misma dirección de memoria, pero en
cambio la operación:

``` C++
*a = *b;
```

hace que el valor apuntado por `a` sea el mismo que el valor apuntado
por `b`.

## Funciones y arreglos

En C++ los arreglos y las cadena de caracteres estilo C se pasan por
referencia (o dirección). Esto significa que cuando una función utiliza
un arreglo como parámetro, la función puede eventualmente modificar los
elementos del arreglo.

``` C++
#include <iostream>
#include <iomanip>

// Imprime un arreglo en filas por columnas
void print(const int* arreglo, int filas, int columnas);

// Rellena un arreglo con numeros enteros entre un mínimo y un maximo.
void fill(int* arreglo, int minimo, int maximo);

// Funciones y arreglos
int main()
{
    constexpr int limite = 120;
    int arreglo[limite];

    // Rellena el arreglo con numeros del 0 al 120
    fill(arreglo, 0, limite);

    // Imprime 10 filas y 12 columnas
    print(arreglo, 10, 12);

    return EXIT_SUCCESS;
}

// Imprime un arreglo en filas por columnas
void print(const int* arreglo, int filas, int columnas) 
{
    for (int y = 0; y < filas; ++y ) {
        for (int x = 0; x < columnas; ++x ) {
            std::cout << std::setw(3) << arreglo[columnas * y + x] << ' ';
        }
        std::cout << '\n';
    }
}

// Rellena un arreglo con numeros enteros  
void fill(int* arreglo, int minimo, int maximo)
{
    for (int i = minimo; i < maximo; ++i) {
        arreglo[i] = i;
    }
}
```

Para recibir un arreglo, la lista de parámetros de una función debe
especificar que espera recibir un arreglo como un puntero,

``` C++
// El arreglo se pasa como un puntero
void fill(int* arreglo, int minimo, int maximo);
```

o como un arreglo:

``` C++
// El tamaño del arreglo no se requiere en los corchetes.
void fill(int arreglo[], int minimo, int maximo);
```

Al pasar un arreglo a una función, siempre se debe especificar el número
de elementos en un parámetro independiente. Los elementos del arreglo no
se copian cuando se pasa el arreglo a la función.

:::: note
::: title
Note
:::

**Los arreglos decaen en punteros**

Cuando se pasa un arreglo a una función se pasa como un puntero al
primer elemento. El puntero no contiene información sobre el tamaño de
elementos que contiene el arreglo, ni ningún tipo de informacion
adicional.
::::

Las funciones que aceptan arreglos pueden modificar los elementos del
arreglo original. El nombre del arreglo es la dirección en memoria del
primer elemento del arreglo. Al pasar la dirección inicial del arreglo,
la función sabe exactamente dónde se almacena el arreglo completo en la
memoria.

Aun cuando el nombre del arreglo se pase por valor, sus elementos se
pueden cambiar, como si se hubieran pasado por referencia. Para evitar
este comportamiento es necesario usar el calificador `const`.

``` C++
void print(const int* arreglo, int filas, int columnas) 
```

Cuando a un arreglo usado como parámetro se le antepone el calificador
`const`, los elementos del arreglo se hacen constantes en el cuerpo de
la función, y cualquier intento de modificar un elemento del arreglo en
el cuerpo de la función produce un error de compilación. Declarar un
argumento como puntero de tipo `const` le dice al compilador que el
valor del objeto apuntado por el argumento no puede (y no deve) ser
cambiado por la función.

En C++, el tamaño de un arreglo se especifica entre corchetes después
del nombre de la variable, no después del tipo.

``` C++
constexpr int limite = 120;
int arreglo[limite];

// Rellena el arreglo con numeros del 0 al 120
fill(arreglo, 0, limite);
```

El número de elementos se debe proporcionar como un valor literal de
tipo entero, o como una expresión constante (calificada como `const` o
`constexpr`). Ya que el compilador tiene que saber cuánto espacio de
pila deve asignar. No se puede utilizar un valor calculado en tiempo de
ejecución.

El uso de arreglos estilo C se deve de usar con precaución. En el caso
de funciones que necesitan acceso directo a los datos de un arreglo se
pueden usar varios mecanismos de acceso como los proporcionados por la
clase `vector` y la clase `array` que saben su tamaño y además contienen
una función miembro llamada `data()` que devuelve un puntero al primer
elemento de la secuencia. Otro contenedor de la biblioteca estandar que
se puede usar en estos casos, es la clase `span` que representa una
vista. Esta clase ofrece una alternativa segura para acceder de forma
indirecta a los datos de un arreglo a través de una función.

### La clase span

Un objeto de tipo `span` es una clase que proporciona una vista ligera
sobre una secuencia contigua de objetos. El `span` proporciona una forma
segura de iterar, recorrer y de indexar los objetos que se almacenan en
arreglos estilo C, vectores y objeto de tipo `array`.

``` C++
#include <algorithm>       // sort
#include <iostream>        // cout
#include <span>            // span

// Invierte los elementos de un rango
void invertir(std::span<int> rango);

// Imprime el contenido de un rango
void print(std::span<int> rango);

// La clase span
int main() 
{
    int arreglo[] = {0, 1, 2, 3, 4, 5, 6, 7, 8, 9};
    print(arreglo);            // 0 1 2 3 4 5 6 7 8 9 
    invertir(arreglo);       
    print(arreglo);            // 9 8 7 6 5 4 3 2 1 0 

    return EXIT_SUCCESS;
}    

// Invierte los elementos de una secuencia usando sort()
void invertir(std::span<int> rango) 
{
    std::sort(std::begin(rango), std::end(rango), std::greater<>());
}

// Imprime el contenido de una secuencia de enteros
void print(std::span<int> rango) 
{
    for (const auto& el : rango) {
        std::cout << el << ' ';
    }

    std::cout << '\n';
}
```

La clase `span` es una plantilla que describe una secuencia de elementos
almacenados de forma contigua en la memoria como un arreglo o un vector.
Esta plantilla de clase se puede usar para encapsular cualquier
secuencia de elementos, como es el caso de las funciones que esperan un
puntero al primer elemento de un arreglo.

``` C++
// La clase span es un clase ligera que se puede pasar por valor, por que
// solo almacena un puntero y el número de elementos de la secuencia. 
void print(std::span<int> rango); 
```

A diferencia de las clases `array` o `vector`, un `span` no \"posee\"
los elementos que contiene. Tampoco libera ningún elemento almacenado
porque no almacena nada, solo proporciona una vista que se puede usar
para acceder y modificar los elementos de una secuencia.

``` C++
int arreglo[] = {0, 1, 2, 3, 4, 5, 6, 7, 8, 9};

// Un tipo span es una alternativa segura y ligera para 
// acceder a una secuencia de objetos que se van a devolver 
// mediante un puntero y un índice.
std::span<int> vista = arreglo;

for (int& num : vista) {
    num *= 2;                 // Duplica cada valor del arreglo
    std::cout << num << ' ';
}
```

La asignación realiza una copia superficial del puntero de datos y del
tamaño del arreglo. Una copia superficial es segura porque `span` no
asigna memoria:

``` C++
std::cout << vista.size() << std::endl;   // 10
```

La función miembro `size()` de la clase `span` devuelve el número de
elementos que contiene el arreglo.

### Referencias a valores-r

Cuando una función modifica un argumento que se pasa por referencia,
modifica el objeto original, no una copia local del objeto. Para evitar
que una función modifique este tipo de argumento, es necesario calificar
el parámetro como `const &`, sin embargo las referencias solo pueden
tomar valores-l.

A partir de C++11 para controlar explícitamente los argumentos que se
pasan por referencia a valor-l o referencias a valor-r, se usa el signo
doble `&&` en el parámetro para indicar que el valor que espera una
función es una referencia a un valor-r, en el contexto de las plantillas
se denominan referencias universales (o de reenvio):

``` C++
#include <iostream>               // cout
#include <string>                 // string
#include <utility>                // move

// Función que acepta una referencia a un valor-l
void fun(std::string& str) 
{
    std::cout << "modificar(" << str << ")\n";
}

// Función que acepta una referencia constante a un valor-l
void fun(const std::string& str) 
{
    std::cout << "copiar(" << str << ")\n";
}

// Función que acepta una referencia a un valor-r
void fun(std::string&& str) 
{
    std::cout << "mover(" << str << ")\n";
}

// Referencias y funciones
int main() 
{
    std::string s1{"std::string& str"};
    const std::string s2{"const std::string& str"};

    fun(s1);                       // modificar(std::string& str)
    fun(s2);                       // copiar(const std::string& str)
    fun("string&&");               // mover(string&&)

    // Esto invoca a fun(std::string&& str), sin esta
    // sobrecarga invoca a fun(const std::string& str).
    fun(std::move(s1));            // mover(string&&)

    // Las referencias a valores-r son valores-l  
    // cuando se usan en expresiones. 
    std::string&& s3 = "valor-r";
    fun(s3);                        // modificar(valor-r)
    fun(std::move(s3));             // mover(valor-r)

    return EXIT_SUCCESS;
}
```

Las referencias a valores-r se usan para extender el tiempo de vida de
un objeto temporal.

``` C++
// Toma la dirección del objeto temporal.
// La clase string tiene un operador de asignación por movimiento
std::string&& s3 = "valor-r";
```

El operador de referencia `&` requiere un valor-l como entrada, porque
solo un valor-l tiene una dirección que se puede guardar. En cambio el
operador `&&` puede tomar valores-r como entrada. Una referencia a un
valor-r es una referencia que sirve para tomar los recursos de un objeto
temporal o de un objeto que está a punto de ser destruido o que está a
punto de salir de su ámbito. Para distinguir entre estos dos tipos, el
estandar los llama referencias a valor-l `&`, y referencias a valor-r
`&&`.

:::: note
::: title
Note
:::

**Copia y movimiento**

La función `move` transfiere la propiedad de un objeto a otro sin hacer
una copia. Algunas clases son propietarias de recursos como memoria,
identificadores y otros elementos. Cuando una clase es propietaria de
recursos, se puede definir un constructor de movimiento y un operador de
asignación por movimiento. El compilador usa estos miembros especiales
durante la resolución de sobrecarga. Los contenedores de la biblioteca
estándar invocan al constructor de movimiento en objetos que se mueven e
invocan al constructor de asignación por copia en objetos que se copian.
::::

No se puede pasar una referencia `&` (valor-l), a una función que espera
una referencia `&&` (valor-r). Un valor-r se deve pasar moviéndolo
(usando `move`) o reenviándolo (usando `forward`) explícitamente. La
función `move()` está disponible en la cabecera `<utility>`:

``` C++
fun(std::move(s3));             // Bien, move() devuelve un valor-r 
```

Una referencia a un valor-r se comporta igual que una referencia a un
valor-l, excepto que el valor se enlaza a un valor temporal. Las
referencias a valores-r se introdujeron en C++ para tratar con la
semántica de movimiento que esta diseñada para que un objeto \"mueva\"
sus recursos a otro objeto, dejando el objeto en un estado vacío, aunque
valido.

## Funciones en línea

Una función en línea es una función definida con la palabra clave
`inline`.

``` C++
// Calcula el factorial de un número entero
inline int factorial(int n)
{
    return (n < 2 ) ? 1 : n * factorial(n − 1);
}
```

El especificador `inline` le indica al compilador que inserte una copia
del cuerpo de la función en cada uno de los lugares en los que se
invoque la función.

Las funciones `inline` están sujetas a optimizaciones de código que no
están disponibles para las funciones normales. El uso de estas funciones
puede agilizar la ejecución de un programa porque eliminan la sobrecarga
asociada a la llamada de la función. Sin embargo la llamada en línea
solo se produce si el análisis de costos y beneficios del compilador
muestra que merece la pena.

:::: warning
::: title
Warning
:::

El uso de funciones `inline` crea archivos ejecutables más grandes ya
que el código para la función tiene que repetirse varias veces.
::::

A partir de C++ 17, este comportamiento también se aplica a variables.

``` C++
#ifndef MATH_H
#define MATH_H
#include <atomic>

// Un archivo de cabecera: math.h
namespace math 
{
    // Las funciones incluidas en varias fuentes
    // pueden ser calificadas como inline
    inline int factorial(int n)
    {
        return (n < 2 ) ? 1 : n * factorial(n - 1);
    }

    // Las variables con enlace externo incluidas en 
    // varias fuentes pueden ser calificadas como inline
    inline std::atomic<int> contador(5);
}

#endif
```

Las funciones y variables calificadas como `inline` eliminan el
obstáculo de empacar código (declarar y definir funciones por separado)
en archivos de cabecera.

``` C++
#include <iostream>
#include "math.h"

// Calcula el factorial de un número entero
int main()
{
    int num = math::contador;                        // 5
    std::cout << math::factorial(num) << std::endl;  // 120

    return EXIT_SUCCESS;
}
```

En el caso de las llamadas a funciones, el uso de `inline` provoca que
el compilador sustituya la llamada a la función por el código que
conforma su cuerpo. Este modificador es especialmente relevante para
incrementar el rendimiento de una aplicación, sobre todo en el caso de
variables o funciones que se utilizan con gran frecuencia y que además
son pequeñas. El compilador trata las funciones `inline` como
prioridades. No hay ninguna garantía de que las funciones se insertarán
en línea. Y no se puede forzar al compilador para que inserte una
función `inline` determinada. La posibilidad de que las funciones en
línea dependan de alguna entrada de información no especificada en
tiempos de compilación, hace imposible garantizar que cada llamada de
función marcada como `inline` realmente lo sea.

Si necesitas la garantía para que un valor sea calculado en tiempo de
compilación, es necesario usar la palabra clave `constexpr` o
`consteval` y asegurarte de que todas las funciones y variables usadas
en la evaluación también sean también `constexpr`.

## Funciones en tiempo de compilación

Se puede declarar una función como `constexpr` cuando el valor que
produce la función se puede determinar en tiempo de compilación.

``` C++
// Eleva al cubo un número entero
constexpr auto cubo(int num)
{
    return num * num * num;
 }
```

Una función `constexpr` puede devolver un valor constante que se puede
evaluar en tiempos de compilación.

``` C++
const int n1{3};                 // n1 es const
constexpr int n2{4};             // n2 es constexpr

constexpr auto c1 = cubo(n1);    // Bien: una expresión constexpr que se
static_assert(cubo(n1) == 27);   // puede evaluar en tiempo de compilación

const int c2 = cubo(n2);          // Bien: una expresión const que se
static_assert(c2 == 64);          // puede evaluar en tiempo de compilación

constexpr int c3 = cubo(n1 + 3); // Bien: una expresión constexpr que se
static_assert(c3 == 216);        // puede evaluar en tiempo de compilación

std::cout << c1 << '\n';         // 27
std::cout << c2 << '\n';         // 64
std::cout << c3 << '\n';         // 216
```

Para que una función declarada como `constexpr` se pueda evaluar en
tiempo de compilación los argumentos deben ser declarados como valores
constantes. La función `static_assert` evalúa una expresión en tiempo de
compilación. Esta función bloquea el proceso de compilación si la
comprobación no satisface los requisitos comprobados.

``` C++
int n1{3};                       // n1 no es constante
constexpr auto c1 = cubo(n1);    // Error: El valor de n1 no se puede
                                 // evaluar en tiempo de compilación.
```

Es un error usar argumentos no constantes para iniciar una función
`constexpr`. La razón es sencilla, el calificador `constexpr` solo se
puede asignar a una constante que puede ser determinada en tiempos de
compilación.

:::: warning
::: title
Warning
:::

Si un argumento no representa una expresión constante, significa que no
se puede utilizar como valor para un parámetro de función `constexpr`,
ni para definir el tamaño de un arreglo, ni dentro de la función
`static_assert`.

Para que una función se pueda evaluar en tiempo de compilación es
necesario que los valores usados como argumentos también se puedan
evaluar en tiempo de compilación como `constexpr`, `const` o literales.
::::

Lo interesante sobre las funciones `constexpr` es que se pueden usar
como funciones normales:

``` C++
int n3{5};                      // Bien: se puede evaluar en 
std::cout << cubo(n3);          // tiempos de ejecución.
```

Una función `constexpr` normalmente se ejecuta más rápido que una
función normal, ya que el resultado de la evaluación se calcula cuando
se compila el programa.

:::: note
::: title
Note
:::

En C++11 para que una función pueda ser evaluada en tiempos de
compilación, la función debe ser convenientemente simple, debe contener
una declaración `return`. Además no deve producir efectos secundarios.

A partir de C++14, cualquier función puede ser calificada como
`constexpr`, solo se excluyen las funciones declaradas como `static`, o
que usen sentencias `try`.
::::

Una función `constexpr` permite la recursión y el uso de expresiones
condicionales. Al igual que las funciones en línea, las funciones
`constexpr` obedecen la regla de una sola definición.

### Funciones noexcept

Además de los calificadores `inline` y `constexpr` una función puede
usar la palabra clave `noexcept`, `noexcept` es una versión mejorada de
`throw()` que le permite al compilador implementar excepciones sin la
sobrecarga en tiempos de ejecución que impone `throw()`. Este
especificador es parte del tipo, así que puede aparecer como parte del
declarador de una función.

Por ejemplo esta declaración de función le informa al compilador que la
función `dividir` puede lanzar excepciones:

``` C++
// La función dividir puede lanzar excepciones.
double division(double x, double y) noexcept(false);
```

Y es igual a esta otra que no usa el especificador `noexcept`:

``` C++
// La función dividir puede lanzar excepciones.
double division(double x, double y);
```

De forma predeterminada, todas las funciones en C++ pueden y
eventualmente lanzan excepciones en tiempos de ejecución.

Para informarle al compilador que una función no lanza excepciones es
necesario usar el operador `noexcept`. Este operador se puede usar solo,
o como una expresión. Por ejemplo, esta declaración le dice al
compilador que la función dividir no lanza excepciones.

``` C++
// La función dividir no lanza excepciones.
double division(double x, double y) noexcept(true);
```

Y es igual a esta otra:

``` C++
// La función dividir no lanza excepciones.
double division(double x, double y) noexcept;
```

El operador `noexcept` realiza una comprobación en tiempo de compilación
de la expresión usada entre paréntesis. Devuelve `true` si la expresión
(evaluada en tiempos de compilación) se evalúa como verdadera.

``` C++
#include <iostream>

// El operador noexcept realiza una comprobación en tiempo de
// compilación, devuelve true si la expresión no lanza excepciones. 
int main()
{
    void fun1();               // Puede lanzar excepciones
    void fun2() noexcept;      // No puede lanzar excepciones

    std::cout << std::boolalpha
              << "fun1(); se evalúa como:          "  
              << noexcept(fun1()) << '\n'                // false
              << "fun2() noexcept; se evalúa como: " 
              << noexcept(fun2()) << '\n';               // true 

    return EXIT_SUCCESS;
}
```

El especificador `noexcept` es solamente una \"función\" que le informa
al compilador que una función puede eventualmente lanzar o no una
excepción, para que el compilador pueda realizar ciertas optimizaciones
en tiempos de compilación. Su valor predeterminado es `true`, esto
significa que cuando la expresión no usa los paréntesis una función
declarada como `noexcept` no puede lanzar excepción.

``` C++
#include <iostream>

// Lanza una excepción de tipo "out_of_range" si el valor de b es 0.
double div1(double a, double b);

// No lanza excepciones, solo muestra un mensaje de error
double div2(double a, double b) noexcept;

// Usando el declarador noexcept
int main()
{
    // La función div2 no lanza excepciones, solo muestra un error
    std::cout << div2(2, 0) << std::endl;

    // Atrapa la excepción y muestra un mensaje de error
    try {
        std::cout << div1(2, 0) << std::endl;
    } catch (const std::out_of_range& error) {
        std::cout << error.what() << '\n';
    }

    return EXIT_SUCCESS;
}

// Lanza una excepción si el valor de b es 0
double div1(double a, double b)
{
    if (b == 0) {
        throw std::out_of_range("Excepción: No se puede dividir entre 0.");
    }

    return a / b;
}

// La función no lanza excepciones, solo muestra un mensaje de error
double div2(double a, double b) noexcept
{
    if (b == 0) {
        std::cerr << "Error: No se puede dividir entre 0." << std::endl;
        // Podemos terminar el programa usando:
        // std::exit(1);
        // o podemos devolver un código de error.
    }

    return a / b;
}
```

Una función que no lanza excepciones, se deve declarar como `noexcept`.

``` C++
// No lanza excepciones
double div2(double a, double b) noexcept
```

Este tipo de función, devuelve el control al código que la llamo usando
un mensaje de error o un valor (o un código de error), este es el
comportamiento de las funciones estilo C.

``` C++
// Puede lanzar una excepción
double div1(double a, double b);
```

Las funciones que lanzan excepciones, se pueden usar dentro de código
que puede atrapar la excepción (y hacer algo con ellas):

``` C++
// Atrapa la excepción si b es 0.
try {
    std::cout << div1(2, 0) << std::endl;
} catch (const std::out_of_range & e) {
    std::cout << e.what() << '\n';
}
```

Cuando una función lanza una excepción, se puede atrapar la excepción
dentro de bloques `try-catch`. En este caso la función lanza una
excepción de tipo \"out_of_range\" si el valor de `b` es 0.

:::: tip
::: title
Tip
:::

En C++ se usan las excepciones en lugar de los códigos de error como la
forma preferida de notificar y controlar las condiciones de error que
pueden surgir durante la ejecución de un programa.
::::

El especificador `noexcept` es una versión mejorada de `throw()` cuyo
uso ha sido desaprobado en C++.

## Funciones consteval

C++ 20 introduce dos palabras clave: `consteval` y `constinit`.

1.  La palabra clave `consteval` se utiliza para declarar funciones que
    pueden ser evaluadas en tiempo de compilación. Esto significa que la
    función siempre se ejecutará durante la compilación y no en tiempo
    de ejecución.
2.  La palabra clave `constinit` se utiliza para asegurar que una
    variable con duración de almacenamiento estática o de hilo sea
    inicializada en tiempo de compilación.

Aunque ambas palabras sirven para declarar valores constantes su uso es
muy diferente.

### La palabra clave consteval

La palabra clave `consteval` antes del nombre de una función crea una
\"función inmediata\".

``` C++
// Función inmediata
consteval tipo nombre(tipo args) 
{
    // código de la función
}
```

La invocación de una función inmediata crea un valor constante que se
puede utilizar en tiempo de compilación. Es decir, una función inmediata
se ejecuta en tiempo de compilación.

``` C++
#include <iostream>                  // cout

// Eleva al cubo el valor de 'num'
consteval auto cubo(int num)
{
    return num * num * num;
}

// Usando una función inmediata
int main()
{
    const int num1{3};                // num1 es constante
    constexpr int num2{4};            // num2 es constexpr

    static_assert(cubo(3) == 27);     // Se evalúa en tiempo de compilación

    const auto res1 = cubo(num1) ;    // Bien: una expresión constante que
    static_assert(res1 == 27);        // se evalúa en tiempo de compilación.

    constexpr auto res2 = cubo(num2); // Bien: una expresión constexpr que
    static_assert(res2 == 64);        // evalúa en tiempo de compilación.

    return EXIT_SUCCESS;
}
```

En el primer caso, `3` es un valor literal que se puede usar como una
expresión constante.

``` C++
cubo(3);                        
```

En el segundo caso `num1` es una constante que se puede usar como
argumento para la función, ya que el valor se puede evaluar en tiempo de
compilación.

``` C++
const int num1{3}; 
cubo(num1);
```

En este caso la palabra clave `const` se asegura que el valor de la
variable no pueda cambiar después de su inicialización. En el tercer
caso `num2` es `constexpr` esto significa que se puede evaluar en tiempo
de compilación.

``` C++
constexpr int num2{4};
cubo(num2);
```

La palabra clave `constexpr` se utiliza para declarar variables y
funciones que pueden ser evaluadas en tiempo de compilación. Sin
embargo, no garantiza que todos los valores se puedan evaluar en tiempo
de compilación.

:::: tip
::: title
Tip
:::

A diferencia de las funciones declaradas como `constexpr` una función
declarada como `consteval` solo se puede usar con valores conocidos en
tiempo de compilación, mientras que una función `constexpr` se puede
usar con valores conocidos en tiempo de compilación, así como valores
obtenidos en tiempo de ejecución.
::::

:::: warning
::: title
Warning
:::

La palabra clave `consteval` no se puede aplicar a destructores ni
funciones que alojan o desalojan memoria. Cuando se declara o define una
función solo se puede usar uno de los dos especificadores: `consteval` o
`constexpr`.
::::

Sin embargo, una variable no se puede usar como una expresión constante.
Por ejemplo, esto no es válido:

``` C++
// Error: el valor de 'num3' no se puede usar en una expresión constante
int num3{5};
cubo(num3);      // Error: 'cubo(num3)' no es una expresión constante
```

La variable `num3` no es una expresión constante, en consecuencia la
llamada no se puede evaluar en tiempo de compilación y el compilador
genera un error. Una función inmediata es una función implícitamente
`inline` y tiene los mismos requerimientos que una función `constexpr`.

- Puede tener bucles y condiciones.
- Puede tener más de una instrucción.
- Puede invocar funciones `constexpr`.
- Puede usar variables que hayan sido inicializadas con expresiones
  constantes.

Sin embargo no puede tener:

- Datos estáticos o de tipo `thread_local`
- Usar bloques `try` `catch`.
- Invocar o usar funciones no `consteval` o `constexpr`.

Todas las dependencias de una función inmediata se deben de resolver en
tiempo de compilación.

### La palabra clave constinit

La palabra clave `constinit` se utiliza para garantizar que una variable
con duración de almacenamiento estático o con almacenamiento basado en
hilos sea inicializada en tiempo de compilación. Esto es útil para
asegurar que la variable esté correctamente inicializada antes de su
primer uso.

``` C++
#include <chrono>       // seconds
#include <iostream>     // cout
#include <thread>       // this_thread

// Una variable con almacenamiento estático
constinit const static int total{10};

// La palabra clave constinit
int main()
{ 
    // Una variable con almacenamiento de hilos
    constinit thread_local int num{1};

    // Muestra un mensaje cada 1 segundo
    while (num <= total) {
        std::this_thread::sleep_for(std::chrono::seconds{1});
        std::cout << "Tarea: " << num << " de " << total << '\n';
        ++num;
    }

    return EXIT_SUCCESS;
}
```

Las variables `thread_local` tienen un almacenamiento basado en hilos.
Estas variable son creadas la primera vez que las usa un hilo y su
tiempo de vida queda ligado al tiempo de vida del hilo al que
pertenecen. Las variables `static` tienen un almacenamiento que inicia
cuando el programa las evalúa y termina cuando el programa finaliza.

La palabra clave `constinit` declara que una variable tiene
inicialización estática, es decir, inicialización cero e inicialización
constante, de lo contrario el programa está mal formado.

#### Inicialización a cero

La inicialización a cero ocurre cuando una variable se inicializa con el
valor cero. Esto se puede hacer explícitamente o implícitamente. Por
ejemplo:

``` C++
int a = 0;            // Inicialización explícita a cero
int b{};              // Inicialización implícita a cero
```

En el caso de `int b{}`, la variable `b` se inicializa a cero
automáticamente. Esto es útil en tipos de datos que no son triviales,
como estructuras o clases, donde todos los miembros se inicializan a
cero.

#### Inicialización constante

La inicialización constante se refiere a la inicialización de una
variable con un valor constante que se conoce en tiempo de compilación.
Esto significa que el valor de la variable no cambiará durante la
ejecución del programa. Por ejemplo:

``` C++
const int c = 1;      // Inicialización constante
```

En este caso, `c` es una constante y su valor es 1. No se puede
modificar después de su inicialización.

## Funciones recursivas

La recursión es una técnica de programación en la que una función se
llama a sí misma para resolver un problema. Este enfoque se utiliza para
descomponer un problema complejo en subproblemas más pequeños y
manejables. La recursión funciona usando una condición base. Este es el
caso más simple que se puede resolver directamente sin usar llamadas
recursivas. La función se llama a sí misma con un argumento modificado,
acercándose cada vez más a la condición base.

La recursión puede simplificar la solución de problemas que tienen una
estructura repetitiva como los fractales y algoritmos de búsqueda. Sin
embargo, puede ser menos eficiente en términos de tiempo y espacio, ya
que cada llamada recursiva añade un objeto a la pila de llamadas, lo que
puede llevar a un desbordamiento de pila si la recursión es demasiado
profunda

### Usando funciones recursivas

Las funciones recursivas poseen tres características.

1.  **Recursión**: la función se llama a sí misma (usando su nombre).
2.  Las funciones recursivas contienen al menos **una condición** que
    las detiene. En cada llamada la función comprueba esta condición, si
    no se cumple; la función se vuelve a llamar a sí misma.
3.  Un **caso base**: Es el objetivo de la condición, por lo general se
    coloca dentro de una condición. Una vez que se cumple la condición,
    el caso base se ejecuta y la función termina.

Una función recursiva es una función que se llama a si misma:

``` C++
#include <iostream>              // cout
#include <iomanip>               // setw

// Calcula la secuencia de delannoy
int delannoy(int n, int k) 
{   // El caso base  
    if ((n == 0) || (k == 0)) {  
        return 1;
    }

    // Función que se llama a si misma de forma recursiva
    return delannoy(n - 1, k) +
           delannoy(n, k - 1) +
           delannoy(n - 1, k - 1);
}

// Imprime el triángulo de tribonacci
int main()
{
    std::size_t filas {10};
    std::cout << "El triángulo de Tribonacci\n"
              << "Introduce el número de filas: ";
    std::cin >> filas;        

    for (std::size_t k = 0, j = filas; k < filas; ++k, --j) {
        std::size_t n = 0;
        const std::size_t limite = k;
        std::cout << std::setw(j * 3) << std::left << ' ';
        while (n <= limite) {
            std::cout << std::setw(6) << delannoy(k - n, n);
            ++n;
        }
        std::cout << '\n';
    }
}
```

Muchos problemas que pueden ser resueltos usando bucles (o iteración),
también pueden resolverse usando recursión por ejemplo la sucesión de
tribonnacci, presentada al inicio de este capítulo, se puede calcular de
forma recursiva:

``` C++
#include <iostream>

// Calcula la serie de tribonacci de forma recursiva.
int tribonacci(int num);

// Imprime la serie de tribonacci.
void serie_de_tribonacci(int limite);

// La serie de tribonacci
int main()
{
    const int limite = 20;
    serie_de_tribonacci(limite);

    return EXIT_SUCCESS;
}

// Calcula la serie de tribonacci de forma recursiva
int tribonacci(int num)
{
    if (num < 3) {
        return 0;
    }
    if (num == 3) {
        return 1;
    }
    return tribonacci(num - 1) +
           tribonacci(num - 2) +
           tribonacci(num - 3);        
}

// Imprime la serie de tribonacci.
void serie_de_tribonacci(int limite)
{
    for (int i = 1; i <= limite; i++) {
        std::cout << tribonacci(i) << ' ';
    }
}

// 0 0 1 1 2 4 7 13 24 44 81 149 274 504 927 1705 3136 5768 10609 19513 
```

La recursión es útil en operaciones de ordenamiento y acoplamiento.
Ordenar datos puede ser un proceso recursivo en el cual el mismo
algoritmo se aplica a un conjuntos de datos cada vez más pequeño. Un
ejemplo es el conocido algoritmo por ordenamiento llamado `quicksort`.

## Funciones abreviadas

A partir de C++ 20 se puede usar la palabra clave `auto` para que el
compilador deduzca los parámetros de una función. Semánticamente una
función que usa la palabra clave `auto` y una plantilla de función que
usa la palabra clave `template` son completamente equivalentes. La única
diferencia es la sintaxis más corta de las funciones abreviadas.

Antes de C++ 20, solo se podía usar la palabra clave `auto` como un
marcador de posición para el valor devuelto, pero a partir de C++ 20, se
puede usar la palabra clave `auto` para pasar parámetros a una función.
La sintaxis es una extensión natural del lenguaje usado para deducir el
tipo de los parámetros, a si como el valor devuelto por una función.

### Deducción de tipos

Se puede usar la palabra clave `auto` para que el compilador deduzca el
tipo de un parámetro de forma automática.

``` C++
#include <iostream>                                  // cout

// Devuelve un valor entre el máximo y el minimizo
auto clamp(auto minimo, auto maximo, auto valor) 
{
    return std::min(std::max(valor, minimo), maximo);
}

// Devuelve un valor entre un valor máximo y mínimo 
int main()
{ 
    std::cout << std::clamp(0, 255, 300)   << '\n'    // 255
              << std::clamp(0, 255, 120)   << '\n'    // 120
              << std::clamp(0.0, 1.0, 0.5) << '\n'    // 0.5 
              << std::clamp(0.0, 1.0, 1.5) << '\n';   //   1

    return EXIT_SUCCESS;
}
```

La palabra clave `auto` es un marcador de posición que le pide al
compilador que deduzca de forma automática el valor de una expresión.
Esto evita la sobrecarga de funciones, ya que una misma función puede
aceptar diferentes tipos que se pueden evaluar de forma similar.

``` C++
auto num1 = clamp(0, 252, 120);             // El tipo devuelto es int 
auto num2 = clamp(0.0, 1.0, 1.5);           // El tipo devuelto es double
```

Aunque la función `clamp` no usa la palabra clave `template` que se usa
para declarar plantillas, la función es en realidad una plantilla. El
estandar llama a este tipo de sintaxis \"plantillas de función
abreviada\".

Una función abreviada puede ser instanciada usando corchetes, al igual
que las plantillas:

``` C++
auto num1 = clamp<int>(0, 252, 120);         // El tipo devuelto es int 
auto num2 = clamp<int>(0.0, 1.0, 1.5);       // El tipo devuelto es int
```

Los parámetros de una función declarados con `auto` pueden usar
declaradores como `auto*`, `auto&`, y `const auto&` para deducir
punteros, referencias y referencias constantes respectivamente.

:::: note
::: title
Note
:::

**La palabra clave auto**

La palabra clave `auto` es una palabra reservada y heredada de C. En
versiones anteriores de C++, se podía usar para indicar explícitamente
que una variable tiene una duración de almacenamiento automática. A
partir de C++11, la palabra clave `auto` es un declarador que deduce
tipos a partir de una declaración con inicialización:

- Si el valor inicial es un valor literal deduce el tipo (no constante)
  del valor literal.
- Si el valor inicial es una referencia, la ignorá.
- Si el valor inicializador es calificado con `const` o `volatile`, los
  ignora, si se aplican sobre el objeto (calificadores de primer nivel).
- Si el inicializador es una expresión deduce el tipo (no constante) que
  resulte de evaluar la expresión (tomando en cuenta los puntos
  anteriores).
- Si el inicializador es una función, deduce el tipo devuelto por la
  función (tomando en cuenta los puntos anteriores).

La manera en que funciona `auto` hace que sea imposible deducir
referencias, a menos que se definan explícitamente.

``` C++
int num = 23;
auto fun() { return num; }        // El tipo es int
const auto& fun() { return num; } // El tipo es const int&
```
::::

El hecho de que `clamp` sea una plantilla implica que el compilador
necesita acceder a su definición en cada unidad de compilación en que se
invoque.

``` C++
#include <iostream>

// Declaración de función
auto clamp(auto minimo, auto maximo, auto valor);

// Error: Uso de la función antes de la deduccion de tipo.
int main()
{
    std::cout << clamp(0, 2550, 100);        
    return EXIT_SUCCESS;
}

// Definición de función
```

La declaración y definición de una función que devuelve un valor de tipo
`auto` deve de ocurrir antes de invocar la función, por que el
compilador deve inferir el tipo devuelto por la función antes de
invocarla de lo contrario se genera un error en tiempos de compilación.
Esto también ocurre con las plantillas.

### Plantillas de función variádica

El uso de la palabra clave `auto` también se puede usar para crear
plantillas de función variádicas:

``` C++
#include <iostream>        // cout

// Función variádica
auto sumar(auto ...args)
{
    std::cout << __PRETTY_FUNCTION__ << std::endl;
    return (... + args);
}

// Suma la lista de argumentos pasados a la función
int main()
{
    std::cout << sumar(10, 20)        << '\n'  // 30
              << sumar(10.5, 1.2)     << '\n'  // 11.7
              << sumar(1, 2, 3, 4, 5) << '\n'; // 15

    return EXIT_SUCCESS;
}
```

Una plantilla de función variádica es una función que acepta al menos un
paquete de parámetros.

``` C++
// La función acepta un paquete de parámetros llamado args
auto sumar(auto ...args);
```

Un paquete de parámetros se declara usando puntos suspensivos. A la
izquierda del nombre significa que el nombre es un paquete de
parámetros.

``` C++
// El compilador expande el paquete de parámetros y suma 
// cada uno de los elementos usando un plegado unario.
return (... + args);
```

A la derecha del nombre significa que el nombre es un patrón que le dice
al compilador que expanda el paquete de parámetros en nombres
independientes. El plegado unario se utiliza para reducir un paquetes de
parámetros aplicándole un operador específico, en este caso se usa el
operador suma.

:::: tip
::: title
Tip
:::

La macro `__PRETTY_FUNCTION__` imprime el nombre de la función, así como
el número de argumentos pasados a la función, esta macro es útil para
ver los tipos de los argumentos pasados en la invocación de la función.
::::

Las plantillas variadicas toman de C, el uso de los puntos suspensivos
para denotar cero o más argumentos en el intento por introducir un
remplazo de tipo seguro para las funciones variadicas que usan punteros.
Un paquete de parámetros proporciona una forma segura de pasarle una
lista arbitraria de diferentes tipos a una función o a una plantilla.

### Usando decltype

Otra forma de deducir el valor de retorno de una función es usando la
expresión `decltype(auto)` para pedirle al compilador que infiera
(automáticamente) el tipo que devuelve la función.

``` C++
#include <iostream>                         // cout

// Divide dos numeros
decltype(auto) dividir(auto a, auto b)
{
    if (b == 0) {
        throw std::out_of_range("Excepción: No se puede dividir entre 0.");
    }

    return a / b;
}

// Deducción de tipo usando decltype(auto)
int main()
{
    std::cout << dividir<double>(3, 5) << '\n'   // 0.6
              << dividir(3.0, 5)       << '\n'   // 0.6
              << dividir(5.0, 3.0)     << '\n';  // 1.66667

    return EXIT_SUCCESS;
}
```

En C++11, `auto` es un tipo de valor válido que indica al compilador que
infiera el tipo a partir de la instrucción `return`. A partir de C++ 14,
se puede usar la expresión `decltype(auto)` que a diferencia de `auto`
puede inferir el tipo de una referencia o un puntero.

Cuando el tipo es devuelto por una expresión `decltype(auto)`, el valor
es obtenido pasando el valor devuelto por la función en la expresión
`decltype()`

``` C++
int num = 23;

// El tipo devuelto es int, igual que decltype(x)
decltype(auto) fun1()  
{ 
    return num; 
}  

// El tipo devuelto es int&, igual que decltype((x))
decltype(auto) fun2()  
{ 
    return(num); 
}
```

Solo se puede usar la expresión `decltype(auto)` es un error usar otros
tipos de especificadores como `const decltype(auto)&`. Además,
`decltype()` es un declarador que copia el tipo de algo existente:

- Si entre los paréntesis hay un valor literal, deduce el tipo (no
  constante).
- Si entre los paréntesis hay una expresión o una función, deduce el
  tipo que resulta de evaluar la expresión sin evaluarla realmente o si
  es una función sin invocarla.
- Si entre los paréntesis hay una expresión entre paréntesis, la trata
  como un valor-l y deduce la referencia.

En realidad `decltype` está pensado para usarse en contextos complejos,
o para cuando el tipo de un parámetro depende de los valores pasados a
una plantilla.

### Tipo de retorno al final

Tradicionalmente, en C++, el tipo de retorno de una declaración de
función se escribe antes del nombre de la función. Sin embargo, una
declaración de función también se puede escribir usando el tipo de valor
devuelto, después de la lista de argumentos. Por ejemplo las siguientes
dos declaraciones de función son equivalentes:

``` C++
// Convierte un tipo entero a cadena de texto
std::string convertir(int a);           // El valor devuelto como prefijo
auto convertir(int a) -> std::string;   // El valor devuelto como sufijo 
```

Un tipo de valor devuelto de forma \"normal\" se coloca en el lado
izquierdo de una función. Un tipo de valor devuelto \"al final\" se
coloca en el lado derecho del nombre de la función y es precedido por el
operador flecha `->`.

``` C++
#include <iostream>      // cout
#include <string>        // string

// Convierte un tipo entero a cadena de texto (Declaración)
auto convertir(int a) -> std::string;

// Convierte un valor entero en una cadena de texto
int main()
{
    std::string txt = convertir(23);
    std::cout << txt << std::endl;        
    return EXIT_SUCCESS;
}

// Convierte un tipo entero a cadena de texto (Definición)
auto convertir(int a) -> std::string
{
    return std::to_string(a);
}
```

Esta forma de declarar funciones es útil en declaraciones de funciones y
plantillas, en donde el tipo devuelto por la función depende de los
argumentos de la plantilla o de los parámetros declarados con la palabra
clave `auto`.

El operador flecha, por lo general se usa con `decltype` para usar
parámetros en lugares en donde se pueden usar tipos específicos

``` C++
auto sumar(auto a, auto b) -> decltype(a + b) 
{ 
    return a + b; 
}
```

Hay mucha semejanza entre esta sintaxis y la sintaxis usada en
expresiones lambda. Sin embargo esta sintaxis se puede utilizar para
declarar cualquier función, en especial cuando se usa la palabra clave
`auto`.

### Conceptos y funciones

Los conceptos son una mejora al sistema de plantillas de C++. Pero su
funcionalidad no queda limitada al ámbito de las plantillas, también
pueden ser usados en otros contextos como funciones y variables.

``` C++
#include <concepts>                 // integral
#include <iostream>                 // cout

// El concepto integral es satisfecho únicamente si el tipo es un entero 
constexpr auto sq(std::integral auto val) { return val * val; }

// Eleva un valor al cuadrado
int main()
{
    std::cout << sq(3)   << '\n';   // 9         
    return EXIT_SUCCESS;
}
```

Los conceptos son como funciones que el compilador evalúa en tiempo de
compilación para determinar si un argumento satisface las restricciones
\"impuestas\" por el parámetro de una función.

``` C++
std::integral auto valor = 3;
std::cout << sq(3)     << '\n';      // 9
std::cout << sq(2*3)   << '\n';      // 36
```

Básicamente podemos usar un concepto en cualquier lugar en que se use la
palabra clave `auto`, `auto*` y `auto&`. Esto significa que podemos usar
conceptos para definir variables, deducir tipos y valores de retorno de
funciones, en expresiones lambda, funciones y plantillas.

Las restricciones son requisitos que se pueden imponer a un parámetro de
una función o plantilla. Como la existencia de un operador, un
constructor, o un tipo en específico para que una llamada de función sea
efectiva.

``` C++
// Error: Restricción no satisfecha.
// La función requiere de un tipo integral 
sq("3");
```

Comparado con los tipos incorporados, las restricciones de tipo
documentan el código, y si ocurren errores, estos son fáciles de
encontrar, ya que el compilador señala la definición de variable y el
lugar en donde se intento usar de forma incorrecta el tipo de una
variable (en tiempo de compilación).

Debido a la sintaxis abreviada de plantillas, una variable o función que
usa la palabra clave `auto` en el fondo es una plantilla. La funciones
que usan las palabra clave `auto` son \"funciones abreviadas\".
