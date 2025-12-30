# Tipos de datos

::: {.meta title="Tipos de datos simples" capitulo="3" description="Tipos de datos en C++ y forma de usarlos." keywords="tipos de datos simples"}
:::

La primera tarea de un programador es guardar los datos que usan sus
algoritmos en la memoria de la computadora. La información que se guarda
en el interior de la memoria se almacena en modo binario. Para la
computadora no hay diferencia entre los datos guardados en una posición
y otra. Ella solo ve unos y ceros. Es mediante el uso de declaraciones
como le decimos al compilador el modo en que queremos interpretar y
manipular la información (binaria) almacenada en la memoria.

Al valor de un dato que puede ser representado y manipulado en un
lenguaje de programación se le conoce como tipo y a la acción de asignar
un tipo a un identificador se le llama tipificación. El tipo especifica
el valor que puede almacenar una variable y determina las operaciones
que se pueden realizar con esa variable. La tipificación es una de las
características esenciales en un lenguaje de programación
\"estáticamente tipado\" como C++. La noción de tipos estáticos y la
verificación de tipos en tiempos de compilación es fundamental para el
rendimiento, la mantenibilidad y la expresividad propia del lenguaje.

Este capítulo describe la forma de usar los tipos incorporados en el
lenguaje.

## Datos

Para leer un dato, necesitamos leerlo de alguna parte, es decir;
necesitamos saber de qué \"lugar\" de la memoria (de la computadora)
podemos leer algo. En C++, a este \"lugar\" se le llama objeto. El
término \"objeto\", por lo tanto se refiere simplemente a una área de la
memoria en la cual se guarda (o almacena) un dato. C++ es un lenguaje
estáticamente tipado esto significa que el tipo de un valor deve
conocerse en tiempo de compilación.

### Guardando datos en variables

Para guardar un valor, devemos declarar primero el tipo de dato que
queremos guardar en esa variable.

``` C++
#include <iostream>                       // cout

/* 
* Según la historia, el inventor del ajedrez le pidió al rey un grano 
* de trigo por la primera casilla del tablero, dos para la segunda, 
* cuatro para la tercera, ocho para la cuarta y así sucesivamente 
* hasta llegar a la sexagésima y última casilla del tablero. 
*/
int main() 
{
    const int num_casillas{64};           // Número de casillas del tablero.
    unsigned long long granos_inicio{1};  // Número inicial de granos.   
    unsigned long long total_granos{0};   // El total de granos de trigo

    // En cada iteración, se suma el número actual de granos 
    for (int i = 0; i < num_casillas; ++i) {
        total_granos += granos_inicio;
        granos_inicio *= 2;
    }

    // Imprime los resultados obtenidos
    std::cout << "Según la historia, el inventor del ajedrez recibió: " 
              << total_granos << " granos de trigo." << std::endl;

    return EXIT_SUCCESS;                  // 18446744073709551615
}
```

En este ejemplo, se usa un tipo `int` para guardar el número de casillas
y se usa el tipo `long long` para almacenar el resultado del calculo,
que es un valor de 64 bit\'s. En cada iteración, se suma el número
actual de granos de trigo al total, y luego se multiplica por 2 para
obtener el número de granos de la siguiente casilla.

Podemos guardar un número entero en una declaración de tipo `int`
siempre y cuando el valor almacenado no sobrepase los límites mínimos o
máximos que puede almacenar el tipo.

``` C++
// Almacena un tipo entero
int num_casillas = 64;
```

Para enteros más grandes se puede usar el tipo `long long` que es el
tipo más grande que se puede almacenar en una variable de tipo entero:

``` C++
// Un tipo entero que almacena un valor de 64 bit's
unsigned long long total_granos = 18446744073709551615;
```

Cuando un tipo no puede guardar un valor, por que el valor es más grande
que el valor maximo que puede almacenar el tipo, ocurre un
**desbordamiento**. Un desbordamiento es un comportamiento indefinido.

### Acceso a objetos en memoria

Una vez que un objeto se ha guardado en la memoria podemos acceder al
objeto usando su nombre directamente o usando su dirección (de memoria)
de forma indirecta, mediante una referencia o a través de un puntero.

``` C++
#include <iostream>                       // cout 

// Diferentes formas de acceder a objetos guardados en la memoria
int main()
{
    int num{100};                         // Un objeto en memoria
    int num2 = num;                       // Copia el objeto
    int &ref = num;                       // Acceso por referencia
    int *ptr = &num;                      // Acceso por puntero

    std::cout << "Valor: "                << num  << '\n';
    std::cout << "Copia: "                << num2 << '\n';
    std::cout << "Referencia: "           << ref  << '\n';
    std::cout << "Dirección de memoria: " << ptr  << '\n';
    std::cout << "Puntero: "              << *ptr << '\n';

    return EXIT_SUCCESS;
}
```

Un objeto es como una \"caja\" en la memoria de la computadora. Una caja
en la que se pueden colocar, modificar o tomar objetos de un tipo
previamente declarado (o convertido explicitamente). Es un error acceder
a un objeto antes de ser declarado.

### Tipos de datos

C++ incorpora una colección de tipos básicos que incluyen números
enteros con diferentes tamaños, números reales, caracteres y valores
boléanos. También tiene tipos compuestos que vienen incorporados en la
biblioteca estándar. Ademas permite crear tipos de datos más complejos
usando clases y estructuras.

Los tipos simples, vienen incorporados en el lenguaje y las operaciones
que aceptan, las construye el compilador (como sumar, restar etc.) Los
tipos compuestos pueden incorporar a su vez otros tipos de datos como
tipos básicos, tipos compuestos, punteros y referencias. Los tipos
abstractos (TAD) son creados por el programador como los vectores y las
cadenas de texto de la biblioteca estándar. Es tarea del programador
agregar las operaciones que soportan estos objetos.

Los tipos de datos se pueden dividir en varias categorías:

``` C++
+--------------+
|Tipos de datos|
+------+-------+
       |
       |
+---------------+---------------+---------------+
|               |               |               |
v               v               v               v
+----+----+   +------+-----+   +-----+----+   +------+------+  
| Simples |   | Compuestos |   | Punteros |   | Referencias |
+----+----+   +------+-----+   +-----+----+   +------+------+
|               |                |               |
|               v                |               |
v            +--+--+             v               v
+----------->| TAD |<------------+---------------+
    +-----+

    Tipos de datos de C++
```

Por ejemplo:

``` C++
int num{100};             // num es un tipo entero (simple)
double decimal{0.9};      // decimal es un tipo double (simple) 
bool falso{false};        // falso es un tipo boleano  (simple)
struct Punto{ };          // Punto es un tipo compuesto (una estructura)  
class Arreglo{ };         // Arreglo es un tipo compuesto (una clase) 
enum Opciones{ }          // Opciones es un tipo compuesto (una enumeración)
```

Aparte de los tipos simples y compuestos, existe otra categoría que
incluye a los punteros y las referencias.

``` C++
Punto punto;             // punto es una intancia de Punto
Punto* ptr = &punto;     // ptr es un puntero a Punto 
Punto& ref = punto;      // ref es una referencia a punto
```

