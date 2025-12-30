# Estructura léxica

::: {.meta title="Estructura léxica" capitulo="2" description="Componentes léxicos de un programa" keywords="tokens, separadores, identificadores, palabras clave, signos"}
:::

Cada lenguaje de programación contiene un conjunto de palabras clave,
reglas y sintaxis propia, que lo vuelven distinto a otros lenguajes.
Estas características forman parte de la estructura léxica. La
estructura léxica de un lenguaje de programación es el conjunto de
reglas elementales que especifican la forma correcta de escribir un
programa, su objetivo es definir las posibles combinaciones de símbolos,
palabras clave y reglas para formar un programa sintácticamente
correcto.

C++, como la mayoría de los lenguajes de programación utiliza secuencias
de texto que incluyen palabras, números y signos a los cuales se les
conoce como tokens. Los tokens son secuencias de caracteres que tienen
un significado especial para el compilador, como identificadores,
palabras clave, comentarios y separadores. La estructura léxica es el
equivalente a las reglas gramaticales de un lenguaje.

Este capítulo presenta una introducción a los elementos léxicos que
conforman el código fuente de un programa sintácticamente correcto.

## El conjunto de caracteres

El código fuente de un programa experimenta varias transformaciones
antes de convertirse en un programa ejecutable. El primero paso
involucra el procesamiento de las cabeceras `#include`. Este primer paso
involucra el procesamiento condicional de directivas, para producir lo
que el estándar llama: **unidad de traducción**. El término
\"traducción\" por tanto, abarca la interpretación como la compilación.
Una de las primeras tareas del compilador es leer los caracteres físicos
del archivo fuente y traducir los caracteres al conjunto de caracteres
estándar.

### El conjunto de caracteres fuente

Para el compilador, un archivo fuente es simplemente una secuencia de
caracteres. Cuando el compilador lee un archivo fuente, asocia cada uno
de los caracteres físicos, al conjunto de caracteres en tiempo de
compilación, el cual es llamado \"conjunto de caracteres fuente\". El
conjunto de caracteres fuente que reconocen todos los compiladores,
incluye como mínimo: 96 caracteres que incluyen el espacio en blanco, el
tabulador horizontal, el tabulador vertical, el avance de página y el
carácter de fin de línea. Los cuáles se representan de esta forma:

``` C++
Espacio              ' '
Tabulador horizontal '\t'
Tabulador vertical   '\v'
Avance de página     '\f'
Fin de línea         '\n'
```

y los siguientes 91 caracteres:

    a b c d e f g h i j k l m n o p q r s t u v w x y z
    A B C D E F G H I J K L M N O P Q R S T U V W X Y Z
    0 1 2 3 4 5 6 7 8 9
    _ { } [ ] # ( ) < > % : ; . ? * + - / ^ & | ~ ! = , \ " ’

El conjunto de caracteres en tiempos de ejecución puede ser diferente al
conjunto de caracteres fuente (aunque a menudo son iguales). Si los
conjuntos de caracteres son diferentes, el compilador convierte
automáticamente todos los caracteres y cadenas de texto literales del
conjunto de caracteres fuente, al conjunto de caracteres de ejecución.

En el conjunto de caracteres, los valores numéricos tienen dos
restricciones:

1.  El carácter `nulo` siempre tiene un valor de `0`.
2.  Los dígitos siempre deben tener valores secuenciales que comienzan
    en `0`.

En C++ se utilizan, principalmente dos conjuntos de caracteres. El más
común es ASCII (Código Estándar Americano para el intercambio de
Información) y Unicode. Ambos códigos se basan en la asignación de un
código numérico a cada uno de los tipos de caracteres. Técnicamente, los
caracteres del conjunto fuente se asocian a Unicode y de Unicode al
conjunto de caracteres soportado por el conjunto de caracteres de
ejecución.

### El conjunto de caracteres ASCII

El conjunto de caracteres estándar más usado dentro del código fuente,
es el código ASCII básico, este conjunto utiliza 7 bits (128
caracteres), mientras que algunas implementaciones utilizan el código
ASCII extendido que usa 8 bits (256 caracteres).

El siguiente programa imprime la tabla ASCII de 128 caracteres:

``` C++
#include <cctype>    // isprint
#include <iomanip>   // setw, setfill
#include <iostream>  // cout
#include <string>    // string
#include <vector>    // vector

// Programa que imprime la tabla ASCCI básica
int main()
{
    // Los primeros caracteres no imprimibles de la tabla ascci
    std::vector<std::string> no_imprimibles {
        "\\0", "SOH", "STX", "ETX", "EOT", "ENQ", "ACK", "\\a",
        "\\b", "\\t", "\\n", "\\v", "\\f", "\\r", "SO", "SI",
        "DLE", "DC1", "DC2", "DC3", "DC4", "NAK", "SYN", "ETB",
        "CAN", "EM", "SUB", "ESC", "FS", "GS", "RS", "US", "SP",
    };

    // Una cadena de texto para crear el borde de la tabla
    std::string guiones{"+--------+--------+--------+--------+"
                        "--------+--------+--------+--------+"};
    // El titulo de la tabla
    std::cout << std::setw(45) << "La tabla ASCCI\n" << guiones << "\n";

    // Imprime la tabla en 16 filas y 8 columnas
    for (std::size_t i = 0; i < 16; ++i) {
        for (std::size_t j = 0; j < 128; j += 16) {
            const char c = i + j;
            std::cout << "|"
                      << std::setw(3) << std::setfill('0') << i + j
                      << std::setw(4) << std::setfill(' ');
            if (!std::isprint(c)) {
                std::cout << (c == 127 ? "DEL" : no_imprimibles[c]) << " ";
            } else {
                std::cout << c << " ";
            }
        }
        std::cout << "|\n";
    }
    std::cout << guiones << "\n";        
    return EXIT_SUCCESS;
}
```

En este ejemplo, el programa usa un bucle doble, para iterar primero
sobre un rango entre 0 y 16, el cual representa el número de filas de la
tabla, mientras que el segundo bucle lo hace en el rango de 0 a 128,
incrementando el número de columnas cada 16 iteraciones, es decir cada 8
columnas (`128/16=8`). Los caracteres de la tabla ASCCI, son
representados por valores numéricos.

Los caracteres que van del 0 al 32 son llamados por el estándar
caracteres no imprimibles, porque no tienen una representación que se
pueda incluir en la salida. Para detectar estos caracteres se usa la
función `isprint`, que devuelve `true` si un valor alfanumérico no es
imprimible.

Esta es la salida del programa:

    La tabla ASCCI
    +--------+--------+--------+--------+--------+--------+--------+--------+
    |000  \0 |016 DLE |032     |048   0 |064   @ |080   P |096   ` |112   p |
    |001 SOH |017 DC1 |033   ! |049   1 |065   A |081   Q |097   a |113   q |
    |002 STX |018 DC2 |034   " |050   2 |066   B |082   R |098   b |114   r |
    |003 ETX |019 DC3 |035   # |051   3 |067   C |083   S |099   c |115   s |
    |004 EOT |020 DC4 |036   $ |052   4 |068   D |084   T |100   d |116   t |
    |005 ENQ |021 NAK |037   % |053   5 |069   E |085   U |101   e |117   u |
    |006 ACK |022 SYN |038   & |054   6 |070   F |086   V |102   f |118   v |
    |007  \a |023 ETB |039   ' |055   7 |071   G |087   W |103   g |119   w |
    |008  \b |024 CAN |040   ( |056   8 |072   H |088   X |104   h |120   x |
    |009  \t |025  EM |041   ) |057   9 |073   I |089   Y |105   i |121   y |
    |010  \n |026 SUB |042   * |058   : |074   J |090   Z |106   j |122   z |
    |011  \v |027 ESC |043   + |059   ; |075   K |091   [ |107   k |123   { |
    |012  \f |028  FS |044   , |060   < |076   L |092   \ |108   l |124   | |
    |013  \r |029  GS |045   - |061   = |077   M |093   ] |109   m |125   } |
    |014  SO |030  RS |046   . |062   > |078   N |094   ^ |110   n |126   ~ |
    |015  SI |031  US |047   / |063   ? |079   O |095   _ |111   o |127 DEL |
    +--------+--------+--------+--------+--------+--------+--------+--------+

El valor `0` de la tabla ASCCI, equivale al carácter nulo `\0`. El cual
se usa como finalizador en cadenas de caracteres estilo C. Y en algunas
implementaciones se usa como valor de la macro `NULL` que representa un
valor nulo. Algunos caracteres como el espacio, el tabulador o el final
de línea son llamados separadores, porque su función es separar
elementos léxicos en el código fuente. Estos caracteres solo pueden
usarse junto con la barra inclinada que escapa el valor literal del
carácter.

Los caracteres ASCII se procesan normalmente usando el tipo `char`, que
asocia cada carácter a un código numérico que se almacena en un byte,
mientras que lo caracteres Unicode usan el tipo `wchar_t`, que puede
almacenar un rango más amplio de caracteres, normalmente 2 bytes en
sistemas Windows y 4 bytes en sistemas Linux.

La biblioteca estándar contiene un conjunto de funciones que permiten
trabajar con caracteres de forma individual. Estas funciones se definen
en el archivo de cabecera `cctype`. Para trabajar con cadenas de texto
(cadenas de carácteres) se utiliza el tipo `string` definido en el
archivo de cabecera `<string>`.

``` C++
#include <cctype>                         // isalpha, isupper, isdigit
#include <iostream>                       // cout
#include <string>                         // string
#include <vector>                         // vector    

