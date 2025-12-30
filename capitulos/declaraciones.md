# Declaraciones

::: {.meta title="Declaraciones" capitulo="4" description="Como se declaran y definen nombres en un programa y el tiempo de vida" keywords="declaración, definición, variables, constantes, ámbito"}
:::

Para usar un nombre dentro de un programa es necesario declararlo
primero. Una declaración es una instrucción que introduce un
identificador dentro de un programa. Además de introducir un nombre, una
declaración puede opcionalmente definir un valor, porque una declaración
puede ser al mismo tiempo una definición. Dependiendo del lugar en donde
se haya creado una declaración se puede hacer referencia a su valor.
Existen declaraciones que son locales, estas no se pueden llamar desde
fuera del bloque de código que las contiene, otras declaraciones son
globales y están disponibles en todo el programa.

El tiempo de vida de una declaración depende de la categoría de
almacenamiento que use. Algunas declaraciones son automáticas, se crean
y se destruyen cuando salen de su ámbito, mientras que otras
declaraciones continúan existiendo hasta el final del programa.

Este capítulo presenta la forma de declarar y definir nombres dentro de
un programa.

## Variables

Un objeto, también llamado variable, es una ubicación en el espacio de
almacenamiento y su interpretación depende de dos atributos
fundamentales:

1.  Un **calificador de almacenamiento**: que determina el tiempo de
    duración del objeto en memoria.
2.  Un **tipo** que determina el significado de los valores encontrados
    en el objeto identificado.

Una variable tiene un alcance léxico (o ámbito). El alcance léxico es la
región del programa dentro de la que es visible y un enlace que
determina si el alcance de un nombre se conoce sólo en el archivo fuente
en el que se declara, o en varios archivos fuente externos (agregados en
el enlazado).

### Anatomía de una variable

Toda variable debe declarar un tipo, un nombre y opcionalmente definir
un valor:

``` C++
Categoría de
almacenamiento                Un valor
  |                             |
  v                             v
static const double temperatura{24.5};     // <- termina en punto y coma
        ^     ^          ^
        |     |          |
        |  El tipo   El nombre 
        |          (declarador)
   Modificador
   de acceso
```

En este caso:

**La categoría de almacenamiento** determina el tiempo de vida del
objeto. La categoría de almacenamiento puede ser: `static` cuando la
variable es estática, o `extern` si está declarada y definida en
archivos diferentes.

**El modificador de acceso** nos dice si la variable se puede modificar.
El modificador puede ser `const` o `volatile`. Si es un valor constante
este debe ser declarado con la palabra clave `const` y si el hardware
puede modificarlo, se usa el calificador `volatil`.

**El tipo de dato**. Una variable debe declarar el tipo de dato que
puede guardar. Por ejemplo, los números enteros se guardan en variables
de tipo `int`, un carácter en un tipo `char`, los números reales en un
tipo `double`, etc.

C++ es un lenguaje estáticamente tipado. Esto significa que el tipo de
cada entidad (objeto, valor, nombre o expresión) debe ser conocido por
el compilador antes de poder usarse.

:::: note
::: title
Note
:::

**Tipificación**

Algunos lenguajes de programación requieren que a todas las variables se
les asigne un tipo antes de que se puedan utilizar, otros lenguajes no
necesitan este requisito permitiendo que las variables puedan tomar
diferentes tipos en cualquier momento. Estas dos opciones se denominan
tipificación fuerte, estricta o estática y tipificación débil, no
estricta o dinámica dependiendo del lenguaje (y del autor).
::::

**El declarador**. Un declarador se compone de un nombre obligatorio y
opcionalmente de algún operador pre-fijo como un puntero, una referencia
o un operador post-fijo como los corchetes `[]` o los paréntesis `()`.

+---------------+---------------+----------------------------------+
| Declarador    | Posición      | Significado                      |
+===============+===============+==================================+
| `*`           | Prefijo       | Puntero                          |
+---------------+---------------+----------------------------------+
| `const*`      | Postfijo      | Puntero constante                |
+---------------+---------------+----------------------------------+
| `*const`      | Prefijo       | Puntero a constante              |
+---------------+---------------+----------------------------------+
| `*volatile`   | Prefijo       | Puntero volatil                  |
+---------------+---------------+----------------------------------+
| > &           | Prefijo       | Referencia a valor izquierdo     |
+---------------+---------------+----------------------------------+
| &&            | Prefijo       | Referencia a valor derecho       |
+---------------+---------------+----------------------------------+
| auto          | Prefijo       | Deducción de tipo                |
+---------------+---------------+----------------------------------+
| \[\]          | Postfijo      | Arreglo                          |
+---------------+---------------+----------------------------------+
| ()            | Postfijo      | Función                          |
+---------------+---------------+----------------------------------+
| −\>           | Postfijo      | Valor de retorno de una función  |
+---------------+---------------+----------------------------------+

: Declaradores

**Un identificador** es simplemente el nombre de una variable, y como
tal se rige por las reglas y restricciones básicas del lenguaje.

**Un valor opcional** el valor que la variable debe almacenar. El valor
puede ser declarado mediante asignación directa usando el operador `=`,
o mediante inicialización uniforme usando llaves `{}` o mediante
conversión implícita usando paréntesis `()`.

Una vez que una variable declara un tipo, el tipo no pueda cambiarse, ni
tampoco se pueden almacenar datos de diferentes tipos en la misma
variable. Este mecanismo ayuda a mejorar los tiempos de ejecución de un
programa, ya que no hay necesidad de comprobar los tipos cuando se
ejecuta el programa.

## ¿Qué son las declaraciones?

Una declaración es una sentencia que introduce crea y opcionalmente
inicializa uno o varios identificadores. Puede tener efectos:

- En una aserción estática.
- Puede controlar la instanciación de plantillas.
- Puede usar atributos.
- O no hacer nada (una declaración vacía: `;`).

Además de introducir nuevos nombres, una declaración especifica la forma
en que el compilador debe interpretar un identificador y los atributos
de un nombre, ademas tiene otros usos:

- Declarar variables.
- Especificar una clase, un tipo, o un enlace a un objeto o función.
- Declarar funciones virtuales o en línea.
- Calificar una declaración como `const` o `volatile`.
- Asociar un nombre con un valor constante.
- Declarar una enumeración.
- Declarar un nuevo tipo (Una clase, una estructura o un objeto).
- Especificar un sinónimo o crear un alias para un tipo (usando
  `using`).
- Especificar un espacio de nombres.

Cuando se declara una variable basta con usar un tipo de dato y un
identificador:

``` C++
extern int a;                           // Declara a
extern const int c;                     // Declara c
int max(int, int);                      // Declara la función max
struct E;                               // Declara la estructura E
typedef int Int;                        // Declara Int como alias de int
extern class A;                         // Declara la clase A como externa
using N::d;                             // Declara d, usando N::d

// Otras formas
enum X : int;                           // Declara X como un int
using matriz = std::vector<int>;        // Declara a matriz como un alias
static_assert(X::y == 1, "¡Fallo!");    // Declara una aserción
template <class T> class C;             // Declara una plantilla de clase
;                                       // Una declaración vacía
```

Una declaración introduce un identificador y describe su tipo, puede ser
una variable, una función, estructura, enumeración o clase. En estos
casos, el nombre debe ser declarado, antes de usarse de lo contrario el
compilador mostrará un error.

``` C++
#include <iostream>                       // cout

// Suma los dígitos de un número entero (declaración de función)
// 911 = 9 + 1 + 1 = 11 = 1 + 1 = 2
std::size_t sumar_digitos(std::size_t num);

// Suma los dígitos de un valor entero
int main()
{
    std::cout << "Introduce un número: ";
    std::size_t num;
    std::cin >> num;
    std::cout << "La suma de dígitos de: "
              << num << " es "
              << sumar_digitos(num);

    return EXIT_SUCCESS;
}

// Define la función sumar_digitos (definición de función)
std::size_t sumar_digitos(std::size_t num)
{
    if (num == 0) {
        return 0;
    } else if (num % 9 == 0) {
        return 9;
    } else {
        return num % 9;
    }
}
```

En C++, las declaraciones se pueden colocar en cualquier parte de un
programa, pero por practica general, una variable se declara lo más
cerca posible del lugar en el que se va a usar.

:::: warning
::: title
Warning
:::

Si intentas usar el valor de un identificador que no ha sido declarado
obtendrás un error de compilación. También es un error llamar a una
función no definida.
::::

Una declaración es lo único qué el compilador necesita para aceptar
referencias a ese identificador. El valor de la declaración puede
asignarse más tarde o puede modificarse a lo largo del programa.

### Declaración uniforme

Cuando se conoce el valor de una variable se puede usar una declaración
de inicialización uniforme. Una declaración uniforme usa llaves para
declarar el valor de un tipo.

``` C++
int a{1};             // Declaración uniforme: usa llaves {}
double b{1.5}         // Declaración y definición de un tipo doble
double c = {1.5}      // El signo = es redundante   
```

Sin embargo hay que tener cuidado:

``` C++
int ai2{10.2};        // Error: conversión de flotante a entero
int a3 = {10.2};      // Error: conversión de flotante a entero
```

La inicialización uniforme con llaves permite usar las llaves para
indicar el valor de cualquier tipo de dato. No realiza conversiones, por
esta razón solo acepta valores del mismo tipo que espera la variable.
Todos los tipos de datos de C++ se pueden inicializar con una
declaración uniforme.

### Declaración y asignación

Cuando se declara una variable con un valor inicial, el valor puede
usarse inmediatamente. Para cambiar el valor de una variable se usa el
operador de asignación `=`:

``` C++
int num{100};         // La variable num es igual a 100,
num = 200;            // ahora es 200
num = 200.9           // aun es 200 (sorpresa)
```

Cuando se hace una asignación, el compilador de forma implícita realiza
conversiones entre tipos para asegurarse que el tipo usado, corresponda
con el tipo que espera la variable. Por esta razón se puede asignar un
tipo `double` en la tercera línea, sin embargo el compilador lo
convierte a entero antes de asignarlo a la variable de tipo entero.