Los punteros y referencias también son tipos que pueden manipular datos,
solo que tienen una sintaxis un poco diferente.

## Tipos simples

Los tipos simples del lenguaje son tipos aritméticos como números
enteros, números reales, valores boléanos, caracteres y un valor
especial llamado `void` que se puede usar en algunas circunstancias.

``` C++
/
|
|                        | char     
|           | Sin signo <  wchar_t 
|           | (unsigned) |      
| Carácter  <           
|           | Con signo  | signed char    
|           | (signed)  <  signed wchar_t 
|                        |
|
|           |            | short         
|           | Sin signo <  int           
| Enteros  <             | long          
|           |            | long long
Tipos de    |           |                
datos      <           |            | signed short  
simples     |           | Con signo <  signed int    
|                        | signed long  
|                        | signed long long  
|
|                        | float         
|          | Con y sin  <  double          
| Reales  <    Signo     | long double   
|          |             | long long double   
|
| Boléano < bool
|          
| Vacío   < void
\      
```

Los tipos simples tienen una propiedad que los distingue de los tipos
compuestos, son indivisibles así que mantienen una relación de orden
entre sus elementos. Además se les pueden aplicar operaciones
relacionales, aritméticas y lógicas.

Existe un \"tipo de valor especial\" declarado por la palabra clave
`void`, `void` no es un tipo de dato porque no contiene un valor, pero
se puede utilizar por ejemplo, para indicarle al compilador que una
función no devuelve un valor.

### El tamaño de un tipo

