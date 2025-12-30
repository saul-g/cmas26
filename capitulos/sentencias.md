# Sentencias

::: {.meta title="Tipos de sentencias en C++" capitulo="6" description="Controlando el flujo de ejecución de un programa." keywords="sentencias, estructuras control, selección, iteración, saltos"}
:::

Básicamente los programas son bloques de instrucciones que se ejecutan
en un orden especifico. Por lo general, una instrucción después de otra.
Las sentencias permiten alterar el orden secuencial de las instrucciones
y permiten ramificar los bloques de código evaluando alternativas o
ejecutando repetidamente un fragmento de código.

Las sentencias son la columna vertebral de la programación, ya que se
encargan de manejar y manipular directamente la estructura y ejecución
lógica del código. A las sentencias también se les conoce como
estructuras de control, ya que permiten tomar decisiones lógicas dentro
de un programa. Las decisiones pueden ser ciertas o falsas o pueden
basarse en repeticiones; repetir una tarea hasta que se cumpla una
condición o pueden hacer que el código salte de una tarea a otra.

El objetivo de una sentencia es modificar el flujo secuencial y el
comportamiento lineal de un programa. Este capítulo presenta las
sentencias y la forma de convertir en \"inteligentes\" a los bloques de
código.

## El flujo de ejecución de un programa

En C++, una sentencia es cualquier expresión que termine en punto y
coma, esto incluye cualquier bloque de instrucciones encerrado entre
llaves.

``` C++
// una sentencia
sentencia;   
```

Un bloque de instrucciones es considerado como una sola sentencia, sin
importar el número de instrucciones que agrupe.

``` C++
// Las sentencias dentro de un bloque son ejecutadas 
// en el mismo orden en que aparecen en el código.    
{
    sentencia_1;
    sentencia_2;
    sentencia_3;
    {
        sentencia_n;   
    } 
} 
```

La función de los bloques es permitir que grupos de sentencias sean
tratados como si fueran una sola sentencia. Los bloques también ayudan a
restringir el ámbito léxico de las variables.

### La ejecución secuencial de un programa

Básicamente los programas son bloques de código que se ejecutan en el
orden en que fueron escritos. Uno después de otro. En C++ la ejecución
secuencial de un programa inicia con la primera instrucción dentro de la
función `main()`.

``` C++
#include <iostream>           // cout
#include <fstream>            // ofstream

// Crea una imagen svg que muestra un mensaje de texto
int main() 
{
    // Abre un archivo de texto para escribir en él.
    std::ofstream svg("texto.svg");
    if (!svg) {
        std::cerr << "No se pudo crear el archivo SVG." << std::endl;
        return EXIT_FAILURE;
    }

    // Las medidas del svg
    const int width  = 400;
    const int height = 400;

    // Le agrega la cabecera y los datos al svg (un mensaje de texto)
    // Las comillas deben escaparse con la barra inclinada.
    svg << "<svg width=\"" 
        << width  << "\" height=\"" 
        << height << "\" xmlns=\"http://www.w3.org/2000/svg\">\n"
        << "<g style=\"font-size: 50pt; fill: white; stroke: black\">\n"
        << "<text x=\"20\" y=\"150\">¡Hola mundo!</text>\n"
        << "</g>\n";           

    // Cierra el svg y el archivo
    svg << "</svg>";
    svg.close();
    std::cout << "Archivo SVG generado exitosamente." << std::endl;

    return EXIT_SUCCESS;
}
```

En este ejemplo, el programa inicia creando un archivo `svg`, si el
archivo no se puede abrir o no se puede crear por cualquier motivo,
muestra un mensaje de error y el programa termina. La sentencia `if` se
encarga de tomar esta decision:

``` C++
if (!svg) {
    // No se pudo abrir el archivo
}
```

Si el programa puede abrir el archivo entonces escribe los datos
siguiendo el flujo secuencial del programa que inicia escribiendo la
cabecera del archivo svg, luego se agregan las etiqueta que aplican el
estilo y un mensaje de texto. Una vez que el programa termina de usar el
archivo, lo cierra.

En este caso, el programa crea una imagen `svg` que puede abrirse con
cualquier navegador y la imagen muestra el mensaje de texto \"¡Hola
mundo!\".

:::: tip
::: title
Tip
:::

Las sentencias y los bloques de sentencias se ejecutan en el orden en
que fueron escritas. La función `main()` termina cuando el flujo de
control evalúa la palabra clave `return`. El cuerpo de una función
también es una sentencia.
::::

Las sentencias especifican y controlan el flujo de ejecución de un
bloque de código. Si en el bloque no hay sentencias de selección o de
salto, el bloque se ejecuta de forma secuencial siguiendo el orden
\"natural\" del código (de arriba, hacia abajo). Una vez que todas las
sentencias se han evaluado y el flujo alcanza el final de ejecución, el
programa termina.

### Bloques inteligentes

Una estructura de control secuencial se compone de instrucciones que se
ejecutan de forma consecutiva, una tras otra, siguiendo una línea de
flujo. Sin embargo, la mayoría de los programas no se limitan a
secuencias lineales. Durante su ejecución, un programa puede repetir
segmentos de código, tomar decisiones y bifurcaciones. Para realizar
estas tareas se usan las sentencias de control que sirven para
especificar lo que tiene que hacer el código, cuando lo tiene que hacer
y bajo qué circunstancias lo puede hacer.

El siguiente ejemplo muestra cómo controlar el flujo de ejecución de un
programa usando un bucle `do while` para que el programa interactúe con
el usuario directamente.

``` C++
#include <iostream>                 // cout
#include <chrono>                   // chrono
#include <random>                   // mt19937 
#include <string_view>              // string_view  

// Programa que implementa el juego piedra, papel o tijeras
int main()
{
    std::cout << "\n\tPiedra, Papel o Tijeras\n\n";
    constexpr size_t num_juegos{50};  // El valor máximo de juegos

    // Inicializa el generador de números aleatorios
    static std::mt19937 rng;
    rng.seed(std::chrono::high_resolution_clock::now()
             .time_since_epoch().count());

    // Genera un numero aleatorio entre 1 y 3
    std::uniform_int_distribution<int> numero(1, 3);

    // Función lambda que muestra la opción seleccionada y un mensaje
    auto seleccionar = [](int opcion, std::string_view texto)
    {
        std::cout << texto;
        switch (opcion) {
            case 1: std::cout << "Papel   \n"; break;
            case 2: std::cout << "Tijeras \n"; break;
            case 3: std::cout << "Piedra  \n"; break;
        }
    };

    // El bucle do while se ejecuta hasta que se alcance el número de juegos
    std::size_t juegos = 1;            // Número de juegos 
    do {
        std::cout << "¿Cuantos juegos quieres jugar? (1, 50): ";
        std::cin >> juegos;
    } while (juegos < 1 || juegos > num_juegos);

    // Los contadores para la estadísticas del juego
    std::size_t perdidos{0};
    std::size_t ganados{0};

    // El bucle for se encarga de la lógica del juego
    for (std::size_t i = 1; i <= juegos; ++i) {
        std::cout << "\nNúmero de juego: " << i << '\n';

        // El jugador selecciona una opción
        std::size_t opcion {0};
        while (true) {
            std::cout << "Papel=1   Tijeras=2   Piedra=3 \n"
                      << "Selecciona una opción (1, 2, 3): ";
            std::cin >> opcion;
            if (opcion != 1 && opcion != 2 && opcion != 3) {
                std::cout << "Opción no valida.\n";
            } else {
                seleccionar(opcion, "Tu opción.......");
                break;
            }
        }

        // La cpu selecciona una opción
        std::size_t cpu = numero(rng);
        seleccionar(cpu, "Opcion de cpu....");

        // Determina el ganador e incrementa los contadores
        if (cpu == opcion) {
            std::cout << "Empate. Nadie gana.\n";
        } else if ((cpu > opcion && (opcion != 1 || cpu != 3))
                || (cpu == 1 && opcion == 3)) {
            std::cout << "¡Cpu gano!\n";
            ++perdidos;
        } else {
            std::cout << "¡Tu ganaste!\n";
            ++ganados;
        }
    }

    // Si el juego termina, muestra las estadísticas del juego
    int empates = juegos - (perdidos + ganados);
    std::cout << "\nEstadísticas:\n"
              << "Cpu gano......... " << perdidos << " Juego(s).\n"
              << "Humano gano...... " << ganados  << " Juego(s).\n"
              << "Empates.......... " << empates  << " Juego(s).\n" 
              << "Total de juegos.  " << juegos   << " Juego(s).\n" 
              << "¡Gracias por jugar!\n";

    return EXIT_SUCCESS;
}
```

