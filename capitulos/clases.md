# Clases

::: {.meta title="Clases" capitulo="9" description="Como usar las clases" keywords="clases"}
:::

La base de la programación orientada a objetos se centra en la
abstracción, encapsulación y ocultación de datos. La abstracción de
datos es una técnica de programación (y diseño) que se usa para crear
estructuras de datos y operaciones para trabajar con esos datos. En C++
esto se hace a través de objetos especializados llamados clases. Las
clases son la columna vertebral de C++, ya que proporcionan una forma
directa de extender los tipos del lenguaje (los incorporados y los
compuestos). Cada clase representa un tipo y cada tipo mantiene su
propia representación que le dice al compilador como almacenar los
valores y las operaciones disponibles para manipular esos valores.

En C++, a los valores se les llama datos miembro y a las operaciones
funciones miembro. La tarea de las funciones miembro es proveer la
interfaz entre los datos privados de la clase y el programa. A este
proceso de aislamiento entre los datos y el acceso directo por el
programa se le conoce como ocultamiento. La ocultación se utiliza para
separar los detalles entre la forma en cómo se guardan los datos y la
forma en cómo se utilizan. La implementación esta oculta a los clientes
de la clase, mientras que la interfaz presenta las operaciones que el
cliente de la clase puede ejecutar para crear y usar objetos de ese
tipo.

Encapsular un objeto en una clase ofrece muchas ventajas. Todo está en
un mismo lugar, esto facilita la manipulación y el tratamiento de los
datos. Asimismo, los clientes de la clase, es decir; las partes del
programa que utilizan la clase pueden utilizar una instancia sin
preocuparse por la forma en cómo trabaja o como se almacenan los datos,
ya que sólo las funciones encapsuladas dentro de la clase pueden acceder
a ellos.

Este capítulo describe la forma de crear nuevos tipos usando clases.

## Tipos definidos por el usuario

C++ proporciona varios tipos incorporados que operan a bajo nivel como
`char`, `int` y `double`. El compilador sabe cómo representar estos
tipos y las operaciones que soportan. Los tipos incorporados reflejan de
forma directa y eficiente las capacidades del hardware convencional,
pero no proporcionan facilidades de alto nivel para escribir
aplicaciones más avanzadas. En lugar de aumentar los tipos incorporados,
el lenguaje proporciona un conjunto sofisticado de mecanismos de
abstracción que permiten diseñar e implementar nuevos tipos de datos,
con representaciones convenientes y soporte de operaciones para que
otros programadores utilicen de forma simple y elegante estos tipos. El
estándar los llama **tipos definidos por el** **usuario**.

La biblioteca estándar consiste principalmente de tipos definidos por el
usuario, como las clases `string`, `vector` o `array`. Estos tipos son
ejemplos de objetos de alto nivel que usan las facilidades
proporcionadas por el lenguaje para extender el sistema de tipos. C++
proporciona clases, estructuras, uniones y enumeraciones para crear
tipos a medida. A diferencia de los tipos incorporados por el
compilador, los tipos definidos por el usuario no soportan operaciones
predefinidas como operaciones aritméticas o de comparación. Es
responsabilidad del programador de la clase proporcionar las operaciones
a las que puede someterse un objeto de un tipo en particular.

### Estructuras

El primer paso en la creación de un tipo es encapsular los elementos que
\"abstractamente\" representan un objeto en una estructura de datos
común. Por ejemplo, un tipo `Racional` consta de un numerador y un
denominador:

``` C++
// Estructura que almacena un número racional (una fracción)
struct Racional {
    int numerador;    
    int denominador;

    // Devuelve el valor decimal que representa la fracción
    double decimal() const 
    { 
       return static_cast<double>(numerador) / denominador; 
    } 
}
```

La clase `Racional` es una estructura de datos que almacena dos números
enteros y una función que devuelve un valor decimal. Una variable de
tipo `Racional` se puede usar de esta forma:

``` C++
Racional r1{1, 2};                   
std::cout << r1.decimal() << std::endl;

// Asigna un nuevo valor al numerador y al denominador
r1.numerador   = 2;
r1.denominador = 3;
std::cout << r1.decimal() << std::endl;

// Error: no se puede dividir el numerador entre 0
Racional r2{2, 0};
std::cout << r.decimal() << std::endl;
```

Mientras `r1` es un número racional valido, `r2` resulta en un error en
tiempo de compilación, porque no se puede dividir el numerador entre
`0`. Para corregir esto, podemos usar un constructor que acepte dos
argumentos y valide el denominador.

``` C++
#include <stdexcept>                 // invalid_argument

// Un tipo Racional consta de un numerador y un denominador distinto de 0.
struct Racional {
    int numerador{0};
    int denominador{1}; 

    // El constructor lanza una excepción si el denominador es 0.
    Racional(int num, int den) : numerador{num}, denominador{den}
    {
        if (denominador == 0) {
            throw std::invalid_argument("División entre 0.");
        }
    }

    // Devuelve el valor decimal que representa la fracción
    double decimal() const 
    { 
       return static_cast<double>(numerador) / denominador; 
    }
} 
```

La única restricción que requiere la creación de un tipo racional es que
el denominador sea diferente de cero. Ahora, esto lanza una excepción:

``` C++
Racional r2{2, 0};               // Error: argumento no valido
```

Sin embargo, nada impide esto:

``` C++
Racional r3{2, 3};               // Bien: es un número racional
r3.denominador = 0;              // Error: el denominador es cero
```

Al usar una estructura, todos los miembros de datos de la clase son
públicos por defecto. Esto tiene ventajas, ya que los datos se pueden
manipular de forma directa, pero también tiene desventajas ya que expone
los datos directamente al usuario. Para objetos que solo almacenan datos
como registros, lo ideal es usar estructuras con miembros públicos, pero
para objetos que manipulan datos, lo ideal es separar los datos (la
representación) de las operaciones necesarias para su manipulación. El
mecanismo que proporciona C++ para este propósito son las clases.

### Clases

Separar los datos de las operaciones tiene sus ventajas, como la
capacidad de utilizar los datos de forma arbitraria. Sin embargo, es
necesario mantener una conexión entre la representación de los datos y
las operaciones necesarias para manipularlos. Para hacer esto tenemos
que distinguir entre la interfaz de un tipo (que es utilizada por todos
los usuarios de la clase) y su implementación (la parte que tiene acceso
a los datos privados de la clase).

``` C++
#include <stdexcept>       // invalid_argument

// Clase que almacena un número racional 
class Racional {
public:         
    // Crea un tipo racional. Lanza una excepción si el denominador es cero.
    Racional(int n, int d = 1) : num{n}, den{d} { normalizar(); }

   // Devuelve el valor decimal que representa la fracción
   double decimal() const 
   { 
       return static_cast<double>(num) / den; 
   }

private:
    int num{0};           // El numerador
    int den{1};           // El denominador

    // Lanza una excepción si el denominador es cero.
    void normalizar() 
    {
        if (den == 0) {
            throw std::invalid_argument("El denominador es 0.");
        }    
        // Aquí se normalizan los valores usando la función gcd.   
    }       
}; 
```

A diferencia de las estructuras, las clases mantienen la representación
inaccesible a los usuarios esto facilita y garantiza el uso constante de
los datos y permite mejoras (o cambios más adelante) en la
representación de los datos.

#### Clases vs Estructuras

En este punto es necesario hacer un paréntesis e introducir una curiosa
observación ¿Por qué las palabras clave `struct` y `class` hacen
exactamente lo mismo?

La respuesta es sencilla: compatibilidad. Cuando C++ fue desarrollado
fue construido como extensión del lenguaje C. C tiene estructuras pero
las estructuras de C (en aquel entonces) no tenían funciones miembro, ni
especificadores de acceso. Stroustrup, el creador de C++, construyo las
clases usando tipos `struct` pero les cambió el nombre a `class` para
representar las funciones ampliadas del lenguaje, sin embargo se
conservaron los tipos `struct` para mantener compatibilidad con C y con
el tiempo las estructuras evolucionaron y se convirtieron en clases
también.

No hay una regla específica, sobre cuando usar una clase en lugar de una
estructura o viceversa sin embargo hay algunas prácticas comunes:

- Usa una estructura cuando necesites mantener un registro de datos.
- Usa una clase cuando necesites usar datos privados.
- Prefiere una clase cuando necesites diseñar una interfaz.
- Minimiza la exposición de miembros de datos usando clases.