``` C++
// La coma puede inicializar varias variables (con valores indefinidos)
int a, b, c;          

// y puede asignar el mismo valor a todas las variables 
a, b, c = 100;
```

Si necesitas definir más de una variable puedes usar el operador `coma`
si las variables son del mismo tipo y requieren del mismo valor.

#### Inicialización vs asignación

La inicialización es el proceso de asignar un valor a una variable en el
momento en que se declara.

``` C++
int x = 23;       // Inicialización con signo igual
int y(23);        // Inicialización con paréntesis
int z{23};        // Inicialización uniforme (C++11 en adelante)
```

Según el estándar esta acción se llama inicialización y puede adoptar
diferentes formas:

1.  **Inicialización por copia**: `int x = 23;`
2.  **Inicialización directa**: `int x(23);`
3.  **Inicialización uniforme (o por lista de inicialización)**:
    `int x{23};`

De estas tres formas, la inicialización uniforme es la más segura ya que
evita conversiones entre diferentes tipos.

La asignación es el proceso de dar un nuevo valor a una variable ya
existente.

``` C++
x = 23;            // Asignación: cambia el valor de x
```

Según el estándar esta acción se llama asignación, y se realiza con el
operador `=`, que equivale a copiar el valor del lado derecho en el
objeto del lado izquierdo.

### Errores en declaraciones

Cuando una variable es declarada, el compilador puede atrapar
operaciones no válidas. Por ejemplo, una variable puede ser declarada
como `int`, pero el programador puede accidentalmente asignar un tipo no
numérico al valor de la variable. En un lenguaje de programación
dinámicamente tipado, la variable puede cambiar el tipo, introduciendo
errores en el programa de forma silenciosa. En C++, el compilador
reporta asignaciones equivocadas como errores de tipo, porque una vez
que una variable es declarada, no puede cambiar su tipo.

``` C++
decimal     = 0.123;            // Error: No se declara un tipo
char saludo = "Hola mundo";     // Error: Tipos diferentes
int numero  = "5";              // Error: "5" es una cadena 

// Declara una función, pero no la define
int fun(int);
fun(100);                       // Error función no definida
```

Sin embargo, existen casos en los que es posible evadir esta
restricción. C++ posee un mecanismo interno conocido como conversión de
tipos, que convierte un tipo de dato a otro de forma implícita. Por
ejemplo un valor decimal a un entero o un carácter a su valor ASCII.

``` C++
int num1 = 3.14;                // Bien: num1 es 3
int num2(3.14);                 // Bien: num2 es 3
int letra = 'A'                 // Bien: 65 es el código ASCII de A
(letra + letra);                // Bien: 130 (65+65)
```

Esto solo ocurre cuando se usa el operador de asignación o los
paréntesis.

``` C++
int num{3.14};                  // Error
float decimal{5}                // Error
```

La inicialización uniforme no permite la conversión implicita, asi que
esto es un error.

:::: warning
::: title
Warning
:::

**Inicialización uniforme vs asignación**

En muchas lenguajes de programación el signo de `=` sirve para asignar
directamente un valor a una variable. Pero en C++ estas dos operaciones
son diferentes:

``` C++
int numero{100};            // Definición directa
int numero = 100;           // Declara y asigna un valor
```

En C++ cuando se asigna un valor usando el operado `=` lo que estamos
haciendo es copiar el valor literal en una variable (dos operaciones).
Esto no ocurre con la inicialización uniforme, la cual le asigna el
valor a la variable en una misma operación.
::::

La tipificación fuerte en C++ es muy flexible, pero es segura, ya que el
lenguaje puede realizar comprobaciones de rutina en tiempos de
compilación para comprobar que los parámetros de una variable sean del
tipo correcto, antes de que se ejecute el programa. Las ventajas de un
lenguaje estáticamente tipado son que los errores relativos a tipos se
capturan o se detectan durante la compilación, antes de que se ejecute
el programa, de esta forma el programa se puede ejecutar más
eficientemente, dado que no hay necesidad de hacer ninguna verificación
de tipos en tiempos de ejecución del programa.

## ¿Qué son las definiciones?

Además de introducir un identificador, una declaración puede
opcionalmente definir un valor, porque una declaración puede ser al
mismo tiempo una definición.

``` C++
int a;                                  // Define a
extern const int c = 1;                 // Define c
int f(int x) { return x + a; }          // Define f y define x
struct S { int a; int b; };             // Define S, S::a, and S::b
struct X {                              // Define X
    int x;                              // Define miembros no estáticos
    static int y;                       // Declara un miembro estático y
    X(): x(0) { }                       // Define el constructor de X
};

int X::y = 1;                           // Define X::y
enum {uno, dos};                        // Define uno y dos
namespace N{int d;}                     // Define N y N::d
namespace N1 = N;                       // Define N1
X intX;                                 // Define intX
```

Algunas entidades, incluidas las funciones, las clases, las
enumeraciones y las variables constantes, deben definirse además de
declararse. Una definición proporciona información que le permite al
compilador reservar memoria para objetos o generar código para funciones
y clases.

### La regla de definición única

La única restricción en la definición de variables es que no se puede
redefinir un identificador, ya que solo puede haber una definición de
una variable. A esto se le conoce como \"regla de definición única\".

``` C++
int valor{65};       
char valor{'A'};                         // Error
```

En este caso el compilador se queja, de que el identificador ya ha sido
declarado.

:::: tip
::: title
Tip
:::

**Regla de definición única**

Ninguna unidad de traducción puede contener más de una definición de
cualquier variable, función, clase, enumeración o plantilla.
::::

Una declaración proporciona información al compilador sobre las
características de un tipo o una función pero no define su código. Una
definición, por otra parte define el código, algunas variables útiles, o
el código ejecutable. Se requiere una y solamente una definición de cada
entidad en un programa.

### Declaración y definición de funciones

Una definición de función incluye el cuerpo de la función:

``` C++
#include <iostream>                               // cout

// Dos declaraciones con el mismo nombre (que devuelven diferentes tipos)
constexpr int max(int a, int b);                 // tipo int
constexpr double max(double a , double b);       // tipo double

// Sobrecarga de funciones
int main()
{
    std::cout << max(100, 200)     << '\n'
              << max(100.0, 200.0) << '\n';
}

// Devuelve el valor máximo entre dos enteros
constexpr int max(int a, int b) 
{ 
    return a > b ? a : b; 
}

// Devuelve el valor máximo entre dos decimales
constexpr double max(double a, double b) 
{ 
    return a > b ? a : b; 
}
```

Las declaraciones que no son definiciones incluyen declaraciones de
clase sin usar miembros y las declaraciones de funciones no incluyen el
cuerpo de la función. La declaración de una función es llamada
`prototipo`. Esta forma de declaración proporciona información sobre el
tipo de los argumentos y el valor de retorno de la función, esto le
permite al compilador realizar la conversión correcta y le ayuda a
determinar de forma segura el tipo de datos que maneja la función.

Es legal declarar varias funciones con el mismo nombre, siempre y cuando
acepten parámetros o valores diferentes. Una declaración simplemente
introduce un nombre mientras que una definición define el cuerpo entre
llaves de una función (o una clase).

## El ámbito

En C++ los nombres usados como identificadores únicamente pueden ser
usados en ciertas regiones de un programa. Esta área es llamada
comúnmente ámbito, el estándar la llama \"región declarativa\". El
ámbito se encarga esencialmente de determinar la visibilidad y el tiempo
de vida de los identificadores usados en un programa.

Hay esencialmente dos tipos de ámbitos: global y local.

1.  **Global** nombres fuera de bloques.
2.  **Local** nombres dentro de espacios de nombres, funciones, clases y
    bloques entre llaves.

Los nombres declarados dentro de un bloque entre llaves son nombres
locales, mientras que los nombres declarados fuera de un bloque son
nombres globales para el bloque.

``` C++
int x = 20;                             // variable global

{
    int y = 30;                         // variable local
    std::cout << x << ", " << y;
}
```

Los nombres locales no pueden ser accedidos desde fuera del bloque, sin
embargo los nombres dentro de un bloque, pueden modificar el valor de
una variable definida fuera.

``` C++
int x = 0;                             // variable global

void incrementar() 
{
    ++x;                               // Modifica la variable global
}
```

En el ámbito global se declaran identificadores en el nivel superior de
un bloque, en el ámbito local, se declaran los nombres dentro de
espacios de nombres, funciones, clases y bloques entre llaves.

``` C++
#include <iostream>         // cout      

// Una variable global
int contador = 0;  

// Función que modifica la variable global 
void incrementar_global() 
{
    ++contador; 
    std::cout << "Contador global: " << contador << std::endl;
}

// Función que modifica una variable local
void incrementar_local() 
{
    int contador = 5;        // La variable local oculta a la global
    ++contador;              // contador es 6 
    std::cout << "Contador local:  " << contador << std::endl;
}

// Variables locales y globales
int main() 
{
    std::cout << "Inicio del programa\n";

    incrementar_global();     // Usa la variable global
    incrementar_local();      // Usa una variable local con el mismo nombre
    incrementar_global();     // La variable global sigue existiendo
    incrementar_local();      // La función se reinicia con el mismo valor

    return EXIT_SUCCESS;
}
```

En este ejemplo, la función `incrementar_global()` modifica la variable
global llamada `contador`. Mientras que la función `incrementar_local()`
también define una variable local llamada `contador`, que **no afecta**
a la global. Al ejecutar el programa, la variable global mantiene su
valor entre cada llamada, mientras que la variable local se reinicia
cada vez que se ejecuta la función.

La diferencia principal entre un nombre global y uno local es el
alcance. Mientras que una variable global puede ser accedida en
cualquier parte del código (que se declare antes), una variable local
solo puede ser accedida dentro del bloque de código, que la declara o
define.

### Variables globales