Una declaración es una sentencia y una expresión se convierte en una
sentencia cuando termina en punto y coma. Pero a diferencia de una
expresión, una sentencia de control no tiene un valor. Ya que las
sentencias de control se usan para especificar el orden de ejecución del
código.

:::: note
::: title
Note
:::

Una expresión que asigna un valor es una sentencia de asignación.
Cualquier expresión que termine en punto y coma `;` forma una sentencia.
C++ ejecuta las sentencias evaluando la expresión. Todas las
declaraciones se evalúan cuando el flujo de control pasa por la
declaración, con excepción de los objetos declarados como `static` que
solo se inicializan la primera vez.
::::

Una forma de hacer \"inteligentes\" a los bloques de código es alterando
su comportamiento predeterminado de ejecución. Por ejemplo, una
sentencia `do-while` puede impedir que el orden de ejecución secuencial
termine el programa a menos que se cumpla alguna condición.

``` C++
// Número de juegos para determinar a un ganador
std::size_t juegos = 1;
do {
    std::cout << "¿Cuantos juegos quieres jugar? (1, 50): ";
    std::cin >> juegos;
} while (juegos < 1 || juegos > num_maximo_juegos);
```

mientras que la sentencia `switch` se utiliza para seleccionar una
opción de un conjunto de opciones:

``` C++
switch (opcion) {
    case 1: std::cout << "Papel   \n"; break;
    case 2: std::cout << "Tijeras \n"; break;
    case 3: std::cout << "Piedra  \n"; break;
}
```

Hay otros tipos de sentencias, como `if-else` que se hacen que el
programa ejecute una acción u otra.

## Tipos de sentencias

Las sentencias de control se dividen básicamente en tres categorías:

1.  **Condicionales**: son sentencias que hacen que un programa ejecute
    o ignore una declaración dependiendo del valor de una expresión. Por
    ejemplo: la sentencias `if-else` o la sentencia `switch`. Las
    estructuras condicionales se utilizan para indicar que se debe
    evaluar una condición y a partir del resultado ejecutar el bloque de
    declaraciones correspondiente. Los bloques se delimitan con llaves
    `{ }`, sin embargo la sintaxis permite omitir las llaves cuando hay
    una sola declaración.
2.  **Cíclicas** son sentencias que se ejecutan en forma de bucles (o
    ciclos). Los bucles ejecutan repetidamente una declaración en base a
    ciertas condiciones iníciales. Por ejemplo, el bucle `for` espera un
    valor inicial, una condición o un valor de asignación, mientras el
    bucle `while` y `do while` evalúan una expresión condicional. A
    estas estructuras también se le conoce como iterativas o de
    repetición ya que permiten ejecutar una o varias declaraciones, un
    número determinado de veces o indefinidamente mientras se cumpla la
    condición.

- **Saltos** son declaraciones que terminan un programa, como la
  sentencia `return` o saltan de una parte del programa a otra, como las
  sentencias `break` y `continue`. Estas sentencias permiten terminar un
  bucle, o una función.

El lenguaje permite usar sentencias en todos los lugares en donde se
puede usar una declaración. Según Stroustrup, la razón para permitir
esto, es para minimizar los errores causados por variables no
inicializadas. Posponer la definición de una variable hasta que esté
disponible un inicializador es una forma sencilla de tomar decisiones.

``` C++
int num;
std::cout << "Introduce un número: ";
std::cin >> num;
```

La razón más frecuente para declarar una variable sin un inicializador
es que la variable requiera de una sentencia para inicializar algún
valor.

### Programación estructurada

Las bases de la programación estructurada fueron llevadas a la práctica
por Niklaus Wirdth -inventor de Pascal, Modula-2 y Oberon-. Según Wirdth
cualquier problema algorítmico puede resolverse con el uso de tres tipos
de instrucciones:

- **Secuenciales**: Instrucciones que se ejecutan en orden lineal. El
  flujo del programa ejecuta una instrucción tras otra.
- **Alternativas**: Instrucciones en las que se evalúa una condición y
  dependiendo si el resultado es verdadero o no, el flujo del programa
  se dirige a una instrucción diferente.
- **Iterativas**. Instrucciones que se repiten continuamente hasta que
  se cumple una determinada condición.

Tres lenguajes de programación clásicos, Fortran, Pascal y C,
representan el arquetipo de la programación estructurada.

### Tipos de sentencias según C++

El estándar clasifica las sentencias en diferentes categorias.

``` C++
/
| de etiqueta
| de expresión
| compuestas
| de selección
Sentencias  <  de iteración
| saltos
| de declaración
| bloques de excepciones
| bloques sincronizados y atómicos
\
```

Las sentencias más comunes son las sentencias de selección `if` que
permiten evaluar alternativas.

## Evaluando alternativas

Las estructuras de selección, son sentencias condicionales que se
utilizan para indicar que se debe evaluar una condición y a partir del
resultado ejecutar el bloque de declaraciones correspondiente. Esto se
puede hacer usando las palabras clave `if` y `switch`. Una sentencia
`if` puede ir sola, o en compañía de la palabra clave `else`. Mientras
que una sentencia `switch` debe ser usada en conjunto con las palabras
clave `case` y `break`.

### La sentencia if

La sentencia `if` es una estructura de control que permite tomar
decisiones, o por decirlo de otra forma, permite ejecutar o saltar una
acción. Esta palabra clave se utiliza sola, o en compañía de la palabra
clave `else`.

:

    (Inicio)
       |
       v
    +---- false ---(expresión)--- true ----+
    |                                      |
    |                                      v
    No hace nada                         +-----+----+
    |                                |  Bloque  |
    |                                +-----+----+
    |                                      |
    v                                      v 
    +----------------->( )<----------------+
       |
       v
     (Fin)

La sentencia `if` ejecuta una acción sólo cuando la condición evaluada
es verdadera, de lo contrario el flujo de ejecución lo salta. Esta es su
sintaxis:

``` C++
if (expresión == true) {
    // Ejecuta este bloque;
}
```

De esta forma se evalúa la expresión (entre paréntesis), si el valor
resultante es verdadero, el programa ejecuta la declaración contenida
dentro de las llaves. Si la expresión resultante es falsa, no hace nada.
Por ejemplo, podemos saber si un número es primo usando varias
condiciones `if`:

``` C++
#include <iostream>                    // cout

// Función que devuelve true si un número es primo
bool es_primo(uint64_t num) noexcept;

// Programa que comprueba si el número introducido por el usuario es primo.
int main()
{
    int64_t num;
    std::cout << "Introduce un número positivo: ";
    std::cin >> num;

    if (num < 0) {
        std::cout << "¡El número introducido es negativo!";
        return EXIT_FAILURE;
    }

    std::cout << num << (es_primo(num) ? "" : " no" ) << " es primo\n";

    return EXIT_SUCCESS;
}

// Función que devuelve true si un número es primo
bool es_primo(uint64_t num) noexcept
{
    // Si el número es menor que 2 no es primo.
    if (num < 2) {          
        return false;
    }

    // Si el número es divisible entre 2 no es primo, a menos que sea 2. 
    if (num % 2 == 0) {
        return num == 2;
    }

    // Si el número es divisible entre 3 no es primo, a menos que sea 3.
    if (num % 3 == 0) {
        return num == 3;
    }

    // Comprueba si el número es divisible usando factores primos
    uint64_t primo{5};    
    while (primo * primo < num) {
        if (num % primo == 0) {
            return false;
        }
        primo += 2;
        if (num % primo == 0) {
            return false;
        }
        primo += 4;
    }

    return true;
}
```

Cualquier valor diferente a cero se considera como verdadero, así que se
puede omitir la comparación si el valor puede ser evaluado directamente
como verdadero o falso.

``` C++
bool valor{true};

if (valor == true) {
    // Se ejecuta este bloque;
}
```

Es equivalente a este código:

``` C++
if (valor) {
    // Se ejecuta este bloque;
}
```

Una buena razón para usar esta opción, es evitar errores difíciles de
detectar como este:

``` C++
if (valor = true) {
    // Asigna el valor true a valor
}
```

Al usar el operador de asignación `=` en lugar del operador de
comparación de igualdadad `==`, en vez de comprobar la expresión se le
asigna un valor a la variable. Este tipo de error no es detectado por el
compilador, por que es legal la sintaxis. Pero este si:

``` C++
if (true = valor) {        
    // Error de compilación
}
```

Esto es un error de sintaxis, por que no se le puede asignar un valor a
una palabra clave.

### Encadenando condiciones

La sintaxis para la condición `if`, requiere de una expresión entre
paréntesis, pero se pueden utilizar los operadores relacionales para
combinar y encadenar varias expresiones en una misma evaluación, por
ejemplo:

``` C++
int num{0};
std::cout << "Introduce un número entero: ";
std::cin >> num;

// Operador relacional AND 
if (num % 2 == 0 && num % 3 == 0) {
    std::cout << num << " es divisible por 2 y 3.\n";
}

// Operador relacional OR 
if (num % 2 == 0 || num % 3 == 0) {
    std::cout << num << " es divisible por 2 o 3.\n";
}

// Expresión relacional AND
if ((num % 2 == 0 || num % 3 == 0) && !(num % 2 == 0 && num % 3 == 0)) {
    std::cout << num << " es divisible por 2 o 3, pero no por ambos.\n";
}
```

La evaluación por corto circuito siempre devuelve un valor boléano. A sí
que es seguro evaluar varias condiciones en una misma expresión, usando
paréntesis si las reglas de precedencia o la evaluación asi lo requiere.

### Sentencia if con inicializador

Para evitar el uso incidental de una variable sin inicializar, una
práctica común es introducir la variable en el ámbito más pequeño
posible. Por lo general se suele retrasar la definición de una variable
local hasta que se le puede asignar un valor inicial. De esta forma se
evita el conocido problema de usar una variable sin un valor inicial.
Una forma elegante de hacer esto es declarar una variable dentro de una
condición.

``` C++
#include <iostream>                      // cout

// La sentencia if con un valor inicializador
int main()
{
    int num;
    std::cout << "Introduce un número entero: ";
    std::cin >> num;

    // Asigna un valor y comprueba la condición 
    // El valor debe ser mayor que cero.
    if (int val = 0; num > val) {
        std::cout << num << std::endl;
    }

    return EXIT_SUCCESS;
}
```

En este ejemplo la variable `val` es declarada e inicializada y su valor
es comprobado como condición. El ámbito de `val` se extiende desde el
punto en que se declara, hasta el final de la sentencia controlada por
la condición. Esto significa que la variable `val` solo existe dentro
del bloque `if`.

A partir de C++ 20, la sintaxis permite usar dos expresiones dentro de
los paréntesis. Una expresión inicializadora y una condición:

``` C++
if(T x = valor; condición(x)) {
    // Ejecuta este bloque, solo si la condición es verdadera.
    // x es válido solo en este bloque.
} 
```

De esta forma se puede declarar una variable dentro de los paréntesis y
se puede comprobar al mismo tiempo. Esto significa también, que la
variable declarada es visible únicamente dentro del bloque.

Para comprobar más de una condición se puede usar el operador coma junto
con los operadores relacionales:

``` C++
// La sentencia if con dos valores inicializadores
// El valor debe ser mayor que cero y menor que 100.
if (int val1 = 0, val2 = 100; num > val1 && num < val2) {
    std::cout << num;        
}
```

En este caso las variables, `val1` y `val2` solo existen dentro del
bloque `if` (entre llaves).

:::: note
::: title
Note
:::

C++, tuvo muchas fuentes de inspiración; una de ellas fue Algol 68 del
que adoptó el concepto de sobrecarga de operadores y la libertad de
colocar una declaración en cualquier lugar en el que pueda aparecer una
sentencia.
::::

La sentencia `if` es una estructura de control de una sola salida, ya
que ejecuta un bloque o lo salta. Pero usada en conjunto con la palabra
clave `else` permite seguir dos líneas de control.

### La condicional else

La segunda forma de una declaración `if` introduce una cláusula `else`
que se ejecuta cuando la expresión evaluada es falsa. Esta es su
sintaxis:

``` C++
if (expresion_inicial; expresión_condicional) {
    declaración1;
} else {
    declaración2;
}
```

Este tipo de estructura, ejecuta la `declaración1` si y solo si, la
expresión es verdadera y ejecuta la `declaración2` solo si la expresión
condicional es falsa.

::: parsed-literal

(Inicio)

:   | 

    v

+\-\-\-- false \-\-\-\--(expresión)\-\-\-\-- true \-\-\--+ \| \| v v
+\-\-\--+\-\-\-\--+ +\-\-\--+\-\-\--+ \| Bloque \| \| Bloque \|
+\-\-\--+\-\-\-\--+ +\-\-\--+\-\-\--+ \| \|
+\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\--\>(
)\<\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\--+ \| v (Fin)
:::

En este tipo de sentencias, siempre se ejecuta uno u otro bloque. Nunca
se ejecutan los dos (o ninguno).

``` C++
// Comprueba si el número es primo
if (es_primo(num)) {
    std::cout << num << " es primo";
} else {
    std::cout << num << " no es primo";
}
```

La sentencia `if else` es muy parecida al operador condicional (`? :`):

``` C++
// El primer operando es la condición, el segundo operando es el valor
// de la expresión, si la condición es verdadera, y el tercer operando 
// es el valor de la expresión si la condición es falsa.
std::cout << num << (es_primo(num) ? "" : " no" ) << " es primo\n"; 
```

Aunque son muy parecidas, el operador condicional es una expresión,
mientras que el bloque `if else` es una sentencia. Una expresión puede
usarse dentro de una sentencia, pero una sentencia no puede usarse
dentro de una expresión.

### La sentencia else if

La tercera forma de una sentencia `if` involucra las palabra claves
`else` `if` y opcionalmente una sentencia `else` que se ejecuta si todas
las demás expresiones son falsas. Esta es su sintaxis:

``` C++
if (expresión_inicial; expresión_condicional) {
    declaración1;
} else if (expresión 1) {
    declaración2;
} else if (expresión 2) {
    declaración3;
} 
```

De esta forma la expresión es evaluada en cada bloque condicional, si la
expresión es verdadera se ejecuta el bloque y termina la evaluación. No
importa que aun queden otros bloques. No hay un límite en cuanto al
número de cláusulas que se puedan utilizar.

La sentencia `else if` puede usar una clausula `else`:

``` C++
if (expresión_inicial; expresión_condicional) {
    declaración1;
} else if (expresión 1) {
    declaración2;
} else {
    declaración3;
}
```

Si todas las condiciones son falsas se ejecuta el bloque `else`. De esta
forma, siempre se ejecuta al menos un bloque de código.

:

    (Inicio)
       |
       v                      
    +---- false ---(expresión)--- true -------+
    |                                         |
    |        Primera condición if             v
    v                                   +-----+----+
    +--- false --(expresión)--- true ----+                |  bloque  |
    |                                    |                +-----+----+
    v    Segunda condición else if       v                      |
    +-----+----+                         +-----+----+                 |
    |  bloque  |                         |  Bloque  |                 |
    +-----+----+                         +-----+----+                 |
    |                                    |                      |
    v                                    |                      |
    (expresión)                               |                      |
    |                                    |                      |
    v                                    |                      |
    +----+-----+    Ultima condición else     |                      v
    |  bloque  |---------------------------->( )<--------------------+
    +----------+                              |
       v
     (Fin)

Las sentencia `else if` evalúa cada expresión por separado:

``` C++
#include <iostream>  // cout

// Programa que calcula el índice de masa corporal de una persona
int main()
{
    std::cout << "Calcula el índice de masa corporal\n";

    double estatura;
    std::cout << "¿Estatura en metros?: ";
    std::cin >> estatura;
    if (estatura < 1 || estatura > 2.50) {
        std::cout << "Estatura fuera de rango [1.0 - 2.50]";
        return EXIT_FAILURE;
    }

    double peso;
    std::cout << "¿Peso en kilogramos?: ";
    std::cin >> peso;
    if (peso < 1 || peso > 300) {
        std::cout << "Peso fuera de rango [1 - 300]";
        return EXIT_FAILURE;
    }

    // Calcula el imc (imc = kg * m²)
    double imc = peso / (estatura * estatura);

    // Muestra los resultados
    std::cout << "Tu imc           = " << imc << " kg/m^2\n"
              << "El imc ideal es  = " << "18.5-24.9 kg/m^2\n"
              << "La diferencia es = " << imc - 24.9 << " kg/m^2\n";

    // Evalua cada expresión por separado
    if (imc < 18.5) {
        std::cout << "Peso bajo";
    } else if (imc < 25) {
        std::cout << "Peso normal";
    } else if (imc < 30) {
        std::cout << "Tienes sobrepeso";
    } else {
        std::cout << "Tienes obesidad";
    }

    return EXIT_SUCCESS;
}
```

Cada bloque `else if` se encarga de comprobar una expresión. Si la
expresión es verdadera ejecuta el bloque de código correspondiente, si
la expresión es falsa se evalúa la condición siguiente, hasta llegar al
final. Si cualquiera de las expresiones es verdadera la evaluación
termina. Este tipo de sentencia es útil, porque evita que se aniden
sentencias `if` y `else`.