Una estructura se puede usar sin un constructor porque todos sus
miembros son públicos por defecto. Sin embargo una clase que tiene
miembros privados, no se puede inicializar completamente sin el uso de
un constructor.

En C++, una estructura es muy parecida a una clase, salvo que sus
miembros son públicos por defecto. Puedes declarar una estructura de
forma similar a una clase, y puedes pasarle exactamente los mismos
miembros y funciones (incluidos, los constructores). De hecho, si sigues
las buenas prácticas de programación de declarar (siempre)
explícitamente las secciones privadas y públicas de una clase o una
estructura, no hay diferencias.

## La palabra clave class

Para crear una clase se utiliza la palabra reservada `class` seguido del
nombre de la clase (por lo general, la primera letra en mayúscula) y
llaves. Es un error de sintaxis, omitir el punto y coma final.

``` C++
class Nombre {    
    Nivel de acceso:
    // Datos y funciones miembro
};
```

Dentro de las llaves se declaran los niveles de acceso y en cada nivel
de acceso se declaran los miembros que puede contener la clase:

**Datos miembro:**

- Datos miembro no estáticos.
- Datos miembro estáticos (declarados como `static`).

**Funciones miembro:**

- Funciones miembro no estáticas.
- Funciones miembro estáticas (declaradas como `static`).

**Tipos anidados:**

- Clases anidadas y enumeraciones.
- Declaraciones de tipos (alias usando `typedef` o `using`).
- El nombre de la clase dentro de su propia definición.

**Enumeradores** de cualquier tipo.

**Plantillas** de variable, de función o de clase.

Todos los miembros de una clase se deben declarar o definir dentro de la
clase (en medio de las llaves). No se pueden agregar más miembros a una
clase que ha sido declarada o definida.

### Control de acceso y encapsulación

Cuando definimos una clase es necesario forzar a los programas que
utilizan la clase a usar la interfaz pública. La interfaz de una clase
separa los datos que son privados y las funciones públicas que permiten
interactuar con la clase.

``` C++
class Clase {    
public:
    // La interfaz: 
    // Contiene funciones de acceso y asignación que le
    // permiten al cliente interactuar con la clase.  

protected:
    // Relaciones de herencia:
    // Contiene datos y funciones que se pueden 
    // compartir mediante relaciones de herencia.

private:
    // La implementación:
    // Contiene los datos y las funciones que
    // no están diseñadas para el uso general.
};
```

En C++ se utilizan **tres especificadores de acceso** para hacer cumplir
la encapsulación y ocultación de datos:

1.  Los miembros definidos después del especificador `public` son
    públicos y pueden ser accedidos a través de cualquier objeto de la
    clase. Los miembros públicos definen la interfaz de la clase.
2.  Los miembros definidos después del especificador `private` son
    privados y solo pueden ser accedidos por las funciones miembro
    dentro de la clase, Las secciones privadas encapsulan es decir,
    ocultan la implementación.
3.  Hay un tercer especificador llamado `protected` que mantiene
    privados a los miembros de una clase y que a diferencia de
    `private`, permite el acceso a miembros de clases derivadas. Este
    especificador es usado en relaciones de herencia.

Cuando una clase tiene miembros de datos privados es necesario crear
funciones de acceso y de asignación para obtener y cambiar los valores
de estos datos. Las funciones de acceso y de asignación pueden ser
operadores y funciones especificadas dentro de la etiqueta `public`
estas funciones son usadas para interactuar con los miembros privados de
la clase.

### Funciones miembro

Como cualquier otra función, el cuerpo de una función miembro es un
bloque de código y como tal, puede usar modificadores de acceso como
`const` o `constexpr` si la función devuelve un valor constante o el
valor puede ser obtenido y calculado en tiempos de compilación.

Dependiendo de la clase y la tarea, una función miembro se puede
especificar con alguna de las siguientes palabras clave:

- `const` indica que no se puede modificar el objeto devuelto por la
  función.
- `constexpr` indica que la función puede ser evaluada en tiempos de
  compilación o en tiempo de ejecución.
- `consteval` indica que la función \"solo\" puede ser evaluada en
  tiempos de compilación.
- `final` indica que la función no puede ser sobrescrita en una clase
  derivada.
- `noexcept` indica que la función no lanza excepciones.
- `virtual` indica que la función miembro puede ser reemplazada en una
  clase derivada.
- `override`, indica que la función miembro debe reemplazar una función
  virtual de una clase base.
- `static` indica que la función miembro no está asociada con una clase
  en particular.

Las palabras clave `const`, `constexpr`, `consteval` y `noexcept` se
comportan de forma similar en funciones ordinarias y funciones miembro,
mientras que las palabras clave `virtual` y `final` solo se utilizan en
relaciones de herencia.

### Creando un tipo usando una clase

En el siguiente ejemplo, se crea un tipo `Racional` usando una clase. La
clase se define en un archivo de cabecera llamado \"racional.h\". En
Matemáticas, un tipo racional es un número que tiene un numerador y un
denominador. También se le conoce como fracción.

``` C++
#ifndef RACIONAL_H_
#define RACIONAL_H_

#include <cmath>                  // La función pow
#include <cstdint>                // El tipo int64_t 
#include <compare>                // El operador <=>
#include <iosfwd>                 // Los operadores de E/S     
#include <numeric>                // La función gcd
#include <stdexcept>              // Excepción: invalid_argument    

/*
* Clase que representa un número racional.
*
* Un "número racional" es un número que puede representarse
* como el cociente de dos números enteros: a/b donde a, es el
* numerador y b el denominador, que siempre es distinto de cero.
*
**/
class Racional {
public:
    using T = int64_t;           // El tipo que almacena la clase

    // El constructor predeterminado
    constexpr Racional() { }

    // Un constructor sobrecargado  
    explicit constexpr 
    Racional(const T n, const T d = 1) : num(n), den(d) { normalizar(); }

    // El destructor (no hace nada)
    constexpr ~Racional() { }     

    // Función que asigna nuevos valores a la clase. Devuelve un racional
    constexpr Racional&
    asignar(const T n, const T d = 1) { return *this = Racional{n, d}; }

    // Función que devuelve el valor del numerador
    constexpr T numerador() const noexcept { return num; }

    // Función que devuelve el valor del denominador
    constexpr T denominador() const noexcept { return den; }

    // Función que convierte la fracción a decimal
    constexpr double
    decimal() const { return static_cast<double>(num) / den; }

   // Operadores unarios
   // Operadores aritméticos       
   // Operadores aritméticos y de asignación
   // Operadores de comparación
   // Operadores y funciones friend

private:
    T num{0};                    // Almacena el valor del numerador
    T den{1};                    // Almacena el valor del denominador 

    // Reduce los valores. Lanza una excepción si el denominador es 0.
    constexpr void normalizar()
    {
        if (den == 0) {
            throw std::invalid_argument("Error: división entre cero.");
        }

        // Reduce el numerador y el denominador usando el mcm
        T mcm = std::gcd(num, den);

        // El denominador siempre es positivo
        if (den < 0) {
            mcm = -mcm;
        }

        num /= mcm;
        den /= mcm;
    }
};

// Funciones no miembro

#endif // racional.h   
```

En este punto, surge una pregunta inevitable. ¿Por qué agregamos un
nivel de acceso adicional? Después de todo, es más simple usar los
miembros de datos directamente o usar una estructura, en vez de acceder
a través de funciones adicionales. La respuesta es simple. Las funciones
de acceso permiten separar los detalles sobre la forma en cómo se
guardan los datos y cómo se utilizan. Esto le permite al creador de la
clase cambiar la forma de guardar lo datos sin tener que reescribir de
nuevo todas las funciones que utilizan esos datos.

:::: warning
::: title
Warning
:::

**Funciones miembro constexpr**

Una función miembro definida como `constexpr` es `inline` por defecto
esto significa que la función debe ser definida en el mismo archivo en
que se declara.
::::

Si una función que necesita saber el valor de un miembro de datos para
realizar un cálculo tiene acceso directo a una propiedad, cuando la
propiedad necesite ser reescrita, modificada o el autor decida cambiarle
el nombre, todas las funciones que usen esa propiedad necesitan ser
modificadas. En cambio si se utiliza un función de acceso, se pueden
realizar cambios en cualquier miembro de datos, sin afectar a las
funciones de acceso que usan los usuarios de la clase, ya que a estas
funciones no les interesa saber cómo, ni donde se guardan los datos.