// Agrupa caracteres ASCCI por categorias
int main()
{
    // Un vector que almacena tipos string
    std::vector<std::string> categoria {
        "Mayúsculas... ",                 // categoria[0]
        "Minúsculas... ",                 // categoria[1]
        "Dígitos...... ",                 // categoria[2]
        "Signos....... ",                 // categoria[3]
    };

    int limite = 128;                     // Carácteres en la tabla ASCII
    for (int c = 0; c < limite; ++c) {
        if (std::isalpha(c)) {            // ¿Es una letra?
            if (std::isupper(c)) {        // ¿Es una mayúscula?
                categoria[0] += c;
            } else {
                categoria[1] += c;        // ¿Es una minúscula?
            }
        } else if (std::isdigit(c)) {     // ¿Es un digito?
            categoria[2] += c;
        } else if (std::ispunct(c)) {     // ¿Es un signo de puntuación?
            categoria[3] += c;
        }
    }

    // Imprime el vector con las cadenas de texto
    for (const std::string& s : categoria) {
        std::cout << s << '\n';
    }

    return EXIT_SUCCESS;
}
```

Este programa muestra por salida, algo como esto:

    Mayúsculas... ABCDEFGHIJKLMNOPQRSTUVWXYZ
    Minúsculas... abcdefghijklmnopqrstuvwxyz
    Dígitos...... 0123456789
    Signos....... !"#$%&'()*+,-./:;<=>?@[\]^_`{|}~

La tabla ASCII agrupa los caracteres que representan números en el rango
que va del 48 al 57 de forma continua, estos caracteres son llamados
dígitos. La función `isdigit` devuelve `true` si un carácter o un valor
numérico representa un digito. La función `isdigit()` se implementa de
esta forma:

``` C++
// Devuelve true si el carácter representa un dígito
bool isdigit(const char c) 
{ 
    return ('0' <= c) && (c <= '9'); 
}
```

La función `isalpha` devuelve `true` si un carácter o un valor numérico
representa una letra (mayúscula o minúscula). La función `isalpha` se
implementa de esta forma:

``` C++
// Devuelve true si el carácter representa una letra
bool isalpha(const char c)
{
    return (('a' <= c) && (c <= 'z')) || (('A' <= c) && (c <= 'Z'));
}
```

Los caracteres alfabéticos están separados en mayúsculas y minúsculas.
Las mayúsculas están definidas en el rango que va del 65 al 90 y la
minúsculas, del 97 al 122.

``` C++
// Devuelve true si el carácter representa una letra en mayúsculas
bool isupper(const char c) 
{ 
    return (('A' <= c) && (c <= 'Z')); 
}    

// Devuelve true si el carácter representa una letra en minúsculas
bool islower(const char c) 
{ 
   return (('a' <= c) && (c <= 'z')); 
}
```

El siguiente programa usa las funciones `isalpha()` e `isdigit()` para
cifrar un mensaje de texto, usando rot13 para cifrar letras y rot5 para
cifrar números.

``` C++
#include <cctype>                          // isalpha, isdigit
#include <iostream>                        // cout
#include <string>                          // string

// Cifra o descifra un mensaje de texto usando rot13
std::string rot13(const std::string& texto) 
{
    std::string salida;          
    for (char letra : texto) {
        // Si es una letra usa rot13
        if (std::isalpha(letra)) {        
            char c = std::isupper(letra) ? 'A' : 'a';
            letra = ((letra + 13 - c) % 26) + c;
        }     
        // Si es un digito usa rot5              
        if (std::isdigit(letra)) {         
            int rot5 = (letra < '5') ? 5 : -5;
            letra = ((letra + rot5) - letra) % 26 + letra;
        }
        salida += letra;   
    }

    return salida;
}

// Programa que cifra un mensaje de texto usando rot13
int main()
{
    std::string texto_plano{"La próxima versión de C++ es 26"};
    std::string texto_cifrado = rot13(texto_plano);      

    // Imprime la cadena de texto original y la cifrada
    std::cout << "Texto plano      : " << texto_plano          << '\n'
              << "Texto cifrado    : " << texto_cifrado        << '\n'
              << "Texto descifrado : " << rot13(texto_cifrado) << '\n';

    return EXIT_SUCCESS;
}
```

En este ejemplo, el programa usa el número 13 como clave para los
caracteres alfabéticos y el número 5 para los dígitos. El programa
desplaza la posición de cada letra (o digito) del mensaje por el número
de posiciones de la clave. Por ejemplo la letra \'A\' tiene la posición
\'65\' en la tabla ASCII mientras que la letra \'a\' tiene la 97. Una
vez que se desplazan 13 posiciones la \'A\' se convierte en \'N\' y la
\'a\' en \'n\'. El operador modulo se encarga de que las letras no se
salgan del rango válido de caracteres. Las letras que no son ASCII no
cambian.

Existen otros caracteres especiales como los signos de puntuación y los
operadores.

``` C++
// Devuelve true si un carácter representa un operador
bool ispunct(const char c)
{
    return ('+' == c) || ('-' == c) || ('*' == c) || ('/' == c) ||
           ('^' == c) || ('<' == c) || ('>' == c) || ('=' == c) ||
           (',' == c) || ('!' == c) || ('(' == c) || (')' == c) ||
           ('[' == c) || (']' == c) || ('{' == c) || ('}' == c) ||
           ('%' == c) || (':' == c) || ('?' == c) || ('&' == c) ||
           ('|' == c) || (';' == c);
}
```

Los operadores tienen un significado especial, dependiendo de la
posición que ocupen en el código fuente. La función `ispunct()`
comprueba si un valor representa un signo de puntuación. Hay otras
funciones como `isspace` que detecta espacios en blanco, o la función
`toupper` que convierte una letra mayúscula en minúscula y `tolower` que
hace lo contrario. Todas estas utilidades están incluidas en el archivo
de cabecera `<cctype>` y en el espacio de nombres `std`.

### Caracteres Unicode

Unicode es un superconjunto de caracteres ASCII y Latin-1, que
virtualmente soporta \"todos\" los lenguajes usados. Los caracteres
Unicode también son llamados caracteres universales. Se puede
especificar cualquier carácter Unicode en el archivo fuente de un
programa usando un carácter Unicode de la forma: `\uXXXX` (la u
minúscula) o `\UXXXXXXXX` (la U, en mayúsculas), en el cual `0000XXXX` o
`XXXXXXXX` representa el valor hexadecimal del carácter.

``` C++
// Representación Unicode de pi
std::cout << "\u03c0";                            
```

Hay algunas restricciones:

- Siempre se deben utilizar cuatro u ocho dígitos hexadecimales.
- No se puede utilizar un carácter Unicode para especificar caracteres
  fuera del rango `0-0x20` o `0x7F-0x9F`.
- No se puede usar la barra inclinada en medio de un carácter Unicode.

No se puede utilizar directamente un carácter Unicode en el código
fuente de esta forma:

    double π = 3.14159L;                             // Error
    std::cout << "\π";                               // Error

Aunque se puede usar el valor numérico del carácter Unicode, usando la
barra inclinada y su valor hexadecimal:

``` C++
double \u03c0 = 3.14159L;                        // Bien
std::cout << \u03c0;                             // 3.14159
```

La forma en que se asocian los caracteres depende de la implementación.
Algunos compiladores no reconocen caracteres Unicode, o solo soportan un
conjunto limitado. Por práctica general y compatibilidad, Unicode no
debe usarse en identificadores.

## Elementos léxicos

Una vez que el compilador ha leído los caracteres fuente, divide el
archivo en elementos léxicos (o tokens) del preprocesador, usando los
llamados separadores. El compilador intenta juntar tantos caracteres
contiguos mientras pueda construir un símbolo válido.