Un nombre global es aquél que se declara fuera de cualquier clase,
función o espacio de nombres.

``` C++
// Nombres globales
#include <cmath>       // abs
#include <iostream>    // cout, cin
#include <iomanip>     // setw, setfill
```

Todos los nombres introducidos por las directivas `#include` son
globales, al igual que los nombres usados al principio de un programa.

``` C++
// Nombres globales
int numero{3144};
int limite{9999};

int main() { }
```

Los nombres globales son accesibles desde cualquier función dentro del
mismo archivo (o desde otros archivos si se usa la palabra clave
`extern`) y existen durante toda la ejecución del programa.

:::: warning
::: title
Warning
:::

**Evita usar variables globales**

El uso de variables a nivel de bloques ayuda a separar el alcance léxico
de las variables y es una buena práctica. Muchos de los errores básicos
se dan al sobrescribir valores globales en bloques locales de código.
Usar nombres globales se considera una mala práctica, por esta razón se
usan los espacios de nombres.
::::

Sin embargo existen algunos nombres con un espacio de nombres global
implícito, como el espacio de nombres `std`.

``` C++
std::abs               // std es un espacio de nombres global
std::cout             
```

El ámbito de los nombres globales se extiende desde el punto de
declaración hasta el final del archivo en el que se declaran.

#### Ocultando nombres globales

Es posible ocultar una variable global usando un nombre local con el
mismo nombre. El compilador busca (y usa) primero las variables locales.
Por ejemplo, si se define una variable `x` como global y luego se usa
una variable `x` dentro de un bloque, la variable local oculta la
variable global. A esto se le conoce como **ocultación de nombres**.

``` C++
// x es una variable global (esta fuera de un bloque).
int x{100};

{   
    // x es una variable local.
    int x{50};
    std::cout << x;                                 // 50
}
```

Sin embargo aún es posible usar la variable global usando el operador de
resolución de ámbito: `::`.

``` C++
#include <iostream>                                 // cout

// x es una variable global (esta al inicio del programa).
int x{100};

// Ocultación de nombres
int main()
{   
    // x es una variable local, está dentro del bloque main.
    int x{50};

    std::cout << "x       = " << x         << '\n'  //  50
              << "::x     = " << ::x       << '\n'  // 100
              << "x + ::x = " << (x + ::x) << '\n'; // 150

    return EXIT_SUCCESS;
}
```

Se pueden tener varias variables con el mismo nombre en un mismo
programa; siempre y cuando se encuentren en ámbitos distintos y no
infrinjan la regla de definición única. En la última línea se suman dos
variables con el mismo nombre, sin embargo la variable definida fuera
del bloque usa el operador de resolución de alcance: `::x`.

:::: note
::: title
Note
:::

Una variable local, puede tener el mismo nombre que una variable global,
pero es totalmente independiente de ella. El cambio de valor de la
variable local no tiene efectos en la variable global (y viceversa).
::::

Un nombre es global si se presenta fuera de cualquier función o clase o
es precedido por el operador unario `::`.

### Ámbitos locales

Existen varios tipos de ámbitos locales:

- **Ámbito a nivel de bloques** Los nombres declarados entre llaves o
  entre instrucciones `for`, `if`, `while` o `switch` son visibles hasta
  el final del bloque de instrucciones.
- **Ámbito de espacio de nombres** Un nombre que se declara dentro de un
  espacio de nombres, fuera de cualquier definición de clase o
  enumeración o bloque de función, es visible desde su punto de
  declaración hasta el final del espacio de nombres. Un espacio de
  nombres se puede definir en varios bloques y a través de archivos
  diferentes.
- **Ámbito de función** Una función tiene ámbito (de función), lo que
  significa que es visible en todo el cuerpo de la función, incluso
  antes de su punto de declaración.
- **Ámbito de clases** Los nombres de los miembros de una clase también
  tienen un ámbito, que se extiende a lo largo de la definición de la
  clase, independientemente del punto de declaración.

El alcance de una variable puede ser local si se declara dentro de un
bloque de código encerrado entre llaves.

``` C++
#include <iostream>                          // cout

// Ámbitos a nivel de bloques
int main()
{
    // Una variable local a nivel de función
    int x = 100;  
    {
        // Una variable local a nivel de bloques
        int x = 200;          
        x += 200; 
        std::cout << "x = " << x << '\n';   // x es 400 
    }

    std::cout << "x = " << x << '\n';       // x es 100 

    return EXIT_SUCCESS;
}
```

Las declaraciones dentro de bloques solo son accesibles después de haber
sido definidas. Cada bloque crea un ámbito y en cada ámbito los nombres
usados son los nombres locales, si no se encuentra un nombre, el
compilador lo busca en el ámbito superior y si no lo encuentra lanza un
error en tiempo de compilación.

#### Variables no definidas

Las variables globales son automáticamente inicializadas por el programa
cuando se definen. Todos los tipos numéricos se inicializan a `0`,
mientras que el tipo `char` se inicializan con el valor `'\0'`. Sin
embargo las variables locales no definidas, no tienen un valor usable:

``` C++
#include <iostream>                   // cout

// Una variable global (sin un valor)
int x;

// Diferencia entre una variable global y una local
int main()
{
    // Una variable local (sin un valor)
    int y;      
    std::cout << "x = " << x << '\n'   // El valor de x es 0
              << "y = " << y << '\n';  // El valor de y es indefinido

    return EXIT_SUCCESS;
}
```

Cuando una variable local es definida si un valor no es inicializada por
el programa. Es responsabilidad del programador inicializar las
variables locales.

:::: warning
::: title
Warning
:::

**Variables no inicializadas**

Una variable no inicializada contiene un valor indefinido. Un valor
indefinido puede ser cualquier valor, sin embargo no es un valor usable.
::::

Una variable local, no se puede usar fuera de su ámbito (definido por
llaves). Por ejemplo, si declaras una variable `x` en un bloque, `x`
solo es visible dentro del cuerpo del bloque. Tiene ámbito local.

``` C++
#include <iostream>                 // cout

// Bloques y ámbitos anidados
int main()
{
    std::cout << "Bloques anidados:";
    {  
        int x{100};                // Bloque 1
        std::cout << "\nbloque 1,  x = " << x;
        {  
            int x{200};            // Bloque 2
            std::cout << "\nbloque 2,  x = " << x;
            {   
                int x{300};        // Bloque 3
                std::cout << "\nbloque 3,  x = " << x;
            }
            {   
                // Bloque 4 (está dentro de bloque 2)
                std::cout << "\nbloque 4,  x = " << x;
            }            
        }
    }

    return EXIT_SUCCESS;
}
```

Puedes haber otras variables con el mismo nombre en un mismo programa,
siempre y cuando se encuentren en ámbitos distintos y no infrinjan la
regla de definición única, no se producirá ningún error.

#### El punto de declaración

El punto de declaración para un nombre ocurre inmediatamente después de
que se complete su declarador y antes del inicializador (si existe).

``` C++
int x{10};      // x vale 10
{ 
    int x = x;  // x es indefinido.
}  
```

En este ejemplo, el valor del segundo `x` es inicializado con un valor
indeterminado, no con el valor de la variable `x` definida fuera del
bloque, porque la variable `x` (dentro del bloque) aun no entra a su
punto de declaración. Esto es un comportamiento indefinido.

## Espacio de nombres

Un espacio de nombres es una región declarativa que proporciona un
ámbito a los identificadores declarados dentro de las llaves: como
variables, funciones y clases. Esta es su sintaxis:

``` C++
namespace nombre {
    // código
} 
```

Las llaves delimitan los espacios de nombres. Las variables y todos los
elementos declarados dentro de las llaves son llamados miembros. Todos
los miembros de un espacio de nombres son invisibles desde fuera. Y al
igual que los bloques de código, los espacios de nombres no necesitan
punto y coma.

``` C++
// un espacio de nombres llamado math
namespace math {
    const double pi{3.14159265358979};

    double circunferencia(double radio) 
    { 
        return 2 * pi * radio; 
    };
} 
```

En este ejemplo se define un espacio de nombres llamado `math`, el cual
contiene dos miembros la variable `pi` y la función `circunferencia()`.
Por lo general, los espacios de nombres se utilizan para organizar el
código en grupos lógicos y para evitar conflictos de nombres que pueden
producirse cuando se utilizan diferentes bibliotecas que usan nombres
comunes.

### Acceso a miembros en espacios de nombres

Todos los identificadores usados dentro de un espacio de nombres son
visibles entre sí, sin usar un calificador. Sin embargo, para hacer los
elementos visibles fuera del espacio de nombres se debe invocar el
espacio de nombres al que pertenecen. Hay dos formas de hacer esto. El
primero y el más conocido es preceder el nombre del identificador con el
operador de resolución de alcance:

``` C++
double circ = math::circunferencia(5.0); 
```

También, se puede usar la directiva `using` para importar directamente
el espacio de nombres (completo):

``` C++
using namespace math;

double circ = circunferencia(5.0); 
```

O se puede importar algún miembro en específico:

``` C++
using math::circunferencia;

double circ = circunferencia(5.0); 
```

Hay que tener cuidado, cuando se importa un espacio de nombres de forma
global, ya que todos los miembros definidos dentro del espacio de
nombres se importan al programa actual, no importa que se usen o no.

### Extendiendo los espacios de nombres

Los espacios de nombres pueden extenderse. El ejemplo siguiente extiende
el espacio de nombres `math` en otro espacio que usan el mismo nombre:

``` C++
#include <iostream>                                   // cout

// Un espacio de nombres llamado math
namespace math {
    const double pi {3.14159265358979};
    double circunferencia(double radio) { return 2 * pi * radio; }; 
}

// Otro espacio de nombres llamado math (definido en otra parte)
namespace math {

    // funciones definidas en math
    double restar(double a, double b) { return a - b; }
    double sumar(double a, double b) { return a + b; }
    double multiplicar(double a, double b)  { return a * b; }

    double dividir(double a, double b)
    {   // Lanza un excepción si b es 0;
        if (b == 0) {
            throw("División entre cero");
        }
        return (a / b);
    }
}

// Espacios de nombres
int main()
{
    double a{100.0};
    double b{200.0};

    std::cout << math::restar(a, b)      << '\n'   // -100
              << math::sumar(a, b)       << '\n'   // 300
              << math::multiplicar(a, b) << '\n'   // 20000
              << math::dividir(a, b)     << '\n'   // 0.5
              << math::circunferencia(5) << '\n'   // 31.4159
              << math::pi                << '\n';  // 3.14159

    return EXIT_SUCCESS;
}
```

Es posible declarar entidades dentro de un espacio de nombres y
definirlas fuera, solo es necesario adjuntar el nombre del espacio de
nombres a un nombre de una entidad declarada, usando el operador de
resolución de alcance:

``` C++
#include <iostream>                 // cout
#include <cmath>                    // abs

// Extiende el espacio de nombres std
namespace std  {
    // Devuelve el restante (positivo) de i % j.
    int modp(int i, int j);
}

// Extiende el espacio de nombres std
int main()
{
    std::cout << std::modp(26, 7);   // 5
}

// Define la función modp (modulo) fuera del espacio de nombres estándar
int std::modp(int i, int j)
{   
    // Lanza una excepción si j es 0
    if (j == 0) {
        throw("No se puede dividir i entre 0");
    }

    int valor = i % j;
    if (valor < 0) {
        valor += std::abs(j);
    }

    return valor;
}
```

En este ejemplo, la función `modp()` se adjunta al espacio de nombres
`std` usando el operador `::`, sin embargo hay que tener en cuenta, que
para definir una función (o cualquier objeto) fuera de un espacio de
nombres, es necesario declararlo primero dentro del espacio. Esto es muy
parecido a declarar miembros de clase, o declarar prototipos de función.
Este mecanismo permite crear un espacio de nombres a lo largo de varios
ficheros, esta es la forma en que funciona el espacio \"std\", que usan
todas las bibliotecas estándar. Sin embargo extender el espacio de
nombres `std` es considerado una mala practica.

### Espacios de nombres anidados

Al igual que los bloques, los espacios de nombres se pueden anidar; es
decir se pueden declarar dentro de otro espacio.

``` C++
#include <iostream>                  // cout

// Espacios anidados
namespace uno {
    int x;
    namespace dos {
    int x;
        namespace tres {
            int x;
        }
    }
}

// Espacios de nombres anidados
int main()
{   
    using uno::x;    // Un alias al espacio de nombres uno

    // Asigna un valor a todas las variables
    x = 100;    
    uno::dos::x = 200;
    uno::dos::tres::x = 300;

    std:: cout << "Espacios de nombres anidados: \n"
               << "uno::x =            " << uno::x             << '\n'
               << "uno::dos::x =       " << uno::dos::x        << '\n'
               << "uno::dos::tres::x = " << uno::dos::tres::x  << '\n';

    return EXIT_SUCCESS;
}
```

Para acceder a un espacio de nombres anidado, solo es necesario adjuntar
el operador de resolución de alcance `::` para encontrar la jerarquía
que ocupa el espacio anidado dentro de los bloques.

### Espacios de nombres anónimos

También se pueden crear espacios de nombres anónimos:

``` C++
#include <iostream>                // cout

// Un espacio de nombres anónimo
namespace {
    const double e{2.7182818284590452};
}

// Formas de acceder a un espacio de nombres anónimo
int main()
{  

    std::cout <<   e << '\n'     // Usa el nombre de la variable
              << ::e << '\n';    // Usa el operador ::
}
```

Un espacio de nombres anónimo, no utiliza un nombre, solo la palabra
clave `namespace` y llaves. Los miembros de un espacio de nombres
anónimo se acceden directamente usando su nombre. También pueden ser
accedidos usando el operador `::` como si fueran variables globales.

## Creando alias

Hay dos formas de declarar alias, es decir nombres alternativos en un
programa. Usando las palabras clave `typedef` y `using`. Es importante
señalar que esta dos declaraciones no introducen nuevos tipos; si no que
simplemente introducen nuevos nombres para tipos existentes. Por lo
general `typedef` se usa en código antiguo heredado de C, mientras que
en C++ se acostumbra usar `using`, ya que esta palabra clave tiene
varios usos.

### La palabra clave typedef

La palabra clave `typedef` introduce un nuevo nombre a partir de un
nombre conocido dentro un ámbito.

``` C++
// Declara real como un alias para un tipo long long 
typedef unsigned long long real;
```

Ahora `real` es un sinónimo del tipo `unsigned long long` y se puede
usar para declarar y definir variables de este tipo. Por ejemplo:

``` C++
real num{1000L};
```

Se pueden utilizar declaraciones `typedef` para construir nombres más
cortos o más significativos para algunos tipos definidos por el lenguaje
o para usar tipos declarados. Los nombres creados con `typedef` permiten
encapsular detalles de la implementación que pueden cambiar, así en
lugar de cambiar un tipo en todo el código, solo se cambia en un solo
lugar.

``` C++
// declara decimal como un alias de tipo long double
typedef long double decimal;

decimal valor{10.5};     // Bien: decimal es un tipo double
double decimal{1.0};     // Error: el nombre decimal ya ha sido usado
```

Los nombres declarados con `typedef` ocupan el mismo espacio de nombres
que otros identificadores (excepto las etiquetas de instrucción). Por
consiguiente, no se puede utilizar el mismo identificador que use un
nombre declarado previamente, excepto en una declaración de tipo de
clase.

### La palabra clave using

La palabra clave `using` permite especificar un alias que sirve como un
nombre alternativo para un tipo de nombre existente.

``` C++
using real = unsigned long long;
```

Es importante entender que no estamos definiendo un nuevo tipo de dato.
Solo definimos un nombre alternativo para `unsigned long long`.

``` C++
real numero{10L}; // Define e inicializa un tipo unsigned long long
```

No hay diferencia entre esta definición y usar el tipo estándar. Se
puede usar tanto el tipo estándar como el alias, aunque no hay razón
para usar ambos. Por lo general, siempre se usa la palabra clave `using`
para definir un alias de tipo en C++. De hecho, si no fuera por código
heredado de C, la palabra clave `typedef` no tendría razón de existir.

#### Usando alias

La declaración `using` tiene varios usos que dependen del contexto:

1.  Se puede usar para introducir nombres definidos dentro de espacios
    de nombres.
2.  Importar entidades específicas de un espacio de nombres.
3.  Crear nombres alternativos para tipos.

En el primer caso `using` introduce un espacio de nombres completo, en
el segundo permite usar nombres específicos, si no hay razón para
importar el espacio de nombres completo, en el tercer caso simplemente
crea un alias para un tipo.

``` C++
#include <iomanip>            // setw
#include <iostream>           // cout 

// Crea una tabla de multiplicar
int main()
{
    using namespace std;      // Importa el espacio de nombres std
    using num = std::size_t;  // Crea un alias de tipo entero sin signo
    const num limite{10};     // num es un tipo, no es una variable

    cout << setw(14) << "Las " << "tablas de multiplicar\n  |";

    // Imprime la primera fila (1-10)
    for (num i = 1; i <= limite; ++i) {
        cout << setw(4) << i;
    }

    cout << "\n--+-----------------------------------------\n";

    // Imprime las filas y columnas de 1-10
    for (num j = 1; j <= limite; ++j) {
        cout << j << " |";
        for (std::size_t k = 1; k <= limite; ++k)  {
            cout << setw(4) << j * k;
        }
        cout << '\n';
    }

    return EXIT_SUCCESS;
}
```

La declaración `using` introduce un nombre como sinónimo de una entidad
declarada en otro lugar. Permite usar un nombre único sin la
calificación explícita.

``` C++
using num = std::size_t;         // Crea un alias llamado num
const num limite{9};             // Usa el alias como si fuera un tipo
```

Un nombre definido por una declaración `using` es un alias para su
nombre original. No afecta al tipo, la vinculación, ni los demás
atributos de la declaración original.

## El tiempo de vida de una variable

Todas las variables tienen un tiempo de vida específico. Las variables
existen en el punto en que se declaran y en algún punto antes de que
finalice el programa son destruidas. El tiempo de duración de una
variable en particular, es determinado por la duración del almacenaje
usado.

C++ tiene diferentes tipos de almacenaje:

1.  Las variables definidas como **estáticas** usan la palabra clave
    `static`. Estas variables tienen una duración de almacenaje estático
    y existen desde el punto en que se declaran y continúan existiendo
    hasta que el programa finaliza.
2.  Las variables definidas dentro de un bloque (no definidas como
    estáticas), tienen una duración automática de almacenaje. Estas
    variables existen a partir del punto en que se declaran hasta el
    final del bloque que las contiene. A estas variables se les conoce
    como variables **automáticas** o variables **locales**. Las
    variables automáticas tiene un ámbito local, es decir un ámbito
    basado en bloques.
3.  Las variables dinámicas permiten alojar memoria en tiempos de
    ejecución del programa. Estas variables tienen una duración de
    almacenaje dinámico. Existen desde el punto en que se crean hasta
    que se libera la memoria para destruirlas. Una variable dinámica se
    crea usando el operador `new` y se borra usando el operador
    `delete`.
4.  Las variables declaradas con la palabra clave `thread_local` tienen
    una duración del almacenaje basado en hilos. Existen mientras el
    hilo se ejecute.

Dentro de su ámbito a una variable se le puede hacer referencia, se le
puede asignar un valor, o se puede usar en una expresión. Fuera de su
ámbito, no se pueden referir por su nombre, porque no son visibles, sin
embargo algunas variables pueden existir fuera de su ámbito, aunque no
se pueda hacer referencia a su nombre. Esto pasa con variables con
almacenaje estático y dinámico.

### Variables locales

A diferencia de las variables globales que tienen un tiempo de vida que
depende de la duración del programa, las variables locales tienen un
tiempo de vida limitado y dependen del bloque en el que están definidas.