:::: tip
::: title
Tip
:::

**Ventajas de la encapsulación**

La encapsulación proporciona dos ventajas importantes. Primero el código
del usuario no puede corromper inadvertidamente el estado de un objeto
encapsulado. Segundo la implementación de una clase encapsulada puede
cambiar en un cierto plazo sin requerir cambios en el código a nivel del
usuario.
::::

Definiendo a los miembros de datos como privados, el autor de la clase
está libre de hacer cambios en los datos, sin que las partes que usen
los datos se vean afectadas. Por otra parte, si la implementación cambia
sólo el código privado de la clase necesita ser examinado para
implementar los cambios. Si los datos públicos cambian, cualquier código
que utilice esos datos queda roto y entonces es necesario localizar y
reescribir todo el código que use la vieja implementación antes de que
el programa se pueda utilizar de nuevo.

:::: note
::: title
Note
:::

**Ocultación de datos**

Encapsulamos una clase definiendo los miembros en la implementación como
privados. Mientras que la interfaz se declara como pública. Esta
separación es fundamental para aislar los componentes que pueden ser
accedidos, de aquellos que deben mantenerse ocultos o privados. Oculto
no significa que no puedan verse, solo significa que no pueden ser
modificados directamente por otra clase.
::::

Otra ventaja de hacer a los miembros de datos privados es que los datos
son protegidos contra errores que los usuarios puedan introducir. Si hay
un fallo en el funcionamiento que corrompa el estado de un objeto, los
lugares para buscar el fallo se localizan únicamente en el código que es
parte de la implementación, de esta forma la búsqueda de errores es
limitada. Esto facilita la corrección de problemas y el mantenimiento de
programas, ya que los cambios de diseño no vuelven obsoleto al programa
rápidamente.

#### Funciones de acceso y asignación triviales

Una función de acceso o asignación es trivial, si no proporciona
semántica adicional a los miembros privados de una clase como
comprobaciones o validaciones. Por ejemplo:

``` C++
// Clase con funciones de acceso y asignación triviales
class Punto { 
public:
    Punto(int x1, int y1) : x{x1}, y{y1} { }

    // Funciones de acceso
    int get_x() const { return x; }
    int get_y() const { return y; }

    // Funciones de asignación
    void set_x(int x1) { x = x1; }
    void set_y(int y1) { y = y1; }

private:
    int x{0};
    int y{0};
};
```

Cuando las funciones de acceso y asignación son triviales, no mantienen
ninguna invariante. Tanto `x` como `y` son solo valores:

``` C++
struct Punto {
    int x{0};
    int y{0};
};
```

En este caso es mejor usar una estructura como una colección de valores,
para que `x, y` sean públicos. Según el estándar, se deben evitar
nombres con prefijos como `get` y `set` que no agregan ningún
significado semántico a una clase.

## Sobrecarga de operadores

La sobrecarga de operadores es uno de los mecanismos básicos del
lenguaje que facilitan el uso y la manipulación de los tipos
incorporados por el lenguaje, así como de los tipos definidos por el
usuario. La **sobrecarga** es una propiedad que describe un
procedimiento estándar que consiste en utilizar el mismo nombre para una
función, un operador o una plantilla que realiza operaciones similares
que se comportan de forma distinta cuando se aplica a diferentes tipos
de objetos.

La sobrecarga de operadores permite que los tipos definidos por el
usuario se comporten como los tipos incorporados. Todas las operaciones
que se pueden realizar en un tipo entero se pueden usar en un tipo
definido por el usuario. Esto incluye operaciones aritméticas, de
asignación, de comparación, operaciones booleanas, lógicas etc.

### Sobrecarga de operadores unarios

Los operadores que requieren un solo operando, como el operador de
incremento `++` y el operador de decremento `--` se denominan operadores
unarios. Los operadores unarios cuando se declaran como funciones
miembro no toman argumentos:

``` C++
// El operador unario positivo: +r
constexpr Racional operator+() const { return *this; }

// El operador unario negativo: -r
constexpr Racional operator-() const { return Racional{ -num, den}; }

// El operador de negación: !r
constexpr bool operator!() const { return !num; }

// El operador de incremento versión prefijo: ++r.
// Incrementa en 1 el valor y devuelve el valor incrementado.
constexpr const Racional& operator++() { num += den; return *this; }

// El operador de decremento versión prefijo: --r.
// Decrementa en 1 el valor y devuelve el valor decrementado.
constexpr const Racional& operator--() { num -= den; return *this; }
```

Dentro de una clase puede haber varios operadores sobrecargados, siempre
y cuando reciban o devuelvan valores diferentes.

``` C++
// El operador de incremento postfijo: r++.
// Incrementa en 1 el valor, pero devuelve el valor sin modificar.
constexpr Racional operator++(int)
{
    Racional r{*this};  // copia el valor original
    num += den;         // incrementa en 1 el valor actual
    return r;           // y devuelve el valor original
}

// El operador de decremento postfijo: r--.
// Decrementa en 1 el valor y devuelve el valor sin modificar.
constexpr Racional operator--(int)
{
    Racional r{*this};  // copia el valor original
    num -= den;         // decrementa en 1 el valor actual
    return r;           // y devuelve el valor original
}
```

Un operador sobrecargado se define como si fuera una función, solo que
el nombre de la función es `operator@`, donde la `@` representa el
símbolo del operador.

El número de argumentos depende de dos factores:

1.  Si es un operador unario (un argumento) y si es un operador binario
    (dos argumentos).
2.  Si el operador es definido como una función global: un argumento
    para los unarios y dos para los binarios y si es una función
    miembro: cero argumentos para los unarios y uno para los binarios.
    En este caso el objeto `*this` (el puntero interno usado por la
    clase), se convierte en el argumento del lado izquierdo del
    operador.

Los operadores unarios declarados dentro de una clase, no toman
argumentos este es el caso de los operadores `+`, `-`, `++` y `--`, sin
embargo la versión postfijo de los operadores de incremento y
decremento, toman un argumento simbólico de tipo `int`, para que
funcione la sobrecarga.

### Operadores de asignación

Los operadores de asignación
`=*, =/, =%, =+, =-, =<<, =>>, =&, =ˆ, =|, =,` y `=` almacenan el valor
de la operación en el objeto especificado por el operando izquierdo.
Básicamente, hay dos tipos de asignación:

**Asignación simple**, en la que el valor del segundo operando se
almacena en el objeto especificado por el primer operando. Por ejemplo:

``` C++
Racional r1{1, 2};
Racional r2 = r1;               // Asigna (o copia) el valor de r1 en r2
std::cout << r1 << std::endl;   // 1/2    
```

**Asignación compuesta**, en la que se realiza algún tipo de operación
antes de almacenar el resultado en el primer operando.

``` C++
Racional r1{1, 2};
Racional r2{3, 4};    
r1 += r2;                       // Suma y asignación   
std::cout << r1 << std::endl;   // 5/4
```

El resultado de una expresión de asignación es siempre un valor que
tiene una dirección. Estos operadores tienen asociatividad de derecha a
izquierda. El operando izquierdo debe ser un objeto modificable.

``` C++
// Suma y asigna un número racional: a += b
constexpr Racional& operator+=(const Racional& r)
{
    T mcd = std::gcd(den, r.den);
    den /= mcd;
    num = num * (r.den / mcd) + r.num * den;
    mcd = std::gcd(num, mcd);
    num /= mcd;
    den *= r.den / mcd;
    return *this;
}

// Resta y asigna un número racional: a -= b
constexpr Racional& operator-=(const Racional& r)
{
    T mcd = std::gcd(den, r.den);
    den /= mcd;
    num = num * (r.den / mcd) - r.num * den;
    mcd = std::gcd(num, mcd);
    num /= mcd;
    den *= r.den / mcd;
    return *this;
}

// Multiplica y asigna un número racional: a *= b
constexpr Racional& operator*=(const Racional& r)
{
    const T gcd1 = std::gcd(num, r.den);
    const T gcd2 = std::gcd(r.num, den);
    num = (num / gcd1) * (r.num / gcd2);
    den = (den / gcd2) * (r.den / gcd1);
    return *this;
}

// Divide y asigna un número racional: a /= b
constexpr Racional& operator/=(const Racional&  r)
{
    if (r.num == 0) {
        throw std::domain_error("Error: Racional no valido.");
    }
    if (num == 0) {
        return *this;
    }
    const T mcd1 = std::gcd(num, r.num);
    const T mcd2 = std::gcd(r.den, den);
    num = (num / mcd1) * (r.den / mcd2);
    den = (den / mcd2) * (r.num / mcd1);
    if (den < 0) {
        num = -num;
        den = -den;
    }
    return *this;
}
```