Cuando se necesitan ejecutar diferentes declaraciones usando la misma
expresión es mejor usar una sentencia `switch` que no necesita evaluar
cada condición por separado.

### La sentencia switch

Cuando se necesita evaluar repetidamente una misma expresión es mejor
usar una sentencia `switch`. Una sentencia `switch` maneja distintas
situaciones que se ejecutan bajo una misma condición inicial. Esta es su
sintaxis:

``` C++
switch (expresion_inicial; valor_inicial) {
    case a:
        bloque a;
        break;

    case b:
        bloque b;
        break;

    case n:
        bloque n;
        break;

    default: 
        bloque por defecto;
}
```

La sintaxis para usar la sentencia `switch` es la siguiente: después de
la palabra clave `switch` sigue una expresión inicial (opcional), un
valor entre paréntesis y un bloque de código entre llaves. Dentro del
bloque se colocan etiquetas con la palabra clave `case` (que manejan los
distintos casos) seguidas por una expresión y dos puntos. La etiqueta
`case` es como una declaración, solo que en lugar de declarar una
etiqueta con un nombre, asocia una expresión con una declaración.

    (Inicio)
       | 
       v
    +-----+-----+
    | expresión |
    +-----+-----+
       |
       v
    +----------------+--------+-------+----------------+
    |                |                |                |
    v                v                v                v
    (case a)         (case b)         (case c)        (default)
    |                |                |                |
    v                v                v                v
    +-----+-----+    +-----+-----+    +-----+-----+    +-----+-----+
    |  bloque a |    |  bloque b |    |  bloque c |    |  bloque d |
    +-----+-----+    +-----+-----+    +-----+-----+    +-----+-----+
    |                |                |                |
    break            break            break              |
    |                |                |                |
    v                v                v                v
    +----------------+------>( )<-----+----------------+
       |
       v
     (Fin)

Cuando se ejecuta una clausula `switch`, el compilador calcula el valor
de la expresión y después busca el nombre de una etiqueta `case` cuya
expresión sea igual al valor de la expresión. Si encuentra una etiqueta
que coincida con el valor, ejecuta el bloque de código con las
declaraciones contenidas y salta a otra parte del programa usando la
cláusula `break`. Si no encuentra una coincidencia busca una declaración
por `default` (un valor predeterminado). Si no hay ningún valor
predeterminado, la declaración `switch` salta al bloque de código
siguiente.

``` C++
#include <iostream>                        // cout
#include <chrono>                          // chrono

// Muestra el año de acuerdo al calendario chino
int main()
{
    using namespace std::chrono;
    year_month_day fecha = floor<days>(system_clock::now());

    // Calcula el año actual
    int num = static_cast<int>(fecha.year());

    // Devuelve el año actual, de acuerdo al calendario chino 
    std::cout << num << " es el año del ";
    switch (num % 12)  {
        case  0: std::cout << "Mono";      break;
        case  1: std::cout << "Gallo";     break;
        case  2: std::cout << "Perro";     break;
        case  3: std::cout << "Cerdo";     break;
        case  4: std::cout << "Rata";      break;
        case  5: std::cout << "Buey";      break;
        case  6: std::cout << "Tigre";     break;
        case  7: std::cout << "Conejo";    break;
        case  8: std::cout << "Dragon";    break;
        case  9: std::cout << "Serpiente"; break;
        case 10: std::cout << "Caballo";   break;
        case 11: std::cout << "Cabra";     break;
    }

    return EXIT_SUCCESS;
}
```

A diferencia de una sentencia `if` que espera un valor boléano o evalúa
la expresión a un valor boléano, la sentencia `switch` evalúa la
expresión a un valor de cualquier tipo.

Un error muy común en el uso de la declaración `switch` es olvidar
utilizar la palabra clave `break` para \"romper\" la ejecución del
bucle. La declaración `break` ocasiona que el programa termine la
declaración `switch` y continúe con las declaraciones que siguen en el
programa. Si no se usa la palabra clave `break` se evalúan todas las
etiquetas `case`.

### Devolviendo valores

Cuando se usa una sentencia `switch` dentro de una función, en lugar de
usar la palabra clave `break` se puede usar la palabra clave `return`
para devolver un valor. Ambas declaraciones sirven para terminar el
`switch` y previenen la ejecución del siguiente caso dentro del código.

``` C++
// Función que devuelve el día de la semana como una cadena de texto
std::string dia(int dia)
{
    switch (dia) {
        case 1: return "Domingo";
        case 2: return "Lunes";
        case 3: return "Martes";
        case 4: return "Miercoles";
        case 5: return "Jueves";
        case 6: return "Viernes";
        case 7: return "Sabado";

        default: return " ";
    }
}
```

En este ejemplo la declaración `switch` evalúa la expresión y comprueba
las etiquetas `case` en el orden en que aparecen en el bloque de código,
hasta encontrar un valor que coincida con el valor buscado. Si ninguna
de las expresiones coincide, simplemente ejecuta el valor por `default`.

``` C++
dia(8);        // Devuelve una cadena vacía.
```

Las cláusulas `case` en declaraciones `switch` especifican únicamente el
punto de inicio del código buscado; no especifican ningún punto final.
En ausencia de una declaración `break`, o `return` una declaración
`switch` continua evaluando cada caso y ejecuta todas las declaraciones
hasta llegar al final del bloque.

## Sentencias de iteración

Las sentencias de iteración son instrucciones que permiten ejecutar una
o varias declaraciones, un número determinado de veces o
indefinidamente, mientras se cumpla cierta condición. En C++ hay tres
estructuras iterativas: `for`, `while` y `do while`. La sentencia `for`
es muy parecida a la sentencia `while` ya que permite ejecutar una
secuencia controlada de código (el cuerpo del bucle) de manera repetida
mientras la condición sea verdadera. Un bucle `do while` a diferencia de
un bucle `while` siempre se ejecuta, al menos una vez, sin importar si
la expresión es verdadera o falsa.

### La sentencia while

Si la sentencia `if` declara una condición, la sentencia `while` declara
un bucle. Un bucle es un fragmento de código que se ejecuta
repetidamente mientras se cumpla una condición. Esta es su sintaxis.

``` c++
while (expresión == verdadera) {
    // Ejecuta estas declaraciones
}
```

Para ejecutar una sentencia `while`, el programa evalúa primero la
expresión entre paréntesis. Si el valor de la expresión es falso, el
flujo de control salta las declaraciones que sirven como cuerpo del
bucle y se mueve al bloque de código siguiente. Sin embargo, si la
expresión es verdadera, el flujo de control ejecuta las declaraciones
dentro del bloque, saltando al inicio y evaluando de nuevo la expresión
en cada ocasión. Es decir, el flujo de control ejecuta el bloque
mientras la expresión sea verdadera. Esto puede crear un bucle
\"infinito\" si la expresión a evaluar es siempre verdadera.

    (Inicio)  
        |
        |<----- Nueva Evaluación --------+
        |                                ^
        |  Mientras la expresión sea     |
        v          verdadera             | 
    +-----+-----+                     +----+---+
    | expresión |------- true ------->| bloque |
    +-----+-----+                     +--------+
        |                     
      false                                      
        |
        v 
      (Fin)

Este es un fragmento de código que usa un bucle `while` para imprimir
los números del 1 al 10.

``` C++
// Una variable que actúa como contador
int contador{1};

// Evalúa la expresión entre paréntesis e incrementa el contador
while (contador <= 10)  {
    std::cout << contador++ << ' ';     // 1 2 3 4 5 6 7 8 9 10  
}
```

En este código, la variable `contador` comienza valiendo `1`, luego su
valor se incrementa en 1, con cada iteración. Una vez que el bucle se
ejecuta 10 veces, la expresión `contador <= 10` es falsa, por lo tanto
la sentencia `while` finaliza, y el flujo se mueve hacia la siguiente
declaración en el programa. Los bucles `while` no usan el punto y coma.
Y al igual que la sentencia `if`, si el bloque contienen una sola
declaración, se pueden omitir las llaves.

``` C++
// Siempre se debe inicializar un contador que no sea static
// de lo contrario la variable tendrá un comportamiento indefinido.
int contador{1};

while (contador <= 10) {
    std::cout << contador << ' ';      // 1 2 3 4 5 6 7 8 9 10 
    ++contador;
}
```

Los bucles también funcionan en sentido inverso, en lugar de incrementar
el contador se puede decrementar su valor.

``` C++
int contador{10};

// Decrementa el contador en la misma expresión 
while (int valor = contador--) {
    std::cout << valor << ' ';        // 10 9 8 7 6 5 4 3 2 1
}
```