``` C++
{
    // Una variable definida dentro de un bloque se crea y se destruye
    // de forma automática cuando el flujo de control la evalúa.
    int a = 100;
}

// En este punto, la variable a no existe.
```

Las variables locales existen a partir del punto en que se declaran
hasta el final del bloque que las contiene, por esta razón también se
les conoce como variables automáticas. Las variables automáticas tiene
un ámbito local, es decir un ámbito basado en bloques.

### Variables externas

Las variables locales solo pueden ser definidas dentro de un fichero. Es
decir son locales al fichero que las contiene, para compartir un valor o
una variable en varios ficheros es necesario usar la palabra clave
`extern`.

``` C++
// Define e inicializa una constante que es accesible por otros ficheros
extern const double pi{3.141592653589793238462};
```

La palabra clave `extern` marca una variable como externa, es decir una
variable que se puede usar en distintos ficheros. El uso del calificador
`const` nos dice que el valor declarado no puede cambiar.

#### Usando extern

Por ejemplo, si tenemos dos archivos con código fuente: `archivo1.cpp` y
`archivo2.cpp` y queremos compartir una variable llamada `version` entre
ambos. Podemos declarar y usar la variable en un archivo:

``` C++
// archivo1.cpp
#include <iostream>          // cout

extern int version;

// Usando una variable externa 
int main() 
{
    std::cout << "Versión actual: " << version << std::endl;
    return EXIT_SUCCESS;
}
```

y definirla en otro:

``` C++
// archivo2.cpp
int version = 1;
```

Al compilar y enlazar los dos archivos, el enlazador se encarga de
resolver la referencia a la variable `version` en `archivo1.cpp` y
enlazarla con la definición en `archivo2.cpp`.

``` C++
$ c++ archivo1.cpp archivo2.cpp -o archivo
```

La palabra clave `extern` le dice al compilador que existe una variable
o una función, aun cuando el compilador no la haya visto en el fichero
que está siendo compilado.

:::: tip
::: title
Tip
:::

Una variable o función `extern` puede definirse en otro fichero o en el
fichero actual, mientras la declaración usa la palabra clave `extern`,
la definición no la necesita.
::::

Cuando se utiliza `extern` con una función, indica que la función se
define en otro lugar, y se puede utilizar implícitamente a menos que se
especifique lo contrario.

``` C++
// archivo1.cpp
#include <iostream>     // cout

// Declara una función externa
extern void fun();

// Invoca una función extern
int main() 
{
    fun();             // Invoca la función        
    return EXIT_SUCCESS;
}
```

Define la función externa:

``` C++
// archivo2.cpp
#include <iostream>

// Muestra un mensaje de texto
void fun() 
{
    std::cout << "Función declarada como extern" << std::endl;
}
```

La palabra clave `extern` tiene varios significados según el contexto en
que se use:

1.  En una declaración de variable `const` (no global), especifica que
    la variable o función se define en otra unidad de traducción.
2.  En una declaración de variable `const` especifica que la variable
    tiene enlazado externo. Todas las variables globales tienen enlazado
    interno de forma predeterminada.
3.  `extern C` especifica que la función se define en otra parte y usa
    la convención de llamadas del lenguaje C. El modificador \"extern
    C\" también se puede aplicar a declaraciones de función.
4.  En una declaración de plantilla, `extern` especifica que ya se ha
    creado una instancia de la plantilla en otro lugar. Esto le indica
    al compilador que puede usar la otra instancia, en lugar de crear
    una nueva en la ubicación actual.

Por defecto, las variables definidas fuera de todas las funciones (con
la excepción de `const`) y las definiciones de funciones tienen un
enlace externo.

:::: tip
::: title
Tip
:::

**Enlazado interno y externo**

Un enlace interno significa que el espacio de almacenamiento usado por
un identificador solo se usa en el fichero en que se compila, mientras
que un identificador con enlazado externo comparte el espacio con todos
los ficheros que se están compilando.

Se puede establecer explícitamente que un identificador tenga enlace
externo definiéndolo como `extern` y se puede tener un enlace interno
explicito utilizando la palabra clave `static`.
::::

Cuando el compilador ve la palabra clave `extern` antes de una
declaración de variable global, busca la definición en otra unidad de
traducción. Las declaraciones de variables que no son `const` en el
ámbito global son externas de forma predeterminada. Pero por lo general
`extern` se aplica a declaraciones que no proporcionan una definición.

### Creando una biblioteca

Una biblioteca compartida es una colección de variables, funciones y
clases que se declaran y definen en una unidad de traducción
independiente y que a diferencia de los archivo ejecutables, no
necesitan una función `main()`. Se puede declarar una biblioteca
compartida (también conocida como biblioteca dinámica) utilizando la
palabra clave `extern`.

``` C++
extern "C" {
    // Declaraciones de funciones o variables de la biblioteca compartida
}
```

El bloque `extern "C"` le indica al compilador que las funciones o
variables declaradas dentro del bloque deben ser enlazadas utilizando el
enlace C, en lugar del enlace C++ predeterminado. Esto garantiza que las
funciones de la biblioteca compartida se enlacen correctamente y se
puedan utilizar en programas de C y C++.

Como es usual, el archivo de cabecera con terminación \".h\" contiene
las declaraciones. En este caso, una declaración de una función que suma
tres números:

``` C++
// biblioteca.h
#ifndef BIBLIOTECA_H
#define BIBLIOTECA_H

#ifdef __cplusplus
extern "C" {
#endif

// Suma tres números de tipo double
double suma(double a, double b = 0.0, double c = 0.0);

#ifdef __cplusplus
}
#endif

#endif // BIBLIOTECA_H
```

y el archivo con extensión \".cpp\" o (\".c\", o \".cc\") que contiene
la definición:

``` C++
// biblioteca.cpp    
#include "biblioteca.h"

// Función que suma tres números de tipo double
double suma(double a, double b, double c)
{
    return a + b + c; 
}
```

En este ejemplo, la biblioteca compartida se define en `biblioteca.cpp`
y se declara en `biblioteca.h` utilizando la palabra clave `extern "C"`.
Para compilar la biblioteca necesitamos crear dos archivos, uno con el
código objeto usado para enlazar el ejecutable y otro con la biblioteca
dinámica con extensión `.dll` o `.so` dependiendo del sistema operativo.

Primero generamos el código objeto:

``` Console
$ c++ -c biblioteca.cpp -o biblioteca.o
```

Y luego creamos la biblioteca dinámica (para Linux):

``` Console
$ c++ -shared -o biblioteca.so biblioteca.o
```

o para Windows:

``` Console
$ c++ -shared -o biblioteca.dll biblioteca.o
```

El programa que use la biblioteca debe incluir la cabecera
`biblioteca.h` para utilizar la función `sumar()`.

``` C++
// programa.cpp
#include <iostream>
#include "biblioteca.h"

// Usando una biblioteca
int main() 
{
    std::cout << sumar(5.0, 4.0, 1.0);   // 10        
    return EXIT_SUCCESS;
}
```

Ahora, es necesario compilar el archivo al que hemos llamado
`programa.cpp` para generar el código objeto:

``` Console
$ c++ -c programa.cpp -o programa.o
```

y enlazar el código objeto con la biblioteca compartida:

``` Console
$ c++ programa.o -o programa -L. -lbiblioteca
```

La opción `-L.` le indica al enlazador que busque las bibliotecas en el
directorio actual, y la opción `-lbiblioteca` le indica al compilador
que enlace el objeto con la biblioteca.

Podemos, evitar la generación del código objeto usando el nombre del
archivo directamente:

``` Console
$ c++ programa.cpp -o programa -L. -lbiblioteca
```

Después de compilar el programa devemos verificar que la función se haya
enlazado correctamente. Basicamente esta es la forma en que se crean y
se usan las bibliotecas en C/C++.

### Variables volatiles

La palabra clave `volatile` indica que varios subprocesos que se
ejecutan a la vez, pueden modificar una variable o un valor. Las
variables que se declaran como `volatile` no están sujetos a
optimizaciones del compilador que supone el acceso por un subproceso
único. Esto garantiza que el valor más actualizado esté presente en todo
momento.

``` c++
volatile int num = 23;
```

El modificador `volatile` suele utilizarse en variables que tiene acceso
a varios subprocesos, pero solo se puede aplicar a las variables de
estos tipos:

- Referencias.
- Punteros.
- Tipos aritméticos como `short`, `int`, `char`, `float` y `bool`.
- Enumeraciones con un tipo base entero.
- Parámetros de tipo genérico que se sabe que son tipos de referencia.

La palabra clave `volatile` sólo se puede aplicar a los miembros de una
clase o una estructura. Las variables locales no se pueden declarar como
`volatile`.

#### Usando variables volatiles

Por ejemplo podemos usar `volatile` para que dos hilos ejecuten una
función de incremento, un número determinado de veces:

``` C++
#include <atomic>           // atomic
#include <thread>           // thread
#include <iostream>         // cout

// Una variable atómica volatil  
volatile std::atomic<int> contador{0};

// Función que incrementa un contador 
void incrementar() 
{
    for (int i = 0; i < 10000; i++) {
        contador++;
    }
}

// Hilos y variables volatiles
int main() 
{
    std::jthread t1(incrementar);
    std::jthread t2(incrementar);
    std::cout << "El valor del contador es: " << contador << '\n';

    return EXIT_SUCCESS;
}
```

En este caso el programa declara una variable `volatile` llamada
`contador` que se incrementa en dos hilos simultáneamente. La palabra
clave `volatile` se utiliza para informar al compilador que el valor de
la variable puede cambiar en cualquier momento, incluso si no se ha
modificado. Esto hace que el compilador genere código que siempre lea el
valor actual de la variable desde la memoria en lugar de utilizar el
valor almacenado en el registro.