Los operadores de asignación por lo general se declaran como funciones
miembro, ya que devuelven una referencia al objeto actual. En este caso
el objeto usado del lado izquierdo es el objeto `*this` que almacena el
valor de la operación.

### Operadores aritméticos binarios

Los operadores aritméticos como la suma, resta, división y
multiplicación por lo general se implementan como funciones miembro o
como funciones libres. Las funciones no miembro son funciones globales
que toman dos argumentos:

``` C++
// Función que suma dos números racionales: a + b
constexpr Racional 
operator+(const Racional& a, const Racional& b) { return Racional{a} += b; }

// Función que resta dos números racionales: a - b
constexpr Racional 
operator-(const Racional& a, const Racional& b) { return Racional{a} -= b; }

// Función que multiplica dos números racionales: a * b
constexpr Racional 
operator*(const Racional& a, const Racional& b) { return Racional{a} *= b; }

// Función que divide dos números racionales: a / b
constexpr Racional 
operator/(const Racional& a, const Racional& b) { return Racional{a} /= b; }
```

En el caso de los operadores aritméticos, la implementación global se
proporcionan para facilitar la propiedad conmutativa y asociativa de las
operaciones, por ejemplo: `a + b` es igual que `b + a`\`.

``` C++
Racional r1{1, 3};
Racional r2{4, 3};
std::cout << r1 + r2 << std::endl;          // 5/3
std::cout << r2 + r1 << std::endl;          // 5/3
```

Esto permite operaciones múltiples como: `a + b + c`.

``` C++
Racional r1{1, 3};
Racional r2{4, 3};
Racional r3{2, 3};    
std::cout << r1 + r2 + r3 << std::endl;      // 7/3
```

Los operadores aritméticos, ordinariamente se declaran como funciones
globales. Estas operaciones devuelven un nuevo objeto como resultado de
la operación.

### Miembros friend

El problema con la encapsulación es que algunas clases, funciones y
operadores necesitan acceder directamente a los miembros privados de
otras clases. En lugar de romper la encapsulación haciendo los miembros
públicos, estos se declaran como miembros amigo. Una clase puede
permitir que otra clase, función u operador tenga acceso a sus miembros
privados declarándola como amigo. Una clase hace a una función amigo
usando la palabra clave `friend`.

``` C++
// Compara dos números racionales: a == b
friend constexpr bool operator==(const Racional& a, const Racional& b) 
{
    // Compara las fracciones cruzadas: a/b
    auto r1 = a.num * b.den;
    auto r2 = a.den * b.num;
    return (r1 == r2);
}
```

Una función `friend` es una función que no es miembro de una clase pero
tiene acceso a los miembros privados y protegidos de la clase.

``` C++
// Se declara dentro de la clase con la palabra clave friend
friend constexpr Racional operator+(const Racional& a, const Racional& b);
```

Una función `friend` se declara (o se define) en la clase que concede el
acceso. La declaración `friend` se puede colocar en cualquier parte de
la declaración de la clase y no es afectada por los modificadores de
control de acceso.

``` C++
// Se define fuera de la clase sin la palabra clave friend.
constexpr Racional operator+(const Racional& a, const Racional& b) 
{
    auto n = a.num * b.den + b.num * a.den;
    auto d = a.den * b.den;
    return Racional{n, d};
}
```

Las funciones `friend` no se consideran miembros de clase, son funciones
externas normales que tienen privilegios de acceso especiales. Los
amigos no se encuentran en el ámbito de la clase y no se llaman mediante
los operadores de selección de miembro `.` o puntero `->` a menos que
sean miembros de otra clase.

### Operadores de comparación

Antes de C++ 20 era necesario escribir todos los operadores de
comparación para un clase que necesite implementar las seis operaciones:
`==`, `!=`, `<`, `<=`, `>`, `>=`. Por lo general cuatro de estas
comparaciones funcionan en términos de `==` y `<`. Lo usual es
implementar estas operaciones como funciones que tomen como entrada un
tipo `const T&` para permitir comparaciones entre tipos convertibles.

#### Comparación de tres vías

C++20 proporciona una nueva forma de abordar las comparaciones. Usando
el operador de nave espacial. Este operador implementa una nueva
funcionalidad conocida como \"comparación de tres vías\". Esta
funcionalidad permite saber si un tipo es menor, mayor o igual que otro
tipo en una misma llamada. La comparación devuelve una categoría de
valor que puede ser comparada con cero. Para obtener esta funcionalidad
es necesario remplazar los operadores usuales por el operador `<=>`:

``` C++
// Sobrecargar el operador de tres vías <=> (nave espacial)
constexpr std::strong_ordering operator<=>(const Racional& r) const 
{
    // Compara las fracciones cruzadas: a/b <=> c/d equivale a a*d <=> b*c
    auto a = num * r.denominador();
    auto b = den * r.numerador();

    if (a < b) {
        return std::strong_ordering::less;
    }
    if (a > b) {
        return std::strong_ordering::greater;
    }

    return std::strong_ordering::equal;
}
```

El compilador genera automáticamente el operador de desigualdad si se
define el operador de igualdad `==`. También genera automáticamente los
cuatro operadores relacionales si se define el operador de comparación
de tres vías `<=>`.

``` C++
Racional r1{4, 5};
Racional r2{3, 10};
Racional r3{8, 10};    
std::cout << (r1 == r2) << std::endl;  // 0
std::cout << (r1 != r2) << std::endl;  // 1
std::cout << (r1 != r2) << std::endl;  // 1
std::cout << (r1 < r2)  << std::endl;  // 0
std::cout << (r1 <= r2) << std::endl;  // 0
std::cout << (r1  > r2) << std::endl;  // 1
std::cout << (r1 >= r2) << std::endl;  // 1
std::cout << (r1 == r3) << std::endl;  // 1
```

El estándar proporciona tres categorías para el ordenamiento:

- **Ordenamiento fuerte** o `strong_ordering` implica que únicamente una
  de las tres comparaciones puede ser verdadera `a < b`, `a > b`,
  `a == b`, si `a == b` entonces `T(a) == T(b)`.
- **Ordenamiento débil** o `weak_ordering` implica que únicamente una de
  las tres comparaciones puede ser verdadera `a < b`, `a > b`, `a == b`,
  si `a == b` entonces `T(a)` no necesariamente es igual que `T(b)`. Es
  decir: son equivalentes, pero no son iguales.
- **Ordenamiento parcial** o `partial_ordering` implica que únicamente
  una de las tres comparaciones puede ser verdadera `a < b`, `a > b`,
  `a == b`, si `a == b` entonces `T(a)` no necesariamente es igual que
  `T(b)`. Es decir, los elementos no pueden ser comparados.

En estos ejemplos `T()` denota una función que accede únicamente a los
atributos de salida. Por ejemplo la clase `vector` tiene un tipo de
ordenamiento `strong_ordering`, aunque dos vectores tengan los mismos
elementos pueden tener capacidades diferentes. En este caso, la
capacidad del vector no es un atributo de salida. Un ejemplo de
ordenamiento débil es una cadena de texto que puede compararse con otra
cadena de texto ignorando mayúsculas y minúsculas. Un ejemplo de
ordenamiento parcial son los tipos decimales como `float` y `double` que
pueden ser convertidos a `NaN` y `NaN` no puede ser comparado con ningún
tipo.

Las categorías de ordenamiento forman jerarquías, que pueden ser
convertidas entre sí, un tipo con ordenamiento fuerte puede ser
convertido a un tipo con ordenamiento débil o parcial, mientras que un
tipo con ordenamiento débil solo puede ser convertido a uno parcial.

### Operadores de extracción e inserción de flujo

Los operadores de extracción `>>` e inserción `<<` de flujo, toman como
argumento izquierdo una referencia a un flujo de tipo `std::ostream&` y
`std::istream&` respectivamente y del lado derecho toman un tipo
definido por el usuario. Estos operadores pueden ser implementados como
funciones miembro `friend`:

``` C++
// Imprime un número racional en la salida estándar: "n/d"
friend std::ostream& operator<<(std::ostream& os, const Racional& r)
{
    if (r.num == 0 || r.den == 1) {
        return os << r.num;
    }
    return os << r.num << '/' << r.den;
}