``` C++
Palabra reservada 
    |  Separador  
    |  | Identificador
    |  |  | Signos de puntuación
    v  v  v  v
    int main()   
    {            
        return 0; <- Signo de puntuación  
    }     ^    ^  
    ^     |  Valor literal
    |  Palabra reservada
Signos de puntuación
```

Hay cinco tipos de tokens: identificadores, palabras clave, valores
literales, símbolos de puntuación y operadores. El espacio, la
tabulación horizontal, la tabulación vertical, el avance de página, el
carácter de fin de línea y los comentarios son llamados colectivamente
\"espacios en blanco\" y sirven como separadores. Los separadores son
ignorados por el compilador.

### Tipos de tokens

Un *token* es el elemento mínimo en un programa que es significativo
para el compilador. Los *tokens* se usan para crear categorías
sintácticas mas complejas como las declaraciones y expresiones. La
siguiente tabla muestra los diferentes tipos de tokens y su significado.

  -----------------------------------------------------------------------
  Tipo de símbolo      Descripción                         Ejemplos
  -------------------- ----------------------------------- --------------
  Identificadores      Nombres que identifican a objetos,  cout, std, x,
                       funciones, variables, etc.          y, var, num

  Palabras reservadas  Palabras que tienen un significado  int, double
                       especial para el compilador.        for, auto

  Valores literales    Valores que pueden aparecer         \"Hola\", 100
                       directamente en el código fuente.   234.45, \'c\'

  Signos de puntuación Caracteres especiales que definen   {}, (), ::, ;
                       la estructura de un programa.       

  Operadores           Caracteres con significado especial \<\<, \>\>,
                       que realizan operaciones            ++, \--+, -,
                       Matemáticas ,lógicas, relacionales, \*, &&, \<,
                       comparación.                        \>, ==, =
  -----------------------------------------------------------------------

  : Tipos de *tokens*

Los *tokens* suelen estar separados por un \"separador\", que puede ser
uno o varios:

- Espacios en blanco.
- Tabulaciones horizontales o verticales.
- Líneas.
- Comentarios (de una o varias líneas).

El compilador descompone un programa en *tokens* y comprueba que los
símbolos del lenguaje (palabras clave, operadores, signos, variables,
etc.) se hayan escrito correctamente, si no es asi, lanza un error de
sintaxis. El compilador suele ser muy preciso en estos casos.

## Identificadores

Un identificador es un **nombre** que define un valor dentro de un
programa. Técnicamente, los identificadores son secuencias de caracteres
colocados después del tipo, que se usa para denotar:

- El nombre de un objeto o variable.
- Un nombre de clase, estructura o unión.
- Un nombre de tipo enumerado.
- El miembro de una clase, estructura, unión o enumeración.
- Una función o una función miembro de clase.
- Un alias.
- El nombre de una etiqueta.
- El nombre de una macro.

La mayoría de los programadores restringen el conjunto de caracteres
usados, al rango de letras: `a-z` y `A-Z` y al guion bajo, aunque el
estándar permite el uso de otros caracteres, no todos los compiladores
lo soportan.

``` C++
a b c d e f g h i j k l m
n o p q r s t u v w x y z
A B C D E F G H I J K L M
N O P Q R S T U V W X Y Z _
```

Un identificador puede comenzar con una letra o con un guion bajo (`_`).
Los caracteres que le siguen pueden contener letras, dígitos, guiones
bajos, números, etc.

``` C++
i, j, k                 // Índices
T                       // El tipo de una plantilla
nombre_de_variable      // Nombre de una variable
MACRO                   // Una Macro
variable                // Nombre de una variable
constante               // Nombre de una constante
sumar                   // Nombre de una función 
Color                   // Nombre de una estructura
UnaClase                // Nombre de una clase 
```

No se permiten dígitos como primer carácter, para que el compilador
pueda distinguir los identificadores de los números, sin embargo si se
pueden usar números después (o enmedio) de una letra.

``` C++
pa5o                    // Bien: Dígitos enmedio de caracteres
paso1                   // Bien: Dígitos al final de un carácter
2paso                   // Error: Dígito al inicio
privado_                // Caso especial: usar con precaución   
publico+                // Bien: Símbolo después de un carácter
paso 03                 // Error: Separador entre identificadores
_privado                // Caso especial: usar con precaución
__TIME__                // Caso especial: No se recomienda su uso
bool                    // Error: Palabra reservada
sin                     // Error: Función definida en la cabecera cmath
```

No se pueden usar separadores enmedio de un identificador. El guion bajo
`_` es un caso especial y se reserva para uso interno del lenguaje por
lo que no es recomendable su uso. Algunos nombres pueden entran en
conflictos con nombres definidos en alguna biblioteca como `sin`,
definido en la cabecera `cmath`.

``` C++
int NUMERO              // C++ distingue entre mayúsculas 
int numero              // y minúsculas (Estos nombres son diferentes).
```

C++ distingue entre mayúsculas y minúsculas, `numero` es diferente de
`Numero`, de `NUMERO` y de `NumerO`.

``` C++
el_nombre_de_una_variable_muy_larga   // Un nombre muy largo
```

La longitud de un identificador debe ser proporcional a la función que
realiza, si se usa dentro de un bucle por ejemplo, un solo carácter
usado como índice puede ser suficiente. Por cierto, algunos compiladores
(y el sentido común), restringen la longitud de un identificador a 32
caracteres como máximo.

### Utilidad de los identificadores

Los identificadores pueden ayudar a que el código sea más legible y
también pueden ayudar a explicar lo que hace el código sin necesidad de
usar comentarios extra. Por ejemplo estas convenciones son muy comunes.

``` C++
int i, j, k;                 // Indices en bucles 
#define PI 3.1416            // Macros en mayúsculas
bool sumar_total();          // ¿Que hace la función? 
struct Fecha{};              // Nombres que denotan objetos
void borrar();               // Verbos que implican acciones
class Fraccion{} ;           // Clases, la primera letra en mayúsculas
```

Hay varias formas de escribir identificadores que usan varias palabras,
para suplir los espacios en blanco. Estas dos, son las notaciones más
comunes:

**Notación camello:**

``` C++
int sumarTotal;               // Usa una letra en mayúsculas
```

**Notación con guiones:**

``` C++
int sumar_total;              // Usa el guion bajo como separador
```

Es muy común usar el guion bajo `_` como separador (en lugar de espacios
en blanco). Por ejemplo: `sumar_total`, aunque también es común usar la
notación camello `sumarTotal`, que usa la letra `T` en mayúscula en
lugar del guion bajo. No hay ninguna regla práctica, que impida usar uno
u otro estilo. Existe otro estilo, que incluye el tipo de dato (o
prefijo) en el nombre, llamado **Notación húngara**.

``` C++
bool bAbrir;                  // b = es un valor boléano 
int iContador;                // i = tipo entero (int, short, ...)
char* pNombre;                // p = es un puntero
```

Al usar la notación Húngara, el tipo y algunas veces también el ámbito
de una variable se usan como prefijo para algunas variables. La notación
húngara es potencialmente útil en lenguajes débilmente tipados o que no
usan espacios de nombres como C. Pero, no hay ninguna razón para
codificar el tipo de una variable en el nombre en C++. Aparte de esto,
los prefijos puede impedir la legibilidad del código.

## Palabras clave

Las palabras clave son palabras reservados que tienen un significado
especial para el compilador. Estas palabras no pueden usarse como
identificadores en un programa, porque tienen un significado especial.
Al igual que cualquier lenguaje, C++ reserva ciertas palabras clave para
ser usadas por el lenguaje. A estas palabras se les conoce como palabras
reservadas, porque no pueden ser usadas como identificadores regulares,
ya que el lenguaje las usa internamente. Si intentas usar una, se
produce un error en tiempo de compilación.

Estas son palabras reservadas en C++:

``` none
alignas continue friend register true alignof decltype goto
reinterpret_cast try asm default if return typedef auto delete
inline short typeid bool do int signed typename break double long
sizeof union case dynamic_cast mutable static unsigned catch else
namespace static_assert using char enum new static_cast virtual
char16_t explicit noexcept struct void char32_t export nullptr
switch volatile class extern operator template wchar_t const false
private this while constexpr float protected thread_local
const_cast for public throw
```

Las palabras clave:
`and, and_eq, bitand, bitor, compl, not, not_eq, or`, `or_eq, xor` y
`xor_eq` son alternativas simbólicas para: `&&, &=, &`,
`|, ~, !, !=, ||, |=, ^ y ^=` respectivamente. Estas palabras tampoco
pueden usarse como identificadores, aunque se desaconseja su uso en
versiones modernas de C++.