Muchos bucles tienen una variable que actúa como contador. Los nombres
más comunes para esta variable son: `i`, `j` y `k`, si quieres que tu
código sea más comprensible puedes darle un nombre más descriptivo.

``` C++
#include <iostream>                  // cout
#include <iomanip>                   // setw

// Imprime los números menores que el limite
int main()
{
    const int limite{999};           // El número máximo
    int num{1};                     

    // Imprime un rango de elementos del 1-999 en filas de 10
    while (num <= limite) {
        std::cout << std::setw(3) << num << ' ';
        if (num % 10 == 0) {         // Agrega un salto de línea
            std::cout << '\n';       // cada 10 números. 
        }  
        ++num;                    
    }

    return EXIT_SUCCESS;
}
```

Un bucle `while` termina la ejecución del código cuando la expresión
entre paréntesis es falsa. ¿Pero qué pasa cuando la evaluación de la
expresión es siempre verdadera? Bueno, en estos casos se crea un **bucle
infinito**. Un bucle infinito puede dejar un programa en un estado
indefinido (iterando hasta que se acabe la memoria), sin embargo un
bucle puede romperse dentro del propio bloque, si se cumple otra
condición.

### Rompiendo un bucle infinito

Usualmente en cada bucle, una o más variables cambian y si las variables
cambian, la acción que se ejecuta también cambia, esto significa que los
valores de la expresión son diferentes con cada nueva iteración. Por lo
tanto, es importante evitar que una expresión sea siempre verdadera, o,
si es el caso; buscar otra forma de romper el bucle cuando se cumpla una
condición esperada.

``` C++
#include <iostream>                    // cout
#include <iomanip>                     // setw

// Imprime los números primos menores que el limite
int main()
{
    constexpr int limite{999};         // El número máximo
    int primo{2};                      // El primo más pequeño
    int total{0};                      // Total de primos encontrados 

    // Usa un bucle anidado para detectar números primos
    while (primo <= limite) {
        bool es_primo{true};           // Inicialmente, el número es primo.

        int factor = 2;                // El factor para dividir el número.
        while (factor < primo) {       // Mientras que el factor sea menor
            if (primo % factor == 0) { // Si el número se puede dividir 
                es_primo = false;      // entre el factor, no es primo.
                break;                 // Rompe el bucle.
            }
            ++factor;                  // Incrementa el factor en uno.
        }

        if (es_primo) {               // Si el número es primo, lo muestra.
            std::cout << std::setw(3) << primo << ' ';
            ++total;                  // Incrementa el total   
            if (total % 10 == 0) {    // y agrega un salto de línea 
                std::cout << '\n';    // cada 10 números
            }
        }

        ++primo;                    // Intenta con el número siguiente
    }

    std::cout << "\nNúmeros primos encontrados: " << total << std::endl;

    return EXIT_SUCCESS;
}
```

En este ejemplo, aunque la condición `factor < primo` pueda ser
verdadera, es decir; mientras el número sea menor que el factor se
ejecuta el bucle de forma indefinida, sin embargo, la condición:

``` C++
if (primo % factor == 0) {
    // rompe el bucle de forma prematura
    break;
}
```

puede romper el bucle prematuramente usando la sentencia `break`, una
vez que el número se puede dividir entre el factor, en este caso el
número no es primo, asi que no es necesaria la comprobación.

``` C++
#include <iostream>               // cout 
#include <string>                 // string
#include <vector>                 // vector

// Función que lee múltiples líneas desde la entrada estándar
std::vector<std::string> leer_lineas() 
{
    std::vector<std::string> lineas; 
    std::string linea;            
    while (std::getline(std::cin, linea)) {
        // Si la línea está vacía, rompe el bucle while
        // y deja de leer de la entrada estándar
        if (linea.empty()) {
            break;
        }
        std::cout << "... ";    
        lineas.push_back(linea);
    }

    return lineas; 
}

// REPL interactivo que puede leer varias líneas
int main() 
{
    std::cout << "REPL interactivo\n";
    std::cout << "Ingresa varias líneas:\n";
    std::cout << "una línea vacía para terminar:\n"; 

    std::vector<std::string> resultado;
    while (true) {
        std::cout << "<<< ";
        resultado = leer_lineas();

        // Si la primera palabra es salir, rompe el bucle
        if (resultado[0] == "salir") {
            break;
        }

        // Replica la línea introducida 
        for (const auto& linea : resultado) {
            std::cout << linea << '\n';
        }       
    }

    return EXIT_SUCCESS;
}
```

Una forma prematura (y común) de terminar un bucle `while` es usando una
sentencia `break` para evitar un flujo de ejecución \"infinito\".

### La sentencia do while

La sentencia `do while` es muy parecida al bucle `while`, excepto que la
expresión del bucle se comprueba en la parte inferior del código, en
lugar de la parte superior, esto hace que el cuerpo del bucle sea
ejecutado al menos una vez. Esta es su sintaxis:

``` C++
do {
    declaraciónes;

} while (expresión);
```

Hay un par de diferencias sintácticas entre los bucles `do while` y
`while`. Primero, el bucle `do` requiere tanto de la palabra clave `do`
(que marca el inicio del bucle) y la palabra clave `while` (que marca el
final e introduce la condición del bucle). El bucle `do while` termina
en punto y coma. El bucle `while` no.

    (Inicio)
       | 
       v
    +-----+----+
    |  bloque  |<-----------+
    +-----+----+            ^
       |                 |
       |               true
       v                 | 
    +-----+-----+           | 
    | expresión |---------->+
    +-----+-----+      
       | 
     false
       |
       v 
     (Fin)

Normalmente, un bucle `do while` se utiliza para ejecutar alguna acción
y comprobarla, mientras la condición sea verdadera, el bucle se ejecuta
una y otra vez.

``` C++
#include <iostream>                   // cout
#include <fstream>                    // ofstream 

// Devuelve true si el número es primo
bool es_primo(uint64_t num) noexcept;

// Dibuja la espiral de Ulam en un archivo svg
int main() 
{   
    // Comprueba la entrada del usuario (solo acepta números positivos)
    int puntos;    
    do {
        std::cout << "Programa que dibuja la espiral de Ulam\n"
                  << "Introduce el número de puntos: ";
        std::cin >> puntos;        
    } while (puntos < 0);

    const int width = 400;             // El tamaño del svg
    std::ofstream svg("ulam.svg");     // Abre un archivo para escritura
    svg << "<svg width=\""             // Escribe la cabecera del svg
            << width << "\" height=\"" // como si fuera un archivo de texto.
            << width << "\" xmlns=\"http://www.w3.org/2000/svg\">\n";

    // El tamaño del circulo
    int cx    = width / 2;             // La posición cx del circulo
    int cy    = width / 2;             // La posición cy del circulo
    int len   = 5;                     // La separación entre los puntos 
    int radio = len / 2;               // El radio del circulo  

    // Variables que controlan la dirección del circulo
    int dir = 1, cont = 1, turno = 0, limite = 0;

    // Si el número es primo dibuja un circulo color negro
    for (int i = 1; i < puntos; ++i) {        
        if (es_primo(i)) {
            svg << "<circle cx=\"" << cx << "\" cy=\"" << cy << "\" r=\"" 
                 << radio << "\" style=\"fill:black\" />\n";
        }

        // Mueve la dirección de las coordenadas cx, cy
        switch (dir) {
            case 0: cx += len; break;   // Derecha
            case 1: cy -= len; break;   // Arriba
            case 2: cx -= len; break;   // Izquierda
            case 3: cy += len; break;   // Abajo
        }

        ++cont;                         // Incrementa el contador
        if (cont >= limite) {           // Calcula la nueva dirección
            cont = 0;
            turno = (turno + 1) % 2;
            dir = (dir + 1) % 4; 
            if (turno == 0) {
                ++limite;
            }
        }
    }

    svg << "</svg>";                      // Cierra el svg
    svg.close();                          // Cierra el archivo  
    std::cout << "Espiral de Ulam generada en 'ulam.svg'" 
              << "con " << puntos << " puntos"<< std::endl;

    return EXIT_SUCCESS;
}
```

Aunque la condición sea falsa, el bucle `do while` siempre se ejecuta al
menos una vez. Una vez que la condición es verdadera el flujo de
ejecución salta a otra parte del programa. Este tipo de bucle es usado
un poco menos que su contraparte `while`. En la práctica es poco común
ejecutar al menos una vez un bucle. Pero en ocasiones resulta útil, como
por ejemplo al pedir datos al usuario o presentar un menú de opciones.