// Lee un número racional de la entrada estándar: n/d
friend std::istream& operator>>(std::istream& is, Racional& r)
{
    T n{0};
    T d{1};
    auto estado = is.flags();   // Guarda el estado (de E/S).
    is >> std::skipws;          // Salta los espacios en blanco
    if (is >> n) {
        if (char sep{}; is >> sep && sep == '/') {
            is >> d;
        }
    }
    is.flags(estado);           // Restaura el estado original.
    r.asignar(n, d);
    return is;
}
```

o pueden ser implementadas como funciones no miembro (fuera de la
clase).

``` C++
// Función no miembro que muestra un número racional en la salida estándar
std::ostream& operator<<(std::ostream& os, const Racional& r)
{
    if (r.numerador() == 0 || r.denominador() == 1) {
        return os << r.numerador();
    }
    return os << r.numerador() << '/' << r.denominador();
}
```

Como no pueden acceder a los datos privados de la clase usan las
funciones miembro (a través de una instancia) para el acceso y
asignación de datos.

### Funciones no miembro

Los creadores de clases definen a menudo funciones auxiliares, que le
permitan a la clase agregar, leer, imprimir o manipular datos. Aunque
estas funciones definen operaciones que son conceptualmente parte de la
interfaz, estas funciones no forman parte de la clase. Definimos
funciones no miembro como cualquier otra función. Y como con cualquier
otra función, podemos separar la declaración de función de la
definición.

``` C++
// Racional literal: 1_r (equivale a 1/1)
constexpr Racional operator""_r(unsigned long long r) 
{ 
    return Racional(r); 
}

// Devuelve el valor absoluto de un número racional: abs(r)
constexpr Racional abs(const Racional& r)
{
    return r.numerador() >= 0 ? r : -r;
}

// Devuelve la potencia de un número racional: r^n
constexpr Racional pow(const Racional& r, const int n)
{
    return Racional(std::pow(r.numerador(), n), std::pow(r.denominador(), n));
}
```

Las funciones que son conceptualmente parte de una clase, pero no están
definidas dentro de la clase, se declaran (o se definen) en la misma
cabecera en la que se define la clase. De esta forma los usuarios que
usen la clase solo necesitan incluir el fichero `.h` para utilizar
cualquier parte del interfaz.

## Instancias de una clase

Para crear una instancia de una clase necesitamos conocer tres cosas:

1.  ¿Cuál es el nombre de la clase?
2.  ¿Dónde está definida?
3.  ¿Qué operaciones soporta?

Una vez que sabemos estas tres cosas podemos usar un tipo definido por
el usuario de esta forma:

``` C++
#include <iomanip>                // setprecision, left
#include <iostream>               // cout 
#include <vector>                 // vector
#include "racional.h"             // Racional

// Función que calcula un número de Bernoulli
Racional bernoulli(std::size_t limite) 
{
    auto nums = std::vector<Racional>();

    for (std::size_t i = 0; i <= limite; ++i) {
        nums.emplace_back(1, (i + 1));
        for (std::size_t j = i; j >= 1; --j) {
            nums[j - 1] = j * Racional(nums[j - 1] - nums[j]);
        }
    }
    return nums[0];
}

// Programa que calcula los primeros 32 números de Bernoulli 
int main()
{
    std::cout << std::fixed << std::setprecision(4) << std::left;
    for (std::size_t n = 0; n <= 32; n += n >= 2 ? 2 : 1) {
        auto b = bernoulli(n);
        std::cout << "B(" << std::setw(2) << n << ") = " 
                  << std::setw(20) << b.decimal() << " = " 
                  << b << '\n';
    }

    return EXIT_SUCCESS;
}
```

La encapsulación de datos es la característica más notable de una clase.
Los datos declarados como privados no son accesibles desde fuera, ya que
sólo los miembros encapsulados dentro de la clase puede acceder a ellos.
Tanto los datos como las funciones pertenecen exclusivamente a la clase
en que se definen y sólo pueden ser usados por los miembros públicos a
través de una **instancia** de la clase.

### Acceso a miembros de una clase

Dentro de una clase, una función miembro es como una función ordinaria.
Con la diferencia de que el ámbito de la función miembro se limita al
bloque de la clase que la contiene

``` C++
#include <iostream>
#include "racional.h"

Racional r1{1, 3};
std::cout << r1.decimal() << std::endl;

r1.asignar(2, 4);
std::cout << r1 << std::endl;    

try {
    Racional r{};
    r.asignar(1, 0);
} catch (const exception& ex) {
    cout << "Excepcion: " << ex.what() << endl;
}
```

La notación de punto proporciona acceso a los miembros públicos de una
clase que no ha sido declarada como puntero. Las clases declaradas como
punteros usan una notación diferente (el operador flecha), debido a la
precedencia del operador punto.

### Selección de miembros usando punteros

Para simplificar el acceso de componentes y miembros de clases a través
de punteros, C++ proporciona un operador especial con forma de flecha,
llamado operador de acceso a miembros: `->`. El operador flecha consiste
de dos símbolos consecutivos: un guión y el signo mayor que. Esta es su
sintaxis:

``` C++
puntero -> funcion_mienbro(); 
```

Equivale a una declaración como esta:

``` C++
(*puntero).funcion_mienbro();
```

Los paréntesis son necesarios debido a que el operador `.` tiene una
mayor precedencia que el operador de indirección `*`.

``` C++
Racional r{4, 3};

// pr es un puntero a r
Racional* pr = &r;

// usa el operador de indirección
std::cout << (*pr).numerador() << std::endl;
```

Es equivalente a:

``` C++
// usa el operador flecha
std::cout << pr->numerador() << std::endl;   
```

El acceso a través del operador flecha `->` elimina el uso de paréntesis
y del operador de indirección `*`.

``` C++
#include <memory>                         // unique_ptr, make_unique

// Acceso mediante notación de punteros "inteligentes"
std::unique_ptr<Racional> r(new Racional{1, 6});
std::cout << r->numerador();              // 1
```

En este caso, la clase `unique_ptr<T>()` crea un puntero único, lo que
equivale a usar:

``` C++
// Función que devuelve un puntero único
auto r = std::make_unique<Racional>(1, 6);
std::cout << r->denominador();             // 6
```

El operador flecha `->` también funciona con punteros inteligentes y con
notaciones más complejas, como el acceso a miembros de tipo puntero.

## Constructores

Las clases controlan la inicialización de un objeto definiendo uno o más
funciones miembro especiales conocidas como **constructores**.

``` C++
// La tarea del constructor es inicializar los miembros de datos de la clase
class Clase {    
public:             
    // El constructor tiene el mismo nombre que la clase 
    Clase() { }      

    // El constructor puede usar sobrecarga
    Clase(int num) : dato(num) { }

    // El destructor tiene el mismo nombre que la clase más el símbolo ~
    ~Clase() { }
private:        
    int dato{0};
};
```

El constructor es una función miembro que se ejecuta siempre que se crea
una instancia de la clase. Los constructores tienen el mismo nombre que
la clase a la que pertenecen. A diferencia de las funciones, los
constructores no pueden devolver ningún valor, pero tienen un cuerpo
(posiblemente vacío) y aceptan una lista de parámetros (posiblemente
vacía).

**¿Que es un objeto?**

Este es un buen momento para hablar un poco sobre el término objeto. En
C++, usualmente; un objeto es una región de la memoria que puede
contener datos y además tiene un tipo. El estándar dice que un objeto en
C++ tiene:

- Un tipo
- Un valor (que puede ser indeterminado)
- Un nombre (opcional)
- Un tamaño (que se puede determinar con `sizeof`)
- Requerimientos de alineación (que se puede determinar con `alignof`)
- Duración de almacenamiento (automática, estática, dinámica, basada en
  hilos)
- Un tiempo de vida (temporal o ligado a la duración del almacenamiento)

Algunos programadores utilizan el término objeto para referirse
únicamente a las variables o a los valores de los tipos de una clase.
Otros hacen una distinción entre objetos con nombre y objetos sin
nombre, usando la palabra \"variable\" para referirse a los objetos con
nombre o identificadores. En algunos casos se hace una distinción entre
objetos y valores, también se usa el término objeto en datos que pueden
ser cambiados por el programa y se usa el término valor para datos que
son inalterables. En este libro, usamos el término lo más general
posible, asumiendo que un objeto es una región de memoria que tiene un
tipo. A sí que utilizamos libremente el término objeto, sin importar si
el objeto es un tipo incorporado o un tipo definido por el usuario, si
tiene un nombre, o si es una constante o un valor mutable.

Una clase puede tener varios constructores. Al igual que la sobrecarga
de funciones, los constructores se pueden diferenciar por el número o el
tipo de sus parámetros.

``` C++
class Clase {    
public:                             
    Clase();                        // Constructor predeterminado
    Clase(int n);                   // Constructor sobrecargado 
    Clase(const Clase& n);          // Constructor de copia
    Clase(Clase&& n) noexcept;      // Constructor de movimiento
    ~Clase();                       // Destructor
};
```

Un constructor también tiene otras funciones además de controlar el
tiempo de vida de una clase, como copiar o mover los recursos de la
clase. La razón por la que se definen los constructores es para
garantizar y simplificar la inicialización \"segura\" de las clases. Por
defecto, el compilador define cada una de estas operaciones, si no se
declara un constructor de forma explícita.

### Constructores constexpr y consteval

A diferencia de las funciones miembro, los constructores no pueden ser
declarados como `const`, aunque pueden ser declarados como `constexpr`,
si los valores son conocidos en tiempo de compilación:

``` C++
// Clase con un constructor declarado como constexpr
class Texto {
public:
    template<std::size_t N>    
    constexpr Texto(const char(&txt)[N]) : len{N - 1}, texto{txt} {}