### Identificadores reservados

Todos los identificadores con enlace externo son siempre reservados. Si
un identificador es reservado significa que no se puede redefinir. Sin
embargo, esto no es una restricción para la compilación y algunas veces
es posible usar un nombre o redefinir otro deliberada o
inadvertidamente, sin ver ningún mensaje de error por parte del
compilador. Esto pasa cuando se usa un identificador cuyo nombre es
igual a una función de alguna biblioteca.

Algunos identificadores que se deben evitar son nombres de funciones
globales, identificadores del sistema y nombres usados por el estándar
ANSI C. Por ejemplo la biblioteca `cmath` define una función llamada
`acos`. Todas las rutinas de la cabecera `<cmath>` tienen una versión
común como `acos`, una que toma un tipo flotante como `acosf`, y una que
toma un tipo de doble precisión como `acosl`. Esto, significa que los
tres identificadores `acos`, `acosf` y `acosl` son reservados, solo si
se usa la cabecera `cmath`.

Algunos identificadores son reservados sólo en el ámbito global y otros,
solo son reservados en archivos. La forma más sencilla de evitar
colisiones entre nombres es evitar usar cualquier identificador que
pertenezca al sistema, al estándar, o a las cabeceras de forma global.

### Macros

La directiva `#` crea constantes simbólicas (constantes representadas
como símbolos) y macros (operaciones definidas como símbolos).

Esta es su sintaxis:

``` C++
#define identificador texto-de-reemplazo
```

Cuando aparece una línea como esta en un archivo, todas las ocurrencias
del identificador en el archivo actual se reemplazan por el
\"texto-de-reemplazo\" antes de que se compile el programa.

Por ejemplo:

``` C++
#define PI 3.14159   
```

En este caso la macro `PI` reemplaza todas las ocurrencias de la
constante simbólica PI por la constante numérica 3.14159. Las constantes
simbólicas permiten crear un nombre para una constante y utilizar el
nombre a lo largo del programa.

Todas las directivas del preprocesador empiezan con `#` y sólo pueden
aparecer espacios en blanco antes de una directiva del preprocesador en
una línea. Las directivas del preprocesador no son instrucciones de C++,
por lo que no terminan con punto y coma (`;`). Las directivas del
preprocesador se procesan por completo antes de que empiece la
compilación.

#### El shell de Bourne

A finales de los años 70\'s, Steve Bourne escribió un programa llamado
*shell* (un intérprete de comandos) para la versión 7 de UNIX en los
laboratorios Bell. Bourne decidió utilizar el preprocesador de C, para
que la sintaxis de C se pareciera a la del Algol-68. Muchos años antes,
en la universidad de Cambridge en Inglaterra, Steve había escrito un
compilador para el Algol-68, y había encontrado que era más sencillo
depurar código que tenía \"declaraciones finales\" como `if-elif` o
`case-esac`. Steve pensaba que no era sencillo leer el codigo mirando
las llaves \"{ }\". Como consecuencia, estableció varias definiciones
para usar el preprocesador como remplazo:

``` C
#include <iostream>                  // Constantes simbólicas:
#define IF if(                       // Remplaza IF por if(
#define THEN ){                      // Remplaza THEN por ){   
#define ELSE } else {                // Remplaza ELSE por } else { 
#define FI ;}                        // Remplaza FI por ;}
#define WHILE while (                // Remplaza WHILE por while (  
#define DO ){                        // Remplaza DO por ){ 
#define OD ;}                        // Remplaza OD por ;} 
#define INT int                      // Remplaza INT por int
#define BEGIN {                      // Remplaza BEGIN por {
#define END }                        // Remplaza END por }
#define RETURN(val) return(val);     // Las macros son funciones
#define PRINT(txt) std::cout << txt; // definidas como símbolos.
```

Esto le permitió escribir código como este:

``` C
// Función que suma los dígitos de un número entero
INT sumar_digitos(INT num)     
BEGIN        
    IF num < 0 THEN 
        num = -1 * num 
    FI       
    INT suma = 0;
    WHILE num > 0 THEN 
        suma = suma + (num % 10);
        num = num / 10;
    END    
    RETURN(suma)
END

// Programa que suma los dígitos de un número entero
INT main()
{
    INT total = sumar_digitos(123456789);
    PRINT(total)                      // 45
    RETURN(0)
}
```

Aunque el programa no luce como un típico programa en `C++`. Es válido y
es sintácticamente correcto. Aunque esta práctica es poco recomendable y
tiene poco que ver con C++, tiene un propósito real, mostrar como el
compilador analiza los tokens de un programa y demuestra cómo se puede
eventualmente remplazar cualquier \"token\" de un programa antes de
compilarlo.

:::: note
::: title
Note
:::

El dialecto del Algol-68 alcanzó el estatus de legendario, tal como el
shell de Bourne que lo llevo mucho más alla de los laboratorios Bell,
aunque disgusto a muchos programadores de C, que se quejaron por la
sintaxis usada por Bourne, que hizo complicado mantener y modificar el
código fuente.
::::

El preprocesador elimina los comentarios y convierte el codigo anterior,
en algo como esto:

``` C
int sumar_digitos(int num)
{
    if(num < 0){ 
        num = -1 * num;
    }
    int suma = 0;
    while ( num > 0 ) {
        suma = suma + (num % 10);
        suma = num / 10;
    }
    return suma;
}

int main()
{
    int total = sumar_digitos(119765);
    std::cout << total;
    return 0;
}
```

El compilador proporciona un modo de ejecutar únicamente el
pre-procesamiento en los archivos fuente. Usando la opción -E.

``` Console
c++ -E -Wall macros.cpp -o macros.i    
```

A menos que se especifique lo contrario, la opción `-o` muestra la
salida del preprocesador en un archivo de texto llamado `macro.i`. Las
ultimas líneas del fichero de salida contienen el código fuente del
programa pre-procesado.

#### Macros y palabras clave reservadas

Algunas macros son palabras reservadas. La siguiente lista enumera las
macros estándar definidas por el preprocesador:

``` C++
__LINE__
__FILE__
__DATE__
__TIME__
__cplusplus
__STDC__
```

Estas macros pueden usarse directamente, de esta forma:

``` C++
#include <iostream>                 // cout

// Programa que imprime la fecha y la versión de C++ usada
int main()
{
    std::cout << "Fecha         : " <<  __DATE__    << '\n'
              << "Versión de C++: " <<  __cplusplus << '\n';
    return 0;
}    
```

Todas las macros que empiezan con el guion doble `__` son palabras
reservadas por el estándar.

- Las variables `__LINE__` y `__FILE__` representan la línea actual y el
  fichero que está siendo procesado.
- La variable `__DATE__` contiene la fecha actual, en el formato
  `mes/dia/año`.
- La variable `__TIME__` representa el tiempo actual, en el formato
  `hora:minutos:segundos`. Esta variable representa la hora en que el
  fichero fue compilado, no necesariamente el tiempo actual.
- La variable `__cplusplus` únicamente es definida cuando se compila un
  programa en C++. En algunos compiladores antiguos, esta variable es
  nombrada `c_plusplus`.
- La variable `__STDC__` es definida cuando se compila un programa en C,
  aunque, también puede ser definida cuando se compila con C++.

Los propios compiladores implementan sus propias palabras clave,
identificadores y macros específicas.

## Separadores

Los separadores están constituidos por uno o varios espacios en blanco,
tabuladores y caracteres de fin de línea. Su papel es ayudar al
compilador a descomponer el programa fuente en cada uno de los *tokens*
que lo forman. Por ejemplo:

``` C++
int main() { return0; };     // Error, return0 no fue declarado
int main() { return  0;}     // Bien, espacios extra
intmain()  { return  0;}     // Error, no hay espacio entre identificadores
int main(){return 0;}        // Bien, espacios mínimos    
int main( ) { return  0;}    // Bien, espacios extras   

int 
main() { return  0;}         // Bien, línea extra

main() { /*ok*/ return  0; } // Bien, un comentario en línea
```

Al compilador no le importan los espacios en blanco, los tabuladores, ni
las líneas extra. Únicamente los *tokens*. Si no puede reconocer un
identificador, una palabra clave o un signo de puntuación, detendrá el
proceso de compilación y mostrara un error de sintaxis.

### Espacio entre separadores

Es conveniente introducir espacios en blanco en el código, aun cuando no
sea estrictamente necesario, con objeto de mejorar la legibilidad del
código fuente. Por ejemplo esta función:

``` C++
bool es_vocal(const char c)
{   
    return ('a' == c) || ('e' == c) || ('i' == c) || ('o' == c) || 
           ('u' == c) || ('A' == c) || ('A' == c) || ('A' == c) ||
           ('A' == c) || ('A' == c);
}
```

es idéntica a esta otra, que ocupa menos líneas y no usa espacios.

``` C++
bool es_vocal(const char c) {
    return ('a' == c)||('e' == c)||('i' == c)||('o' == c)|| 
           ('u' == c)||('A' == c)||('A' == c)||('A' == c)||
           ('A' == c)||('A' == c);
}
```

o, a esta otra, que ocupa una línea extra (y sigue siendo válida).

``` C++
bool 
es_vocal(const char c)  
{
    return ('a' == c) || ('e' == c) || ('i' == c) || ('o' == c) || 
           ('u' == c) || ('A' == c) || ('A' == c) || ('A' == c) ||
           ('A' == c) || ('A' == c);
}
```

Para el compilador no hay ninguna diferencia entre estas tres funciones.
Cuando compila el programa todos los espacios en blanco, líneas en
blanco, saltos de línea, comentarios y demás información \"innecesaria\"
se elimina del programa.

### Espacios o tabuladores

Por lo general se usan dos o cuatro espacios para indentar el código,
también se pueden usar tabuladores, aunque no es muy común hacerlo. Lo
que no se debe hacer, es mezclar tabuladores y espacios.

``` C++
// Devuelve true si el caracter es un signo de puntuación
bool es_signo(const char c)
{   
    return ('+' == c) || ('-' == c);     // Usa 4 espacios
}
```

Los espacios en blanco son invisibles para el compilador, pero no para
el ojo humano. Los separadores ayudan a mejorar la legibilidad de un
programa y son útiles para indentar el código. Indentar el código es una
forma conveniente de agregar estructura a los bloques.

Existen un par de excepciones a la regla. Por ejemplo, las directivas
solo se pueden escribir en una sola línea, al igual que las macros.

``` C++
// Las directivas y macros solo pueden ocupar una línea 
#include <iostream>  
#define NULO '\0'

// Error: La directiva espera un nombre
#include 
    <string>

// Error: La macro no tiene un nombre
#define 
    NULO '\0'
```

Para usar varias líneas se usa la barra invertida al final de cada
línea. Cuando el compilador encuentra una barra inclinada concatena la
macro en una sola línea.

``` C++
// Bien: La diagonal concatena macros y texto
#define   \
    NULO '\0'
```

Las cadenas de texto literales de varias líneas pueden usar un salto de
línea, o la barra inclinada para usar varias líneas.

``` C++
// Una línea que usa comillas dobles
"Hola, mundo";

// Una línea que continua usando la barra inclinada
"Hola, \
mundo";

// Se pueden usar varias palabras separadas, entre comillas
"Hola, " "mun" "do";

// El carácter de fin de línea '\n' agrega un salto de línea
"Hola, \n"
"Mundo \n";
```

Las cadenas de texto siempre usan comillas dobles. No se pueden mezclar
comillas dobles y comillas simples en el código fuente. La barra
inclinada es un signo de puntuación especial, que permite concatenar
cadenas de texto y escapar caracteres.

## Signos de puntuación

Los signos de puntuación son caracteres especiales que tienen un
significado sintáctico y semántico para el compilador, pero que no hacen
ninguna tarea por si mismos. Algunos signos de puntuación pueden
aparecer solos o en combinación (o pares), otros signos solo son
significativos para el preprocesador.

Cualquiera de los siguientes símbolos se considera un signo de
puntuación:

    { ( %: . ^ . = != -= &= } ) %:%: + & .* == << += |= [ <: 
    ; - | -> < >> *= ^= ] :> : * ? ->* > <<= /= ++ # <% ... 
    / : ~ <= >>= %= -- ## %> , % :: ! >=

Algunos símbolos como la llaves, los paréntesis y los corchetes
`{}, ()`, `[]` siempre van en pares.

``` C++
int a;            // Las declaraciones terminan con punto y coma 
a = 50;           // Al igual que las asignaciones, 
int fun(){};      // las definiciones
++a;              // y las expresiones. 
```

En C++, el punto y coma se usa como separador de instrucciones. Las
expresiones, declaraciones y definiciones terminan con punto y coma,
mientras que las llaves definen bloques de código.

``` C++
{
    // Un bloque de código
} 
```

Cada bloque de código crea un ámbito léxico (local) en el que se pueden
declarar otros identificadores, aun con el mismo nombre que el declarado
afuera.

La coma sirve como separador de variables y como separador de elementos
en listas de inicialización.

``` C++
// Las tres variables se inicializan con el mismo valor. 
int a, b, c = 10;

// La coma separa elementos en listas de inicialización
int std::vector rango{0, 1, 2, 3, 4, 5, 6, 7, 8, 9};
```

Existen otros símbolos como los puntos dobles `::` que le dicen al
compilador, el espacio de nombres al que pertenece un objeto.

## Comentarios

Los comentarios son una de las partes importantes del código fuente, ya
que ayudan a entender lo que hace un programa. El objetivo de los
comentarios es servir de explicación o aclaración sobre cómo está hecho
un programa, de forma que pueda ser entendido por una persona diferente
o por el propio programador tiempo después.

### Tipos de comentarios

C++ soporta dos estilos de comentarios.

**Comentarios con doble barra**

- Cualquier fragmento de texto que inicie con dos barras inclinadas `//`
  es tratado como un comentario de una línea.

**Comentarios con barra y asterisco**

- Cualquier texto entre los caracteres `/*` y `*/` (incluyendo varias
  líneas) es tratado como un comentario.

Por convención los comentarios de una línea usan las dos barras y los
comentarios de varias líneas usan la barra y el asterisco, aunque se
pueden usar de forma opuesta.

### Comentarios de una línea

Para escribir comentarios de una línea, por lo general se utilizan dos
barras inclinadas `//` antes del comentario. Las barras inclinadas se
pueden colocar al inicio o (en cualquier parte) del lado derecho del
código.

Por ejemplo.

``` C++
// Si la letra es mayúscula c vale 65 si no 97
char c = std::isupper(letra) ? 'A' : 'a';    // A=65, a=97
```

La barra y el asterisco, pueden hacer el mismo trabajo:

``` C++
/* Si la letra es mayúscula c vale 65, si no 97 */
char c = std::isupper(letra) ? 'A' : 'a';  /* A=65, a=97 */
```

No hay ninguna regla que impida usar uno u otro estilo, aunque es más
común usar las barras dobles en comentarios de una línea y destinar la
barra y el asterisco para comentarios que abarquen varias líneas como la
documentación.

### Comentarios de varias líneas

Para escribir comentarios que abarquen varias líneas se utiliza por lo
general las marcas de inicio y cierre respectivamente: `/* */` (una
barra diagonal y un asterisco).

Cualquier sección de texto en medio `/*` de estas dos marcas `*/` es
ignorada completamente por el compilador.

``` C++
#include <cctype>                                 // isalpha, isdigit
#include <iostream>                               // cout
#include <string>                                 // string

/* 
* Cifra una cadena de texto usando dezplazamiento. 
* 
* Desplaza cada letra del mensaje por la letra que está trece 
* posiciones por delante en el alfabeto conservando las letras 
* mayúsculas y minúsculas. Los dígitos los dezplaza 5 posiciones. 
* Las letras que no son ASCII no se modifican. 
*
* @param {texto}   Una cadena de tipo string para (des)cifrar.
* @return {string} Devuelve una cadena de tipo string (des)cifrada.
*  
* @example:
*     std::cout << cifrar("C++ 26") << std::endl;  // P++ 71
*/
std::string cifrar(const std::string& texto) 
{
    // Una cadena para guardar el texto (des)cifrado
    std::string mensaje;

    // Comprueba cada letra del texto
    for (auto letra : texto) {
        // Si es una letra usa rot13        
        if (std::isalpha(letra)) {        
            auto c = std::isupper(letra) ? 'A' : 'a';
            letra = ((letra + 13 - c) % 26) + c;
        }
        // Si es un digito usa rot-5
        if (std::isdigit(letra)) {         
            int digito = (letra < '5') ? 5 : -5;
            letra = ((letra + digito) - letra) % 26 + letra;
        }
        // Agrega la letra al mensaje de salida
        mensaje += letra;              
    } 

    return mensaje;           
}

// Programa que (des)cifra un mensaje de texto
int main()
{
    std::string texto_plano{"La proxima versión de C++ es la 26"};
    std::string texto_cifrado = cifrar(texto_plano);

    // Imprime las cadenas de texto
    std::cout << "Texto cifrado : " << texto_cifrado         << '\n';
    std::cout << "Texto plano   : " << cifrar(texto_cifrado) << '\n';

    // Texto cifrado : Yn cebkvzn irefvóa qr P++ rf yn 71
    // Texto plano   : La proxima versión de C++ es la 26
    return EXIT_SUCCESS;
}
```