``` C++
#include <iostream>                // cout

// Programa que muestra un menú usando un bucle "do while"
int main()
{
    int opcion = -1;

    do {
        // Muestra el menú
        std::cout << "Menú (Selecciona una opción)\n\n"
                  << "1 : Continuar\n"
                  << "2 : Guardar y Salir\n"
                  << "3 : Salir\n";

        // Recoge la entrada del usuario
        std::cin >> opcion;

        // Comprueba el valor introducido y muestra la opción seleccionada
        switch (opcion) {
        case 1:
            std::cout << "Seleccionaste la opción 1\n\n" << "\tContinuar...\n";
            break;
        case 2:
            std::cout << "Seleccionaste Guardar y Salir\n\n"
                      << "\tGuardando...\n";
            opcion = 3;           // Le asigna un nuevo valor a opción
            break;
        case 3:
            std::cout << "Seleccionaste salir.\n\n" << "\tSalir...\n";
            break;
        default:
            std::cout << "\n\nOpción selecionada no valida\n\n";
        }
    } while (opcion != 3);

    return EXIT_SUCCESS;
}
```

La secuencia de ejecución del código es fácil de seguir. La variable
`opcion` tiene un valor predeterminado de `-1`. Cuando el menú es
mostrado, el usuario debe introducir algún valor. El valor es evaluado
en la sentencia `switch` y dependiendo del valor introducido se muestra
un mensaje diferente. Si el usuario selecciona la opción `1` el programa
muestra un mensaje y vuelve a mostrar el menú, pero cuando se introduce
el valor `2`, el valor de la variable `opción` es cambiado a `3` esto
permite terminar el bucle sin evaluarlo de nuevo, ya que la comprobación
se lleva a cabo al final del código. Si se selecciona una opción no
válida se usa la etiqueta `default`, que muestra el menú de nuevo.

### La sentencia for

La sentencia `for` proporciona un bucle iterador mucho más conveniente
que la sentencia `while`. La palabra clave `for` simplifica los bucles
en un patrón común. Muchos bucles tienen una variable que actúa como
contador. Esta variable es inicializada antes de que empiece el bucle y
se comprueba en cada iteración, de esta forma el contador se incrementa
o se actualiza al final del cuerpo del bucle, justo antes de que la
variable se compruebe de nuevo. Para este tipo de bucles, la
inicialización, la comprobación y la actualización son tres
manipulaciones cruciales que se llevan a cabo sobre una variable. La
sentencia `for` junta estos tres procesos en una sola expresión y hace
que la expresión sea parte explicita de la sintaxis.

``` C++
for (variable a inicializar; condición a probar; valor a incrementar) {
    // Código a ejecutar mientras la condición sea verdadera
}
```

Inicializar, comprobar e incrementar son tres expresiones (separadas por
punto y coma) que son responsable por inicializar, comprobar e
incrementar la variable del bucle. Poniéndolas juntas en una sola línea,
hace más fácil comprender que es lo que hace el bucle y previene de
muchos errores, como olvidar inicializar o incrementar la variable
inicial o final del bucle.

    (Inicio)  
       |
       v
    +------+------+
    | inicializar |  
    +-----+-------+  
      |
      +<------------ Nueva evaluación ----------+
      |                                         ^
      |                                         |
      v                                         |
    +-----+-----+             +--------+     +------+------+
    | comprobar |--- true --->| bloque |---->| incrementar |
    +-----+-----+             +--------+     +-------------+
      |                Ejecutar bloque
    false             
      | 
      v
    (Fin)

Por ejemplo, para imprimir los números del 1 al 10. En contraste con el
ejemplo que usa el bucle `while`:

``` C++
for(int contador = 0; contador <= 10; ++contador) { 
    // El bucle inicia el contador y comprueba la condición 
    // antes de incrementar de nuevo el valor. Una vez
    // que la comprobación es falsa, es decir el valor es mayor
    // o igual que 10, el bucle termina. 
    std::cout << contador << ' ';
}
```

La expresión de inicialización se evalúa una vez, antes de que el bucle
inicie. Esto es útil, si la expresión tiene efectos secundarios (una
asignación por ejemplo). La expresión de comprobación se evalúa antes de
cada iteración y controla el cuerpo del bucle que se ejecuta. Si la
comprobación se evalúa como `true`, la sentencia que es el cuerpo del
bucle se evalúa. En cada iteración la expresión de incremento se evalúa.
Esta expresión debe tener efectos secundarios para que sea útil.
Generalmente usando una expresión de asignación, o usando los operadores
`++` o `--`.

``` C++
#include <iostream>                //  cout

// Calcula los divisores de un número entero
int main()
{
    std::cout << "Calcula los divisores de un número entero\n";

    // Comprueba la entrada (solo acepta números positivos)
    int num{0};    
    do {
        std::cout << "Introduce un número positivo: ";
        std::cin >> num;
    } while (num <= 0);

    // Divide la variable número entre todos los números menores,
    // si el valor es 0, lo imprime y aumenta el contador.
    int total{0};    
    for (int i = 1; i <= num; ++i) {
        if ((num % i) == 0) {
            std::cout << i << ' ';
            ++total;
        }
    }

    // Muestra el total de divisores y termina el programa
    std::cout << "\nTotal de divisores: " << total << '\n';

    return EXIT_SUCCESS;
}
```

El bucle `for` también funciona en sentido inverso:

``` C++
#include <iostream>               // cout

// Bucle que itera en sentido inverso
int main()
{
    for(int i = 10; i != 0; --i) {
        std::cout << i << ' ';  // 10 9 8 7 6 5 4 3 2 1
    }
    return EXIT_SUCCESS;                   
}
```

En este ejemplo mientras el valor de `i` sea diferente de cero, el bucle
continua iterando y disminuyendo el valor de `i` en 1.

#### Bucles anidados

Dentro del cuerpo de un bucle `for` se pueden evaluar varias
condiciones, inclusive se pueden usar otros bucles `for` anidados.

``` C++
#include <iostream>         // cout
#include <iomanip>          // setw

// Bucle anidado que itera en filas (de forma horizontal)
int main()
{
    const int filas = 10;
    const int columnas = filas * filas;

    // Itera en forma horizontal (filas)
    for (int i = 0; i < columnas; i += filas) {
        for (int j = 0; j < filas; ++j) {
            std::cout << std::setw(2) << j + i << ' ';
        }
        std::cout << '\n';
    }

    return EXIT_SUCCESS;
}
```

Por lo general los valores de un bucle se incrementan en 1, de esta
forma el bucle se ejecuta en forma horizontal (o en filas), pero los
bucles `for` también se pueden usar en dirección vertical, incrementando
el valor usado como contador con cualquier otro valor.

``` C++
#include <iostream>         // cout
#include <iomanip>          // setw

// Bucle anidado que itera en columnas (de forma vertical)
int main()
{
    const int filas = 10;
    const int columnas = filas * filas;

    // Itera en forma vertical (columnas)
    for (int i = 0; i < filas; ++i) {
        for (int j = 0; j < columnas; j += filas) {
            std::cout << std::setw(2) << j + i << ' ';
        }
        std::cout << '\n';
    }

    return EXIT_SUCCESS;
}
```

Los bucles `for` pueden ser más complejos, ya que en algunas ocasiones
se necesita que varias variables cambien con cada iteración del bucle.
Para estas (únicas) situaciones, se usa el operador `coma` ya que este
operador, provee una forma de combinar varios inicializadores y valores
de incremento dentro de una misma expresión que puede usarse en un
bucle.

``` C++
// Usa dos variables de control en un mismo bucle
for (int i = 0, j = 10 ; i <= 10 ; ++i, --j) {
    std::cout << i << ' '
              << j << '\n';
}
```

En este ejemplo, mientras el valor de `i` se incrementa en 1, el valor
de `j` disminuye en 1, con cada nueva iteración.

#### Iteración de secuencias

Existe una versión especial del bucle `for` que se usa para iterar
rangos o \"secuencias iterables\". Esta es su sintaxis.

``` C++
for (declaración : rango) { 
    declaraciones;
}
```

Este tipo de estructura, es llamado **bucle for basado en rangos**. Para
iterar sobre un rango de elementos, es necesario declarar alguna
variable con el tipo de elementos que contenga el rango. Un rango puede
ser cualquier secuencia de elementos del mismo tipo. Como `vector`,
`array`, `map`, `set`, inclusive se pueden usar cadenas de texto, o
cualquier secuencia iterable (una secuencia que defina las funciones
`begin` y `end`). Los dos puntos que separan la declaración, del rango
son parte explicita de la sintaxis.