    constexpr ~Texto() {}      // El destructor debe ser constexpr
    const std::size_t len{};   // Devuelve el tamaño de la cadena de texto

private:
    const char* const texto{}; 
};
```

También se puede declarar un constructor como `consteval`. Un
constructor declarado como `consteval` \"siempre\" se evaluá en tiempos
de compilación, mientras que un constructor declarado como `constexpr`
puede ser evaluado \"opcionalmente\" en tiempos de compilación o en
tiempos de ejecución.

``` C++
#include <iostream>            // cout  

// Clase con un constructor consteval
class Texto {
public:
    // El constructor acepta una cadena de caracteres literal.
    template<std::size_t N>
    consteval Texto(const char(&txt)[N]) : len{N - 1}, texto{txt} {}

    constexpr ~Texto() {}      // El destructor no puede ser consteval
    const std::size_t len{};   // Devuelve el tamaño de la cadena de texto

    // Compara dos cadenas de texto y devuelve un valor booleano
    constexpr bool operator==(const Texto& t) const 
    { 
        return texto == t.texto; 
    }

    // Sobrecarga el operador de salida
    friend std::ostream& operator << (std::ostream& os, const Texto& t) 
    { 
        return os << t.texto; 
    }

private:
    const char* const texto{};  
};

// Evaluá una cadena de texto literal en tiempo de compilación
int main()
{
    // Un tipo literal se puede determinar en tiempo de compilación.
    constexpr Texto t1{"Una cadena de texto literal"};
    constexpr Texto t2 = t1;  // Asigna el valor de t1 a t2

    // Compara las cadenas en tiempo de compilación.
    static_assert(t1 == "Una cadena de texto literal");
    static_assert(t1 == t2);
    static_assert(t1.len == 27);    
    std::cout << t1 << " con " << t1.len << " caracteres." << std::endl;
    // Una cadena de texto literal con 27 caracteres.

    return EXIT_SUCCESS;
}
```

Cuando se crea un objeto `const` dentro de una clase, el objeto no asume
que es constante hasta después de que el constructor termina la
inicialización de la clase. Así, que los constructores pueden escribir
en objetos `const` y `constexpr`, durante su construcción.

### El constructor predeterminado

Cuando no se define un constructor para una clase (o una estructura), el
compilador crea un constructor predeterminado.

``` C++
class Racional {
public:
    // Clase que no define un constructor   