El tamaño de un tipo de dato, es decir el numero de bits que ocupa en
memoria varia de una maquina a otra. El estándar garantiza un mínimo
requerido. El tamaño que ocupa el tipo `char`, el cual es normalmente de
1 byte (8 bit\'s). Sin embargo los compiladores pueden usar diferentes
tamaños en diferentes plataformas.

+-----------+---------+
| Tipo      | > Bytes |
+===========+=========+
| char      | > 1     |
+-----------+---------+
| bool      | > 1     |
+-----------+---------+
| wchar_t   | > 2     |
+-----------+---------+
| short     | > 2     |
+-----------+---------+
| int       | > 4     |
+-----------+---------+
| long      | > 8     |
+-----------+---------+
| float     | > 4     |
+-----------+---------+
| double    | > 8     |
+-----------+---------+

: Tipos simples y su tamaño

El estándar garantiza que un tipo `int` debe ser tan grande como un tipo
`short` (en una arquitectura de 16 bytes), un tipo `long` debe ser igual
que un tipo `int` (en una arquitectura de 32 bytes) y un tipo
`long long` debe ser al menos tan grande como un tipo `long` (en una
arquitectura de 64 bytes).

### Obteniendo el tamaño de un tipo

El tamaño de un tipo se obtiene usando el operador `sizeof` que devuelve
el tamaño de cualquier tipo o valor literal en bytes. Por ejemplo:

``` C++
#include <iostream>                                // cout

// El tamaño de algunos tipos de datos
int main()
{
    std::cout << "Tipo      " << "Tamaño"          << " \n"
              << "char      " << sizeof(char)      << " byte\n"
              << "int       " << sizeof(int)       << " bytes\n"
              << "short     " << sizeof(short)     << " bytes\n"
              << "long      " << sizeof(long)      << " bytes\n"
              << "bool      " << sizeof(bool)      << " bytes\n"
              << "float     " << sizeof(float)     << " bytes\n"
              << "double    " << sizeof(double)    << " bytes\n"
              << "long long " << sizeof(long long) << " bytes\n";

    return EXIT_SUCCESS;
}
```

El operador `sizeof` puede evaluar cualquier tipo de dato usando el
nombre entre paréntesis. También puede evaluar una expresión, incluso
puede evaluar valores literales como cadenas de texto.

``` C++
sizeof "hola"    // 5 bytes
```

Cuando se aplica el operador `sizeof` a una expresión los paréntesis son
opcionales. En estos casos la expresión no se evalúa. Solo se obtiene el
tamaño de almacenamiento usado.

#### Valores máximos y mínimos de un tipo

El valor máximo y mínimo de un tipo puede variar. Por lo general se toma
como base el tipo `char` (en específico la macro `CHAR_BIT`), que ocupa
8 bits (un byte). Todos los tipos simples del lenguaje, usan un mínimo y
un máximo que puede varíar entre diferentes implementaciones (o
compiladores).

Por ejemplo, estos son los límites máximos y mínimos definidos por las
macros de la cabecera `<limits>`:

``` C++
#include <iostream>     // cout
#include <climits>      // CHAR_BIT, CHAR_MAX, INT_MAX...

// Limites definidas en la cabecera climits
int main()
{
    std::cout << "Núm. de bits por byte..........." << CHAR_BIT  << '\n'
              << "Valor máximo de char............" << CHAR_MAX  << '\n'
              << "Valor mínimo de char............" << CHAR_MIN  << '\n'
              << "Valor máximo de short..........." << SHRT_MAX  << '\n'
              << "Valor mínimo de short..........." << SHRT_MIN  << '\n'
              << "Valor máximo de int............." << INT_MAX   << '\n'
              << "Valor mínimo de int............." << INT_MIN   << '\n'
              << "Valor máximo de long............" << LONG_MAX  << '\n'
              << "Valor mínimo de long............" << LONG_MIN  << '\n'
              << "Valor máximo de unsigned char..." << UCHAR_MAX << '\n'
              << "Valor máximo de unsigned short.." << USHRT_MAX << '\n'
              << "Valor máximo de unsigned int...." << UINT_MAX  << '\n'
              << "Valor máximo de unsigned long..." << ULONG_MAX << '\n';

    return EXIT_SUCCESS;
}
```

Los límites estándar se encuentran disponibles en los archivos de
cabecera `climits`, `cfloat` y `numeric_limits`. Además, la cabecera
`limits` contiene una clase de plantilla llamada `numeric_limits` que se
puede usar para obtener el tamaño de un tipo. Por ejemplo para obtener
el tamaño máximo y mínimo de un tipo entero:

``` C++
#include <iostream>                  // cout
#include <limits>                    // numeric_limits

// Devuelve el tamaño máximo y mínimo de un tipo entero 
int main()
{
    std::cout << "Valor mínimo de int: "
              << std::numeric_limits<int>::min() << '\n'
              << "Valor máximo de int: "
              << std::numeric_limits<int>::max() << '\n';

    return EXIT_SUCCESS;
}
```

La forma de usar la clase `numeric_limits` es proporcionándole un tipo
en medio de corchetes y llamando a las funciones estáticas `min()` y
`max()` usando el operador de resolución de alcance, para obtener el
valor mínimo y el valor máximo del tipo en específico.

### Modificadores de tipo

Existen dos modificadores llamados `unsigned` y `signed` que pueden
usarse en tipos aritméticos enteros para definir explícitamente el signo
del tipo. Un tipo `unsigned` denota valores sin signo o positivos,
mientras que un tipo `signed` puede almacenar ambos. Cuando se omite, el
tipo admite valores positivos y negativos.

``` C++
int numero{-100} ;            // entero con signo (implícito)
unsigned int numero1{100};    // entero sin signo (explícito)
signed int numero3{-100};     // entero con signo (explícito)
```

Existe un modificador llamado `long` que hace más \"largo\" un tipo
`int` o a más \"largo\" un tipo `double` y un modificador llamado
`short` que hace más \"corto\" un tipo `int`. Estos modificadores pueden
usarse solos o precediendo la palabra `int`.

``` C++
#include <iostream>                        // cout

// Modificadores de tipo
int main()
{
    unsigned short int num1{1};            // 2 bytes
    unsigned long int num2{1l};            // 4 bytes 
    unsigned long long int num3{1LL};      // 8 bytes 

    // Por lo general 2, 4, y 8 bytes
    std::cout << "unsigned int           " << sizeof(num1) << " bytes\n"
              << "unsigned long int      " << sizeof(num2) << " bytes\n"
              << "unsigned long long int " << sizeof(num3) << " bytes\n";

    return EXIT_SUCCESS;
}
```

Se permite cualquier orden: `unsigned short int` o `short int unsigned`
ambos se refieren al mismo tipo de dato.

#### Errores con modificadores de tipo

Hay que tener cuidado cuando se usan tipos con signo y sin signo:

``` C++
unsigned int numero{-100};     // Error de conversión en inicialización
```

Cuando se intenta usar un modificador con un dato de diferente signo,
ocurre un error de conversión. Con la inicialización uniforme (las
llaves) el compilador detiene el proceso de compilación.

Esto no sucede con los paréntesis:

``` C++
unsigned int numero(-100);     // Error: desbordamiento
```

ni con la asignación directa:

``` C++
unsigned int numero = -100;    // Error: desbordamiento 
```

En estos casos, se produce un error de desbordamiento aunque el
compilador no emita ninguna advertencia.

## Números enteros

Un numero entero es una secuencia de dígitos que no tiene un punto
decimal o un exponente. Puede tener un valor negativo cuando es menor
que 0 o puede ser positivo si no tiene signo.

La jerarquía de tamaños para el tipo entero es: `short`, `int` y `long`.
Un entero puede ser `signed` o `unsigned` cuando no guarda el valor del
signo y puede usar el modificador `long` para crear el tipo de entero
más grande que puede guardar C++, el tipo `long long`.

### El tipo short

El tipo de datos `short` se almacena como un entero de 2 bytes en
cualquier sistema. Puede representar números enteros sin signo en el
intervalo de `-32,768` negativo a `32,767` positivo:

``` C++
#include <iostream>                                  // cout
#include <limits>                                    // numeric_limits

// Devuelve el tamaño máximo y mínimo de un tipo short 
int main()
{
    std::cout << "Valor mínimo de signed short: "    // -32768
              << std::numeric_limits<signed short>::min() << '\n'
              << "Valor máximo de signed short: "    //  32767
              << std::numeric_limits<signed short>::max() << '\n';

    return EXIT_SUCCESS;
}
```

La versión con signo puede almacenar como mínimo `0` y como máximo
`65535`:

``` C++
#include <iostream>                                   // cout
#include <limits>                                     // numeric_limits

// Devuelve el tamaño máximo y mínimo de un tipo short sin signo
int main()
{
    std::cout << "Valor mínimo de unsigned short: "    //    0
              << std::numeric_limits<unsigned short>::min() << '\n'
              << "Valor máximo de unsigned short: "    // 65535
              << std::numeric_limits<unsigned short>::max() << '\n';

    return EXIT_SUCCESS;
}
```

La única ventaja de usar un tipo `short` es el ahorro de espacio. En un
sistema de 16 bits un tipo `short` tiene el mismo tamaño que un tipo
`int`. En sistemas de 64 bits rara vez se utiliza.

### El tipo int

El tipo `int` representa un valor entero que se almacena como un valor
entero de 4 bytes. Puede representar números enteros en el intervalo de
`-2,147,483,648` (negativo) a `2,147,483,647` (positivo).

``` C++
#include <iostream>                               // cout
#include <limits>                                 // numeric_limits

// Devuelve el tamaño máximo y mínimo de un tipo int con signo
int main()
{
    std::cout << "Valor mínimo de signed int: "    // -2147483648
              << std::numeric_limits<signed int>::min() << '\n'
              << "Valor máximo de signed int: "    // 2147483647
              << std::numeric_limits<signed int>::max() << '\n';

    return EXIT_SUCCESS;
}
```

Los objetos de tipo `int` pueden ser declarados como `signed int` o
`unsigned int`. El tipo `signed int` es sinónimo de `int`, así que no
necesita declararse explícitamente.

Cuando se declaran enteros positivos (o enteros sin signo) se puede usar
la letra `u` o `U` para declarar un tipo `unsigned int`. Por ejemplo:

``` C++
unsigned int num = 50u;
```

Los tipos sin signo solo pueden almacenar números positivos:

``` C++
#include <iostream>                                // cout
#include <limits>                                  // numeric_limits

// Devuelve el tamaño máximo y mínimo de un tipo int sin signo
int main()
{
    std::cout << "Valor mínimo de unsigned int: "  //  0
              << std::numeric_limits<unsigned int>::min() << '\n'
              << "Valor máximo de unsigned int: "  // 4294967295
              << std::numeric_limits<unsigned int>::max() << '\n';

    return EXIT_SUCCESS;
}
```

Un tipo `int` es dos veces más grande que un tipo `short` sin embargo un
tipo `int` es más rápido, esta es la razón por la que solo tiene sentido
usar un tipo `short` en plataformas y sistemas donde se necesite ahorrar
espacio.

### El tipo long

El tipo `long` ocupa como mínimo 4 bytes. En sistemas de 32 bits ocupa
el mismo espacio que un tipo `int`. En sistemas de 64 bits ocupa el
doble.

``` C++
#include <iostream>                                 // cout
#include <limits>                                   // numeric_limits

// Devuelve el tamaño máximo y mínimo de un tipo long sin signo
int main()
{
    std::cout << "Valor mínimo de signed long: "   
              << std::numeric_limits<signed long>::min() << '\n'
              << "Valor máximo de unsigned short: "  
              << std::numeric_limits<signed long>::max() << '\n';

    return EXIT_SUCCESS;
}
```

Al igual que `int`, hay dos versiones una con signo y otra sin signo:

``` C++
#include <iostream>                                  // cout
#include <limits>                                    // numeric_limits

// Devuelve el tamaño máximo y mínimo de un tipo long con signo
int main()
{
    std::cout << "Valor mínimo de unsigned long: "      
              << std::numeric_limits<unsigned long>::min() << '\n'
              << "Valor máximo de unsigned short: "     
              << std::numeric_limits<unsigned long>::max() << '\n';

    return EXIT_SUCCESS;
}
```

Para crear un valor literal de tipo `long` se puede usar la letra `L`
junto al valor numérico o una `l` en minúsculas.

``` C++
long largo{2'147'234L};    // Un tipo long
```

También se puede usar el modificador `long` sobre un tipo `long` para
obtener un rango de datos más amplio. Por ejemplo el tipo
`unsigned long` `long` puede almacenar valores positivos entre 0 y
18446744073709551615.

``` C++
#include <iostream>                                 // cout
#include <limits>                                   // numeric_limits

// Devuelve el tamaño máximo y mínimo del tipo unsigned long long
int main()
{
    std::cout << "Valor mínimo de unsigned long long: "  
              << std::numeric_limits<unsigned long long>::min() << '\n'
              << "Valor máximo de unsigned long long: "  
              << std::numeric_limits<unsigned long long>::max() << '\n';

    return EXIT_SUCCESS;
}
```

Un tipo `long long` puede almacenar valores entre --9223372036854775808
y 9223372036854775807.

``` C++
#include <iostream>                                 // cout
#include <limits>                                   // numeric_limits

// Devuelve el tamaño máximo y mínimo de un tipo long con signo
int main()
{
    std::cout << "Valor mínimo de signed long long: "  
              << std::numeric_limits<signed long long>::min() << '\n'
              << "Valor máximo de unsigned long long: "   
              << std::numeric_limits<signed long long>::max() << '\n';

    return EXIT_SUCCESS;
}
```

Este tipo, es el tipo estandar más grande que se puede usar en C++. En
algunas plataformas tiene un tamaño mayor a 64 bytes.

### Enteros a medida

Además de los tipos comunes como: `short`, `int` y `long` que pueden
variar de tamaño en diferentes plataformas, existen tipos a medida.
Estos tipos son útiles cuando se busca mantener la portabilidad entre
diferentes sistemas.

+----------+-------+----------------------+---------------------------+
| Nombre   | Bytes | Valor mínimo         | Valor máximo              |
+==========+=======+======================+===========================+
| uint8_t  | > 1   | 0                    | 255                       |
+----------+-------+----------------------+---------------------------+
| uint16_t | > 2   | 0                    | 65535                     |
+----------+-------+----------------------+---------------------------+
| uint32_t | > 4   | 0                    | 4294967295                |
+----------+-------+----------------------+---------------------------+
| uint64_t | > 8   | 0                    | 18446744073709551615      |
+----------+-------+----------------------+---------------------------+
| int8_t   | > 1   | -128                 | 127                       |
+----------+-------+----------------------+---------------------------+
| int16_t  | > 2   | -32768               | 32767                     |
+----------+-------+----------------------+---------------------------+
| int32_t  | > 4   | -2147483648          | 2147483647                |
+----------+-------+----------------------+---------------------------+
| int64_t  | > 8   | -9223372036854775808 | 9223372036854775807       |
+----------+-------+----------------------+---------------------------+

: Enteros sin signo a medida

Los tipos `unsigned` (o positivos) empiezan con la letra `u`. Por
ejemplo:

``` C++
#include <iostream>                        // cout
#include <cinttypes>                       // tipos a medida
#include <climits>                         // CHAR_BIT

// Tipos a medida definidos en la cabecera cintypes
int main()
{
    uint8_t   t8{1};
    uint16_t t16{1};
    uint32_t t32{1};
    uint64_t t64{1};

    // Imprime el tamaño de los tipos en bit's
    std::cout << sizeof(t8)  * CHAR_BIT << " bit's\n"   // 8 bit's
              << sizeof(t16) * CHAR_BIT << " bit's\n"   // 16 bit's
              << sizeof(t32) * CHAR_BIT << " bit's\n"   // 32 bit's
              << sizeof(t64) * CHAR_BIT << " bit's\n";  // 64 bit's

    return EXIT_SUCCESS;
}
```

Los números enteros se pueden representar en 8, 16, 32 y 64 bits esto da
origen a una escala de números enteros cuyo rango se especifica en el
nombre del tipo.

``` C++
#include <iostream>                         // cout
#include <limits>                           // numeric_limits

// El tamaño de enteros de 64 bits
int main()
{
    std::cout << "Valor minimo de uint64_t: "
              << std::numeric_limits<uint64_t>::min() << '\n'
              << "Valor máximo de uint64_t: "
              << std::numeric_limits<uint64_t>::max() << '\n'
              << "Valor mínimo de int64_t: "
              << std::numeric_limits<int64_t>::min() << '\n'
              << "Valor máximo de int64_t: "
              << std::numeric_limits<int64_t>::max() << '\n';

    return EXIT_SUCCESS;
}
```

Los tipos a medida están definidos en la cabecera `<cintypes>`, sin
embargo algunas implementaciones los incluyen en el espacio de nombres
global.

## Números reales

Los valores de punto flotante representan números decimales. Por ejemplo
`1.5` o `345.9877`. Estos valores tienen una parte entera del lado
izquierdo y una fracción del lado derecho. En Matemáticas se le conoce
como números reales. Estos números amplían el conjunto de operaciones
aritméticas y aportan precisión en los cálculos Matemáticos. Hay tres
tipos de números reales en C++: `float`, `double` y `long double`.

Por lo general un tipo `float` se representa con 32 bits, un tipo
`double` con 64 y un tipo `long double` con 96, inclusive 128 bits. El
tipo `float` y el tipo `double` usan 7 y 16 dígitos de precisión
respectivamente. El tipo `long double` es usado para propósitos
especiales que dependen del hardware.

### El tipo float

El tipo `float` se almacena como un número de 4 bytes (32 bits) de punto
flotante y precisión simple.

``` C++
#include <iostream>                              // cout
#include <limits>                                // numeric_limits
#include <iomanip>                               // setprecision

// Devuelve el tamaño máximo y mínimo de un tipo float
int main()
{
    std::cout << std::setprecision(32)
              << "Valor mínimo de float: " 
              << std::numeric_limits<float>::min() << '\n'
              << "Valor máximo de float: "  
              << std::numeric_limits<float>::max() << '\n';

    return EXIT_SUCCESS;
}
```

Un tipo `float` puede representar números tan grandes como `3.4 x 10^38`
a `1.1 x 10^-38` con una exactitud de aproximadamente siete dígitos. Se
puede añadir el sufijo `f` o `F` en valores literales para representar
un valor de tipo \"flotante\".

Este tipo de dato es útil para aplicaciones que necesitan cálculos
elevados, pero que no requieran de una gran precisión. Si requieres
números mucho mas precisos, usa el tipo `double`.

### El tipo double

El tipo de valor `double` representa un número de 8 bytes (64 bits) de
precisión doble con valores entre `2.22507e-308` y `1.79769e+308`.

``` C++
#include <iostream>                                 // cout
#include <limits>                                   // numeric_limits
#include <iomanip>                                  // setprecision

// Devuelve el tamaño máximo y mínimo de un tipo double
int main()
{
    std::cout << std::setprecision(64) 
              << "Valor mínimo de double: " 
              << std::numeric_limits<double>::min() << '\n'
              << "Valor máximo de double: "  
              << std::numeric_limits<double>::max() << '\n';

    return EXIT_SUCCESS;
}
```

El tipo `double` depende del compilador. En una plataforma de 32 bits
ocupa el mismo espacio que un tipo `float` y en una de 64 ocupa el
doble. Un tipo double tiene una exactitud aproximada de 10 dígitos por
defecto.

### El tipo long double

El modificador `long` hace más \"largo\" un tipo `double`:

``` C++
#include <iostream>                                 // cout
#include <limits>                                   // numeric_limits
#include <iomanip>                                  // setprecision

// Devuelve el tamaño máximo y mínimo de un tipo long double
int main()
{
    std::cout << std::setprecision(64) 
              << "Valor mínimo de long double: " 
              << std::numeric_limits<long double>::min() << '\n'
              << "Valor máximo de long double: "  
              << std::numeric_limits<long double>::max() << '\n';

    return EXIT_SUCCESS;
}
```

Para declarar de forma explícita un tipo `long double` se pueden usar
las letras `LL`:

``` C++
long double{19LL}   // Un tipo long double 
```

El tipo `long double` es el tipo de dato con mayor precisión de C++.
Pero su implementación y precisión dependen del compilador y la
plataforma. En una plataforma de 64 bits ocupa el mismo espacio que un
tipo `double`.

#### Precisión de los números reales

Para controlar la precisión de los números reales (el número de dígitos
a la derecha del punto decimal) se puede usar la función `precision()`
incorporada en el objeto `cout`.

``` C++
#include <iostream>                 // cout

/*
* El desarrollo decimal de la fracción 1/19 tiene un periodo formado por 
* 18 dígitos. Las sucesivas fracciones del denominador contienen los 
* mismos dígitos en el mismo orden cíclico y con ellas se puede construir 
* un cuadrado mágico que suma 81. Este es el cuadrado más pequeño que se 
* puede construir con los decimales de una fracción periódica. Este 
* cuadrado fue diseñado por Harry A. Sayles descrito por W. S. Andrews en
* su libro, "Cuadrados mágicos y Cubos", publicado en 1917.
*/
int main() 
{
    const long double total{19LL};

    // Asigna una precisión de 18 dígitos después 
    // del punto para crear un cuadrado magico.
    std::cout.precision(total-1);  
    for (int i = 1; i < total; ++i) {
        std::cout << i / total << " \n";
    }

    return EXIT_SUCCESS; 
}
```

En este ejemplo, la primera llamada a la función `precision(total-1)`
establece la precisión para todas las operaciones de salida siguientes.
Una llamada a la función `precision()` sin argumentos restaura la
precisión original.

El archivo de cabecera `<iomanip>` contiene una función similar llamada
`setprecision()` que también se puede usar para controlar los dígitos
después del punto decimal:

``` C++
#include <iomanip>           // setprecision

// Asigna una precisión de 18 dígitos después del punto 
for (int i = 1; i < total; ++i) {
    std::cout << std::setprecision(total-1) << i / total << '\n';
}
```

A diferencia de la función mienbro `presicion()` este es un manipular de
flujo que se puede agregar al flujo de salida.

## Valores boléanos

Un valor boléano representa un valor verdadero o falso. Existen
únicamente dos valores posibles para este tipo: `true` o `false`. Estos
valores se representan en C++ como 1 y 0 respectivamente.

``` C++
bool verdadero{true};
bool falso{false};
```

Los valores `true` y `false` mantienen la siguiente relación:

``` C++
// El signo ! invierte el valor del operando a la derecha 
!false == true;                       // false ahora es true
!true == false;                       // true ahora es false
```

Los valores boléanos son útiles en operaciones lógicas o de comparación.

``` C++
#include <iostream>                   // cout

// Usando los manipuladores de flujos boolalpha y noboolalpha.
int main()
{
    bool verdadero {true};            // Un valor verdadero 
    bool falso {false};               // Un valor falso 

    // Muestra el valor literal de true y de false
    std::cout << "El valor de true es : " << verdadero  << '\n'
              << "El valor de false es: " << falso      << '\n'
              << std::boolalpha       // Activa boolalpha
              << "boolalpha activado"                   << '\n' 
              << "El valor de true es : " << verdadero  << '\n'
              << "El valor de false es: " << falso      << '\n'
              << std::noboolalpha     // Restablece la salida
              << "Restablece la salida con noboolalpha" << '\n'
              << "El valor de true es : " << verdadero  << '\n'
              << "El valor de false es: " << falso      << '\n';

    return EXIT_SUCCESS;
}
```

Por defecto el valor de salida de un valor `true` es 1, mientras que
`false` es 0. Para cambiar este comportamiento se usan los manipuladores
de flujos `boolalpha` y `nonboolalpha`. El manipulador `boolalpha`
muestra la cadena literal: `true` y `false`, mientras que `nonboolalpha`
devuelve el flujo de salida a su estado original.

## Caracteres

Un carácter puede contener únicamente una letra. Los caracteres que
reconocen las diferentes computadoras no son estándar; sin embargo, la
mayoría reconoce los caracteres alfabéticos y numéricos de la tabla
ASCII.

``` C++
char variable{'A'};           // define una variable de tipo char
char tabulador{'\t'};         // define un tabulador
char linea{'\n'};             // define un salto de línea
```

Internamente C++ almacena los datos de tipo `char` como valores enteros
entre -128, 127 (`signed` por defecto). A sí que estas dos declaraciones
son iguales:

``` C++
char letra_z{122};
char z{'z'};
std::cout << (z == letra_z);  // true
```

Para definir un tipo `char` se usan comillas simples, no comillas
dobles. Cuando el compilador encuentra un valor entre comillas dobles lo
interpreta como una cadena de caracteres de tipo `string` no como un
tipo `char`.

### Cadenas estilo C

Es posible definir una cadena de caracteres usando un arreglo de tipo
`char`.

``` C++
char mensaje[16] = {"Cadena estilo C"};
std::cout << mensaje;       // Cadena estilo C
```

Un arreglo de caracteres es simplemente una lista separada por comas que
termina con un carácter nulo. Cuando se define un arreglo de caracteres
hay que dejar espacio para el carácter extra, al definir el tamaño de la
cadena de caracteres. A este tipo de cadenas el estándar las llama
cadenas de caracteres estilo C.

``` C++
char mensaje[16]{"Cadena estilo C"};

// Convierte a mayusculas la cadena de texto
for (char& c : mensaje) {
    c = std::toupper(c);
}

std::cout << mensaje;      // CADENA ESTILO C
```

El tamaño de la cadena no puede cambiar pero pueden cambiar los
caracteres almacenados.

### Escape de secuencias

El carácter de barra inclinada `\` tiene un propósito especial; escapar
un carácter. Combinado con el carácter que le sigue representa un
carácter que de otra forma no podría ser representado dentro de una
cadena o un valor literal.

``` C++
#include <iostream>                               // cout

// Escape de secuencias
int main()
{
    std::cout << "Escape se secuencias"             << '\n'
              << "Comillas simples........... \'\'" << '\n'
              << "Comillas dobles............ \"\"" << '\n'
              << "Barra inclinada............ \\  " << '\n'
              << "Barra inclinada doble...... \\\\" << '\n';

    return EXIT_SUCCESS; 
}
```

Por ejemplo `\n` es una secuencia de escape que representa un carácter
de nueva línea. Mientras que `\'` es una secuencia de escape que
representa una comilla simple o un carácter llamado apostrofe muy común
en el Español.

Cada vez que una barra inversa es encontrada dentro de un texto entre
comillas, indica que el siguiente carácter tiene un significado especial
para el compilador. A esto se le conoce como **escapar un carácter**.
Una comilla que es precedida por una barra inversa no termina la cadena
sino forma parte de ella. Cuando se usa un carácter `n` detrás una barra
inversa, se interpreta como otra línea. Lo mismo pasa con la `t` después
de una barra invertida, significa que el carácter es un tabulador.

Una secuencia de escape es útil cuando es necesario incluir un apostrofe
en una cadena literal que contiene comillas simples o cuando es
necesario incluir algún símbolo especial en la salida.

+--------------+-------------------------+
| Secuencia    | Carácter Representado   |
+==============+=========================+
| > \\0        | El carácter nulo        |
+--------------+-------------------------+
| > \\b        | Retorno o retroceso     |
+--------------+-------------------------+
| > \\t        | Tabulación horizontal   |
+--------------+-------------------------+
| > \\n        | Fin de Línea            |
+--------------+-------------------------+
| > \\v        | Tabulación vertical     |
+--------------+-------------------------+
| > \\f        | Avance de Página        |
+--------------+-------------------------+
| > \\r        | Retorno de Línea        |
+--------------+-------------------------+
| > `\\`       | Barra inclinada         |
+--------------+-------------------------+

: Secuencias de escape más comunes

En la tabla se puede ver porque estas secuencias son llamadas secuencias
de escape. La barra inversa permite escapar la interpretación usual del
carácter.

### Caracteres ASCII

El tipo `char` solo puede almacenar caracteres ASCII. Aunque se pueden
representar caracteres fuera del rango de 128. Algunos sistemas
extienden el rango ASCII a 255 caracteres para incluir algunos
caracteres comunes en otros lenguajes.

``` C++
#include <iostream>                    // cout

// Imprime el alfabeto y su valor ASCII
int main()
{
    for (int c = 'A'; c <= 'Z'; ++c ) {
        std::cout << c << " = " 
                  << static_cast<char>(c) << " -> " 
                  << c + ' ' << " = "  // El espacio es el número 32 
                  << static_cast<char>(' ' + c) << '\n';              
    }

    return EXIT_SUCCESS;       
}
```

En el ejemplo, el operador de conversión de tipos estáticos
`static_cast` convierte un valor entero en el carácter equivalente de la
tabla ASCII.

``` C++
// Imprime el rango de caracteres ASCII extendido
for (int c = 0; c <= 255; ++c ) {
    std::cout << c << " = " << static_cast<char>(c) << '\n';
}
```

Algunos caracteres especiales como los valores menores a 32 o mayores a
128 no se muestran en la salida porque no son imprimibles. Sin embargo
todos los caracteres ASCII tienen un valor de tipo entero con el que se
guardan en la memoria. Por esta razón se les pueden aplicar operaciones
aritméticas como si fueran enteros.

### Caracteres extendidos

El tipo `wchar_t` es un tipo simple que puede almacenar un rango más
amplio de caracteres y que cuenta con el respaldo de la implementación.
El nombre del tipo se deriva de la palabra `wide` que extiende el
conjunto de caracteres de un byte a dos (algunas plataformas como Linux
usan 4 bytes). Por contraste, el tipo `char` es conocido como el
conjunto reducido de caracteres porque está limitado a un número
limitado de códigos de caracteres que puede manejar.

``` C++
wchar_t wz{L'Z'};           // Devuelve un numero en algunos sistemas
```

Esta línea, define una variable llamada `wz` como tipo `wchar_t` y
representa un carácter extendido.

Algunos teclados no tienen o no pueden manejar todos los caracteres que
se pueden representarse en otros lenguajes en estos casos se puede usar
la notación hexadecimal. Por ejemplo:

``` C++
wchar_t wn{L'\x0438'};            // Cirílico и
```

El valor entre comillas simples es una secuencia de escape que
especifica la representación hexadecimal del código del carácter. La
barra invertida indica el principio de la secuencia de escape y la `x` o
la `X` después de la barra significa que el código es hexadecimal.

Al igual que los tipos enteros a medida, también existen caracteres a
medida, como: `char16_t` que almacena caracteres codificados como
`UTF-16`, o `char32_t` que almacena caracteres codificados como
`UTF-32`.

``` C++
char16_t letra{u'B'};             // Inicializa con UTF-16 a B
char16_t wn{u'\x0438'};           // Inicializa con UTF-16 a и
```

El prefijo `u` al inicio del caracter literal indica que es `UTF-16`.
Para usar `UTF-32` se usa la `U` en mayúsculas. Por ejemplo:

``` C++
char32_t letter{U'B'};            // Initialize con UTF-32 a B
char32_t cyr{U'\x044f'};          // Inicializa con UTF-32 a я
```

Si el editor y el compilador tienen la capacidad de aceptar y mostrar
caracteres, se puede usar algo como esto:

``` C++
char32_t cyr{U'я'};
```

La librería estándar provee utilidades especiales para trabajar con
caracteres extendidos de tipo `wchar_t`, como `wcout` que sirve para
escribir caracteres extendidos.

``` C++
#include <codecvt>                  // LC_ALL
#include <locale>                   // setlocale
#include <iostream>                 // cout, endl  
#include <set>                      // set

// Devuelve true si un carácter representa una consonante
bool es_consonante(wchar_t caracter) 
{
    // Conjunto de vocales del idioma Español
    const std::wstring vocales  {
        L"AEIOUaeiouÀÁÄÈÉËÌÍÏÒÓÖÙÚÜàáäèéëìíïòóöùúü"
    };

    // Verifica si el carácter no es una vocal
    return (vocales.find(caracter) == std::string::npos);
}

// Caracteres extendidos
int main() 
{
    // Cambia la configuración regional
    std::setlocale(LC_ALL, "es_ES.UTF-8");    

    // Una cadena de caracteres extendidos   
    std::wstring caracteres{L"ABCDEFGHIJKLMNOPQRSTUVWXYZÀÁÄÈÉËÌÍÏÒÓÖÙÚÜ"};

    // Usa caracteres extendidos de tipo wchar_t
    for (wchar_t caracter : caracteres) {
        if (es_consonante(caracter)) {
            std::wcout << caracter << L" es una consonante." << std::endl;
        } else {
            std::wcout << caracter << L" es una vocal." << std::endl;
        }
    }

    return EXIT_SUCCESS;
}
```

Existen también la versión extendida llamada `wcin` que puede leer
caracteres extendidos de la entrada estándar. Por lo general no se
recomienda mezclar caracteres.

### Valores vacíos

La palabra reservada `void` define valores no existentes de un tipo en
una variable o declaración. No se define como un tipo pero se puede usar
para tipificar una función o un miembro de función. Su uso más común es
en funciones que no devuelven valores:

``` C++
// Imprime el número pasado como argumento
void imprimir(int num) 
{
    std::cout << num;
};
```

Una función declarada como `void` no devuelve ningún valor.

Un valor `void` puede usarse para indicar que una función no recibe
parámetros como en la siguiente declaración:

``` C++
int imprimir(void);
```

Este tipo de declaración es (o era) muy común en C, pero no en C++.

Otro uso de los valores vacíos es para indicar que el tipo de datos es
un puntero vacío. Por ejemplo:

``` C++
void *num;
```

Esta línea de código indica que la variable `num` es un puntero en donde
se guarda información de algún tipo. En estos casos es necesario definir
el valor de la referencia más adelante. Para eliminar la ambigüedad.

Un puntero de tipo `void*` puede apuntar a cualquier tipo de dato, pero
no puede desreferenciarse directamente.

``` C++
#include <iostream>        // cout

 // Usando un tipo void
 int main() 
 {
    int num = 123;

    // El puntero apunta a una variable de tipo int
    void* ptr = &num; 

    // Es necesario una conversión explicita para desreferenciarlo
    std::cout << "Valor: " << *(static_cast<int*>(ptr)) << std::endl;

    return EXIT_SUCCESS;
 }
```

La declaración `void *` puede representar a la vez varios tipos de
datos, dependiendo de la operación de conversión usada. La memoria que
hemos \"apuntado\" en el ejemplo anterior, puede almacenar un entero, un
flotante o un carácter. Aunque este tipo de declaración es muy común en
C, es poco común en C++ moderno y en la practica, rara vez se usa.

### Valores nulos

Además del valor `void`, existe un valor nulo heredado de C llamado
`NULL`, que se puede usar con números, caracteres, cadena de texto etc.
El valor `NULL` representa un valor de asignación de \"0\".

``` C++
void* puntero  = NULL; 
int   entero   = NULL;
bool  boleano  = NULL;
char  carácter = NULL;
```

El valor de las variables es 0, sin embargo el valor de la variable de
tipo `char` es: \'0\'. En realidad `NULL` es solo una definición de una
macro como esta:

``` C++
#define NULL 0
```

La palabra `NULL` es común en código antiguo de C/C++. Aunque no es
estándar.

:::: note
::: title
Note
:::

**La palabra clave NULL**

En 1965, El Ingles Tony Hoare trabajo junto con el científico suizo
Niklaus E. Wirth, en el lenguaje de programación ALGOL. Hoare introdujo
la palabra clave Null para identificar las referencia nulas. ALGOL fue
el predecesor de PASCAL. En 2009 Tony Hoare dijo que la introducción de
referencias nulas fue probablemente un error. Hoare argumentaba que las
referencias de tipo Null habían causado muchos problemas cuyo costo
probablemente superaba un billón de dólares.
::::

C++ provee la palabra clave `nullptr` para evitar su uso. Esto es
equivalente al ejemplo anterior:

``` C++
void* puntero{nullptr}; 
int entero{nullptr};
bool boleano{nullptr};
char carácter{nullptr};
```

La palabra clave `nullptr` es estándar, mientras que la macro `NULL`
solo se usa en código antiguo.

## Conversiones de tipo

En algunas ocasiones es necesario usar un tipo de dato en lugar de otro.
Por ejemplo un tipo decimal donde se espera un valor entero o viceversa.
En C++ es posible realizar conversiones entre diferentes tipos de datos.
La conversión es el proceso mediante el cual una variable de un tipo se
transforma en un tipo de dato diferente (o similar).

Por ejemplo, un programa que convierte metros a pies y pulgadas necesita
transformar tipos decimales en tipos enteros.

``` C++
#include <iostream>                // cout

// Convierte un valor decimal a entero
int main()
{
    float altura{1.70};
    altura *= 100.0;               // Un metro tiene 100 cms.
    float pulgada{2.54};           // Una pulgada tiene 2.54 cms.
    float pie{30.48};              // Un pie tiene 30.48 cms.

    int pies = altura / pie;       // Convierte a entero
    int centimetros = pies * pie;  // Convierte a entero

    // Salida: 170 cms. = 5' 7''
    int pulgadas = static_cast<int>(altura - centimetros) / pulgada;
    std::cout << altura << " cms. = " << pies << "' " <<  pulgadas << "''\n";

    return EXIT_SUCCESS;
}
```

El ejemplo muestra lo que pasa cuando se usan datos de diferente tipo.
Por ejemplo cuando se dividen dos valores decimales y se guardan en un
tipo entero el valor resultante se trunca. En este caso, el compilador
realiza la conversión, sin embargo cuando se usa el operador
`static_cast` es el programador, quien le dice al compilador como
interpretar los datos.

### Tipos de conversión

En C++ la conversión puede hacerse de dos formas:

1.  **Implícita**: el compilador usa la conversión implícita para
    convertir un tipo de dato en otro usando el tipo de mayor precisión,
    siempre y cuando los tipos sean compatibles.
2.  **Explicita**: el programador convierte un tipo de dato en otro de
    forma anticipable y programable.

En cualquiera de los dos casos la integridad de los datos no está
totalmente garantizada y puede dar lugar a errores inesperados, si no se
toman las precauciones correctas.

### Conversiones implícitas

El compilador realiza conversiones de tipo automáticas, buscando por lo
general que el resultado de la operación sea del tipo más amplio.
Siempre que es posible, convierte los valores de tal forma que no se
pierda información.

``` C++
int a{100.9};        // Error de conversión en inicialización 
int b = 100.5;       // Bien: b es 100 (Redondea e entero)
int c = 10 / 3;      // Bien: c es 3 (Redondea a entero)
double d = 10 / 3;   // Bien: d es 3.33333
int e(100.9);        // Bien: conversión implícita, e es 100
```

Aunque existen muchas reglas detalladas para efectuar conversiones
implícitas de tipos, las más comunes son estas dos:

1.  **Promoción**: en cualquier operación en la que aparezcan dos tipos
    diferentes se eleva el rango del tipo menor para igualarlo al del
    mayor. El rango de los tipos de mayor a menor es el siguiente:

    ``` C++
    double, float, long, int, short, char
    ```

2.  **Almacenamiento**: en una asignación, el resultado final del
    cálculo se reconvierte al tipo de la variable asignada.

Si no se tiene precaución, las conversiones implícitas pueden dar
resultados inesperados.

``` : C++
int a{1};             // a, es 1
a = 10.5;             // Ahora a, es 10; no 10.5

int b{3};
float c {3.0};  
a / b;                //  3, Tipos int (se redondea el valor)
a / c;                //  3.33, Tipo de mayor precisión: float

double d{10}          // Error, 10 no es double (inicialización uniforme)
int e{1.5}            // Error, 1.5 no es entero
double d(10)          // Bien, conversión implícita (usando paréntesis) 
int e(1.5)            // Bien, conversión implícita (e es 1) 
```

Las conversiones implícitas las realiza el compilador. El compilador
siempre busca la forma de preservar la información y trata de evitar la
pérdida de datos, lo que no siempre es posible. Por ejemplo, la división
entre enteros siempre devuelve un valor entero, por otra parte la
inicialización uniforme (con llaves) no permite la asignación directa de
un tipo de dato diferente al que acepta la variable, mientras que el
operador de asignación (`=`) y los paréntesis pueden aceptar tipos de
datos (numéricos) diferentes.

### Conversiones de tipo explícitas

Una conversión explicita se declara de forma directa cuando se busca
convertir un tipo de dato en otro de forma programable.

Existen dos formas de realizar estas conversiones:

- Usando paréntesis al estilo C.
- Usando operadores de conversión.

Siempre que se realiza una conversión explicita es el programador el
responsable de convertir un dato en otro, aunque el compilador puede
emitir advertencias y errores estos pueden pasar desapercibidos.

#### Conversiones al estilo C

Además de especificar el orden de evaluación de las operaciones en una
expresión, los paréntesis también se utilizan para especificar
conversiones de tipo explícitas. Esta es su forma de uso:

``` C++
double d{65.5};
int a;
a = (int)d;            // 65, convierte de tipo double a int.   
```

Este método funciona como una llamada a una función que convierte un
tipo de dato en otro. Aunque no es una función, ya que los paréntesis
son usados como operadores de conversión explícita. El operador de
conversión convierte un tipo de origen en otro tipo llamado destino. El
tipo de origen proporciona el operador de conversión.

:::: tip
::: title
Tip
:::

**La biblioteca cmath**

Las funciones `std::round()`, `lround()`, y `llround()` definidas en la
cabecera `cmath` permiten redondear números de tipo `int`, `float`, y
`double` respectivamente. En muchos casos, es mejor usar estas funciones
que las conversiones (implícitas o explicitas) que solo truncan los
valores.
::::

Un uso muy común para la conversión de tipos es cuando es necesario
imprimir un carácter ASCII.

``` C++
std::cout << (char)65;
```

En este ejemplo, se convierte un valor entero en un carácter. El tipo se
coloca dentro de los paréntesis `(char)` y el valor afuera. Esto le dice
al compilador que interprete el número 65 como un tipo `char`, no como
un tipo `int`. El equivalente del número 65 es la letra \"A\" en la
tabla ASCII.

``` C++
// imprime los dígitos del 0-9 
for (int i = 0; i < 10; ++i) {
    std::cout << (char)('0' + i);
}
```

Este fragmento de código imprime los números del 0 al 9. El valor
literal \'0\' se convierte a entero y se suma a `i`. El valor resultante
es convertido a tipo `char` y escrito a la salida.

Históricamente las conversiones al estilo C, han sido fuente de
innumerables errores, ya que permiten reinterpretar los bits de un tipo
de dato en otro completamente diferente, sin realizar comprobaciones
entre tipos y sin lanzar advertencias de ningún tipo, lo que vuelve la
conversión insegura. Por esta razón C++ introdujo operadores de
conversión explícitos buscando de alguna forma reducir algunos de estos
peligros.

### Operadores de conversión de tipo

En C++ existen varios operadores que realizan conversiones explicitas de
tipos:

- `static_cast` realiza conversiones entre tipos de forma estática.
- `dynamic_cast` realiza conversiones entre tipos de forma dinámica.
- `reinterpret_cast` realiza conversiones entre tipos reinterpretando
  los datos.
- `const_cast` realiza conversiones entre tipos que son constantes.

La palabra clave `static_cast` es un operador que realiza conversiones
entre tipos de forma estática, es decir en tiempos de compilación.
Mientras que `dynamic_cast` lo hace de forma dinámica o en tiempos de
ejecución, este operador solo convierte punteros o referencias.

Los operadores `const_cast` y `reinterpret_cast` se utilizan como último
recurso, ya que estos operadores presentan los mismos peligros que las
conversiones al estilo C. Sin embargo, se usan para reemplazar
completamente las conversiones antiguas al estilo C.

#### Conversión estática

El operador `static_cast` es similar al operador de conversión estilo C,
pero su nombre es más explícito en relación con la tarea para la que fue
diseñado: convertir tipos en tiempos de compilación.

Esta es su forma de uso.

``` c++
static_cast<tipo>(expresión)
```

Por ejemplo:

``` c++
double d1{10.9};
double d2{25.9};
int c{static_cast<int>(d1) + static_cast<int>(d2)};    // 35
```

La conversión no afecta los valores almacenados en `d1` y `d2`, los
cuales conservan su valor original. El valor que produce cada conversión
se almacena de forma temporal para usarse en la suma y luego se desecha.
Aunque la operación resulta en perdida de datos, el compilador asume que
el programador realiza una conversión segura.

Por supuesto, el valor de `c` puede ser diferente si se usa algo como
esto:

``` c++
int c {static_cast<int>(d1 + d2)};                      // 36
```

El operador `static_cast` convierte la expresión de un tipo en otro,
basado únicamente en los tipos presentes en la expresión.

``` C++
float peso_en_kilos{70.0};
const double libra{0.4535923699993531};
std::cout << peso_en_kilos << " kg = "                  // 70 kg = 154 lb
          << static_cast<int>(peso_en_kilos/libra) << " lb"; 
```

El operador `static_cast` no hace comprobaciones en tiempo de ejecución
para asegurar el tipo correcto de la conversión y por lo general se usa
para convertir tipos numéricos como enumeraciones a enteros, enteros a
decimales, caracteres, etc. Sin embargo falla con referencias, punteros
y tipos polimórficos, en estos casos se usa el operador `dynamic_cast`
que hace comprobaciones en tiempos de ejecución, lo cual agrega
sobrecarga.

### Errores de conversión

Si el compilador detecta una conversión no segura, emite una advertencia
y posiblemente un error. En caso de error, la compilación se detiene,
mientras que una advertencia permite que la compilación continúe, en
cualquiera de los dos casos esto indica un posible error en el código.
Estos es común en desbordamientos, cuando el valor de un tipo sobrepasa
el valor máximo o mínimo que puede almacenar:

``` C++
int i = 2147483649;                    // Advertencia: desbordamiento
int j = -2147483649;                   // Advertencia: desbordamiento
```

El compilador también genera un error si los dos tipos no tienen ninguna
relación entre sí:

``` C++
double a{1.0};
std::string s = static_cast<char>(a);   // Error de conversion
```

Sin embargo, aunque el programa se compile sin advertencias, las
conversiones implícitas pueden generar resultados inesperados:

``` C++
// Conversión implícita que genera un error sin emitir advertencias
unsigned int uno = 1 - 2;
std::cout << uno;                        // 4294967295 
```

El compilador no advierte sobre conversiones implícitas entre enteros
con y sin signo. En casos como este, es mejor evitar por completo las
conversiones (y comparaciones) de tipos con signo y tipos sin signo. En
realidad siempre que se pueda, es mejor evitar las conversiones. Un
programa que no necesita convertir valores de un tipo en otro, ya sea de
forma implícita o explícita tiene seguridad de tipos por definición.