``` C++
std::vector<int> rango{0, 1, 2, 3, 4, 5, 6, 7, 8, 9};

// Imprime la serie de números enteros
for (int num : rango)  {
    std::cout << num << ' ';   // 0 1 2 3 4 5 6 7 8 9 
}
```

En este fragmento de código, el bucle `for` itera automáticamente sobre
todos los elementos contenidos en el `vector`. En este tipo de bucle, no
se requiere de ninguna variable que actué como contador, ni se usa
ninguna condición.

La variable de inicio puede ser declarada usando la palabra clave
`auto`, ya que el tipo puede ser deducido directamente del objeto
iterado. Por supuesto se puede modificar el objeto, usando una
referencia.

``` C++
// Modifica la secuencia usando una referencia
for (auto& num : rango)  {
    num *= 2;
    std::cout << num << ' ';  // 0 2 4 6 8 10 12 14 16 18
}
```

o, se puede usar un calificador `const` para evitar modificar
incidentalmente algún elemento de la secuencia.

``` C++
for (const auto& num : rango)  {
    std::cout << num << ' ';  // 0 2 4 6 8 10 12 14 16 18
}
```

La sintaxis solo requiere de una variable y un objeto que se pueda
iterar. No hay necesidad de especificar el límite, ni el punto de
inicio, ya que el bucle `for` recorre (de forma segura) todos los
elementos de la colección.

### Bucle infinitos

Cuando se usa un bucle `for` al igual que un bucle `while` siempre se
debe controlar la variable que comprueba la iteración (en este caso, la
segunda expresión). Si la evaluación de esta variable siempre es
verdadera. Se produce un bucle \"infinito\". También se crea un bucle
infinito cuando se omiten las expresiones, dejando solo el punto y coma.

``` C++
// Crea un bucle for infinito (omitiendo las expresiones)
for (;;) {
    std::cout << "Presiona una tecla!\n";
    std::cin.get();
}
```

Es equivalente a usar un bucle `while` con una expresión verdadera:

``` C++
// Crea un bucle while infinito 
while (true) {
    std::cout << "Presiona una tecla!\n";
    std::cin.get();
}
```

Un bucle infinito puede dejar un programa en un estado indefinido, si no
se maneja de forma adecuada, por lo general dentro del bloque debe de
haber alguna condición que se encargue de romperlo una vez que se cumple
alguna condición. La forma más sencilla de romper un bucle es usando la
sentencia `break`.

## Saltos

Muchas veces es necesario cancelar una tarea porque se ha cumplido una
condición o es necesario omitir ciertas partes del código, porque se ha
obtenido un resultado, o por el contrario; ya no tiene sentido realizar
una comprobación. En casos como estos se utilizan las estructuras de
control llamadas \"saltos\". Un salto es una sentencia que terminan un
bucle o salta de una parte del código a otro. En C++ se pueden usar las
palabras clave: `break`, `continue`, `goto`, y `return` para realizar
este tipo de tareas.

Los saltos permiten terminar un bucle, una función o saltar de una parte
a otra del código. La sentencia `break` permite que el programa
\"interrumpa\" un bucle. La sentencia `continue` hace que el programa
\"salte\" el resto de código de un bucle y salte al inicio de una nueva
iteración. La sentencia `return` devuelve el valor de una invocación.

### La sentencia break

La sentencia `break` permite salir inmediatamente de un bucle `switch` o
`for`. Esta es su sintaxis:

``` C++
break;              // Termina en punto y coma
```

Es legal únicamente si aparece dentro de un bucle, ya que se usa para
salir o romper prematuramente una iteración, cuando ya no hay necesidad
de seguir iterando a lo largo del bucle o cuando se ha cumplido una
condición.

``` C++
#include <iostream>                       // cout
#include <iomanip>                        // setw 

// Imprime los números del 100-1. Usa break para romper el bucle while
int main()
{
    int num{100};
    const int limite{1};

    // Un bucle infinito
    while (true) {

        // Imprime el número (con 3 espacios) y lo decrementa en 1.
        std::cout << std::setw(3) << num << ' ';
        --num;

        // Agrega un salto de línea cada 10 números
        if (num % 10 == 0) {
            std::cout << '\n';
        }        

        // Detiene la iteración cuando el número es menor que el límite.
        if (num < limite) {
            break;                       // Rompe el bucle
        }        
    }

    return EXIT_SUCCESS;
}
```

Tal y como su nombre lo indica, los `saltos` son sentencias que permiten
que un programa salte de un bloque de código a otro. En el ejemplo, la
palabra clave `break` termina el bucle `while` cuando la condición es
verdadera. No importa si es al inicio o al final del bloque.

Otros tipo de bucle que puede romper la sentencia `break` es el bucle
`for`:

``` C++
#include <array>                 // array
#include <chrono>                // steady_clock
#include <iostream>              // cout, endl
#include <random>                // default_random_engine 

/* 
* El problema de Monty Hall
*
* El programa le pide al usuario que seleccione una puerta, 
* luego abre otra puerta que no tiene el premio y le pregunta 
* si desea cambiar su elección y le muestra si ganó o perdió.
*/
int main() 
{
    // Inicializa el generador de números aleatorios
    auto semilla = 
        std::chrono::steady_clock::now().time_since_epoch().count();
    std::default_random_engine azar(semilla);
    std::uniform_int_distribution<int> numero(0, 2);

    // Usa un array para manejar las puertas
    std::array<int, 3> puertas{0, 1, 2};

    // Elige aleatoriamente una puerta con el premio    
    int premio = numero(azar); 
    int puerta;

    // Verifica que la elección del usuario sea válida
    do {
        std::cout << "Bienvenido al juego de Monty Hall!" << std::endl;
        std::cout << "Selecciona una puerta: " << '\n'  
                  << "  +---+  +---+  +---+  " << '\n'
                  << "  | 0 |  | 1 |  | 2 |  " << '\n'
                  << "  +---+  +---+  +---+  " << '\n';  
        std::cin >> puerta;
    } while (puerta < 0 || puerta > 2);

    // Monty abre una puerta que no tiene el premio y 
    // que no fue elegida por el usuario
    int puerta_abierta;
    for (int i = 0; i < 3; i++) {
        if (i != puerta && i != premio) {
            puerta_abierta = i;
            break;
        }
    }

    std::cout << "Monty abre la puerta "  << puerta_abierta 
              << " y no tiene el premio." << std::endl;

    // Le pregunta al usuario si quiere cambiar su elección
    char cambiar;
    std::cout << "¿Quieres cambiar tu elección? (s/n): ";
    std::cin >> cambiar;

    if (cambiar == 's' || cambiar == 'S') {
        // Cambia la elección del usuario a la puerta restante
        for (int i = 0; i < 3; i++) {
            if (i != puerta && i != puerta_abierta) {
                puerta = i;
                break;
            }
        }
    }

    // Muestra el resultado
    std::cout << "Tu elección final es la puerta "  << puerta << ".\n";
    if (puerta == premio) {
        std::cout << "¡Felicidades! Has ganado el premio." << std::endl;
    } else {
        std::cout << "Lo siento, has perdido. El premio estaba " 
                  << "detrás de la puerta " << premio << ".\n";
    }

    return EXIT_SUCCESS;
}
```

La palabra clave `break` literalmente \"rompe\" el bucle que la
contiene, llevando el flujo de ejecución a la sentencia siguiente. Una
vez que se rompe el bucle, el flujo de ejecución se mueve al bloque de
sentencias siguiente, y si este es otro bucle, la ejecución continua,
porque `break` solo puede romper un bucle.

``` C++
// break solo puede romper un bucle
while (true) {
    while (true) {
        break;         // break: solo rompe el bucle que lo contiene. 
    } 
    // La sentencia break salta al nuevo bucle
}
```

Una sentencia `break` puede romper cualquier bucle, pero no puede romper
bucles anidados. Para romper bucles anidados es necesario usar otra
sentencia `break` o usar una sentencia `goto`.

### La sentencia continue

La sentencia `continue` es parecida a la sentencia `break`, solo que en
lugar de romper el bucle de ejecución salta la iteración actual. Esta es
su sintaxis:

``` C++
continue;          // Termina en punto y coma
```

Cuando la sentencia `continue` se ejecuta, la iteración (actual) termina
y comienza la siguiente iteración del bucle. Esto permite hacer
diferentes cosas, con diferentes tipos de bucles:

- En un bucle `while`, comprueba la expresión al inicio y si es
  verdadera, ejecuta el cuerpo del bucle desde el inicio.
- En un bucle `do while`, salta al final del bucle y comprueba la
  condición antes de reiniciar de nuevo la ejecución del bucle.
- En un bucle `for`, evalúa el incremento de la expresión y comprueba la
  expresión de nuevo para determinar si debe hacer otra iteración.