    // Sobrecarga el operador de salida para mostrar los datos de la clase
    friend std::ostream& operator<<(std::ostream& os, const Racional& r)
    {     
        return os << r.num << '/' << r.den;
    }

private:
    int64_t num;     // Los miembros de datos no tienen un valor
    int64_t den;  
};
```

Cuando una clase no define un constructor el compilador crea un
constructor por defecto, para inicializar los miembros de datos que no
tienen valores. ¿Cómo sabe el compilador que valores usar, si no le
suministramos un valor inicializador a un objeto?

``` C++
Racional r;                   // Una instancia sin inicializar
std::cout << r << std::endl;  // Devuelve valores indefinidos
```

Cuando no asignamos un valor, el compilador asigna un valor por defecto
a cada miembro que no tenga uno. Por ejemplo las cadenas de texto se
inicializan como cadenas vacías, sin embargo algunos tipos incorporados
se inicializan con un valor indefinido.

``` C++
Racional r{};                  // Usa el constructor por defecto
std::cout << r << std::endl;   // r tiene un valor indefinido
```

Las clases controlan la inicialización por defecto definiendo un
constructor predeterminado. Este constructor no toma ningún argumento.
Podemos, decir que un constructor predeterminado es especial en muchas
formas, una en la cual, es que si una clase no define un constructor
explícitamente, entonces el compilador definirá un constructor implícito
de forma predeterminada. Cuando el compilador crea un constructor por
defecto sintetiza la clase. Para la mayoría de clases, este constructor
sintetizado inicializa cada miembro de datos de la siguiente forma:

- Si hay un inicializador en la clase, el compilador lo utiliza para
  inicializar cada miembro.
- Si no hay un inicializador predeterminado el compilador inicializa los
  miembros (posiblemente con un valor indefinido).

Solamente las clases sencillas pueden confiar sus datos en un
constructor por defecto. La razón más común para que una clase deba
definir su propio constructor es que el compilador no puede asignar
valores a todos los tipos. Si definimos un constructor personalizado, la
clase no tendrá un constructor por defecto a menos que definamos ese
constructor nosotros mismos. La regla básica es que si una clase
requiere el control para inicializar un objeto en un caso, entonces la
clase probablemente requiera controlar todos los demás casos.

``` C++
// Clase que tiene un constructor predeterminado
class Racional {
public:
    // El constructor asigna valores a los miembros de datos de la clase
    Racional() : num{0}, den{1} {}

private:
    int64_t num;     // No tiene un valor predefinido
    int64_t den;     // No tiene un valor predefinido
};
```

Una razón para definir un constructor predeterminado es que en algunas
clases, el constructor predeterminado no inicializa los miembros de
datos con un valor adecuado. Hay que recordar que los tipos incorporados
y algunos tipos compuestos (como arreglos y punteros) cuando se definen
dentro de un bloque no tienen un valor definido cuando son inicializados
de forma predeterminada.

``` C++
// Clase con un constructor predeterminado y un constructor sobrecargado
class Racional {
public:
    Racional() : num{0}, den{1} {}   
    Racional(int64_t n, int64_t d) : num{n}, den{d} {}     

private:
    int64_t num;        // No tiene un valor inicial
    int64_t den;        // No tiene un valor inicial
};
```

La misma regla se aplica a los miembros de tipos incorporados que son
inicializados de forma predeterminada. Por lo tanto, las clases que
tienen miembros de tipos incorporados o compuestos deben inicializar los
miembros dentro de la clase o definir ordinariamente un constructor
predeterminado. Si no, los usuarios que usen la clase pueden crear
objetos con miembros indefinidos.

``` C++
// Clase con un constructor predeterminado y un constructor sobrecargado
class Racional {
public:
    Racional() {}       // Los datos tienen un valor predeterminado 
    Racional(int64_t n, int64_t d) : num{n}, den{d} {}   

private:
    int64_t num{0};     // Tiene un valor inicial
    int64_t den{1};     // Tiene un valor inicial
};
```

Las clases que tienen miembros de tipos simples o compuestos pueden
confiar generalmente en el constructor sintetizado y predeterminado que
crea el compilador, únicamente si todos los miembros tienen un valor
inicial o un valor inicializador.

Otra razón por la que algunas clases deben definir su propio constructor
predeterminado es que el compilador no puede crear uno. Por ejemplo, si
una clase tiene un miembro que tiene un tipo de otra clase y esa clase
no tiene un constructor predeterminado, entonces el compilador no podrá
inicializar los miembros de la clase. En estos casos, se debe definir un
constructor. Si no, la clase no tendrá un constructor que se pueda usar
de forma predeterminada.

### La palabra clave default

Una forma explícita de definir un constructor predeterminado es usando
la palabra clave `default` después de la declaración de un constructor.

``` C++
Clase() = default;               // Genera el constructor por default
Clase(const Clase&) = default;   // Genera el constructor de copia
Clase(Clase&&) = default;        // Genera el constructor de movimiento
```

El símbolo `=` y la palabra clave `default` pueden aparecer en la
declaración dentro del cuerpo de la clase o en la definición fuera del
cuerpo de la clase. Es como cualquier otra función, si la palabra clave
`default` aparece dentro del cuerpo de la clase, el constructor será
`inline`; si aparece en la definición fuera de la clase, el miembro no
puede ser `inline` por defecto, aunque puede serlo explícitamente.

``` C++
// Clase con constructores definidos como default
class Racional {
public:
    Racional() = default;   
    Racional(int64_t n, int64_t d) : num{n}, den{d} {}
    Racional(const Racional& r) = default;
    Racional(Racional&& r) noexcept = default;

private:
    int64_t num{0};     // Tiene un valor inicial
    int64_t den{1};     // Tiene un valor inicial
};
```

En este ejemplo se define un constructor predeterminado que no toma
ningún argumento y un constructor de copia y movimiento. Definimos estos
constructores como `default` porque queremos que el compilador genere
una versión sintetizada de estos constructores.

Una forma sencilla de evitar que un miembro de una clase tenga un valor
indefinido es asignándole un valor predefinido dentro de la clase.

### Valores predefinidos

Una clase puede usar valores predefinidos en uno (o todos) los miembros
de datos.

``` C++
// Desde C11 las clases pueden usar valores predefinidos
class Racional {   

private:
    int64_t num{0};    // El numerador y el denominador  
    int64_t den{1};    // usan valores predefinidos.  
};     
```

Los valores predefinidos evitan que un miembro de datos sea inicializado
con un valor indefinido.

``` C++
Racional r;     
std::cout << r ;       // Los valores de r son 0 y 1
```

La ventaja de usar valores predefinidos es que el constructor puede usar
la palabra clave `constexpr`, para decirle al compilador que los valores
de la clase pueden ser conocidos en tiempos de compilación.

### Parámetros de función predeterminados

Al igual que las funciones un constructor puede usar valores
predeterminados:

``` C++
// Un constructor con argumentos predefinidos
class Racional {
public:
    // Un constructor es una función y como tal puede usar
    // valores predeterminados en los parámetros de función.
    Racional(int64_t n = 0, int64_t d = 1) : num{n}, den(d) {}
    ~Racional() {}

private:
    int64_t num{0};    
    int64_t den{1};     
};
```

Usando valores predeterminados en cada uno de los parámetros del
constructor se evitan los constructores sobrecargados y si todos los
parámetros tienen un valor predefinido, no hay necesidad de declarar un
constructor por defecto.

``` C++
Racional r;            // Bien: la clase usa valores predefinidos
Racional r1{};         // Bien: usa el constructor que no toma argumentos
Racional r2{2};        // Bien: usa el constructor que toma un argumento  
Racional r2{1, 2};     // Bien: usa el constructor que toma dos argumentos
```

Cuando un constructor usa parámetros predeterminados, se pueden omitir
los argumentos cuando se crea una instancia de la clase, o se pueden
especificar otros valores de forma explícita. Los argumentos
predefinidos son usados para suplir los argumentos no pasados en la
inicialización de la clase.

``` C++
Racional r4{1, 2, 4}   // Error: demasiados argumentos 
```

Si el número de argumentos es diferente o no coincide con el tipo o con
los argumentos recibidos por el constructor se produce un error de
compilación. En estos casos la clase no se inicializa y no es posible
crear una instancia de la clase.

### La palabra clave explicit

El uso de la palabra clave `explicit` le dice al constructor que no
realice conversiones implícitas.

``` C++
// Clase con un constructor declarado como explicit
class Racional {
public:
    explicit Racional(int64_t n = 0, int64_t d = 1) : num{n}, den{d} {}
    ~Racional() {}

private:
    int64_t num{0};    
    int64_t den{1};     
};
```

La palabra clave `explicit` especifica que un constructor o una función
de conversión o deducción debe ser explicita, esto significa que el
constructor no puede ser usado para realizar conversiones implícitas ni
copias de inicialización.

``` C++
Racional r;                         // Bien: inicialización predeterminada
Racional r1{};                      // Bien: inicialización uniforme
Racional r2 = {};                   // Warning: inicializador no explicito
Racional r3 = {16};                 // Error: conversión no implícita
Racional r4 = Racional(16);         // Bien: inicialización directa
Racional r5(16);                    // Bien: inicialización directa
Racional* r6 = new Racional(10);    // Bien: inicialización directa
Racional r7 = (Racional)16;         // Bien: conversión explicita
Racional r8 = {2 * 2};              // Error: conversión no explicita
Racional r9{2.0};                   // Error: conversión no explicita
Racional r10(2.0);                  // Bien: conversión explicita

// Bien: conversión explicita
Racional r11 = static_cast<Racional>(1, 2); 
```

La sintaxis permite usar los paréntesis `()` para realizar ciertas
conversiones de forma implícita. Esto le permite al compilador convertir
de `double` a `int` o viceversa. La sintaxis para la inicialización
uniforme `{}` es más restrictiva y segura, ya que no realiza
conversiones entre diferentes tipos de datos y lanza un error si los
tipos no coinciden o se intenta hacer algún tipo de conversión.

### Lista de inicializadores de un constructor

Esta es la forma en que se inicializan los miembros de datos definidos
dentro de una clase:

``` C++
class Racional {
public:
    // La lista de inicializadores empieza después de los dos puntos 
    Racional(int64_t n=0, int64_t d=1) : num{n}, den{d} {}        

    // El destructor, no realiza ninguna acción
    ~Racional() {}

private:
    int64_t num{0};    
    int64_t den{1};    
};
```

Lo interesante en estas definiciones es el código entres llaves que
definen el cuerpo de la función. Esta parte es la lista de
inicializadores del constructor, que especifican los valores iniciales
para uno o más miembros de la clase. El inicializador es una lista de
nombres de miembros de datos, que es seguida por el valor inicial de ese
miembro entre paréntesis o llaves.

:::: warning
::: title
Warning
:::

**Orden de inicialización de miembros de datos**

Los miembros de datos de una clase se inicializan en el orden en que
fueron declarados. Si se cambia el orden, el compilador emite una
advertencia sobre el orden de inicialización de la clase.
::::

La inicialización de valores se puede hacer de formas diferentes. La
primera forma usa la sintaxis de dos puntos y usan los miembros de datos
como si fueran funciones:

``` C++
// Con paréntesis, el compilador puede realizar conversiones
Racional(int64_t n = 0, int64_t d = 1) : num(n), den(d) {} 
```

La segunda forma usa llaves:

``` C++
// Con llaves, el compilador no realiza conversiones
Racional(int64_t n = 0, int64_t d = 1) : num{n}, den{d} {}
```

La tercera forma usa el cuerpo del constructor y la asignación de
valores:

``` C++
// Usa el operador de asignación
Racional(int64_t n = 0, int64_t d = 1) 
{ 
    num = n;
    den = d;
}
```

También, se pueden combinar:

``` C++
Racional(int64_t n = 0, int64_t d = 1) : num{n}
{
    // Si un constructor no puede construir un objeto valido,
    // entonces debe lanzar una excepción.
    if (d == 0) {
        throw std::invalid_argument("Error: división entre cero.");
    }    
    den = d;
}
```

Todas las notaciones producen el mismo resultado, sin embargo la tercera
notación permite hacer cosas extra, como comprobar los valores recibidos
o realizar alguna acción específica en el constructor como lanzar una
excepción. Vale la pena añadir, que el constructor tiene un cuerpo entre
llaves. El único trabajo de este constructor es proporcionar a los
miembros de datos sus valores iniciales. Si no hay otro trabajo que
hacer, entonces el cuerpo de la función puede estar vacío.

## El destructor

Las clases usan los constructores para inicializar los miembros de datos
de la clase y usan el destructor para liberar los recursos que el objeto
ha adquirido en tiempo de ejecución y quitar los vínculos con otros
recursos u objetos. Al igual que el constructor, el destructor también
es una función:

``` C++
#include <chrono>               // steady_clock
#include <iostream>             // cout

// Mide el tiempo entre la inicialización y la destrucción de una clase.
class Tiempo {
public:
    using clock = std::chrono::steady_clock;
    using milis = std::chrono::milliseconds;