En este ejemplo, la función `cifrar()` contiene un comentario de varias
líneas que documenta lo que hace una función y la forma de usarla. Los
comentarios dentro de la función documentan fragmentos de código
especifico.

En los comentarios se puede incluir el tipo de parámetros que acepta la
función, así como el valor de retorno usando bloques de documentación.
Los bloques de documentación son muy comunes, ya que existen
analizadores que pueden extraer los comentarios de un programa para
crear documentación de forma automática.

#### Documenta tu código

Cuando escribas funciones y clases documéntalos. Documenta los
parámetros y los valores de retorno, así como las posibles excepciones
que pueda lanzar. Para ello puedes utilizar los llamados bloques de
documentación. Estos bloques son etiquetas que se colocan en los
comentarios de varias líneas, se distinguen de los comentarios por que
comienzan con un prefijo (`@` o `\`) y el nombre de alguna etiqueta, su
objetivo es documentar el programa o alguna pieza de código en
específico.

Algunas utilidades pueden crear documentación de los bloques de código,
de forma automática como `doxygen`.

  --------------------------------------------------------------------
  Etiqueta      Descripción
  ------------- ------------------------------------------------------
  \@author      El nombre del autor del código.

  \@brief       La descripción del código.

  \@param       Describe un parámetro de una función.

  \@tparam      Describe un parámetro de una plantilla.

  \@see         Se refiere a observar o consultar otra función.

  \@throw       Excepción que puede lanzar el código.

  \@version     El número de versión del código.
  --------------------------------------------------------------------

  : Etiquetas para documentación.

Los comentarios son siempre una buena idea. Por lo general es útil
incluirlos al inicio de un programa. El estilo exacto de cada bloque
puede variar, pero siempre es útil escribir al menos, la descripción
sobre lo que hace el programa y la forma en que se usa.

## Bloques de código

La categoría sintáctica elemental que define la estructura y el alcance
léxico del código es el bloque. Un bloque es un conjunto de
instrucciones encerradas entre llaves. Dentro de los bloques de código
se pueden colocar declaraciónes, expresiónes y sentencias:

``` C++
{
    declaración;
    declaración;
}
```

Un bloque puede colocarse en cualquier lugar donde pueda colocarse una
sentencia. Inclusive dentro de otros bloques, para crear bloques
anidados:

``` C++
{
    declaración;

    if(expresión) {
       declaración;
    }        
}
```

Cada bloque de código crea un alcance léxico o ámbito. Un **ámbito** es
un espacio, en el cual los identificadores introducidos por las
declaraciones se pueden usar. Todas las declaraciones contenidas dentro
de un ámbito son locales, mientras que las declaraciones definidas fuera
del ámbito, son globales para al bloque. Esto aplica también a los
bloques anidados.

Es posible crear bloques vacíos sin instrucciones. A esto se le conoce
como declaración nula (o declaración vacía). Para representar un
declaración nula, se coloca un punto y una coma (`;`) donde normalmente
iría una declaración (como en un bucle `for`).

``` C++
;   // una declaración vacía
```

Los bloques de código también se usan en sentencias, tales como `if`,
`else`, `while`, `for` para incluir el código que se debe ejecutar si se
cumple cierta condición:

``` C++
if (condición) {
    // Ejecutar este código
} else {
    // Ejecutar este código
}
```

Las sentencias y declaraciones contenidas dentro de bloques definidos
por llaves, únicamente se ejecutan cuando la expresión incluida dentro
de los paréntesis es verdadera o falsa (según se defina la expresión).
Los bloques pueden contener a su vez otros bloques de declaraciones y
sentencias anidadas.

Las llaves en bloques son opcionales cuando se usa una sola declaración,
el punto y coma no. Este fragmento de código es equivalente al anterior
(Observa la ausencia de llaves):

``` C++
if (condición)
    // Ejecutar este código
else 
    // Ejecutar este código
```

El problema con las declaraciones (de una línea y sin llaves), viene más
tarde cuando es necesario agregar otra declaración es necesario agregar
llaves. Por esa razón, algunos programadores acostumbran usar llaves en
todo momento, aun cuando sólo haya una declaración, otros no. En algunos
casos el compilador puede emitir una advertencia señalando el bloque de
código y la ausencia de llaves.

### ¿El estilo importa?

El tema sobre dónde poner llaves y la forma de usar los espacios en
blanco en un programa es un poco complicado debido a la gran cantidad de
estilos existentes. En C, como en C++, el indentado de código no es un
requisito del lenguaje. Sin embargo ayuda a mejorar la legibilidad y la
estructura de un programa. A continuación se enlistan algunos estilos
que han surgido a través de los años y que sirven como guía para
escribir (y formatear) el código fuente de un programa.

#### El estilo Allman

El estilo Allman nombrado en honor de Eric Allman, es también conocido
como estilo BSD o \"Estilo de llaves en una línea\". Allman escribió
muchas de las utilidades del sistema operativo BSD. En este estilo se
colocan las llaves en la línea siguiente y se identan en el mismo nivel
que la sentencia de control.

``` C++
// Llaves en línea separadas
tipo_devuelto nombre_funcion(args)
{
    for (condición)
    {
        if (otra condición)
        {
            // El código entre llavez
            // El código entre llavez
        }
        else
        {
            // Más código entre llavez
        }
    }

    valor_devuelto;
}
```

La alineación de las llaves con el bloque enfatiza la separación visual
y permite conceptual y programáticamente separar bloques de código.
Aunque esto hace el código más extenso. Una variante de este estilo es
llamada \"Estilo Whitesmiths\" que indenta las llaves de inicio del
bloque.

#### El estilo Kernighan y Ritchie

El estilo Kernighan & Ritchie abreviado como `K&R`, es también llamado
\"El estilo de llaves verdadero\". Este estilo fue usado para escribir
el kernel del sistema operativo Unix, también fue usado en los ejemplos
del libro \"El lenguaje de programación C\". Este estilo también es
conocido como `1TBS` (Abreviación de \"the one true brace style\").

``` C++
// Se omiten llaves y se colocan delante de la línea
tipo_devuelto nombre_funcion(args)
{
    for (condición) {
        if (otra condición) {
            // El código entre llavez
            // El código entre llavez
        } else
           // En una línea se omiten las llavez            
    }

    valor_devuelto;
}
```

En este estilo las llaves de inicio de bloque se colocan en líneas
únicas cuando son funciones, pero en estructuras de control se colocan
delante y se omiten siempre que haya oportunidad (como en sentencias y
declaraciones de una línea).

En este estilo de programación el código es más compacto y muchos
lenguajes de programación que usan llaves (Java por ejemplo) siguen un
estilo muy similar.

#### El estilo GNU

En este estilo las líneas se separan por llaves de inicio y de cierre
con un indentado de dos o cuatro espacios.

``` C++
// Estilo GNU 
tipo_devuelto nombre_funcion(args)
{
    for (condición)
      {
          if (otra condición) 
             {
               // El código entre llavez
               // El código entre llavez
             }
          else 
             {
               // Más código.
             } 
      }

    valor_devuelto;
}
```

Este estilo es muy parecido al estilo Allman y Whitesmiths, sin embargo
el estilo GNU usa llaves en una sola línea, indentado por dos espacios,
excepto cuando se abre una definición de función, donde no se identan,
En este caso, el código contenido es identando por dos espacios entre
las llaves.

Este estilo fue popularizado por Richard Stallman, el cual fue
influenciado por el lenguaje de programación Lisp.

#### El estilo Stroustrup

El estilo de Stroustrup es una variante del estilo `K&R`, que fue usado
en los libros \"Programación: Principios y Practicas usando C++\" y \"El
Lenguaje de programación `C++`. A diferencia de las variantes de una
sola línea y del estilo K&R, Stroustrup usa llaves después de una
sentencia, pero cada sentencia va en una línea separada:

``` C++
// Las llaves se colocan delante de la línea
tipo_devuelto nombre_funcion(args)
{
    for(condición) {
        if (otra condición) {
            // El código entre llavez
            // El código entre llavez
        }
        else {
            // Más código 
        } 
    }
    valor_devuelto;
}
```

Stroustrup extiende el estilo K&R a las clases:

``` C++
class Vector {
public:
    Vector(int s) : elem(new double[s]), sz(s) { } 
    double& operator[](int i) { return elem[i]; }  
    int size() { return sz; }
private:
    double * elem;    
    int sz;        
};
```

El estilo Stroustrup no indenta las etiquetas `public` y `private` y usa
las llaves después del nombre de la clase. También escribe las funciones
que son pequeñas en una misma línea y evita los espacios entre
operadores.

Existen otros estilos como Horstmann, Ratliff o el usado por el kernel
de Linux, pero la mayoría son variantes de una u otra forma de los
estilos Allman y K&R (heredados de C).

:::: tip
::: title
Tip
:::

**Identando el código automáticamente**

Existen programas libres como `indent` o `AStyle` diseñados
específicamente para indentar código automáticamente. Por ejemplo para
indentar un archivo `.cpp` al estilo Kernighan y Ritchie usando
`AStyle`:

``` console
$ AStyle --style=stroustrup codigo.cpp
```

Este programa reconoce algunos estilos de programación clásicos como
java, allman, bsd, kr, stroustrup etc. Además de que se puede adaptar a
un estilo en particular.
::::

Al final como dice Stroustrup, la posición de las llaves no es tan
importante. Escoge un estilo con el que te sientas cómodo y utilízalo de
forma consistente. Ya que la consistencia es la parte más importante de
un estilo, por otra parte es buena idea seguir el estilo que la mayoria
de programadores de C++ están acostumbrados a leer (o escribir).

## Valores literales

Un valor literal es un valor que puede aparecer directamente en un
programa. En C++ hay varios tipos de valores literales:

1.  Enteros literales
2.  Carácter literal
3.  Literales de punto flotante
4.  Literales de cadena de texto
5.  Literales booleanos
6.  Punteros literales
7.  Literales definidos por el usuario

El término \"literal\" es designado por el estándar para nombrar a los
*token\'s* llamados constantes. Los valores constantes no pueden cambiar
el valor que almacenan.

### Enteros literales

Un entero literal es una secuencia de dígitos que no tiene un punto o un
exponente.

``` C++
// Todos lo números representan al 2300
int num_decimal     = 2'300;
int num_octal       = 04374;
int num_hexadecimal = 0x8fc;
int num_binario     = 0b0000000000000000000100011111100;
```

Una secuencia de dígitos puede usar separadores definidos por comillas
simples como `2'300`. Las comillas son ignoradas cuando se determina el
valor. Un literal entero puede tener un prefijo que especifique su base
y un sufijo que especifique su tipo. El primer digito de la secuencia es
el más significante.

Hay cuatro tipos de literales enteros definidos por el estándar:

    +-------------------+
    | Literales Enteros |
    +-------+-----------+
            |
            |
    ^ - - - - -  ^ - - - - - - ^ - - - - - -  ^ 
    |            |             |              |
    |            |             |              |
    +----^----+   +---^---+  +------^------+  +----^----+
    | Binario |   | Octal |  | Hexadecimal |  | Decimal |
    +---------+   +-------+  +-------------+  +---------+

Donde:

1.  **Binario**: Un valor binario (en base 2) comienza con los prefijos
    `0b` o `0B` y consiste de una secuencia de dígitos binarios (0, 1).
2.  **Octal**: Un valor octal (en base 8) comienza con el `0`, y
    consiste de una secuencia de dígitos. Uno de: `0 1 2 3 4 5 6 7`.
3.  **Hexadecimal**: Un valor hexadecimal (en base 16) comienza con los
    prefijo `0x` o `0X`. Uno de:
    `0 1 2 3 4 5 6 7 8 9 a b c d e f A B C D E F`.
4.  **Decimal**: Un entero decimal (en base 10) comienza con un digito
    diferente del `0`, seguido por una secuencia de dígitos. Los valores
    literales pueden ser: sin signo: `u U`, con signo: `- +`, de tipo
    `long` con un valor literal: `l o L` y de tipo `long long` con un
    valor literal `ll o LL`.

Por ejemplo el numero 100 se puede escribir directamente en un programa
de esta forma:

``` C++
int cien = 0b1100100;          // 100 en binario
cien = 0144;                   // 100 en octal
cien = 100;                    // 100 en decimal
cien = 0x64;                   // 100 en Hexadecimal 
```

Los literales 100000 y 100\'000 tienen el mismo valor. Aunque un valor
use comillas y el otro no. Las comillas simples son ignoradas por el
compilador. Los valores enteros pueden usar un signo `+` o `-`, el signo
`+` puede ser omitido.

``` C++
100                // Igual que +100
+100               // Igual que  100 
-100               // Los valores negativos usan el signo -
```

El siguiente programa muestra la representación binaria, decimal, octal
y hexadecimal de un numero entero:

``` C++
#include <bitset>    // bitset
#include <limits>    // numeric_limits
#include <iostream>  // cout
#include <iomanip>   // oct, dec, hex

// Muestra las diferentes representaciones numéricas de un tipo entero.
int main()
{  
    int num{0};
    std::cout << "Introduce un valor númerico: ";
    std::cin >> num;

    std::cout << "Binario     : " << "0b"
              << std::bitset<std::numeric_limits<int>::digits>(num) << '\n'
              << std::oct << "Octal       : " << "0"  << num        << '\n'
              << std::dec << "Decimal     : "         << num        << '\n'
              << std::hex << "Hexadecimal : " << "0x" << num        << '\n';

    return EXIT_SUCCESS;
}
```

En este ejemplo, el trabajo de conversión lo hacen los manipuladores de
flujo declarados en la cabecera `<iomanip>` como `oct`, `dec`, `hex` que
muestran el valor octal, decimal y hexadecimal de una valor entero. Para
la representación en binario, se usa la cabecera `<bitset>`, que es una
plantilla que usa como entrada el límite de un tipo de dato (un entero).
La cabecera `<limits>` contiene los limites máximos y mínimos que pueden
usar de forma segura los tipos de datos básicos del lenguaje.

### Literal de punto flotante

Un literal de punto flotante consisten de una parte entera y un punto
decimal, o parte fraccionaria y opcionalmente un exponente definido por
la letra: `e` o `E` con o sin signo. La parte entera y la parte decimal
se forman por una secuencia de dígitos decimales separados
(opcionalmente) por comillas simples.

``` C++
double val1 = 1.342'432;      // Valor de tipo double
float val2  = 3.2599f;        // Valor de tipo float
```

Los literales de punto flotante representan tipos reales:

- Literales de punto flotante. Una secuencia de dígitos con punto o una
  secuencia de dígitos con punto y separados por comillas.
- Con exponente. Pueden usar la letra `e` o `E` con signo y sin signo:
  `+ -`.
- Tipo `float`. Pueden usar la letra: `f` o `F`.
- Tipo `long double` Pueden usar la letra: `l` o `L`.

Se puede usar la comilla simple para separar los digitos. Por ejemplo
los literales `1.342'432'456'1e-12` y `1.342432456'1e-12` representan el
mismo número. Hay una restricción para las comillas, ya que estas nunca
deben de ir antes del prefijo `e` o `E`.

``` C++
#include <iostream>       // cout

// Tipos decimales
int main() 
{
    float a = 3.14f;     // Literal de tipo float
    float b = 2.71F;     // Literal de tipo float

    double c = 3.14;     // Por defecto, es tipo double

    std::cout << "Valor de a: " << a << std::endl;
    std::cout << "Valor de b: " << b << std::endl;
    std::cout << "Valor de c: " << c << std::endl;

    return EXIT_SUCCESS;
}
```

Los literales `f` y `F` se utilizan para especificar que un número es
del tipo `float` en lugar de `double`. Por defecto, los números con
punto decimal se consideran de tipo `double`.

#### Usando notación científica

La **notación científica** es una forma de representar números en
potencias de 10 mediante el uso de la letra `e` o `E`. Es útil para
trabajar con números muy grandes o muy pequeños en cálculos científicos
o matemáticos. La notación científica en C++ toma el formato:

    número_base e exponente

Donde:

- La `base` es un número en punto flotante (como `1.23` o `5.67`).
- La letra `e` (o `E`) indica \"multiplicado por 10 elevado a\...\"
- El `exponente` es un entero que indica la potencia de 10.

Por ejemplo:

- `1.23e4` equivale a 1.23 veces 10\^4 = 12300.
- `5.67e-3` equivale a 5.67 veces 10\^{-3} = 0.00567.

Cuando se utiliza la notación científica directamente en literales
numéricos. C++ interpreta automáticamente estos valores como tipos
`double` por defecto.

``` C++
#include <iostream>          // cout

// Usando la notación cientifica
int main() 
{
    double num1 = 1.23e4;    // 1.23 * 10^4 = 12300
    double num2 = 5.67e-3;   // 5.67 * 10^-3 = 0.00567

    std::cout << "num1: " << num1 << std::endl;
    std::cout << "num2: " << num2 << std::endl;

    return EXIT_SUCCESS;
}
```

Los literales en notación científica se manejan como `float` (si se usa
`f` al final) o `double` (por defecto).

Se puede usar la biblioteca `<iomanip>` para activar la notación
cientifica, usando el manipulador de salida `scientific`:

``` C++
#include <iostream>        // cout, endl
#include <iomanip>         // scientific 

// Activando la notación cientifica
int main() 
{
    double num = 12345.6789;

    std::cout << "Valor normal: " << num << std::endl;
    std::cout << "Valor en notación científica: "
              << std::scientific << num << std::endl;

    return EXIT_SUCCESS;
}
```

Esta notación es util para trabajar con valores científicos como
constantes físicas, por ejemplo `6.022e23`, el número de Avogadro. Medir
valores pequeños como `1.0e-9`, útil para nano y micro operaciones.

### Carácteres literales

Un carácter literal es una secuencia de caracteres encerrados en
comillas simples como `'x'`, precedidos opcionalmente por una de las
siguientes letras `u`, `U`, o `L`, por ejemplo: `u'y'`, `U'z'`, o
`L'x'`. Un carácter literal que no comienza con un prefijo es un
carácter literal ordinario (`char`) mientras que un carácter que
contiene un sufijo es un carácter extendido o multi-caracter.

- **Carácter:** puede ser un tipo ordinario como `'a'`, usar un prefijo
  como `u U L` o un Multi-carácter como `L'\x0438'` que representa un
  tipo `wchar_t`
- **Secuencias de escape:** uno de estos simbolos:
  `\' \" \? \\ \a \b \f \n \r \t \v`

Algunos caracteres no gráficos como la comilla simple `'`, las comillas
dobles `""`, el signo de interrogación `?` y la barra `\` solo pueden
ser representadas escapando el carácter con un barra inclinada.

    ' '            // El espacio en blanco
    'a'            // La letra a 
    '\t'           // El tabulador (un carácter escapado)
    '\u00A9'       // Un carácter Unicode 

Una secuencia de escape es útil cuando es necesario incluir un símbolo
especial en la salida. El caso más común es el carácter que define el
fin de línea: `'\n'`.

### Literales de cadena de texto

Una cadena de texto literal, es una secuencia de caracteres encerrada
entre comillas dobles, que opcionalmente puede tener un prefijo:
`R, u8, u8R,` o, `uR, U, UR, L`, o `LR`.

Los literales de cadena pueden ser:

- Cadenas de texto encerrada entre comillas dobles.
- Cadenas sin procesar con el prefijo `R`.
- Cadenas de caracteres como secuencias de escape y secuencias Unicode.
- Caracteres: un carácter entre comillas doble o una secuencia de escape
  entre comillas dobles.
- Cadenas con prefijo y con codificación: `u8, U, L`

Una cadena de texto literal siempre va dentro de comillas dobles.

``` C++
"Una cadena de texto"  // Una cadena de texto literal
"n"                    // Un carácter entre comillas dobles
"\t"                   // Una secuencia de escape entre comillas dobles
"\u03c0"               // Una secuencia Unicode    
u8"Año"                // Una cadena codificada como UTF8
R"(Hola)"              // Una cadena no procesada  
LR"(Hola)"             // Una cadena no procesada con prefijo     
uR"(Hola)"             // Una cadena no procesada Unicode     
```

Una cadena literal que comienza con `u8` como `u8"España"` es un literal
inicializado como texto con codificación `UTF-8`. Las cadenas que
inician con `u` se convierten a `char16_t`. Las que inician con `L` en
`wchar_t`, y las que inician con `U` a `char32_t`. Una cadena que
comienza con un prefijo `R` es una cadena literal no procesada.

``` C++
std::cout << R"(Una cadena literal no procesada)";
```

Una cadena no procesada usa como delimitador los paréntesis y usa
comillas dobles después de la `R`. En una cadena sin procesar no es
necesario definir el final de línea de forma explícita y los caracteres
que usan la barra inclinada no son escapados.

### Literales booleanos

Hay únicamente dos valores boléanos: verdadero y falso definidos por las
palabras clave:

``` C++
false  // falso =  0
true   // verdadero = 1
```

Estos valores literales pertenecen al tipo llamado `bool` y representan
el valor de 0 y 1 respectivamente.

### Puntero literal

Un literal nulo puede ser definido usando una palabra clave especial
llamada `nullptr`.

``` C++
int *ptr = nullptr;
```

Este literal tiene un valor nulo y su utilidad radica en que se puede
usar para definir el valor de un puntero nulo en lugar de usar la macro
`NULL` o un valor 0.

### Literales definidos por el usuario

Se pueden crear valores literales a medida agregando un sufijo a un tipo
de dato, usando la palabra clave `operator`, seguido por comillas dobles
antes del sufijo y después del tipo. Por ejemplo:

``` C++
// Literal para usar pulgadas: 1 in = 2.54 cms
constexpr long double operator"" _in(long double centimetros)  
{
    return centimetros * 2.54;
}
```

En este ejemplo la palabra clave `operator` se usa como si fuera una
función, de esta forma el valor del literal es pasado directamente al
objeto función que realiza una operación de conversión. La palabra clave
`constexpr` denota una expresión constante que puede ser calculada en
tiempo de compilación. El valor literal se puede usar de esta forma:

``` C++
std::cout << 3.0_in;    // 7.62 cms
```

En este caso 3 pulgadas equivales a 7.62 centímetros. Podemos usar
valores literales para darle significado a valores constantes. Por
ejemplo, medidas:

``` C++
#include <iostream>      // cout

// Literal para usar metros: 1.70_mts = 170 cms
constexpr long double operator"" _mts(long double metros)  
{
    return metros * 100;
}

// Literal para usar kilogramos: 70.0_kgs = 70 kgs
constexpr long double operator"" _kgs(long double kilos)  
{
    return kilos;
}

// Literal para usar pulgadas: 1.0_in = 2.54 cms
constexpr long double operator"" _in(long double centímetros)  
{
    return centímetros * 2.54;
}

/*
* Calcula el peso ideal de una persona usando las siguientes
* formulas relacionadas con la estatura:
*
* Para la mujer:
*    Peso ideal = 45 kilos + 2.5 kilos por cada pulgada que 
*    exeda de los 1.52 metros.
*
* Para el hombre:
*    Peso ideal = 45 kilos + 2.25 kilos por cada pulgada que 
*    exeda de los 1.52 metros.
*/
int main()
{  
    double estatura;             // La estatura
    std::cout << "Introduce tu estatura [0, 2.50]: ";
    std::cin >> estatura;

    // Comprueba que la estatura este en el rango permitido
    if (estatura < 0 || estatura > 2.10) {    
        std::cout << "Estatura fuera de rango [0, 2.50]";
        return EXIT_FAILURE;
    }

    char genero;                  // El género: Hombre o Mujer
    std::cout << "Genero: ['H' Hombre, 'M' Mujer]: ";    
    std::cin >> genero;
    double peso_ideal;            // El peso ideal
    estatura *= 100;              // Convierte la altura a cms     

    // Calcula el peso ideal basado en la estatura y el genero
    if (genero == 'H') {
        peso_ideal = (estatura - 1.52_mts) / 1.0_in * 2.5_kgs + 45.0_kgs;
    } else if (genero == 'M') {
       peso_ideal = (estatura - 1.52_mts) / 1.0_in * 2.25_kgs + 45.0_kgs; 
    } else {
        std::cout << "No se puede calcular el peso ideal\n";
        return EXIT_FAILURE;
    } 

    // Muestra los resultados     
    std::cout << "Estatura   : " << estatura << " cms.\n"
              << "Genero     : " 
              << (genero == 'H' ? "Masculino": "Femenino")    
              << "\nPeso ideal : " << peso_ideal << " kgs.\n";

    return EXIT_SUCCESS;
}
```

Hay un par de restricciones al crear literales: los literales numéricos
deben usar el tipo `long double`. Por ejemplo `1.0_in`, en lugar de
`1_in`. No se pueden usar letras al inicio como: `kgs` porque esta
notación esta reservadas por el estándar, lo común es usar el guion bajo
al inicio: `_kgs`.

Algunas bibliotecas estándar como `chrono` usan valores literales para
denotar valores específicos como segundos, minutos, horas etc.

``` C++
using namespace std::literals;
std::chrono::duration minuto = 60s;
std::cout << minuto + 30s;            // 90s
```

Estos valores literales están definidos en el espacio de nombres
`std::literals`.