La clase `std::atomic` se utiliza para garantizar que las operaciones de
incremento sean atómicas y no se produzcan condición de carrera. Si no
se utiliza, es posible que los hilos intenten incrementar el valor del
contador de forma simultánea, lo que puede dar lugar a un comportamiento
indefinido.

Es importante agregar que el uso de `volatile` no proporciona ninguna
garantía de sincronización o atomicidad por sí solo. Para operaciones
atómicas o sincronización de variables compartidas entre hilos, es mejor
utilizar los mecanismos de sincronización proporcionados por la
biblioteca estándar como `atomic`, `mutex` y `lock_guard`.

### Variables locales para hilos

Las variables locales para procesos entre hilos se crean cuando se crea
un hilo y se destruyen cuando se destruye el hilo y se almacenan en la
memoria específica del hilo. Estas variables se declaran con la palabra
clave `thread_local`:

``` C++
thread_local int contador = 0;
```

Una variable `thread_local` es una variable que está asociada con un
hilo de ejecución específico. Cada hilo de ejecución tiene su propio
contexto de ejecución y su propio espacio de memoria, lo que significa
que cada hilo puede tener sus propias variables y datos.

#### Usando variables thread_local

Las variables `thread_local` se utilizan a menudo en programación
concurrente y paralela para permitir que diferentes hilos de ejecución
accedan y modifiquen datos compartidos. Sin embargo, el acceso
concurrente a los datos compartidos puede ser peligroso y puede dar
lugar a condiciones de carrera, por lo que es importante utilizar
mecanismos de sincronización adecuados, como `mutex` o `lock_guard`,
para garantizar que solo un hilo a la vez pueda acceder y modificar los
datos compartidos.

``` C++
#include <iostream>          // cout
#include <thread>            // jthread
#include <vector>            // vector

// Función que simula un tarea
void tarea(int id) 
{
    // Cada hilo crea su propia copia del contador
    thread_local int contador{0};
    for (int i = 0; i < 100; i++) {
        ++contador;
        std::cout << "Hilo " << id << " contador: " << contador << '\n';
    }
}

// Ejecuta una tarea usando 5 hilos 
int main() 
{
    std::vector<std::jthread> hilos;

    for (int i = 0; i < 5; i++) {
        hilos.push_back(std::jthread(tarea, i));
    }

    return EXIT_SUCCESS;
}
```

En este ejemplo, creamos una función llamada `tarea` que toma un
parámetro de tipo entero y crea una variable `thread_local` llamada
`contador`. Dentro del bucle `for`, la variable `contador` se incrementa
en 100 ocasiones mientras imprime el valor actual. En la función
`main()`, se crea un vector de hilos y se inicializan cinco hilos que
ejecutan la `tarea` con diferentes valores.

La variable `thread_local` llamada `contador` se crea y se destruye para
cada hilo individualmente, lo que significa que cada hilo tendrá su
propia copia del `contador` que podra modificar sin afectar a las copias
de otros hilos. Esto es útil para almacenar datos específicos de un hilo
que no se comparten con otros hilos.

La variable `thread_local`, al igual que los hilos, solo están
disponible a partir de C++11 en adelante y requieren soporte del
compilador y del sistema operativo.

### Variables dinámicas

Las variables dinámicas se crean explícitamente en tiempo de ejecución
utilizando la función `new` y se destruyen explícitamente utilizando la
función `delete`. A diferencia de las variables automáticas que se
almacenan en la pila, las variable dinámicas se almacenan en el montón
(heap) de la aplicación.

``` C++
int* p = new int(23);   // p apunta a una variable dinámica
delete p;               // destruye la variable dinámica
```

Las variables dinámicas se utilizan para almacenar datos que necesitan
una duración de vida más larga que la función actual.

``` C++
#include <iostream>      // cout

// Creando y destruyendo un puntero
int main() 
{
    // Reserva memoria dinámica para un entero
    int* ptr = new int(23);

    // Imprime el valor del entero
    std::cout << *ptr << std::endl;

    // Libera la memoria reservada
    delete ptr;

    return EXIT_SUCCESS;
}
```

En este ejemplo, se utiliza la función `new` para reservar memoria
dinámica para un entero y se inicializa con el valor `23`. Luego, se
imprime el valor almacenado utilizando el operador de desreferenciación
`*`. Después de utilizar la memoria dinámica, es importante liberarla
utilizando la función `delete` para evitar fugas de memoria. En este
ejemplo, liberamos la memoria reservada antes de terminar el programa.

#### Asignando y liberando memoria

También se puede reservar memoria dinámica para arreglos utilizando la
sintaxis `new T[Número]` para reservar memoria. Por ejemplo para 10
enteros:

``` C++
#include <iostream>                 // cout

// Memoria dinámica: arreglos
int main()
{
    constexpr int elementos{10};    // El número de elementos del arreglo

    // Un arreglo para almacenar los elementos
    int* arreglo = new int[elementos];

    // Asigna un valor a cada elemento del arreglo y lo imprime
    for (int i = 0; i < elementos; ++i) {
        // Le asigna un valor y lo imprime
        arreglo[i] = i;   
        std::cout << arreglo[i] << ' ';
    }

    delete[] arreglo;              // Elimina el arreglo

    return EXIT_SUCCESS;
}
```

En este caso, es necesario utilizar la sintaxis `delete[]` para liberar
la memoria reservada. Los arreglos de arreglos (o matrices) funcionan de
la misma forma:

``` C++
#include <iostream>       // cout
#include <iomanip>        // setw

// Memoria dinámica: matrices
int main()
{
    constexpr int filas{10};
    constexpr int columnas{16};

    // La variable matriz es un puntero que apunta a 
    // un arreglo de enteros de 10 * 16 elementos
    int *matriz = new int[filas * columnas];

    // Imprime los valores de la matriz
    for (int i = 0; i < filas; ++i) {
        for (int j = 0; j < columnas; ++j) {
            matriz[j * filas + i] = j * filas + i;
            std::cout << std::setw(3) << matriz[j * filas + i] << ' ';
        }
        std::cout << '\n';
    }

    delete[] matriz;     // Elimina la matriz

    return EXIT_SUCCESS;
}
```

Es importante tomar en cuenta que la función `new` puede fallar y
devolver un puntero nulo si no hay suficiente memoria disponible en el
sistema. En algunos casos es necesario verificar si el puntero es nulo
antes de utilizarlo para evitar errores de segmentación. Un puntero nulo
equivale a `nullptr`.

``` C++
if (ptr != nullptr) {
   // Bien: El puntero no es nulo tuvo exito la asignación de memoria
}
```

También es recomendable utilizar mecanismos de administración de memoria
más sofisticados, como los proporcionados por la biblioteca estándar
como `unique_ptr` o `shared_ptr`, para evitar errores comunes en la
administración de memoria dinámica.

### Variables estáticas

Un objeto con extensión estática se define usando la palabra clave
`static` en la declaración del tipo de variable.

``` C++
static int cien{100};
```

Un objeto con extensión estática es visible a lo largo de la ejecución
de un programa. Como cualquier variable, puede cambiar de valor, pero a
diferencia de las variables, que una vez que salen de su ámbito
desaparecen, un objeto estático conserva su valor hasta que el programa
finaliza.

``` C++
#include <iostream>       // cout

// Un generador que conserva el estado entre llamadas
void generar(int valor) 
{
    // Un valor estático que es retenido entre llamadas.
    static int contador{0};

    // La variable local desaparece una vez que la función sale de su ámbito.
    int variable{0};

    // Incrementa el contador y la variable cada que se invoca la función.
    contador += valor;
    variable += valor;

    std::cout << "El valor del contador es  : " << contador << '\n'
              << "El valor de la variable es: " << variable << '\n';
}

// Variables estáticas
int main()
{
    // Ejecuta el generador 10 veces
    for (int i = 0; i < 10; ++i) {
        generar(1);
    }

    return EXIT_SUCCESS;
}
```

A diferencia de las variables locales que se crean y se destruyen cada
vez que el flujo de control las evalúa, una variable estática conserva
su estado (valor) entre llamadas. Las variables estáticas se crean la
primera vez que el flujo de control las evalúa y solo se destruyen
cuando el programa finaliza.

Cuando se declara una variable `static`, la variable tiene duración
estática y el compilador la inicializa a 0, a menos que se especifique
otro valor. Normalmente, las variables definidas localmente en una
función desaparecen al final de su ámbito. Cuando se llama de nuevo la
función, el espacio de las variables se vuelve a pedir y las variables
son re-inicializadas. Para que el valor se conserve durante la vida de
un programa es necesario definir la variable como `static` y darle un
valor inicial. La inicialización se realiza sólo la primera vez que se
llama a la función y la información se conserva entre llamadas
sucesivas. De esta forma, una función puede \"recordar\" su valor entre
una llamada y otra.

:::: tip
::: title
Tip
:::

**Duración estática**

Duración estática significa que el objeto o la variable se inicializa
cuando se inicia el programa y se desasigna cuando finaliza el programa.
::::

Lo realmente útil, de una variable `static` es que no está disponible
fuera del ámbito de la función en que se declara, de modo que no se
puede modificar accidentalmente. Esto facilita la localización de
errores.

## Declaración de constantes

Las variables pueden cambiar de valor a lo largo de la ejecución de un
programa, pero un programa también usa valores constantes, es decir
valores que siempre son los mismos. Una **constante** es una variable
calificada con la palabra clave `const` cuyo valor no puede ser
modificado una vez que se ha definido.

Los valores `const` se pueden utilizar en variables, parámetros, valores
de retorno y en punteros. Cuando se utiliza en un parámetro de función,
indica que el valor del parámetro no se puede cambiar dentro de la
función. Cuando se utiliza en un puntero, indica que el puntero no se
puede cambiar para apuntar a una dirección de memoria diferente, aunque
el valor al que apunta el puntero sí se puede cambiar. También se puede
usar para evitar que los parámetros pasados por referencia sean
modificados.

### La palabra clave const