    // El constructor inicia el contador de tiempo.
    Tiempo() { inicio = clock::now(); }

    // El destructor muestra el tiempo transcurrido.
    ~Tiempo() { std::cout << milisegundos() << '\n'; }

    // Calcula el tiempo transcurrido
    inline clock::duration tiempo() noexcept 
    { 
        return clock::now() - inicio; 
    }

    // Mide el tiempo trascurrido en milisegundos
    inline milis milisegundos() noexcept 
    {
        return std::chrono::duration_cast<milis>(tiempo());
    }

private:
    clock::time_point inicio = clock::now();
};

// Clase que mide el tiempo entre su creación y destrucción
int main()
{
    const int limite = 10000000;

    // Cada bloque crea un ámbito diferente
    {
        int valor = 0;        
        Tiempo tiempo{};
        for (auto i = 0; i <= limite; i++) {
            valor += i * 2;
        }
    }

    {
        int valor = 0;
        Tiempo tiempo{};
        for (auto i = 0; i <= limite; i++) {
            valor += i;
        }
    }   

    return EXIT_SUCCESS;
}
```

El constructor usado en un objeto local (o automático) se llama cuando
la ejecución llega al punto en que se define el objeto. El destructor
por otra parte se llama cuando la ejecución sale del alcance del objeto
(es decir, el bloque en el que se define ha terminado de ejecutarse).
Los constructores y destructores en objetos automáticos se llaman cada
vez que la ejecución entra y sale del ámbito del objeto.

Hay dos tipos de clases que necesitan un destructor:

1.  Una clase con recursos que no son manejados por un tipo estándar
    como `vector` o `string`.
2.  Una clase que existe únicamente para ejecutar una acción de
    destrucción, como borrar un puntero o un arreglo o realizar alguna
    acción final.

El nombre del destructor usa el carácter tilde `~` seguido por el nombre
de la clase. El carácter `~` es el operador de complemento a nivel de
bits, y en cierto modo, el destructor es el complemento del constructor.

### Adquisición y eliminación de recursos

El destructor es la contraparte lógica de un constructor que se encarga
de liberar los recursos que adquiere la clase o se encarga de ejecutar
alguna acción final.

``` C++
class Clase {
public:
    Clase(int num) 
    {
        ptr = new int;            // Asigna memoria dinámica
        *ptr = num;          
    }

    ~Clase() 
    {          
        delete ptr;               // Libera la memoria dinámica
    }

private:
    int* ptr;                     // Puntero a un entero        
};
```

En este ejemplo, el destructor se encarga de liberar la memoria que fue
asignada dinámicamente en el constructor mediante `new`.

:::: warning
::: title
Warning
:::

**Fugas de memoria**

Cuando se asigna memoria dinámicamente dentro de una clase (por ejemplo,
con `new`), es importante liberar esa memoria en el destructor para
evitar **fugas de memoria**.
::::

El destructor no recibe parámetros y no devuelve valores. A sí que no
puede especificar un tipo de valor de retorno. Una clase sólo puede
tener un destructor con un nivel de acceso `public`.

``` C++
#include <algorithm>              // for_each
#include <iostream>               // cout
#include <numeric>                // iota  

// Clase que almacena un arreglo de tipos enteros
class Arreglo {
public:
    // Mienbros de tipo 
    using value_type      = int;
    using iterator        = value_type*;
    using const_iterator  = const value_type*;  

    // Constructor que asigna memoria dinámica para el arreglo
    Arreglo(int num) : len(num), bufer(new int[len]) 
    {
        std::cout << "Arreglo con " << len << " elementos creado\n";
    }

    // Destructor que libera la memoria dinámica
    ~Arreglo() 
    {
        delete[] bufer;          // Libera la memoria del arreglo
        std::cout << "\nMemoria del arreglo liberada.\n";
    }

    // Devuelve el número de elementos que contiene el arreglo
    std::size_t size() const noexcept { return len; }

    // Iteradores 
    iterator begin() noexcept { return bufer; }
    iterator   end() noexcept { return bufer + len; }
    const_iterator begin() const noexcept { return bufer; }
    const_iterator   end() const noexcept { return bufer + len; }

private:
    value_type len{0};           // El tamaño del arreglo    
    iterator bufer{nullptr};     // Un puntero a un arreglo dinámico
};

// Liberando memoria
int main() 
{   
    {    
        // Un arreglo para almacenar 100 enteros
        Arreglo arreglo(100);        

        // Inicializa el arreglo con valores incrementales
        std::ranges::iota(arreglo, 0);

        // Imprime el arreglo en la salida estándar
        std::ranges::for_each(arreglo, [](int n) { std::cout << n << ' '; } );
    }

    // Al salir del ámbito, el destructor de Arreglo se llama automáticamente
    std::cout << "El programa ha finalizado." << std::endl;

    return EXIT_SUCCESS;
}
```

Un destructor no puede recibir argumentos y tampoco puede ser
sobrecargado. Una clase solo tiene permitido usar un constructor. Como
caso especial, se puede usar la palabra clave `virtual` cuando la clase
se usa como clase base en relaciones de herencia.

:::: tip
::: title
Tip
:::

**Adquisición de recursos**

Si un constructor adquiere un recurso (para crear o validar un objeto),
el recurso debe ser liberado por el destructor. A la acción de adquirir
y liberar recursos se le conoce como RAII (\"Resource Acquisition Is
Initialization\").
::::

Los destructores al igual que los constructores no devuelven valores,
sin embargo pueden lanzar excepciones, esto es útil, por ejemplo; cuando
es necesario comprobar los recursos adquiridos por una clase, antes de
inicializarla (por el constructor) o después de liberarlos (por el
destructor).

### Destructor virtual

Si una clase es derivada de otra, es importante que el destructor de la
clase base sea **virtual**, para que el destructor de la clase derivada
también se ejecute correctamente cuando se elimine un objeto de la clase
derivada a través de un puntero de la clase base. Si no se marca como
`virtual`, solo se llamará al destructor de la clase base, lo que puede
provocar fugas de memoria.

``` C++
#include <iostream>               // cout

// Clase base con un destructor virtual
class Base {
public:
    virtual ~Base() 
    {  
        std::cout << "Destructor de Base" << std::endl;
    }
};

// Clase derivada que sobrescribe un destructor virtual
class Derivada : public Base {
public:
    ~Derivada() override 
    {  
        std::cout << "Destructor de Derivada" << std::endl;
    }
};

// Destructor virtual
int main() 
{
    // Crea un objeto de tipo Derivada
    Base* ptr = new Derivada();  

    // Invoca al destructor de la clase Derivada y luego al de Base        
    delete ptr;  

    return EXIT_SUCCESS;
}
```

En este caso, primero se ejecuta el destructor de la clase `Derivada` y
luego el de la clase `Base`.

### El destructor predeterminado

Aun y cuando no se proporcione un destructor para una clase, todas las
clases tienen un destructor. Si no se especifica un destructor de manera
explícita, el compilador crea un destructor vacío: `~Destructor() { }`.
Las clases que manejan sus propios recursos usando punteros deben
liberar la memoria cuando la clase sale de su ámbito, sin embargo las
clases que no manejan recursos o no realizan una acción final no
necesitan un destructor.

Una forma de crear un destructor predeterminado, es pedirle al
compilador que sintetice uno, usando la palabra clave `default`:

``` C++
~Destructor() = default;
```

Un destructor predeterminado puede usar la palabra clave `default` para
pedirle al compilador que genere un destructor, este destructor es útil
cuando la clase maneja objetos que se pueden destruir automáticamente
cuando salen de su ámbito, como los punteros inteligentes.

### La palabra clave delete

Un constructor puede ser suprimido explícitamente usando la palabra
clave `delete`. Por ejemplo para evitar la asignación por movimiento. En
este caso el objeto no se puede mover:

``` C++
// No hay un constructor de movimiento
Clase(Clase&&) = delete;        

// No se puede usar la asignación por movimiento    
Clase& operator=(Clase&&) = delete; 
```

O para evitar la copia.

``` C++
// No hay un constructor de copia
Clase(const Clase&) = delete;

// No se puede usar la asignación por copia   
Clase& operator=(const Clase&) = delete;
```

En este caso el objeto no se puede copiar, ya que deshabilitamos la
copia y la asignación. Un destructor sin embargo, no se puede borrar
usando la palabra clave `delete`. Todas las clases tienen uno, aunque no
haga nada (o este vacío).