A diferencia de `break` que rompe el bucle, la cláusula `continue` solo
salta la ejecución actual.

``` C++
#include <iostream>             // cout
#include <iomanip>              // setw

// Imprime solo los números que son impares y menores que 100
int main()
{
    const int limite{100};
    int num{0};

    // Bucle que imprime números impares menores que 100.
    while (num < limite) {        
        ++num;                 // Incrementa el contador

        // Si el número es par, detiene la iteración y salta al 
        // inicio del bucle para comprobar de nuevo la condición.
        if (num % 2 == 0) {
            continue;
        }

        // Imprime un número impar
        std::cout << std::setw(3) << num << ' ';

        // Agrega una línea cada 9 números
        if (num % 9 == 0) {
            std::cout << '\n';
        }
    }

    return EXIT_SUCCESS;
}
```

En este ejemplo, la sentencia `continue` se usa para saltar la iteración
actual del bucle, en caso de que la condición sea verdadera.

``` C++
#include <cstdio>              // popen, fgets, pclose
#include <iostream>            // cout, endl
#include <string>              // string
#include <memory>              // unique_ptr

// Función que ejecuta comandos del sistema 
std::string ejecutar(const std::string& comando) 
{
    std::string resultado;
    char bufer[128];

    // Ejecuta el comando y abre un tuberia de lectura
    std::unique_ptr<FILE, decltype(&pclose)> 
    pipe(popen(comando.c_str(), "r"), pclose);

    // Devuelve un mensaje si ocurre un error
    if (!pipe) {
        return "Error al ejecutar el comando.";
    }

    // Lee la salida línea por línea y la agrega al resultado
    while (fgets(bufer, sizeof(bufer), pipe.get()) != nullptr) {
        resultado += bufer;
    }

    return resultado;
}

// REPL que ejecuta comandos del sistema
int main()
{
    std::cout << "REPL interactivo\n"
              << "Escribe instruccciones como:\n"
              << "!echo Hola mundo\n"
              << "!ls -1 | sort\n"
              << "Escribe 'salir' para terminar.\n";

    std::string linea;
    while (true) {
        std::cout << "<<< ";
        std::getline(std::cin, linea, '\n');
        // Rompe el bucle y el programa termina
        if (linea == "salir") {
            break;
        }
        // Ejecuta el comando en modo interactivo
        if (linea.starts_with("!")) {
            std::string salida = ejecutar(linea.substr(1));
            std::cout << salida << std::endl;
            continue;
        }
        std::cout << linea << std::endl;
    }

    return EXIT_SUCCESS;
}
```

En este ejemplo, el programa puede ejecutar diferentes comandos del
sistema, siempre y cuando empiecen con el signo `!`. A diferencia de
`break` que rompe el bucle `while` cuando se introduce la palabra
`salir`. La palabra clave `continue` solo salta la iteración actual, y
ejecuta de nuevo el bucle, esto le permite al REPL seguir en modo
interactivo para seguir ejecutando comandos.

### La cláusula return

La invocación y la llamada de una función son expresiones y todas las
expresiones producen valores. Una sentencia `return` dentro de una
función especifica el valor de retorno de una función.

``` C++
return;                      // Solo se puede usar dentro de una función
```

La clausula `return` puede aparecer únicamente dentro del cuerpo de una
función o de una expresión lambda. En cualquier otra parte es un error
de sintaxis. Cuando se ejecuta una sentencia `return` la función que la
contiene devuelve el valor a la expresión que la llamo.

``` C++
// Una función con una sentencia return
int cuadrado(int x) 
{ 
    return x * x;             // Devuelve un valor entero 
} 

int num = cuadrado(2)          // La invocación asigna 4 a num.
```

Sin una sentencia `return`, la invocación de una función simplemente
ejecuta cada una de las sentencias del cuerpo de la función hasta llegar
al final del bloque. En estos casos, al invocar la expresión esta se
evalúa como `void`.

``` C++
// Una función que no devuelve ningún valor
void cuadrado(int x) 
{  
    std::cout << x * x; 
}  

cuadrado(2)                   // La invocación imprime 4.
```

La sentencia `return` aparece a menudo como la última declaración en una
función, pero no necesita ser la última:

``` C++
// Suma los dígitos de un número: 921 = 9 + 2 + 1 = 3 
int sumar_digitos(int numero)
{
    if (!numero) {
        return 0;
    } else if (numero % 9 == 0) {
        return 9;
    } else {
        return numero % 9;
    }
}
```

Una función devuelve un valor cuando se ejecuta una declaración
`return`, incluso si aún quedan otras declaraciones en el cuerpo de la
función. Esto es útil, por ejemplo para devolver algún tipo de
resultado, si se cumple cierta condición, sin necesidad de evaluar el
resto de código.

### La sentencia goto

La sentencia `goto` transfiere el flujo de control a una etiqueta
especial, definida por un identificador que termina en dos puntos. Esta
es su sintaxis:

``` C++
// Clausula goto
goto identificador;

// Una etiqueta
identificador:
```

La instrucción usada como etiqueta se define usando dos puntos después
del nombre del identificador. El nombre debe estar en la función actual.
Todos los nombres usados como identificadores son miembros de un espacio
de nombres interno, así que no interfieren con otros nombres de
identificadores.

``` C++
#include <iostream>           // cout
#include <iomanip>            // setw 

// Imprime los números del 100 - 1
int main()
{
    int num{100};
    const int limite{1};

    // Un bucle infinito
    while (true) {
        // Agrega un salto de línea cada 10 números
        if (num % 10 == 0) {
            std::cout << '\n';
        }

        // Detiene la iteración cuando el valor de número
        // es menor que el límite y salta fuera del bucle while.
        if (num < limite) {
            goto fin;    // Busca la etiqueta fin
        }

        // Imprime el número (con 3 espacios) y lo decrementa en 1.
        std::cout << std::setw(3) << num << ' ';
        --num;
    }

    // Una etiqueta para salir de bucle while usando goto
    fin:  

    return EXIT_SUCCESS;
}
```

La sentencia `goto` no puede transferir el control a una ubicación que
omita la inicialización de un identificador definido como etiqueta. El
caso de uso más común es salir de un bucle:

``` C++
#include <iostream>                      // cout 
#include <fstream>                       // ofstream

// Dibuja la alfombra de sierpinsky en un archivo svg
int main()
{
    const int limite{81};                // El número de iteraciones
    const int width = 400;               // El tamaño del svg 
    const double len = width / limite;   // El tamaño de cada cuadrado
    std::ofstream svg("sierpinsky.svg"); // El archivo para crear el svg
    svg << "<svg width=\""               // La cabecera del svg
        << width << "\" height=\"" 
        << width << "\" xmlns=\"http://www.w3.org/2000/svg\">\n";

    // Calcula la posición dx, dy del cuadrado antes de dibujarlo
    for (int x = 0; x < limite; ++x) {
        for (int y = 0; y < limite; ++y) {
            int num{0};           
            for (num = limite / 3; num; num /= 3) {
                const int z = (num * 3);
                if ((x % z) / num == 1 && (y % z) / num == 1) {
                    goto dibujar;        // Usa goto para salir del bucle
                }                        
            }
            dibujar:                     // La etiqueta para goto  
            auto dx = x * len + 0.5;     // La posición x del cuadrado
            auto dy = y * len + 0.5;     // La posición y del cuadrado
            svg << "<rect x=\""          // Dibuja un cuadrado en el svg
                << dx  << "\" y=\""         
                << dy  << "\" width=\"" 
                << len << "\" height=\""             
                << len << ((num == 0)    // El color es blanco o negro 
                    ? "\" style=\"fill:none;" : "\" style=\"fill:black;")
                << "stroke:black;stroke-width:1\"/>\n";             
        }
    }

    svg << "</svg>";                      // Cierra el svg
    svg.close();                          // Cierra el archivo
    std::cout << "Archivo SVG generado exitosamente." << std::endl;

    return EXIT_SUCCESS;
}
```

Por lo general siempre se utilizan la sentencia `break` `continue` y
`return` en lugar de la sentencia `goto`. Sin embargo, y dado que la
sentencia `break` solo salta de un nivel en un bucle, se puede usar una
sentencia `goto` para salir de un bucle anidado.

``` C++
// goto puede salir de un bucle anidado
while (true) {
    while (true) {
        goto fin;                       // goto: es la única sentencia que
    }                                   // puede salir de un bucle anidado
}

fin:                                    // Una etiqueta para salir del bucle
```

La sentencia `goto` puede literalmente \"saltar\" a cualquier parte del
código, siempre y cuando este en la misma función.