La palabra clave `const` define un valor constante. Esto significa que
el tipo de dato contenido no puede ser modificado. Cualquier intento por
alterar el valor de una constante genera una mensaje de error en tiempos
de compilación.

Su uso más simple es definir un valor como constante:

``` C++
const double  e{2.7182818284590452353602};
const double pi{3.1415926535897932384626};
```

El calificador `const` se asegura que la variable no pueda ser
\"modificada\" o \"mutada\" inadvertidamente por el programa.

``` C++
const int cero{0};
cero = 1;                          // Error: variable de solo lectura
```

La palabra clave `const` puede aparecer antes o después del tipo de dato
sin alterar el comportamiento del identificador.

``` C++
// Estas dos definiciones son idénticas
int const cero{0};
const int cero{0};
```

Las constantes deben ser inicializadas cuando se declaran, de lo
contrario el compilador lanzara un error:

``` C++
const int cero;                   // Error: valor constante no inicializado
```

En general, la palabra clave `const` previene que los objetos puedan ser
mutados por el programa, pero su comportamiento es más complejo y por lo
general depende del contexto en que se use.

### Usando constantes

La palabra clave `const` se puede utilizar en diferentes contextos, ya
que tiene muchos usos. Un uso muy común es prevenir que los parámetros
que se pasan a una función no puedan ser modificados dentro de la
función.

``` C++
// Los parámetros a y b se pasan por valor.
int fun1(int a, int b);         

// El parámetro texto se pasa como referencia const.
std::string fun2(const std::string& texto);  
```

En este ejemplo, los dos parámetros que recibe `fun1()` se pasan por
valor, y el parámetro que recibe `fun2()` se pasa como referencia
constante. Cuando los argumentos de una función se pasan por valor o por
referencia constante el programa no puede modificarlos. Sin embargo hay
una sutil diferencia entre ambos. En el primer caso el compilador usa
una copia y en la segunda usa una referencia a la ubicación del objeto.
Para tipos incorporados esta diferencia no es significativa, pero en
tipos grandes el segundo enfoque es el más apropiado.

Las variables declaradas con la palabra clave `const`, pueden ser usadas
para especificar el tamaño de un arreglo:

``` C++
int const numeros{100};
bool primos[numeros];
std::cout << std::size(primos);   // 100
```

Los valores `const` se pueden usar en tiempo de compilación, por ejemplo
en aserciones estáticas:

``` C++
const int cero{0};
static_assert(cero == 0); 
```

Una vez que se define un valor constante, el valor no puede cambiar.

### Referencias const

Se puede utilizar el modificador `const` con referencias para indicar
que el valor al que apunta la referencia no se puede modificar. Una
referencia `const` se declara agregando la palabra clave `const` después
del tipo de dato y antes del nombre de la referencia.

``` C++
int valor = 10; 
int& referencia = valor;                  // Una referencia de tipo int
const int& referencia_constante = valor;  // Una referencia constante
```

En este ejemplo, se declara una variable de tipo entero llamada `valor`
y dos referencias: una `referencia` y una `referencia_constante`. La
referencia normal se declara usando el símbolo `&`, mientras que la
referencia constante se declara como una referencia `const`.

Podemos cambiar el valor que almacena la variable `valor` usando la
referencia:

``` C++
// Bien: Las referencias pueden cambiar el valor al que hacen referencia
referencia = 20;             
std::cout << valor;                        // valor = 20
```

Sin embargo, no podemos cambiar el valor de la referencia constante.

``` C++
// Error: Las referencias constantes no pueden cambiar de valor 
referencia_constante = 30;
```

Se puede modificar el valor de `referencia`, ya que no se ha declarado
como `const`. Sin embargo, se generará un error de compilación si se
intenta modificar el valor de la referencia constante, ya que se ha
declarado como `const`.

Es importante señalar que el modificador `const` solo se aplica al valor
al que apunta la referencia, no a la referencia. La referencia en sí
misma, no se puede reasignar a un nuevo valor una vez que se ha
inicializado en la declaración.

Las referencias `const` se utilizan a menudo como parámetros de
funciones para indicar que el valor pasado como parámetro no se va a
modificar dentro de la función. Esto puede ayudar a prevenir errores y
mejorar la legibilidad del código. Por ejemplo una función que toma una
referencia `const` como parámetro:

``` C++
// El valor del texto no se puede modificar dentro de la función
void ver(const std::string& texto) 
{ 
    std::cout << texto; 
} 
```

En este caso, la función `ver()` toma una referencia `const` llamada
`texto`. Dentro de la función, el texto no se puede modificar, ya que se
ha declarado como `const`. Cualquier intento por modificar el valor
dentro de la función generará un error, ya que `texto` es una referencia
constante.

:::: tip
::: title
Tip
:::

**¿Qué son las referencias?**

Una referencia es un declarador que hace \"referencia\" indirecta a un
tipo de dato. Por lo general, cuando inicializamos un variable, el valor
del inicializador es copiado en el objeto que estamos creando. Cuando
definimos una referencia en lugar de copiar el valor inicializador, el
compilador enlaza la referencia con el inicializador usando su dirección
de memoria.
::::

Si omitimos el calificador `const` cuando usamos una referencia, podemos
modificar el valor original.

### Punteros const

Se puede utilizar el modificador `const` con punteros para indicar que
el puntero o el valor al que apunta el puntero no se puede modificar.

Hay dos formas de utilizar el modificador `const` con punteros:

**Puntero const** cuando se utiliza el modificador `const` antes del
símbolo `*`, se está indicando que el puntero en sí mismo, no se puede
modificar, pero el valor al que apunta sí se puede modificar.

``` C++
int valor = 10;
int* puntero = &valor;                  // Un puntero
const int* puntero_constante = &valor;  // Un puntero constante
```

En este ejemplo, se declara una variable de tipo entero llamada `valor`
y dos punteros: `puntero` y `puntero_constante`. En el primer caso se
declara un puntero que puede modificar el valor de la variable, mientras
que en el segundo caso se declara un puntero constante.

Al igual que las referencias podemos usar el puntero para modificar el
valor de tipo entero:

``` C++
// Bien: puede modificar el valor de forma indirecta
*puntero = 20; 
std::cout << valor;                    // El valor es 20
```

Pero no podemos cambiar el valor usando el puntero constante:

``` C++
// Error: asignación a posición de lectura '* puntero_constante'
*puntero_constante = 20; 
```

Se puede modificar el valor al que apunta el `puntero`, ya que no se ha
declarado como `const`. Sin embargo, se generará un error de compilación
si se intenta modificar el `puntero_constante`, ya que se ha declarado
como `const`. Sin embargo, podemos cambiar el valor apuntado por el
puntero, porque el puntero en sí mismo, no es `const`:

``` C++
int valor2 = 20; 

// Cambia el valor al que apunta el puntero
puntero_constante = &valor2;           
std::cout << *puntero_constante;       // 20 
```

Al contrario de las referencias, los punteros representan dos entidades
diferentes, la dirección que almacena al puntero y la dirección a la que
apunta.

**Puntero a const** cuando se utiliza el modificador `const` después del
símbolo `*`, se está indicando que el valor al que apunta el puntero no
se puede modificar, pero el puntero en sí mismo (su dirección), sí se
puede modificar.

``` C++
int valor = 10; 
int *puntero = &valor;                     // Un puntero 
int* const puntero_a_constante = &valor;   // Un puntero a constante
```

En este ejemplo, se declara una variable de tipo entero llamada `valor`
y dos punteros: `puntero` y `puntero_a_constante`. En el primer caso se
declara un puntero, mientras que `puntero_a_constante` se declara como
un puntero a `const`.

Se puede usar el puntero para modificar el valor:

``` C++
// Bien: cambia el valor apuntado 
*puntero = 30;  
std::cout << valor;                       // El valor es 30  
```

Pero, también se puede usar el puntero a constante para modificar el
valor:

``` C++
// Bien: cambia el valor apuntado  
*puntero_a_constante = 40;          
std::cout << valor;                       // El valor es 40  
```

En este caso se puede modificar el valor apuntado por los dos punteros,
por qué el modificador `const` se aplica al puntero y no al valor
apuntado. En este caso, se genera un error de compilación si se intenta
cambiar el valor del puntero:

``` C++
int valor2 = 20;

// Error:: Asignación a variable de lectura 'puntero_a_constante'
puntero_a_constante = &valor2;          
```

Es importante notar que el modificador `const` solo se aplica al puntero
o al valor al que apunta el puntero, no a ambos. Para indicar que el
puntero, asi como el valor al que apunta el puntero no se pueden
modificar, se deben utilizar dos modificadores `const`:

``` C++
// Un puntero constante a un valor constante
const int* const puntero_constante = &valor;   
```

En este caso ni el puntero, ni el valor apuntado se pueden modificar.

Hay una regla empírica simple para leer a los calificadores `const` de
forma correcta. Si lo lees de derecha a izquierda, entonces cualquier
calificador `const` que aparezca modifica el objeto a su izquierda. Hay
una excepción: si no hay nada a la izquierda, por ejemplo: al principio
de una declaración, entonces `const` modifica el objeto a su derecha.

El uso correcto de `const` ayuda a crear código seguro en C++. El uso de
`const` puede ahorrar muchas horas de problemas y depuración, porque las
violaciones a tipos `const` provocan errores en tiempo de compilación. Y
como efecto secundario, el uso de `const` también puede ayudar a que el
compilador aplique algunos de sus algoritmos de optimización. Eso
significa que el uso apropiado de `const` también ayuda a mejorar el
rendimiento y ejecución de un programa.

### Funciones miembro const

Otro lugar en donde se utiliza el modificador `const` es con funciones
miembro de una clase para indicar que una función no modifica el estado
del objeto.

``` C++
#include <iostream>           // cout

template<typename T>
class Rectangulo {
public:
    Rectangulo(T ancho, T alto) : base(ancho), altura(alto) { }

    // Calcula el area del rectángulo
    T area() const { return base * altura; }

private:
    T base;
    T altura;
};

// Calcula el area de un rectángulo
int main()
{
    Rectangulo<int> rect{4, 3};
    const int area = rect.area();
    std::cout << area;                  // 12

    return EXIT_SUCCESS;
}
```

En este ejemplo, la función miembro `area()` se declara como `const`, lo
que significa que no modifica el estado del objeto. La función
simplemente devuelve el producto de la base por la altura del
rectángulo. Si se intenta modificar el valor de `base` o `altura` dentro
de la función `area()`, el compilador generará un error, ya que no se
puede modificar el estado del objeto dentro de una función miembro
`const`.

Una función miembro `const` se puede llamar en objetos `const` y no
`const`.

## Deducción de tipos

Aunque no es posible declarar una variable sin un tipo, existen dos
mecanismos que le permiten al compilador deducir el tipo de un objeto
inicializado con un valor arbitrario:

1.  `auto` para deducir el tipo de un objeto a partir de su valor
    inicializador. El valor puede ser el tipo de una variable,
    constante, expresión, función etc.
2.  `decltype()` para deducir el tipo de algo que no esté inicializado,
    como el tipo de retorno de una función o el tipo de un miembro de
    una clase.

La deducción de tipos es muy sencilla: `auto` y `decltype()` simplemente
reportan el tipo de una expresión conocida (o reconocida) por el
compilador.

### La palabra clave auto

C++ tiene un mecanismo para declarar variables sin un tipo específico,
usando la palabra clave `auto`. La palabra `auto` le permite al
compilador deducir el tipo, a partir del valor con el que se inicialice
la variable:

``` C++
// auto deduce el tipo (automáticamente) en tiempos de compilación.
auto letra      = 'a';             // auto = char
auto entero     = 12345;           // auto = int
auto decimal    = 1.23f;           // auto = float
auto doble      = 1.23l;           // auto = double
auto sin_signo  = 1234u;           // auto = unsigned int
auto largo      = 1234L;           // auto = long
auto numero     = entero;          // auto = int    
auto numero2;                      // Error: valor inicializador requerido
```

Cuando una declaración ha sido inicializada, no es necesario definir
explícitamente un tipo. En su lugar podemos dejar que la variable tome
el tipo y valor de su inicializador:

``` C++
int total = 12345;
std::cout << total << '\n';         // 12345

// auto toma el tipo y valor de su inicializador
auto resultado = total;             // auto = int
std::cout << resultado << '\n';     // 12345
```

El tipo de la variable `total` es `int`, así que la variable `resultado`
es también de tipo `int`, `auto` traslada el tipo básico del
inicializador y su valor, sin embargo elimina cualquier referencia o
especificador `const`:

``` C++
#include <iostream>                // cout
#include <typeinfo>                // typeid

// Devuelve el tipo de una variable usando typeid
int main()
{
    auto total = 50;               // auto siempre debe ser inicializado
    int& ref_total = total;        // una referencia a total
    auto resultado = ref_total;    // resultado es int no una referencia

    std::cout << typeid(total).name()     << '\n'
              << typeid(ref_total).name() << '\n'
              << typeid(resultado).name() << '\n';

    return EXIT_SUCCESS;
}
```

La salida del programa, muestra que los tres tipos usados son `int`. La
función miembro `typeid` definida en la cabecera `typeinfo` devuelve el
tipo de una variable, abreviado, y la salida depende del compilador.

Los especificadores eliminados, pueden ser aplicados manualmente cuando
se necesiten. El operador `&` crea una referencia (un valor izquierdo)
que se puede aplicar junto con la palabra `auto`. Cuando se usa del lado
derecho de una asignación asigna la dirección de la variable a un
puntero.

``` C++
const auto total = 50;            // auto es const int
const auto& referencia = total;   // una referencia constante
const auto puntero = &total;      // un puntero constante

std::cout << typeid(total).name()      << '\n'  
          << typeid(referencia).name() << '\n' 
          << typeid(puntero).name()    << '\n';
```

Es posible crear una referencia a una referencia, usando dos operadores
`&&`. Esta notación designa un valor derecho. En estos caso `auto`
permite que el compilador deduzca automáticamente una referencia a un
valor izquierdo o derecho, basado en el inicializador.

``` C++
auto total = 2 * 50;           // auto toma el valor de una expresión
auto& referencia = total;      // auto es una referencia a un valor-l
auto&& ref_a_ref = referencia; // auto es una referencia a un valor-r

// Muestra la dirección de memoria de las 3 
// variables que apuntan al mismo valor
std::cout << &total      << '\n'
          << &referencia << '\n'
          << &ref_a_ref  << '\n';
```

La palabra clave `auto` puede ser usada en cualquier declaración de
variable inicializada. Por ejemplo, para deducir el tipo usado en un
bucle `for`.

``` C++
#include <iostream>            // cout
#include <vector>              // vector

// Bucles for "auto"
int main()
{ 
    // Los primos menores a 256
    std::vector<int> primos {
        2, 3, 5, 7, 11, 13, 17, 19, 23, 29, 31, 37, 41, 43, 47, 53,
        59, 61, 67, 71, 73, 79, 83, 89, 97, 101, 103, 107, 109, 113,
        127, 131, 137, 139, 149, 151, 157, 163, 167, 173, 179, 181,
        191, 193, 197, 199, 211, 223, 227, 229, 233, 239, 241, 251
    };

    // Imprime el vector de números primos usando auto
    for (auto num : primos) {
        std::cout << num << ' ';
    }                                 // "2 5 7 11 ..."

    return EXIT_SUCCESS;    
}
```

Podemos resumir el comportamiento de `auto` de la siguiente forma:

- Si el inicializador es un literal deduce el tipo (no constante) del
  valor literal.
- Si el inicializador es una referencia, la ignora.
- Si el inicializador está calificado con `const` o `volatile`, ignora
  los calificadores si se aplican sobre el objeto.
- Si el inicializador es una expresión, deduce el tipo resultante de
  evaluar la expresión (tomando en cuenta los puntos anteriores).
- Si el inicializador es una función, deduce el tipo de retorno de la
  función (tomando en cuenta los puntos anteriores).

No hay mucha ventaja en usar `auto` en lugar de un tipo en asignaciones
simples. Pero puede ayudar significativamente en código más complejo
como plantillas y tipos verbosos.

### El especificador decltype

La palabra clave `decltype` es un declarador de tipo que le indica al
compilador que use el tipo de la variable con el que la expresión,
variable o función fue declarada.

``` C++
// Una variable de tipo entero
int num1 = 10;            

// Asume que el tipo de num1, no es conocido en este punto, 
// y lo deduce (y declara una nueva variable).
decltype(num1) num2;

// decltype: declara un tipo, pero el valor del tipo es indefinido
std::cout << num2 << '\n';    // valor indefinido

// decltype: deduce el tipo, declara una variable con un valor de 100
decltype(num1) num3 = 100;    
std::cout << num3 << '\n';     // 100
```

El especificador `decltype` trabaja como la palabra clave `auto`,
excepto que deduce el tipo exacto declarado por la expresión, incluyendo
referencias.

``` C++
// Una variable de tipo entero
int num = 10;

// Infiere el tipo del valor literal
decltype(10) num2 = 3;    // int&&

// Infiere el tipo de la variable
decltype(num) num3 = 3;   // int&&

// Infiere el tipo usando auto
decltype(auto) num4 = num;

std::cout << num2 << '\n'   // 3
          << num3 << '\n'   // 3
          << num4 << '\n';  // 10
```

La expresión usada por `decltype` se especifica entre paréntesis.
También puede usarse junto con la palabra clave auto y se puede usar en
funciones, para inferir el tipo de retorno de una función, sin importar
si es un valor o una referencia.

``` C++
const std::string palabra{"Hola mundo"};

// decltype deduce el tipo del valor del retorno
decltype(auto) texto() 
{ 
    return palabra; 
}
```

Utilizando `decltype(auto)` se puede usar el comportamiento de
`decltype` en la deducción del tipo de retorno de funciones en lugar de
usar `auto`. Podemos resumir su comportamiento de la siguiente forma:

- Si entre los paréntesis hay un valor literal, deduce el tipo (no
  constante).
- Si entre los paréntesis hay una expresión, deduce el tipo resultante
  de evaluar la expresión (sin evaluarla).
- Si el inicializador es una función, deduce el tipo de retorno de la
  función.

C++ tiene una sintaxis alternativa, que le permite a las funciones
devolver un valor usado el operador flecha `->`. Si conocemos el valor
devuelto podemos usarlo de esta forma:

``` C++
#include <iostream>            // cout

// Deducción de tipos usando el operador flecha 
auto main() -> int 
{
    std::cout << "¡Hola mundo!";
    return EXIT_SUCCESS;
}
```

Si no conocemos el tipo exacto, podemos usar `decltype`. Esto permite
que los parámetros usados se puedan deducir. El uso de `auto` en este
contexto, le permite a la función deducir el tipo que recibe la función,
así como el tipo que devuelve de forma automática.

``` C++
#include <iostream>          // cout

// Función flecha: deduce el tipo que recibe y el que devuelve
auto eco(auto valor) -> decltype(valor) 
{ 
    return valor; 
}

// Deducción de tipos
int main()
{
    const std::string palabra {"Hola mundo"};

    // Imprime el valor pasado como argumento
    std::cout << eco(palabra) << '\n'
              << eco(false)   << '\n'
              << eco(10)      << '\n'
              << eco(10.99)   << '\n';

    return EXIT_SUCCESS;
}
```

Con `auto` y `decltype` se puede deducir el tipo exacto de una
referencia:

``` C++
decltype(auto) referencia(auto& x) { return x; }        // auto&
```

El uso fundamental de la deducción de tipos es reducir la verbosidad del
código y mejorar la legibilidad de un programa, particularmente cuando
se declaran tipos muy complicados, difíciles de escribir o desconocidos
por el programador (pero no por el compilador). Aunque estos mecanismos
son útiles, no se debe abusar de la sintaxis.
