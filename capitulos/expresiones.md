# Operadores y expresiones

::: {.meta title="Operadores y expresiones" capitulo="5" description="Como crear y evaluar expresiones usando operadores" keywords="tipos de valores, expresiones, operadores, orden de evaluación"}
:::

Un operador es un carácter especial formado por uno o más símbolos cuyo
propósito es realizar algún tipo de operación. Una operación puede ser
una simple suma o una resta, pero también puede ser una complicada
combinación de operadores, valores literales y expresiones.

Las expresiones son categorías sintácticas que cuando se evalúan
producen valores directamente en un programa. Las expresiones
aritméticas dan como resultado valores numéricos. Las expresiones
relacionales devuelven valores boléanos y las asignaciones, asignan el
valor de una expresión a una variable. Las expresiones pueden ser
constantes cuando el valor de un cálculo no cambia a lo largo de la
ejecución de un programa o pueden producir efectos secundarios,
asignando el valor de un cálculo a una variable.

Las expresiones son esenciales porque permiten realizar cálculos y
manipular datos de manera eficiente. Se pueden usar en declaraciones o
como argumentos en funciones. Permiten tomar decisiones en estructuras
de control y se pueden usar directamente en operaciones matemáticas,
comparaciones y asignaciones para escribir código más compacto.

Este capítulo describe la forma de usar expresiones y operadores para
producir valores en C++.

## Expresiones

Una expresión combina valores literales, operandos y operadores para
expresar o calcular el valor de una variable. Una expresión no es un
objeto. Solo es una serie de operaciones que se usan en el código fuente
para producir valores.

``` C++
Nombre        Expresión
  |               |
  |      +--------+----------+ 
  |      |                   |
  v      v                   v
double log_10 = 1.0 * 0.434294481903;   <--- Termina en punto y coma
^             ^  ^          ^ 
|             |  |          |
Tipo    Operando Operador Operando
```

Un operando es una entidad en la cual actúa un operador. Un operando
puede ser un objeto, un valor literal o una expresión. Una expresión por
otra parte es una secuencia de operadores y operandos que realizan una
combinación de alguna de estas acciones:

- Calcular un valor.
- Invocar un objeto o función.
- Generar efectos secundarios.

Una expresión puede ser simple cuando contiene una sola expresión o
puede incluir varias sub-expresiones como parte de una expresión más
compleja.

### Expresiones simples

Una expresión simple (o primaria) no incluyen ninguna expresión o
sub-expresión.

``` C++
// Expresiones simples
int numero{0};                               // Asignación uniforme
numero = 10;                                 // Expresión de asignación
++numero;                                    // Expresión prefijo     
numero++;                                    // Expresión postfijo   
(numero)                                     // Expresión de evaluación
(numero < 0)                                 // Expresión de comparación
(numero & 1)                                 // Expresión a nivel de bits 
int cuadrado = (numero * numero);            // Expresión aritmética
auto sumar = [] (auto ...args) {             // Expresión lambda
    return (... + args);                     // Expresión plegable
};  
int total = sumar(10, 13, 20);               // Llamada de función
```

Una expresión simple puede ser una expresión entre paréntesis, un valor
o un nombre, pero también puede ser:

- Un valor literal.
- Un identificador sin calificar.
- Un identificador calificado explícitamente.
- Una expresión prefijo o postfijo.
- Una expresión lambda.
- Una expresión plegable (o fold).
- Una expresión de requisitos.

Cualquier expresión entre paréntesis es clasificada como una expresión
primaria. Los paréntesis preservan el valor, el tipo y la categoría de
valor de la expresión. Una expresión entre paréntesis puede aparecer en
cualquier lugar, en donde se espere una expresión.

Una expresión simple es el componente básico para crear expresiones más
complejas.

``` C++
#include <iostream>                   // cout, endl

// Calcula el valor de log10 usando la serie de Taylor
double log_10(double x) 
{
    if (x <= 0) {
        std::cerr << "Error: log10 solo está definido " 
                  << "para valores positivos.\n";
        return -1;
    }

    double aproximacion = 0.0;
    double base = (x - 1) / (x + 1);
    double termino = base;
    double exponente = 1;

    // Más iteraciones mejoran la precisión
    for (int i = 0; i < 20; ++i) { 
        aproximacion += (termino / exponente);
        termino *= base * base;
        exponente += 2;
    }

    // Divide ln(10) (~2.302585) para obtener log10(x)
    return 2 * aproximacion / 2.302585; 
}

// Calcula el valor de log10
int main() 
{
    double numero = 0.0;
    std::cout << "Introduce un número para calcular el log10: ";
    std::cin >> numero;
    double resultado = log_10(numero); 
    if (resultado != -1) {
        std::cout << "log10(" << numero << ") = " << resultado << std::endl;
    }

    return EXIT_SUCCESS;
}
```

Este código calcula `log_10` utilizando una aproximación basada en la
serie Matemática de Taylor para el logaritmo natural `ln(x)`, usando un
enfoque iterativo. Esta implementación es una aproximación y puede
requerir más iteraciones para mejorar la precisión. La biblioteca
estándar `<cmath>` contiene una función llamada `log10()` que hace esta
tarea de forma más eficiente.

### Expresiones complejas

Cuando una expresión incluye varias expresiones dentro de una misma
expresión, entonces es una expresión compleja. Una expresión compleja
puede evaluar dos o más condiciones en una misma evaluación:

``` C++
#include <cmath>                          // sqrt
#include <iostream>                       // cout

// Devuelve la distancia entre dos puntos
double dist(double x1, double y1, double x2, double y2)
{
    return std::sqrt((x2 - x1) * (x2 - x1) + (y2 - y1) * (y2 - y1));
}

// Calcula la distancia entre dos puntos
int main()
{  
    double distancia = dist(100.0, 100.0, 300.0, 300.0);
    std::cout << distancia << std::endl;   // 282.843

    return EXIT_SUCCESS;
}
```

Una expresión compleja puede contener varias sub-expresiones como parte
de una misma expresión. El valor de la expresión es el resultado total
de la evaluación.

``` C++
// El uso de paréntesis controla el orden de evaluación 
std::sqrt((x2 - x1) * (x2 - x1) + (y2 - y1) * (y2 - y1));
```

La evaluación de una expresión (o una sub-expresión) por lo general
incluye (si, es el caso) la determinación de identidad de un objeto y la
obtención de un valor asignado previamente a otro y el inicio de efectos
secundarios.

El orden de evaluación de las expresiones no está determinado por el
estándar, sin embargo la evaluación de una expresión es determinada por
la presencia de alguna de estas condiciones:

1.  El uso de paréntesis que indican un orden de evaluación específico.
2.  La naturaleza de los operadores involucrados en la expresión
    (asociatividad).
3.  El orden de posición de operandos y operadores (precedencia).

La precedencia de operadores especifica el orden de evaluación de las
operaciones en expresiones que contienen más de un operador. La
asociatividad especifica la forma de evaluar una expresión que contiene
varios operadores con la misma prioridad y los paréntesis se usan para
cambiar el orden de evaluación de la expresión.

## Operandos y operadores

Un operador es un símbolo que acepta como entrada uno o más operandos.
Un operando puede ser una expresión, un valor literal o cualquier objeto
que devuelva un valor. Los operadores pueden ser unarios, binarios y
ternarios, según actúen sobre uno, dos o tres operandos respectivamente.
Pueden tener notación prefija o posfija que se define por la posición en
que se coloca el símbolo respecto a su operando. Los operandos pueden ir
a la derecha o a la izquierda según su asociatividad y dependiendo del
nivel de precedencia pueden cambiar el resultado de evaluación de una
expresión.

### Expresiones y operadores

Una expresión se forma de operadores y operandos:

``` C++
Expresión
    |
+------+------+
|             |
v             v
int total = 200 + 100 * 100;       // 10200
^   ^     ^ 
|   |     |   
|  Operadores 
|              
Operando    
```

En esta declaración se usan dos operadores binarios. El operador de
suma, el signo de `+` y el operador de multiplicación `*` para crear la
expresión: `200 + 100 * 100`. El resultado de la evaluación es `10200`,
ya que la multiplicación tiene precedencia sobre la suma.

La expresión equivale a usar:

``` C++
200 + (100 * 100);                 // La multiplicación ocurre primero
```

Para cambiar el orden de evaluación se usan los paréntesis:

``` C++
int total = (200 + 100) * 100;    // 3000. La suma ocurre primero
```

Un operando puede ir del lado izquierdo, del lado derecho o en ambos
lados, dependiendo de la asociatividad del operador. Inclusive se pueden
usar expresiones con un solo operador.

``` C++
int a{1};           
a++;                               // El operador de incremento posfijo.
++a;                               // El operador de incremento prefijo.
```

La asociatividad de operadores se refiere a la forma de evaluar un
valor.

``` C++
int a = 10;                        // Asigna un valor a la variable a
a = a + 20;                        // Suma y asigna (en dos operaciones)
a += 20;                           // Suma y asigna (en una misma operación)
```

La asociatividad, puede ser de izquierda a derecha o de derecha a
izquierda.

``` C++
#include <iostream>               // cout
#include <vector>                 // vector

// Función que devuelve la media de un vector de enteros
double calcular_media(const std::vector<int>& datos) 
{
    double suma = 0;
    for (int numero : datos) {
        // Suma y asigna el número entero de cada elemento del vector
        suma += numero;
    }

    // Divide el total de la suma entre el número de elementos del vector
    return suma / datos.size();
}

// Calcula el valor medio de un vector
int main()
{  
    std::vector rango {100, 90, 80, 90, 70, 50, 80, 85};
    double media = calcular_media(rango);
    std::cout << media << std::endl;    
    return EXIT_SUCCESS;         // 80.625
}
```

Por ejemplo, el operador de suma y asignación `+=` tiene una evaluación
de izquierda a derecha, mientras que los operadores aritméticos como la
suma o la resta, tienen una asociatividad de derecha a izquierda.

### Clasificación de operadores

Los operadores que requieren un solo operando, como el operador de
incremento `++` se denominan **operadores unarios**. Los operadores que
requieren dos operandos como los operadores aritméticos `+, -, *, /` se
denominan **operadores binarios**. Un **operador ternario**, como el
operador condicional `?:` o el operador de tres vías `<=>`, utilizan
tres operandos y son los únicos operadores ternarios de C++. Aparte de
esta sencilla clasificación, los operadores pueden clasificarse de
acuerdo a la operación y al valor que devuelven.

  ------------------------------------------------------------------------
  Operaciones    Operadores                  Descripción
  -------------- --------------------------- -----------------------------
  Aritméticas    `+ - * / %`                 Calculan valores numéricos

  De asignación  `= += -= *= %= /= &=`       Asignación de valores
                 `|= ^= <<= >>=`             

  Relacionales   `== < > <= >= != <=>`       Comparan relaciones boléanas

  Lógicas        `&& || !`                   Comprueban valores boléanos

  Bit a bit      `& | ^ ~ << >>`             Manipulaciones a nivel de bit

  Conversiones   static_cast, dinamic_cast   Conversiones entre diferentes
                 reinterpret_cast,           tipos de datos.
                 const_cast                  

  De acceso      `:: . *  &  .* -> ->* []`   Acceso a objetos y
                                             direcciones de (memoria)
  ------------------------------------------------------------------------

  : Clasificación de operadores de acuerdo a su operación

La mayoría de los operadores se representan usando caracteres de
puntuación, como `+`, `-`, `*`, `&`, sin embargo existen algunos
operadores especiales que son representados por palabras clave como:

- `static_cast` convierte un tipo a otro tipo (si es compatible).
- `dynamic_cast` convierte una clase base virtual a una clase derivada.
- `const_cast` convierte entre tipos que usan calificadores const.
- `reinterpret_cast` convierte un tipo en otro tipo (al estilo C).
- `new` asigna memoria.
- `delete` desasigna memoria.
- `sizeof` devuelve el tamaño de un tipo.
- `sizeof...` devuelve el tamaño de un paquete de parámetros.
- `typeid` devuelve la información de un tipo.
- `noexcept` comprueba si una expresión puede lanzar excepciones.
- `alignof` consulta los requisitos de alineación de un tipo.

Estos operadores funcionan igual que los caracteres de puntuación, solo
que tienen una sintaxis más explícita. Algunos operadores como `typeid`,
`alignof` y `sizeof` crean expresiones que no se evalúan, ya que estos
operadores consultan las propiedades de sus operandos en tiempo de
compilación.

### Precedencia de operadores

La precedencia define el orden de evaluación de las operaciones que
contienen diferentes operadores. La precedencia de operadores no está
directamente especificada por el estándar, pero se puede derivar de la
sintaxis.

  ------------------------------------------------------------
  Operadores                            Asociatividad
  ------------------------------------- ----------------------
  `::  (resolución de ámbito)`          Ninguna

  `() [] -> .  (acceso)`                Izquierda a derecha

  `! ~ ++ -- \*(tipo) sizeof`           Derecha a izquierda

  `\* / %  \+ \-`                       Izquierda a derecha

  `<< >> <=> < <= > >= == !=`           Izquierda a derecha

  `& ^ \| (bit a bit)`                  Izquierda a derecha

  `&& \|| (lógicas)`                    Izquierda a derecha

  `?; (operador ternario)`              Derecha a izquierda

  `= += -= *= /= %= &= ^= \|= <<= >>`   Derecha a izquierda

  `, (la coma)`                         Izquierda a derecha
  ------------------------------------------------------------

  : Precedencia y asociatividad de operadores

Los operadores se asocian a una expresión a la izquierda o a la derecha.
Según la tabla, los operadores que están en la misma línea están
ordenados en \"orden de precedencia decreciente\", por ejemplo en la
fila, `*`, `/`, `%` , `+`, `-` la mayor precedencia la tiene la
multiplicación y la menor precedencia la tiene la resta.

### Asociatividad de operadores

En la tabla anterior, la columna dos especifica una característica
llamada asociatividad de un operador. La asociatividad especifica el
orden en el cual se realizan las operaciones con igual precedencia. Las
operaciones de `izquierda a derecha` se evalúan empezando con el valor
que está a la izquierda, por ejemplo la resta:

``` C++
// tres variables con el mismo valor
int x = 10, y = 10, z = 10;

// a, z se le resta el valor de (x - y) 
int w = x - y - z;                         // -10
```

Equivale a esta expresión:

``` C++
int w = (x - y) - z;                       // -10   
```

Y a esta otra:

``` C++
int w = ((x - y) - z);                     // -10
```

Mientras que en una operación de derecha a izquierda, la evaluación
inicia con el operador a la derecha:

``` C++
int x = 10;        
x *= 2;                                   // 20 

// Es equivalente a multiplicar (2 * x) y asignar el resultado a x.
x = x * 2; 
```

Los paréntesis permiten controlar la asociatividad de un operador, pero
no pueden cambiar el orden de evaluación de un operador. Cuando se usan
varios operadores en una misma expresión, el compilador sigue un orden
específico, para que la evaluación devuelva el mismo resultado cada vez
que se evalúa la expresión. Por ejemplo, en el siguiente código, primero
se evalúan las dos expresiones entre paréntesis, luego se multiplican
los resultados y por último se lleva a cabo la suma:

``` C++
int a = 2;
int b = (a + 4) * (a / 4) + 10;            // 10
```

Al igual que las expresiones Matemáticas, los paréntesis solo son
necesarios si la expresión se evalúa de forma diferente a como se haría
de acuerdo al orden de las operaciones.

``` C++
int b = a + 4 * a / 4 + 10;                // 14
```

El orden de evaluación de los operadores puede ser de izquierda a
derecha o de derecha a izquierda, pero el orden de evaluación de las
expresiones es indefinido.

La precedencia y asociatividad de los operadores es definida
completamente por el estándar, pero el orden de evaluación de las
expresiones contiene algunas excepciones que se conocen como
\"comportamiento indefinido\". A menos que la definición de un operador
garantice que sus operandos se evalúan en un orden específico, el
compilador puede evaluar los operandos y otras sub-expresiones en
cualquier orden y puede escoger un orden diferente cuando la misma
expresión se evalúe de nuevo. Esto puede parecer contradictorio, pero
existe una buena razón para hacer esto: optimización.

### Comportamiento indefinido

El manejo de desbordamientos, variables sin inicializar, errores de
división entre 0, y otras condiciones de error en la evaluación de
expresiones provocan un comportamiento indefinido. Un **comportamiento
indefinido** se da cuando el estándar no define, ni impone un requisito
para algún tipo de operación especial, como desbordamientos de enteros,
variables sin inicializar, índices fuera de límites etc. Cuando el
compilador detecta una comportamiento indefinido puede hacer lo que sea,
sin embargo la mayoría de los compiladores lanzan una advertencia o
terminan la compilación.

El caso más común de un comportamiento indefinido son las variables sin
inicializar:

``` C++
int x;                    // Comportamiento indefinido x no tiene valor
static int y;             // Comportamiento definido y = 0  
std::cout << x;           // Comportamiento indefinido
std::cout << y;           // Bien y = 0. Comportamiento definido. 
```

Tratar de acceder al valor de una variable no definida genera un
comportamiento indefinido. En muchos casos el error puede pasar
desapercibido, porque el compilador puede asignarle cualquier valor a la
variable. La excepción a la regla son las variables calificadas como
`static`, estas variables tienen un valor inicializador, normalmente
`0`, así que su comportamiento es definido.

La división entre cero produce un comportamiento indefinido:

``` C++
int x = 10 / 0;          // Comportamiento indefinido
```

La división entre `0` Matemáticamente no está definida por lo que no
tiene sentido que devuelva un resultado.

``` C++
double x =  0.0f / 0.0;
double y =  10.0 / 0.0;
double z = -10.0 / 0.0;

std::cout << x << '\n'    // x es nan  (nan = no es un numero) 
          << y << '\n'    // y es inf  (inf = infinito)
          << z;           // z es -inf (-inf = -infinito)
```

Sin embargo la división de punto flotante entre cero devuelve un valor
especial llamado `nan` (si el numerador es `0.0f`), y un valor `inf`
(infinito positivo), si el numerador es positivo o `-inf` (infinito
negativo) si el numerador es negativo.

#### Punto de secuencia

La evaluación de una expresión puede producir efectos secundarios. Un
efecto secundario se produce al acceder a un objeto, modificarlo, llamar
una función o usar alguna utilidad de la biblioteca. Si un efecto
secundario en una misma secuencia de evaluación, depende de evaluar otro
efecto secundario en el mismo objeto, el comportamiento es indefinido.

``` C++
i = i++ + 2;              // Comportamiento indefinido
x = ++x + x++;            // Comportamiento indefinido
```

Usar y modificar el mismo valor en un mismo punto secuencial, también es
un comportamiento indefinido.

:::: tip
::: title
Tip
:::

**¿Qué es un punto de secuencia?**

El estándar especifica que un punto de secuencia es el punto en que
todos los efectos secundarios de la evaluaciones anteriores se han
completado y no queda ningún efecto secundario que aplicar de
evaluaciones posteriores.
::::

Por lo general, siempre es bueno evitar expresiones en las que se
modifiquen los mismos valores en la misma expresión, ya que el
comportamiento depende de la implementación del compilador o de la
optimización utilizada. Estos errores se pueden cometer fácilmente, ya
que una expresión puede ser al mismo tiempo una asignación.

``` C++
int x = 0;
std::cout << x << " " << ++x; // comportamiento indefinido
```

En este caso, el compilador puede evaluar la expresión `++x` antes de
evaluar `x`, en este caso la salida será `1 1`, o puede evaluar primero
`x`, en cuyo caso la salida será `0 1`, o puede devolver cualquier
valor. Porque la expresión tiene un comportamiento indefinido esto
significa que el programa tiene un error, no importa si el código
compila o no.

Hay cuatro operadores que garantizan el orden en el cual los operandos
son evaluados. El primero es el operador lógico `&&` (AND) que garantiza
que el operando izquierdo sea evaluado primero. También garantiza que el
operando de la derecha sea evaluado sólo si el operando de la izquierdo
es verdadero. Los otros tres operadores que garantizan el orden de
evaluación de expresiones son los operadores `||` (O), el operador
condicional `? :` y el operador coma.

## Operadores aritméticos

Los operadores aritméticos realizan operaciones comunes como la suma, la
resta, la multiplicación, la división y el módulo. En estas operaciones
simplemente se evalúan los operadores y se devuelve el valor de la
evaluación. Algunos operadores como el signo de `+` y el signo de menos
`-`, pueden usarse de forma unaria y binaria, mientras que los
operadores de incremento y decremento solo se pueden usar como
operadores unarios.

  ----------------------------------------------------------------
  Operador   Nombre                Sintaxis        Sobrecargable
  ---------- --------------------- --------------- ---------------
  `+`        Suma                  a + b;          Si

  `-`        Resta                 a - b;          Si

  `*`        Multiplicación        a \* b;         Si

  `/`        División              a / b;          Si

  `%`        Restante o módulo     a % b;          Si

  `++`       Incremento prefijo    `++a;`          Si

  `--`       Decremento prefijo    `--a;`          Si

  `++`       Incremento postfijo   `a++;`          Si

  `--`       Decremento postfijo   `a--;`          Si
  ----------------------------------------------------------------

  : Operadores aritméticos.

Los operadores aritméticos siempre devuelven valores numéricos, aunque
no siempre del mismo tipo. Una operación entre enteros siempre devuelve
un valor entero, pero una operación entre tipos diferentes, devuelve el
tipo de mayor precision.

``` C++
#include <iostream>                  // cout

// Programa que evalúa expresiones aritméticas (de tipo double)
int main()
{
    std::cout << "Calculadora aritmética\n";

    while(true) {
        double num1, num2, resultado = 0.0;        
        char operador;             
        std::cout << ">>> ";
        std::cin >> num1 >> operador >> num2;

        // Soporta operaciones básicas: +, -, *,  %, /   
        if (operador == '+') {
            resultado = num1 + num2;
        } else if (operador == '-') {
            resultado = num1 - num2;
        } else if (operador == '*') {
            resultado = num1 * num2;
        } else if (operador == '%') {
            resultado = static_cast<int>(num1) % static_cast<int>(num2);
        } else if (operador == '/') {
            // La división entre 0 provoca un comportamiento indefinido
            if (num2 == 0)  {
                std::cerr << "Error: división entre 0.";
                break;
            } 
            resultado = num1 / num2;  
        } else {
           std::cout << "Operación no soportada.";
           break;
        }

        std::cout << num1 << " " << operador  << ' '
                  << num2 << " = " << resultado << '\n';        

        std::cin.clear();       // Limpia los valores de la entrada
        std::cin.get();         // Espera a que se presione una tecla
    }

    return EXIT_SUCCESS;
}
```

Cuando se suman dos valores enteros, el resultado es un valor entero,
pero cuando se suma un valor entero y un valor decimal, C++ usa el tipo
de mayor precisión para devolver el resultado más exacto. A este proceso
se le conoce como **conversión aritmética**. Esto ocurre con la resta y
la multiplicación.

``` C++
std::cout << 5/2   << '\n'     // 2 valor entero. El resultado se trunca.
          << 5/0   << '\n'     // Error: División entre cero.
          << 5.0/2 << '\n';    // 2.5, entero y decimal = decimal. 
```

Al dividir dos enteros `5/2` el resultado es 2, no 2.5, pero al dividir
`5.0/2`, el resultado es 2.5. Porque en tipos numéricos, el compilador
siempre devuelve el tipo de mayor precisión. Como es usual, la división
entre `0` produce un comportamiento indefinido.

``` C++
5 / 0;        // Error: División entre cero
```

El operador modulo `%` devuelve el residuo de la división, del primer
operando entre el segundo.

``` C++
5 % 2;        // 1  
```

Algunos operadores como el signo de `+`, tienen un comportamiento
distinto, dependiendo del tipo de valor sobre el que actúen. En tipos
numéricos realiza una suma, pero en cadenas de texto concatena los
caracteres. A este comportamiento se le conoce como sobrecarga de
operadores.

``` C++
std::string a{"uno"};
std::string b{"dos"};
std::string c = a + b;            // c = "unodos"
```

Todos los operadores aritméticos pueden sobrecargarse. La **sobrecarga**
es una propiedad que describe un procedimiento estándar que consiste en
utilizar el mismo nombre de una función o un mismo operador para
representar operaciones similares que se comportan de modo distinto
cuando se aplican a diferentes tipos o clases de objetos. Muchos de los
operadores de C++, usan la sobrecarga, como los operadores de entrada y
salida: `>>`, `<<` que también realizan operaciones de bits.

### El operador de incremento

El operador de incremento `++` es un operador unario (aunque con doble
signo) que espera un valor izquierdo (una variable, un elemento de un
arreglo o la propiedad de un objeto), también espera que el operando sea
un valor entero. Su tarea es sumar 1 al número, después asignar el valor
incrementado a la variable, elemento o propiedad, en cada nueva
evaluación.

El operador de incremento puede aparecer de dos formas:

1.  Notación ++prefijo (pre-incremento).
2.  Notación postfijo\-- (post-incremento).

El valor devuelto por la operación, depende de la posición relativa al
operando. Cuando se usa antes del operando, incrementa el valor y
devuelve el valor incrementado. Cuando se usa después del operando
incrementa el valor, pero devuelve el valor sin incrementar.

#### Pre-incremento

En la notación prefijo (a la izquierda), el operador `++` crea una
operación de pre-incremento.

``` C++
#include <iostream>                      // cout

// El operador de incremento con notación ++prefijo 
int main() 
{ 
    int cont{0};
    std::cout << ++cont << '\n';         // 1
    std::cout << ++cont << '\n';         // 2
    std::cout << ++cont << '\n';         // 3

    return EXIT_SUCCESS;              
}
```

El resultado de la operación es el valor del operando después de haber
sido incrementado el valor en 1.

#### Post-incremento

En la notación postfijo (a la derecha) el operador `++` crea una
operación de post-incremento. El resultado de la operación es el valor
del operando antes de haber sido incrementado el valor.

``` C++
#include <iostream>                      // cout

// El operador de incremento con notación postfijo++
int main() 
{ 
    int cont{0};
    std::cout << cont++ << '\n';         // 0
    std::cout << cont++ << '\n';         // 1
    std::cout << cont++ << '\n';         // 2

    return EXIT_SUCCESS;
}
```

Un uso muy común del operador `++` es usarlo para incrementar un
contador dentro de un bucle `for`.

``` C++
#include <iostream>                      // cout
#include <iomanip>                       // setw

// El operador de pre-incremento con notación ++prefijo 
int main() 
{ 
    // Imprime el valor de i 100 veces
    for (int i = 1; i <= 100; ++i) {
        std::cout << std::setw(3) << i << ' ';
        if (i % 10 == 0) {
            std::cout << '\n';          // Agrega una línea cada 10 número
        }        
    }
    return EXIT_SUCCESS;              
}
```

En este ejemplo, con cada iteración el valor de `i` se incrementa en 1,
hasta que alcanza el valor de 100, y el bucle `for` termina.

**Usa la notación pre-fijo**

La notación post-fijo no es común en C++:

``` C++
// En C++ no es común la notación post-fijo
for (int i = 0; i <= 100; i++) {
    std::cout << i << '\n';
}
```

Por lo general el operador de pre-incremento es más rápido que su
contraparte, ya que no requiere hacer una copia del objeto.

``` C++
// En C++ se usa la notación pre-fijo
for (int i = 0; i <= 100; ++i) {
    std::cout << i << '\n';
}
```

Aunque muchos compiladores modernos, pueden optimizar estos dos bucles
en el mismo código ensamblador, una práctica común en C++ es preferir el
uso de la notación pre-fijo `++i` sobre la notación post-fijo `i++`.
Tanto en la versión prefijo como en la versión postfijo, la evaluación
afecta al operando. La principal diferencia entre las dos notaciones es
cuando ocurre el incremento de la expresión. En la notación prefijo, el
incremento ocurre antes de que el valor se utilice en la evaluación de
la expresión, así que el valor de la expresión es diferente al valor del
operando. En la notación postfijo, el incremento ocurre después de que
el valor se utiliza en la evaluación de la expresión, así que el valor
de la expresión es igual que el valor del operando.

### El operador de decremento

El operador de decremento `--` es un operador unario (con doble signo)
que espera un valor izquierdo (una variable, un elemento de un arreglo o
una propiedad de un objeto), también espera que su operando sea un valor
entero. Su tarea es disminuir en 1 el valor y asignar el valor
decrementado a la variable, elemento o propiedad, en cada nueva
evaluación.

El operador de decremento puede aparecer de dos formas:

1.  Notación \--prefijo (pre-decremento).
2.  Notación postfijo\-- (post-decremento).

Al igual que el operador de incremento el resultado depende de la
posición del operador.

#### Pre-decremento

En la notación prefijo el operador `--` ejecuta una operación de
decremento.

``` C++
#include <iostream>                      // cout

// El operador de decremento con notación --prefijo 
int main() 
{ 
    int cont{10};
    std::cout << --cont << '\n';         // 9
    std::cout << --cont << '\n';         // 8
    std::cout << --cont << '\n';         // 7

    return EXIT_SUCCESS;
}
```

El resultado de la operación es el valor del operando \"después\" de
haber sido decrementado en 1.

#### Post-decremento

En la notación postfijo el operador `--` ejecuta una operación de
decremento.

``` C++
#include <iostream>                      // cout

// El operador de decremento con notación postfijo-- 
int main() 
{ 
    int cont{10};
    std::cout << cont-- << '\n';         // 10
    std::cout << cont-- << '\n';         //  9
    std::cout << cont-- << '\n';         //  8

    return EXIT_SUCCESS;              
}
```

El resultado de la operación es el valor del operando \"antes\" de haber
sido decrementado el valor. Cuando el operador `--` se usa antes del
operando (a la izquierda), decrementa y devuelve el valor decrementado.
Cuando se utiliza después (a la derecha), decrementa el valor del
operando, pero devuelve el valor sin decrementar (o el anterior).

Ya que los operadores de incremento y decremento producen efectos
secundarios, usar expresiones que usen estos operadores en macros del
preprocesador puede devolver resultados inesperados.

## Operadores de asignación

C++ usa el operador de asignación `=` para guardar un valor en un objeto
que tenga una dirección, como un valor izquierdo que no sea constante.

``` C++
// La expresión del lado derecho del signo igual puede ser un valor 
// temporal, o un valor que tenga una dirección y la expresión del 
// lado izquierdo debe tener una dirección y no estar calificado 
// como const o constexpr. Es decir, no deve ser un valor constante.
variable = valor;
```

Para evaluar una asignación el compilador evalúa el lado derecho de la
expresión y guarda el resultado en la dirección que obtiene al evaluar
el lado izquierdo. Por ejemplo la expresión:

``` C++
int a;
a = 1;
```

Guarda el entero `1` en la dirección de la variable `a`. La expresión de
asignación devuelve un valor, el cual es el mismo que se guarda en la
variable.

Una misma variable puede estar en ambos lados de una asignación:

``` C++
int a{0};
a = a + 1;
```

Esta expresión incrementa la variable `a` en uno. Es equivalente a usar:

``` C++
++a;
```

o:

``` C++
a += 1;
```

Una expresión de asignación puede inicializar varias variables con
diferentes valores usando el operador coma en una evaluación secuencial:

``` C++
int a, b, c;
a = 10, b = 20, c = 30;
```

Aunque declarar varias variables en una misma declaración es considerado
una mala práctica, juntar variables relacionadas en una misma
declaración puede hacer el código más fácil de entender y de usar:

``` C++
int r, g, b;
Color(r, g, b);
```

Una expresión de asignación directa crea un nuevo valor copiándolo, pero
deja el valor de la fuente intacto:

``` C++
int a{200};   // Inicialización uniforme
a = 100;      // Asigna el valor 100 a la variable "a".
int b;        // Variable no inicializada
b = a;        // Asignación por copia
(a == b);     // true: las dos variables son iguales
```

La copia por asignación, remplaza el contenido del objeto `b` con una
copia de `a` (pero `a` no se modifica).

``` C++
int n = 0;    // La inicialización de una variable, no es una asignación.
n = 1;        // Asignación directa
n = 1.1;      // Conversión a entero y asignación
n = 'b';      // Conversión de char a entero y asignación
```

El operador de asignación `=` espera que el operando del lado izquierdo
sea un valor izquierdo modificable; es decir una variable, una propiedad
o un objeto. Espera también que el operador del lado derecho, sea un
valor compatible con el tipo y que se pueda convertir implícitamente al
tipo que espera el operando izquierdo (antes de copiarlo). Si los tipos
no son compatibles, el compilador lanza un error.

``` C++
string a = "A";         // La inicialización, no es una asignación.
a = "B";                // Asignación directa
string& r = a;          // Una referencia a un valor-l
string* p;              // Un puntero
r = a + "+";            // Asignación usando una referencia
string&& rr = r + "+";  // Una referencia a un valor-r
a = rr;                 // Asignación usando una referencia a un valor-r  
p = &a;                 // Asignación directa a través de un puntero 
```

Cuando el operando izquierdo es una referencia, la asignación modifica
el objeto al cual hace referencia la variable. Cuando el operando
izquierdo es un puntero, la asignación es directa. En tipos compuestos
como las clases, la asignación devuelve un objeto especial llamado
`*this`. El objeto `this` es un miembro especial que representa al
objeto devuelto.

### Moviendo valores

Un valor puede ser copiado, pero también puede ser movido o
intercambiado entre objetos del mismo tipo. La asignación por
movimiento, mueve el valor de un objeto a otro:

``` C++
std::string a;
std::string b; 
a = "Hola";                // Asigna "Hola" a la variable a.
std::cout << a;            // La cadena a es "Hola".
b = std::move(a);          // Asignación por movimiento.
std::cout << b;            // La cadena b es "Hola".    
std::cout << a;            // La cadena a esta vacia.
```

La asignación por movimiento, transfiere los recursos del objeto \"a\"
al objeto \"b\", dejando al objeto \"a\", en un estado valido, aunque
nulo.

En tipos simples, la asignación por copia y por movimiento es idéntica y
se denomina \"asignación directa\". En tipos compuestos, la asignación
por movimiento modifica el contenido del objeto \"a\" con el contenido
del objeto \"b\", dejando al objeto \"a\", listo para ser destruido, o
para asignarle un nuevo valor, pero no se puede usar su valor por que ha
sido movido.

### Intercambio de valores

Si la función `move()`, mueve un objeto, la función `swap()` intercambia
los valores de dos objetos:

``` C++
int b = 100;               // Intercambia los valores
int c = 200;               // de b y c.
std::swap(b, c);
std::cout << c;            // ahora b es 200 y c es 100
```

El intercambio de valores solo funciona en objetos del mismo tipo que no
sean constantes.

Tanto `move`, como `swap` están definidos en el archivo de cabecera
`<utility>`. Por lo general, un tipo simple siempre debe copiarse,
pasarse por valor o referencia, mientras que un tipo compuesto puede
copiarse, pasarse por referencia, o puede moverse (si es muy grande).
Usar la semántica de movimiento en tipos simples no tiene sentido.

### Asignaciones múltiples

Aunque las expresiones de asignación son usualmente muy sencillas,
algunas veces es necesario usar el valor de una expresión de asignación
como parte de una expresión más compleja. Por ejemplo, se puede asignar
y comprobar un valor en una misma expresión:

``` C++
int a;
int b = 1;
bool resultado = (a = b) == 0;
std::cout << resultado;          // false (a y b son 1)  
```

El operador `=` tiene una asociatividad de derecha a izquierda, lo que
quiere decir que cuando se hace una asignación múltiple, las expresiones
se evalúan comenzando con el valor que está a la derecha. Esto permite
hacer varias asignaciones de esta forma:

``` C++
int a, b, c;
a = b = c = 100;                 // Inicializa 3 variables con valor 100.
```

El valor de una expresión de asignación es el valor del operando del
lado derecho. Si este tiene efectos secundarios, el operador `=` asigna
el valor de la derecha a la variable del lado izquierdo, para futuras
referencias al valor de la variable.

### Asignación compuesta

Además del clásico operador de asignación, C++ soporta varias
combinaciones que usan el operador de asignación mediante atajos,
combinando la asignación con otra operación. Por ejemplo el operador
`+=` realiza una suma y una asignación al mismo tiempo.

  ------------------------------------------------------------
  Operador       Ejemplo        Equivale a     Sobrecargable
  -------------- -------------- -------------- ---------------
  +=             a += b         a = a + b      Si

  -=             a -= b         a = a - b      Si

  `*=`           `a *= b`       a = a \* b     Si

  `/=`           `a /= b`       `a = a / b`    Si

  %=             a %= b         a = a % b      Si
  ------------------------------------------------------------

  : Operadores aritméticos de asignación compuesta.

En una asignación compuesta se realiza la operación aritmética antes de
almacenar el resultado en el mismo objeto que se modifica.

``` C++
#include <iostream>                 // cout

// Suma un rango de números elevados al cuadrado
int main()
{
    const int limite{64};
    int total{0};
    int num{1};

    // Imprime una progresión de números enteros
    while (num <= limite) {
        total += num * num;         // Suma y asigna el número al cuadrado
        ++num;                      // Incrementa la variable de control
        std::cout << total << ' ';  // 1 5 14 30 55 91 140 204 285 385...
    }

    return EXIT_SUCCESS;    
}
```

En la expresión `num <= limite` se usa el operador `<=` (menor o igual
que). El resultado de la expresión es un tipo boléano. Una expresión de
tipo boléano únicamente devuelve dos valores: `true` o `false`. La
sentencia `while` se ejecuta mientras la expresión entre paréntesis sea
verdadera. La condición es comprobada antes de ejecutar de nuevo el
bucle.

La expresión:

``` c++
total += num * num;
```

Es equivalente a usar:

``` c++
total = total + num * num;          // La multiplicación ocurre primero.
```

La diferencia entre estas dos expresiones es que el operador `+=`
realiza una suma y una asignación en una misma operación. Y la expresión
de incremento prefijo:

``` c++
++num;
```

incrementa el valor de la variable `num` en 1. La expresión es
equivalente a usar:

``` c++
num = num + 1;
```

El comportamiento de la asignación compuesta es idéntico a la asignación
simple, excepto que la expresión compuesta se evalúa solo una vez y se
comporta como una sola operación.

## Operadores relacionales

Los operadores de comparación se utilizan para comprobar la veracidad o
falsedad de una relación entre dos o más operandos. Las expresiones que
los contienen se denominan expresiones relacionales. Una expresión
relacional puede comparar diferentes expresiones, sin embargo el
resultado de la evaluación siempre da como resultado un valor boléano.

### Tipos de operadores relacionales

C++ contiene 6 operadores binarios que realizan las típicas operaciones
relacionales y un operador adicional que tiene un funcionamiento
especial.

  --------------------------------------------------------------
  Operador    Nombre               Sintaxis      Sobrecargable
  ----------- -------------------- ------------- ---------------
  \<          Menor que            a \< b        Si

  \>          Mayor que            a \> b        Si

  \<=         Menor o igual que    a \<= b       Si

  \>=         Mayor o igual que    a \>= b       Si

  ==          Igual que            a == b        Si

  !=          No es igual que      a != b        Si

  \<=\>       Nave espacial        a \<=\> b \<  Si
                                   0             
  --------------------------------------------------------------

  : Operadores relacionales.

Un operador relacional puede comparar tipos aritméticos como `int`,
`long`, `char`, `double`, `float` y `bool`, punteros, enumeraciones,
cadenas, inclusive contenedores de la biblioteca estándar como vectores.
Los tipos definidos por el usuario deben definir explícitamente estas
operaciones. En tipos triviales se puede utilizar el operador de tres
vías para que el compilador sintetice estas operaciones de forma
automática.

### Operaciones relacionales

Los operadores relacionales comparan el orden relativo (numérico y
alfabético) de dos o más expresiones. Esta es su sintaxis:

``` C++
expresión operador expresión;
```

Hay seis operadores relacionales:

1.  El operador `<` \"menor que\" evalúa como `true` (o verdadero) solo
    si la primera expresión es menor que la segunda.
2.  El operador `>` \"mayor que\" evalúa a verdadero solo si la primera
    expresión es mayor que la segunda.
3.  El operador `<=` \"menor o igual que\" evalúa a verdadero solo si la
    primera de las dos expresiones es menor o igual que la segunda.
4.  El operador `>=` \"mayor o igual que\" evalúa a verdadero solo si la
    primera de las expresiones es mayor o igual que la segunda.
5.  El operador `==` \"igual que\" evalúa a `true` solo si los valores
    de ambas expresiones son iguales.
6.  El operador `!=` \"diferente que\" evalúa a `true` solo si los
    valores de las dos expresiones representan valores diferentes.

Una operador relacional siempre devuelve un valor boléano: `true` o
`false`.

``` C++
#include <iostream>             // cout

// Operaciones relacionales
int main()
{
    int a{1};
    int b{2};
    std::cout << std::boolalpha << "Operaciones Relacionales\n"
              << a << "  < " << b << " = " << ( a < b) <<  '\n'
              << a << "  > " << b << " = " << ( a > b) <<  '\n'
              << a << " == " << b << " = " << (a == b) <<  '\n'
              << a << " <= " << b << " = " << (a <= b) <<  '\n'
              << a << " >= " << b << " = " << (a >= b) <<  '\n'
              << a << " != " << b << " = " << (a != b) <<  '\n';

    return EXIT_SUCCESS;
}
```

Los operadores relacionales son útiles en estructuras de control y
bucles, que necesitan comprobar alguna condición. Por ejemplo en una
sentencia `if`:

``` C++
int a{1};
int b{2};

// Si a es menor que b imprime el valor de a
if (a < b) {
    std::cout << "El valor de a es " << a;
}
```

Se pueden usar los operadores relacionales junto con los operadores
lógicos para comprobar varias condiciones en una misma expresión.

``` C++
// Si a es mayor que 0 y b es mayor que 0.
if (a > 0 && b > 0) {
    std::cout <<  a + b;  // 3
} 
```

Los operadores relacionales se agrupan de izquierda a derecha, así que
el resultado de la operación se encuentra operando de izquierda a
derecha.

La expresión:

``` C++
a > 0 && b > 0         // El operador > tiene mayor precedencia que &&
```

es igual que:

``` C++
(a > 0) && (b > 0)     // Verdadero: si a y b son mayores que 0
```

En la expresión:: `a > 0 && b > 0` se comprueba que los valores de `a` y
`b` sean mayores que 0. El operador lógico `AND` encadena las
expresiones, usando la evaluación de corto circuito. Si la primera
expresión es falsa, devuelve `false`, si no, evalúa la segunda expresión
y devuelve el resultado de esta expresión.

:::: tip
::: title
Tip
:::

**Conversiones aritméticas**

Si los operandos son de tipos diferentes, por ejemplo un tipo `int` y un
tipo `double` se produce una conversión aritmética antes de realizar la
conversión. La conversión usa el tipo de mayor precisión, en este caso
`double` antes de realizar cualquier tipo de comparación. El resultado
de la comparación siempre es verdadero o falso.
::::

Hay que tomar en cuenta que el valor cero, un puntero nulo, un miembro
nulo son todos valores falsos en un contexto boléano, cualquier otro
valor es convertido a verdadero.

:::: warning
::: title
Warning
:::

**Comparaciones exactas entre números reales**

Si se utilizan los operadores relacionales para comparar cantidades
numéricas, hay que recordar que los valores reales no pueden ser
almacenados de forma exacta. A sí que, las expresiones de comparación
como la igualdad, que involucran números reales, a veces se evalúan como
falsas, incluso cuando las cantidades son aparentemente iguales.
::::

Por lo general las comparaciones se deben realizar con objetos del mismo
tipo y crear comparaciones seguras, ya que comparar objetos con tipos
diferentes, puede devolver resultados erróneos.

### Comparaciones seguras

Las comparaciones entre tipos enteros con signo y sin signo son una
fuente constante de errores. Aunque el compilador puede detectar
comparaciones entre enteros con signos diferentes y emitir una
advertencia, muchas veces las advertencias pasan desapercibidas.

``` C++
signed int x = -10;
unsigned int y = 10;

// ¿x es menor que y?
std::cout << x << " < " << y;

// El compilador puede lanzar una advertencia: 
// comparación de enteros con diferente signo.
if (x < y)  {
    std::cout << " Si";      
} else  {
    std::cout << " No";      // Error: -10 < 10 No
}
```

En este ejemplo, la expresión `-10 < 10` se evalúa como falsa, lo que
evidentemente es incorrecto. La razón por la que ocurre esto es debido a
la conversión implícita (y desbordamiento de enteros) que se da cuando
se comparan tipos diferentes. El compilador (gcc) tiene una bandera
especifica para detectar comparaciones entre tipos con diferente signo
llamado: `-wsign-compare`.

La biblioteca estándar contiene varias funciones que pueden realizar
comparaciones de forma segura. Por ejemplo la función `cmp_less`
definida en el archivo de cabecera `<utility>`.

``` C++
signed int a = -10;
unsigned int b = 10;

// ¿a es menor que b?
std::cout << a << " < " << b;

// Usa una comparación segura
if (std::cmp_less(a, b)) {
    std::cout << " Si";     // Bien: -10 < 10 Si
} else {
    std::cout << " No";
}
```

La biblioteca `<utility>` contiene varias utilidades que pueden realizar
tareas de comparación de forma segura. Existe una alternativa para cada
uno de los operadores relacionales:

- `cmp_equal` Equivale al operador `==`.
- `cmp_not_equal` Equivale al operador `!=`.
- `cmp_greater` Equivale al operador \"mayor que\".
- `cmp_greater_equal` Equivale al operador \"mayor o igual que\".
- `cmp_less` Equivale al operadores \"menor que\".
- `cmp_less_equal` Equivale al operador \"menor o igual que\".

Con la ayuda de estas funciones, los errores en operaciones que
involucran comparaciones entre diferentes tipos aritméticos se pueden
evitar.

### Comparación de tres vías

Aparte de los seis operadores relacionales. Existe un operador especial
formado por tres símbolos `<=>` nombrado \"nave espacial\" o de \"tres
vías\". El operador de nave espacial es un operador de comparación de
tres vías, que permite realizar comparaciones relacionales entre tipos
aritméticos.

``` C++
int a{-10};
int b{10};

// Comparaciones de tres vías: que involucran tres
// operandos (a y b deben ser del mismo tipo) 
std::cout << std::boolalpha
          << (a <=> b < 0)  << '\n'    // true
          << (b <=> a > 0)  << '\n'    // true
          << (a <=> b == 0) << '\n'    // false
          << (a <=> b != 0) << '\n';   // true
```

El operador `<=>` devuelve un valor boléano. Si, `a` es menor que `b`
(`a < b`), devuelve un objeto menor que `0`, si, `a` es mayor que `b`
(`a > b`), devuelve un objeto mayor que `0`, y si `a` y `b` son iguales
devuelve un objeto igual que `0`.

:::: tip
::: title
Tip
:::

**¿Por qué se llama nave espacial?**

El origen del nombre se debe a que la figura formada por los tres
símbolos se asemeja a la nave usada en el juego Star Trek creado por
Randal L. Schwartz en Basic. También se dice que se llamó así porque se
parecía al caza de Darth Vader de la saga Star Wars.
::::

El operador de tres vías es un operador que tiene diferentes usos. En
tipos aritméticos realiza las típicas comparaciones relacionales, pero
en tipos compuestos (como clases y estructuras), le pide al compilador
que genera automáticamente una versión sintetizada de cada una de las
operaciones relacionales.

## Operadores lógicos

Los operadores lógicos realizan operaciones booleanas como en Algebra.
Los operadores `AND` y `OR` son usados para extender dos o más
expresiones relacionales y formar expresiones más complejas para
comprobar varias condiciones. Mientras que el operador `NOT` se usa para
invertir el valor boléano de una sola expresión.

  --------------------------------------------------------------------
  Nombre   Operador   Descripción                   Sintaxis
  -------- ---------- ----------------------------- ------------------
  AND      &&         Devuelve true si las dos      expr1 && expr2
                      expresiones son verdaderas,   
                      si no devuelve false.         

  OR       \|\|       Devuelve true si alguna de    expr1 \|\| expr2
                      las dos expresiones es        
                      verdadera, si ambas son       
                      falsas devuelve false.        

  NOT      !          Si la expresión es true,      ! expresión
                      devuelve false. Si la         
                      expresión es false devuelve   
                      true.                         
  --------------------------------------------------------------------

  : Operadores lógicos

Los operadores lógicos `AND` y `OR` son binarios y el operador `NOT` es
unario, pero sin importar el tipo de valor que se evalué en una
expresión, los operadores lógicos siempre devuelven un valor boléano.

### Conjunción lógica

El operador de conjunción lógica `&&` (\"Y\") puede ser comprendido en
distintos niveles. En el nivel más simple, cuándo se usa con operandos
boléanos, realiza la operación booleana `AND` sobre dos expresiones.
Devuelve `true`, únicamente si las dos expresiones son verdaderas.

``` C++
#include <iostream>             // cout

// Usando la conjunción lógica
int main()
{
    bool falso{false};
    bool verdadero{true};

    std::cout << std::boolalpha << "Conjunción lógica\n"
              << "falso && falso         = " << (falso && falso)     << '\n'
              << "falso && verdadero     = " << (falso && verdadero) << '\n'
              << "verdadero && falso     = " << (verdadero && falso) << '\n'
              << "verdadero && verdadero = " << (verdadero && verdadero) 
              << '\n';              

    return EXIT_SUCCESS;
}
```

Esta es la salida:

    Conjunción lógica
    falso && falso         = false
    falso && verdadero     = false
    verdadero && falso     = false
    verdadero && verdadero = true

En una conjunción lógica, si alguna de las expresiones es falsa, la
evaluación de la expresión devuelve `false`, para que el operador `&&`
devuelva `true` ambas expresiones deben ser verdaderas.

En un nivel más avanzado, la conjunción lógica se puede usar para unir
dos o más expresiones relacionales en una misma expresión.

``` C++
bool a = true, b = true, c = true;  

// true: si, a, b y c son iguales.
if ( (a == true) && (b == true) && (c == true) ) {
    std::cout << a;  // true 
}   
```

Se puede reducir a:

``` C++
if (a && b && c) {
    std::cout << a;  // true 
}    
```

Esta expresión se evalúa de la siguiente forma: primero se compara `a`,
si la expresión es verdadera se compara `b`, si es verdadera se compara
`c`. La conjunción lógica realiza una evaluación de corto circuito, es
decir; continua evaluando las expresiones mientras la evaluación sea
verdadera, si alguna de las expresiones produce efectos secundarios
(como asignar un valor a una variable) asigna el valor antes de
continuar con la evaluación siguiente, si alguna condición es falsa
termina la evaluación de la expresión, sin importar que aun queden
expresiones sin evaluar.

Las expresiones lógicas producen valores boléanos, y por lo general se
usan en estructuras de control para comprobar varias condiciones al
mismo tiempo:

``` C++
int a = 0;

// si a o b es cero muestra un mensaje de error
if ( (a == 0) && (b == 0) ) {
    std::cerr << "ERROR: los argumentos son cero.";
}
```

El comportamiento de `&&` es llamado comportamiento de corto circuito,
esto significa que si la primera expresión es falsa, la segunda
expresión no se evalúa, esto es útil para ejecutar código de forma
condicional.

### Disyunción lógica

El operador de disyunción lógica `||` (O) realiza la operación booleana
`OR` en dos operandos.

``` C++
#include <iostream>             // cout

// La disyunción lógica ||
int main()
{
    bool falso{false};
    bool verdadero{true};
    std::cout << std::boolalpha << "Disyunción lógica\n"
              << "falso     || falso     = " << (falso || falso)     << '\n'
              << "falso     || verdadero = " << (falso || verdadero) << '\n'
              << "verdadero || falso     = " << (verdadero || falso) << '\n'
              << "verdadero || verdadero = " << (verdadero || verdadero) 
              << '\n';              

    return EXIT_SUCCESS;
}
```

Esta es la salida que muestra el programa:

``` C++
Disyunción lógica
falso     || falso     = false
falso     || verdadero = true
verdadero || falso     = true
verdadero || verdadero = true
```

Si uno de los dos operandos es `true` devuelve `true`. La única forma en
que la evaluación sea falsa es que ambos operandos sean falsos. Aunque
el operador `||` es usado por lo general como un simple operador boléano
al igual que el operador `AND` también tiene un comportamiento más
complejo que puede usarse para extender y evaluar varias expresiones.

``` C++
bool a = false, b = false, c = true;

// true: si, a, o b, o c es true    
if( (a == true) || (b == true) || (c == true) ) {
    std::cout << true;      // true
}
```

En esta expresión, el operador `||` comienza por evaluar la primera
expresión (la sub-expresión a la izquierda). Si el valor de la
evaluación es verdadero devuelve `true`. De lo contrario evalúa la
segunda expresión (la sub-expresión de en medio), si la evaluación es
verdadera devuelve `true`, de lo contrario evalúa la expresión del lado
derecho y devuelve el valor de esta expresión. La evaluación se
interrumpe solo si alguna de las expresiones es verdadera. La única
forma en que la expresión puede devolver un valor falso, es que todas
las expresiones sean falsas. Esto es equivalente a usar:
`( a || b || c )`.

Un patrón muy común para usar este operador es para seleccionar el
primer valor que sea verdadero de un conjunto de alternativas usando el
comportamiento de corto circuito.

``` C++
if (valor1 == true || valor2 == true) {
    return true;
} 

return false;
```

En este ejemplo se usa el operador condicional para evaluar la condición
`if`, si el `valor1` o el `valor2` es `true`, devuelve `true` de lo
contrario devuelve `false`.

#### Evaluación de corto circuito

Las expresiones conectadas por `&&` y `||` se evalúan de izquierda a
derecha, usando la evaluación de corto circuito, el operador `&&`
termina la evaluación tan pronto como un valor es falso, mientras que el
operador `||` lo hace tan pronto como un valor es verdadero. Este
comportamiento evita evaluaciones innecesarias.

``` C++
bool falso{false};
bool verdadero{true}; 
std::cout << std::boolalpha;

// La evaluación comienza con la expresión a la izquierda.
// El compilador sugiere usar paréntesis [-Wparentheses].
std::cout << (falso || verdadero && falso) << '\n';   // false

// La evaluación por corto circuito, no evalúa la última expresión, 
// ya que la evaluación de las dos primeras es falsa.  
std::cout << (falso || (falso && falso)) << '\n';     // false

// La evaluación por corto circuito no evalúa la segunda, ni la 
// tercera expresión porque la primera expresión es verdadera.
std::cout << (verdadero || (falso && falso)) << '\n'; // true

// La primera expresión es verdadera. La evaluación salta a la 
// segunda expresión. La última expresión es falsa.
std::cout << ((verdadero || falso) && falso) << '\n'; // false
```

El operador `&&` tiene una mayor precedencia que `||`, sin embargo la
precedencia no cambia el orden de evaluación.

La expresión:

``` C++
(falso || verdadero && falso);
```

Es equivalente a usar:

``` C++
((falso || verdadero) && falso);
```

En cualquiera de las dos expresiones, la evaluación comienza con la
expresión que está a la izquierda.

### El operador de negación

El operador de negación `!` es un operador unario, que se coloca antes
de un operando. Su propósito es invertir el valor boléano de su
operando. Esta es su sintaxis:

``` C++
!expresión;
```

Por ejemplo si `x` es verdadero `!x` se evalúa como falso. Si `x` es
falso, `!x` es verdadero.

``` C++
#include <iostream>              // cout

// El operador de negación
int main()
{
    std::cout << std::boolalpha << "El operador !" << '\n'
              << "true:   " << true                << '\n'  // true
              << "!true:  " << !true               << '\n'  // false
              << "!!true: " << !!true              << '\n'  // true
              << "false   " << false               << '\n'  // false
              << "!false  " << !false              << '\n'  // true
              << "!!false " << !!false             << '\n'; // false

    return EXIT_SUCCESS;
}
```

Al ser un operador unitario, tiene una alta precedencia, lo que puede
acarrear algunos problemas. Si quieres invertir el valor de una
expresión como esta: `i & 1`, necesitas usar paréntesis: `!(i & 1)`
(detecta si un número es par).

``` C++
#include <iostream>             // cout
#include <iomanip>              // setw

// Programa que muestra los números pares menores de 100
int main()
{
    for (std::size_t i = 1; i <= 100; ++i) {
        if (!(i & 1)) {
            std::cout << std::setw(2) <<i << ' ';
        }
        // Agrega un salto de línea cada 10 números
        if ((i % 20) == 0) {
            std::cout << '\n';
        }
    }

    return EXIT_SUCCESS;
}
```

En este ejemplo, la expresión `!(i & 1)` usa el operador `&`. Este
operador realiza una operación a nivel de bits. El resultado en cada
posición es 1 si el bit correspondiente en los dos operandos es 1 y 0 en
caso contrario. Por ejemplo, si tomamos el número 10 y 11 obtenemos:

    Binario Decimal                 Binario Decimal
     10       2                       11      3 
    &    1       1                   &    1      1
     --                                --
     00       // 0 es false           01     // 1 es true 

A diferencia de los operadores `&&` y `||`, el operador `!` convierte su
operando a un valor boléano antes de invertir el valor convertido. Esto
quiere decir que `!` siempre devuelve `true` o `false`, y que puede
convertir cualquier valor a su equivalente boléano. Aplicando este
operador dos veces: `!!x` invierte de nuevo el valor del operando.

## Operaciones a nivel de bits

Los operadores de bits realizan manipulaciones a nivel de bits, actúan
sobre expresiones con números enteros representados como cadenas de
dígitos binarios. No realizan las operaciones aritméticas tradicionales,
son operadores aritméticos, porque operan en números enteros y devuelven
valores numéricos, pero todas las operaciones se llevan a cabo en bits
individuales.

### Operadores a nivel de bits

El estándar proporciona varios operadores que se pueden utilizar para
realizar operaciones a nivel de bits:

  --------------------------------------------------------------------
  Símbolo   Nombre                     Operación
  --------- -------------------------- -------------------------------
  &         Y                          Operación AND boléana

  \|        O inclusivo                Operación OR boléana

  \^        O exclusivo                Operación XOR boléana

  \>\>      Desplazamiento derecho     Desplaza bits a la derecha

  \<\<      Desplazamiento izquierdo   Desplaza bits a la izquierda

  \~        Complemento a uno          Complemento
  --------------------------------------------------------------------

  : Operadores a nivel de bits

Los tres primeros operadores `&, |, ^` realizan comparaciones lógicas
entre los bits de ambos operandos de forma parecida a las que realizan
los operadores lógicos. Los operadores de desplazamiento tal y como su
nombre lo indica, mueven los bits de una posición a otra. El operador de
complemento a uno es un operador unario, los restantes son binarios.

:::: tip
::: title
Tip
:::

Las operaciones a nivel de bits son muy parecidas a las operaciones en
conjuntos. `AND` es análoga a la intersección, `OR` es análoga a la
unión y `XOR` es análoga a la diferencia.
::::

Es posible combinar las operaciones a nivel de bits, con la asignación.
La siguiente tabla muestra los atajos de asignación compuesta usados por
los operadores \"binarios\" a nivel de bits.

  ------------------------------------------------------------------------
  Símbolo   Atajo      Equivale a    Operación
  --------- ---------- ------------- -------------------------------------
  &=        a &= b     a = a & b     Asignación AND bit a bit

  `|=`      `a |= b`   `a = a | b`   Asignación OR inclusivo bit a bit

  \^=       a \^= b    a = a \^ b    Asignación OR exclusivo bit a bit

  \>\>=     a \>\>= b  a = a \>\> b  Asignación y desplazamiento derecho

  \<\<=     a \<\<= b  a = a \<\< b  Asignación y desplazamiento izquierdo
  ------------------------------------------------------------------------

  : Operadores y asignaciones compuestas a nivel de bits

La excepción, es el operador unario `~` que intercambia o complementa
todos los bits de su operando. Técnicamente es el complemento de 1. Y
solo se puede usar del lado izquierdo de un operando.

### El operador AND a nivel de bits

El operador de conjunción `&` (Y), realiza la operación booleana `AND`,
a nivel de bits en cada uno de los argumentos que recibe. El operador
`&` funciona como la multiplicación.

``` C++
#include <iostream>                        // cout

// El operador &
int main() {

    std::cout << "El operador &"
              << "\n1 & 1 = " << (1 & 1)   // 1
              << "\n1 & 0 = " << (1 & 0)   // 0
              << "\n0 & 0 = " << (0 & 0)   // 0
              << "\n0 & 1 = " << (0 & 1);  // 0

    return EXIT_SUCCES;
}
```

La operación `&` toma dos números enteros y realiza la operación `AND`
en cada bit. El resultado en cada posición es 1 si el bit
correspondiente en los dos operandos es 1, y 0 en caso contrario. La
secuencia de bits resultante no puede ser más larga que el número más
pequeño.

``` C++
#include <iostream>      // cout

// Operación &
int main()
{
    int a = 28;          //    11100 (0x1C)
    int b = 255;         // 11111111 (0xFF)
    int c = a & b;       // 00011100 (0x1C)

    std::cout << c << '\n'                    // 28
              << std::hex << (0x1C & 0xFF);   // 0x1C

    return EXIT_SUCCESS;
}
```

Por lo general el operador `&` se usar para determinar el estado de un
bit, es decir para determinar si un bit en particular es verdadero (1) o
falso (0). Por ejemplo, en un patrón de bits se puede determinar si un
bit en particular es verdadero usando una operación `&` y usando una
máscara que contenga un valor 1, en el bit que se quiere determinar:

``` C++
10110111                // 183 (0xB7)
&   00100000                //  32 (0x20) (mascara)
---------
00100000                //  32 (0x20)
```

Puesto que el resultado `00100000` es diferente de cero, el bit en el
patrón original es verdadero.

``` C++
0b10110111 & 0b00100000     // 32
```

A esto se le conoce como enmascaramiento de bits. Ya que los valores `0`
enmascaran los bits que no son de interés.

``` C++
#include <iostream>         // cout
#include <string>           // string 

// Cambia una cadena de texto de minúsculas a mayúsculas 
// usando la operación & a nivel de bits
int main()
{
    // letra a      01100001                 65
    // mascara      11011111    <-- (0xDF)  223
    //              ---------
    // a & mascara  01000001    <-- A

    std::string texto{"Mascara de texto"}; 
    const int mascara{0xDF};

    // Solo convierte los caracteres alfanúmericos   
    for (char& c : texto) {
        if (std::isalpha(c)) { 
            c &= mascara;   // Asigna y convierte
        } 
     }
    std::cout << texto;     // MASCARA DE TEXTO

    return EXIT_SUCCESS;
}
```

Este programa convierte un letra minúscula en mayúscula (o viceversa),
usando una máscara con un valor hexadecimal. Por ejemplo, la letra `a`
en minúscula tiene un valor binario de `01100001` mientras que la `A` en
mayúsculas tiene un valor binario de `01000001`. Ambas letras, difieren
únicamente en el tercer bit. En este caso, convertir una letra minúscula
a mayúscula, consiste básicamente en cambiar el bit 3, por un valor 0.

:::: warning
::: title
Warning
:::

**Tipos enteros**

Los operadores para el manejo de bits, necesitan operandos de tipo
entero o cualquiera de sus variantes `char`, `short`, `int`, `long`,
inclusive enumeraciones.
::::

El operado `&` se puede usar en conjunto con los operadores de
desplazamiento de bits: `<<` y `>>` para comprobar el estado de un bit y
en consecuencia modificarlo.

La biblioteca estándar contiene una cabecera llamada `bitset`. Esta
cabecera contiene una clase que emula una matriz de valores boléanos,
optimizada para ocupar poco espacio, ya que cada elemento ocupa un bit.
Esta clase es útil para trabajar con valores a nivel de bit.

``` C++
#include <bitset>            // bitset
#include <iostream>          // cout 

// Convierte un carácter a su representación en bits, usando bitset
int main()
{
    char letra{'a'};         // Una letra en minúsculas

    // Convierte el carácter a bits
    std::bitset<8> bufer(letra);
    std::cout << "Binario    " << bufer              << '\n'
              << "Entero     " << bufer.to_ulong()   << '\n'
              << "Caracter   " 
              << static_cast<char>(bufer.to_ulong()) << '\n'
              << "Dígitos    " << bufer.size()       << '\n';

    // Cambia el bit 5 a 0 (contando de derecha a izquierda)
    // y convierte el carácter a mayúsculas.
    bufer[5] = 0;
    std::cout << "Conversión a nivel de bits: "      << '\n'
              << "Binario:   " << bufer  << '\n'
              << "Entero:    " << bufer.to_ulong()  << '\n'
              << "Caracter:  "
              << static_cast<char>(bufer.to_ulong()) << '\n'
              << "Dígitos    " << bufer.size()      << '\n';

    return EXIT_SUCCESS;
}
```

Usando la clase `bitset` se puede acceder a la posición de cada bit de
forma individual, para modificar la representación en bits de un objeto,
sin necesidad de usar operadores a nivel de bits. El conjunto de bits
devuelto por la clase se puede convertir tanto en valores enteros como
en cadenas binarias.

### El operador OR

El operador `|` realiza la operación booleana `OR` a nivel de bits y
funciona como su contraparte lógica. Devuelve falso si los dos operandos
son falsos, de los contrario devuelve `true`.

``` C++
#include <iostream>                      // cout

// El operador ||
int main()
{
  std::cout << "El operador |"
            << "\n1 | 1 = " << (1 | 1)   // 1
            << "\n1 | 0 = " << (1 | 0)   // 1
            << "\n0 | 0 = " << (0 | 0)   // 0
            << "\n0 | 1 = " << (0 | 1);  // 1

    return EXIT_SUCCESS;
}
```

Un bit es colocado en el resultado siempre y cuando alguno de los dos
bits sea 1. Si los dos bits son 0 el resultado es 0.

``` C++
#include <iostream>                // cout

// Operación OR a nivel de bits
int main()
{
    int a = 28;                    //    11100 (0x1C)
    int b = 255;                   // 11111111 (0xFF)
    int c = a | b;                 // 11111111 (0xFF)

    // 0x1C | 0xFF se evalúa como hexadecimal a 0xFF (255).
    std::cout << "28 | 255    = " << c << '\n'       
              << "0x1C | 0xFF = " << (0x1C | 0xFF);  

    return EXIT_SUCCESS;
}
```

La secuencia resultante de una operación `OR` no puede ser más corta que
el valor más largo evaluado. El operador `|`, al igual que `&` y `^` se
puede usar con valores binarios, decimales, octales y hexadecimales.

### El operador XOR

El operador `^`, realiza la operación de disyunción exclusiva o `XOR` a
nivel de bits. Esto quiere decir que al menos un bit, debe ser verdadero
pero no ambos.

``` C++
#include <iostream>                      // cout

// El operador ^
int main()
{
  std::cout << "El operador ^"
            << "\n1 | 1 = " << (1 ^ 1)   // 0
            << "\n1 | 0 = " << (1 ^ 0)   // 1
            << "\n0 | 0 = " << (0 ^ 0)   // 0
            << "\n0 | 1 = " << (0 ^ 1);  // 1

    return EXIT_SUCCESS;
}
```

El resultado en cada posición es 1, sólo si uno de los bits es 1. No los
dos, y si los dos son cero, devuelve 0.

``` C++
int a = 5;       // 0101 (0x5)
int b = 9;       // 1001 (0x9)
int c = a ^ b;   // 1100 (0x12)

// 0x05 ^ 0x09 se evalúa como hexadecimal a 0x0C.
std::cout << (0b0101 ^ 0b1001) << '\n'                 // 12 (0xC)
          << (a ^ b)           << '\n'                 // 12 (0xC)
          << c                 << '\n'                 // 12      
          << std::hex          << (0x05 ^ 0x09);       //  c (0xC)
```

La operación `^` cambia el valor, pero no elimina los bits ya que el
valor (original) se puede restaurar, volviendo a usar la misma operación
sobre el valor resultante, algo que no se puede hacer con la operación
`AND`, o con `OR`. Un ejemplo del uso de esta tecnica es el cifrado XOR:

``` C++
#include <iostream>               // cout
#include <string>                 // string 

// Encripta texto usando el cifrado XOR
std::string cifrado_xor(const std::string& texto, const std::string& clave);

// Encripta y desencripta texto usando un cifrado XOR
int main()
{
    std::string clave{"CL4V3"};
    std::string texto_plano{"Hola mundo"};
    std::string texto_cifrado = cifrado_xor(texto_plano, clave);

    // Muestra el texto cifrado, algunos caracteres no son imprimibles.
    std::cout << texto_cifrado << std::endl;

    // La salida en formato octal: 13 43 130 67 23 155 71 132 62 134 
    for (char c : texto_cifrado) {
        std::cout << std::oct << static_cast<int>(c) << ' ';
    }    

    std::cout << std::endl;

    // La salida en base 10: "11 35 88 55 19 109 57 90 50 92" 
    for (char c : texto_cifrado) {
        std::cout << std::dec << static_cast<int>(c) << ' ';
    }                            

    std::cout << std::endl;

    // La salida en formato hexadecimal: B 23 58 37 13 6D 39 5A 32 5C 
    for (char c : texto_cifrado) {
        std::cout << std::hex << std::uppercase << static_cast<int>(c) << ' ';
    }     

    // Recupera el mensaje original aplicando la misma función
    std::cout << '\n' << cifrado_xor(texto_cifrado, clave);

    return EXIT_SUCCESS;                   
}

// Encripta texto usando el cifrado XOR
std::string cifrado_xor(const std::string& texto, const std::string& clave)
{
    std::string salida;
    for (std::size_t i = 0, j = 0; i < texto.length(); ++i, ++j)  {
        salida += texto[i] ^ clave[j];
        j %= clave.length();
    }

    return salida;
}
```

El programa cifra una cadena de texto aplicando el operador `XOR` sobre
cada uno de los caracteres utilizando una clave que cambia el valor de
cada letra. Para descifrar el mensaje encriptado, solo hay que volver a
usar el operador `XOR` con la misma clave. La función `main()` muestra
varias salidas usando los manipuladores `oct`, `dec` y `hex` definidos
en la cabecera `<iomanip>` estos manipuladores especifican que la salida
se realizará utilizando el sistema de numeración octal, decimal y
hexadecimal respectivamente. El manipulador `uppercase` convierte en
mayusculas la salida de los valores hexadecimales.

### La negación lógica

El operador de complemento a uno, `~` es un operador unario que realiza
la operación lógica de negación (a nivel de bits).

``` C++
#include <iostream>                    // cout

// El operador ~
int main() {

    std::cout << "El operador ~"
              << "\n~-1 = " << (~ -1)   //  0
              << "\n ~1 = " << ( ~1)    // -2
              << "\n ~0 = " << ( ~0);   // -1

    return EXIT_SUCCESS;
}
```

El operador de complemento: `~` invierte todos los bits en el operando
que actúa. Por la forma en que se representan los números binarios,
aplicar el operador de complemento `~` es el equivalente a cambiar el
signo del valor (entero) y restar 1.

``` C++
(~-1) + 1                //  1
( ~1) + 1                // -1
( ~0) + 1;               //  0
```

El operador `~` solo puede aparecer antes de un operador o expresión que
se evalué como entero.

``` C++
float a = ~0.5;          // Error: argumento no válido en operación de bits
int b = ~(-20 * 10);     // 199 (El valor positivo menos - 1)
int c = ~(20 * 10) + 1;  // -200 (El valor negativo del número + 1)
```

Para obtener el complemento de dos, se debe sumar 1 al resultado, para
obtener el valor negativo de un número.

``` C++
(~1) + 1;                // -1 (Complemento de dos)
```

Esto equivale a cambiar el signo del número.

### Desplazamiento de bits

Los desplazamientos de bits a la izquierda y a la derecha se consideran
operaciones bit a bit porque operan en la representación binaria de un
valor entero, sin embargo; el desplazamiento no opera en bits
individuales, por lo que estas operaciones no se dan \"bit a bit\", sino
en grupos de bits. En estas operaciones un grupo de bits es movido, o
desplazado hacia la izquierda o hacia la derecha. Como el registro de
memoria tienen un ancho fijo, algunos bits puedes ser desplazados hacia
fuera, mientras que en la operación inversa pueden ser desplazados hacia
adentro. La diferencia entre estas operaciones está en la forma en cómo
los desplazamientos determinan los valores de los bits que entran (o
salen) del registro.

#### Desplazamiento a la izquierda

El operador de desplazamiento de bits a la izquierda `<<` mueve todos
los bits del primer operando a la izquierda, el número de lugares
especificados por el segundo operando, el cual puede ser un valor entero
entre 0 y 31 para tipos enteros (de 32 bits).

``` C++
128 << 6;   //   128 =       10000000
            //  8192 = 10000000000000
```

Por ejemplo, en la operación `128 << 6`, el valor binario se desplaza 6
bits (o 6 posiciones) a la izquierda y el valor resultante se llena de
ceros, lo que da como resultado el número 8192.

``` C++
// Multiplicación por 2 usando el desplazamiento a la izquierda
for (uint8_t i = 0; i <= 10; ++i) {
    std::cout << "2 x " << i << " = " << ((i << 1)) << "\n";
}
```

En números enteros sin signo, el desplazamiento a la izquierda equivale
a multiplicar por 2 el valor desplazado.

#### Desplazamiento a la derecha

El operador de desplazamiento de bits a la derecha `>>` mueve todos los
bits del primer operando a la derecha, el número de lugares
especificados por el segundo operando. Los bits que son intercambiados a
la derecha se pierden.

``` C++
128 >> 6;   //   128 =  10000000
            //     2 =  00000010
```

Los bits de relleno a la izquierda dependen del signo del operando
original. Si el primer operando es positivo, el resultado tiene ceros en
los bits más altos. Si el desplazamiento a la izquierda equivale a una
multiplicación por 2, el desplazamiento a la derecha equivale a una
división por 2.

``` C++
// División entre 2 usando el desplazamiento de bits a la derecha.
for (uint8_t i = 0; i <= 20; i += 2) {
    std::cout << "2 % " << i << " = " << ((i >> 1)) << "\n";
}
```

En la división (usando el desplazamiento hacia la derecha), se pierde el
bit menos significativo, dando como resultado un truncamiento del
resultado (un redondeo hacia abajo). Así que, `6/2` es igual a `3` (lo
usual), pero `7/2` no es 3.5, si no 3, porque el valor resultante se
redondea.

#### Empacando y desempacando bits

Las operaciones de desplazamiento se usan para mover bits hacia la
izquierda o hacia la derecha de acuerdo al tamaño de un tipo en
especifico. El desplazamiento a la izquierda es útil para empacar un
grupo de valores de menor tamaño en un tipo de mayor tamaño. Por ejemplo
un color almacenado como un entero de 32 bytes. Se puede formar por
cuatro canales de color de 8 bytes cada uno: `8 * 4 = 32`.

:::: warning
::: title
Warning
:::

**Operaciones entre bits**

Las operaciones entre bits no son portables debido al tamaño diferente
que pueden tener algunos tipos como los caracteres o los enteros en
diferentes plataformas. Por esta razón se utilizan tipos a medida como
`uint8_t` para tipos `char`, `uint32_t` para tipos enteros de 32 bytes o
`uint64_t` para tipos enteros de 64 bytes, estos tipos garantizan un
tamaño estándar en cualquier implementación.
::::

Para el empaquetado de enteros es necesario desplazar a la izquierda los
valores de los bytes: 24, 16 y 8 respectivamente (el cuarto valor no se
desplaza). Para empaquetarlos, en una sola operación se usa el operador
`|`, como cada canal solo puede almacenar un valor máximo (entre 0 y
255), se usa el operador `&`. Los paréntesis son necesarios debido que
el operador `&` tiene una precedencia menor que `<<`.

``` C++
// 4 enteros de 8 bytes
uint8_t r{128};
uint8_t g{ 64};
uint8_t b{ 32};
uint8_t a{255};

// Un entero de 32 bytes. Cada canal se desplaza 8 posiciones a 
// la izquierda (32/4 = 8). El operador & se encarga que el
// valor no sobrepase el valor máximo del canal 255 o 0xFF.
uint32_t color = ((r & 0xFF) << 24) |         // Rojo
                 ((g & 0xFF) << 16) |         // Verde
                 ((b & 0xFF) <<  8) |         // Azul
                  (a & 0xFF);                 // Alfa

std::cout << color;                           // 2151686399 
```

El entero resultante tiene 32 bytes y un valor de `2151686399`. Este
entero contiene los cuatro canales empaquetados. El proceso de
desempaquetado es muy similar:

``` C++
// Un entero (positivo) de 32 bytes
uint32_t color{2151686399};

// Enteros entre 0 y 255
uint8_t r = (color & 0xFF000000) >> 24;        // Rojo
uint8_t g = (color & 0x00FF0000) >> 16;        // Verde
uint8_t b = (color & 0x0000FF00) >>  8;        // Azul
uint8_t a = (color & 0x000000FF);              // Alfa

// Convierte el valor a entero para mostrarlo en la consola
std::cout << static_cast<int>(r) << ", "
          << static_cast<int>(g) << ", "
          << static_cast<int>(b) << ", "
          << static_cast<int>(a);              // 128, 64, 32, 255  
```

Para el desempaquetado se usa el operador de desplazamiento `>>` que
desplaza los bytes 8 posiciones a la derecha. El operador `&` se asegura
que el valor no sea mayor que el valor máximo que puede almacenar el
tipo. Por ejemplo para un entero (positivo) de 32 bytes el valor máximo
que puede almacenar es: `4278190080` o `0xFF000000` (en hexadecimal),
mientras que en 24 bytes, se puede almacenar como máximo `16711680`, y
16 bytes pueden almacenar como máximo `65280` y en 8 bytes `255`. La
notación hexadecimal en estos casos resulta más cómoda y corta.

Como tratamos con tipos de 8 bytes, es necesario convertirlos a enteros,
ya que la salida los interpreta como tipos `char`.

### El orden de los bytes

El término \"orden de bytes\" se refiere a la forma en que los bytes de
los datos binarios se transmiten o almacenan en la memoria de la
computadora. El almacenamiento puede variar según la arquitectura y el
sistema operativo que se esté utilizando. Hay dos formatos comunes:

**Big Endian:** En este formato, el byte más significativo se almacena
en la dirección de memoria más baja. Todos los bytes se almacenan de
izquierda a derecha, con el byte de mayor peso a la izquierda.

**Little Endian**: En este formato, el byte menos significativo se
almacena en la dirección de memoria más baja. Todos los bytes se
almacenan de derecha a izquierda, con el byte de menor peso a la
derecha.

Para visualizar la diferencia podemos usar un número representado en
binario. Por ejemplo el número `123`:

- **Big-endian**: `00000000000000000000000001111011`
- **Little-endian**: `01111011000000000000000000000000`

El bit más significativo (MSB, por sus siglas en inglés) es el bit de
mayor peso en la representación binaria de un número. Se encuentra a la
izquierda de la cadena de bits y se utiliza para representar el signo o
el valor más alto de un número. Cuando el MSB es 0, el número es
positivo, y si el MSB es 1, el número es negativo. Por ejemplo el número
-123 se representa de esta forma:

- **Big-endian**: `10000101111111111111111111111111` (el byte más grande
  primero).
- **Little-endian**: `11111111111111111111111110000101` (el byte más
  pequeño primero).

En el formato Big-endian, el byte más grande se encuentra primero,
mientras que en Little-endian el byte más pequeño se encuentra primero.

#### La endianidad de una plataforma

A partir de C++ 20 la endianidad de una plataforma se puede comprobar
usando tres enumeraciones: `little`, `big` y `native`:

``` C++
#include <iostream>                     // cout
#include <bit>                          // little, big, native

// Comprueba la endianidad de la plataforma
int main() 
{
    if (std::endian::little == std::endian::native ) {
        std::cout << "Little Endian";
    } else if (std::endian::big == std::endian::native)  {
        std::cout << "Big Endian";        
    } else {
        std::cout << "Valor no identificado";
    }

    return EXIT_SUCCESS;
}
```

La enumeración devuelta por `endian::native` es un valor constante que
se puede usar para determinar la endianidad de una plataforma. Estas
enumeraciones constantes están definidas en el archivo de cabecera
`<bit>`, el cual contiene entre otras cosas, utilidades para trabajar
con bit\'s.

#### Conversiones

El orden de bytes es usado en los protocolos de red debido a la
necesidad de transmitir datos entre diferentes sistemas y arquitecturas
de manera coherente. La conversión entre **big-endian** y
**little-endian** se realiza simplemente intercambiando los bytes en la
memoria. Por ejemplo un número de 32 bits:

``` C++
#include <bitset>                       // bitset
#include <iostream>                     // cout

// Función que invierte el orden de los bytes de un entero (de 32 bits)
uint32_t bytes_swap(uint32_t num) 
{
   return ((num >> 24) & 0xFF)       |
          ((num <<  8) & 0xFF0000)   |
          ((num >>  8) & 0xFF00)     |
          ((num << 24) & 0xFF000000);

}

// Programa que invierte el orden de los bytes de un tipo entero
int main() 
{
    uint32_t valor = 123;

    // El valor original en bits:     00000000000000000000000001111011
    std::cout << "Valor original : " << std::bitset<32>(valor) << '\n'; 

    // De little endian a big endian: 01111011000000000000000000000000
    uint32_t big = bytes_swap(valor);
    std::cout << "Big Endian     : " << std::bitset<32>(big) << '\n'; 

    // De big endian a little endian: 00000000000000000000000001111011
    uint32_t little = bytes_swap(big);
    std::cout << "Little Endian  : " << std::bitset<32>(little) << '\n';

    return EXIT_SUCCESS;
}
```

El ordenamiento big-endian es el ordenamiento usado en los protocolos de
red, como en el conjunto de protocolos de Internet, donde se le conoce
como orden de red, transmitiendo primero el byte más significativo.

#### La función byteswap

En C++23 la cabecera `bit` proporciona una función llamada `byteswap()`.
Esta función invierte el orden de los bytes:

``` C++
#include <bitset>                       // bitset, byteswap
#include <iostream>                     // cout

// Invierte el orden de los bytes
int main() 
{
    uint32_t valor = -123;

    // El valor original en bits:     00000000000000000000000001111011
    std::cout << "Valor original : " << std::bitset<32>(valor) << '\n'; 

    // De little endian a big endian: 01111011000000000000000000000000
    uint32_t big = std::byteswap(valor);
    std::cout << "Big Endian     : " << std::bitset<32>(big) << '\n'; 

    // De big endian a little endian: 00000000000000000000000001111011
    uint32_t little = std::byteswap(big);
    std::cout << "Little Endian  : " << std::bitset<32>(little) << '\n';

    return EXIT_SUCCESS;
}
```

La función `byteswap()` acepta como entrada un tipo entero, la función
es una plantilla, asi que eventualmente puede deducir el tipo de
cualquier valor entero.

## El operador condicional

El operador `?;` permite escribir expresiones condicionales. En este
tipo de expresiones se evalúan tres operandos. Esta es su sintaxis:

``` C++
expresión_a_evaluar ? es_verdadera : es_falsa;
```

El primer operando de la expresión aparece antes del operador `?`, el
segundo entre el operando `?` y `:` y el tercero después de `:`

``` C++
// Función que devuelve el valor absoluto de un número entero
int abs(int num)
{
    return num < 0 ? -num : num;
}
```

El valor de la izquierda del signo de interrogación \"escoge\" cuál de
los dos valores devolverá la expresión. Cuando es verdadero, se elige el
valor intermedio y cuando es falso, se escoge el de la derecha. Solo uno
de los dos valores es evaluado.

En una expresión ternaria, los operandos pueden ser de cualquier tipo.

``` C++
// Función que devuelve el máximo común divisor de dos números enteros
int gcd(int a, int b) 
{ 
    return b ? gcd(b, a % b) : a; 
}
```

En este caso, el primer operando es evaluado e interpretado como un
valor boléano. Si el valor del primer operando es verdadero se evalúa el
segundo operando, y se devuelve su valor. Si el primer operando es
falso, se evalúa el tercer operando y se devuelve su valor. Únicamente
uno de los dos operandos es evaluado, nunca se evalúan ambos.

Por lo general el operador condicional se utiliza para reemplazar a las
sentencias `if` y `else`. Por ejemplo, esta función equivale al ejemplo
anterior, pero es más verbosa:

``` C++
// Devuelve el máximo común divisor de dos números enteros
int gcd(int a, int b)
{
    if (b == 0) {
        return a;
    } else {
        return gcd(b, a % b);
    }
}
```

En muchos casos, los símbolos `? :` tienden a hacer que el código sea
más difícil de leer, pero hay ocasiones en la que el operador ternario
es más claro, ya que puede enfatizar lo que sucede realmente en una
expresión, en lugar de usar sentencias `if` y `else`.

## El operador coma

La coma se puede usar como separador en algunos contextos, como listas
de argumentos, declaraciones y expresiones. También se puede usar como
operador para evaluar secuencialmente declaraciones y expresiones.

``` C++
// Las expresiones separadas por coma se evalúan de izquierda a derecha.
double x = 10, y = 20, z = 30;
```

No se debe confundir su uso como separador y operador, ambas operaciones
son completamente diferentes. El uso más común de la coma es en la
expresión de incremento (o decremento) de un bucle `for`.

``` C++
#include <iostream>                      // cout

// Usando el operador coma
int main()
{   
    // La coma permite agrupar dos declaraciones
    // donde normalmente se espera una.
    for (int i = 0, j = 1; i < 10; ++i, j *= 2) {
        std::cout << i << " * " << j << " = "<<  i*j << '\n';
    }

    return EXIT_SUCCESS;
}
```

La instrucción `for` sólo permite que se ejecute una expresión al final
de cada bucle. La coma se utiliza para que varias expresiones se traten
como una sola y de esta forma, se evite la restricción impuesta por la
propia sintaxis.

### Evaluación secuencial

La evaluación secuencial, usa la coma. En este tipo de operación se
evalúan los operandos secuencialmente de izquierda a derecha:

``` C++
int b = 0;
int a = (b = 5, b + 5);

std::cout << a << '\n'          // 10
          << b << '\n';         // 5
```

Dos expresiones separadas por coma son evaluadas de izquierda a derecha.
El operador de la izquierda siempre es evaluado y todos los efectos
secundarios son completados antes de que el operador de la derecha sea
evaluado. El operador coma no realiza ningún tipo de conversión entre
sus operandos.

``` C++
#include <iostream>             // cout

// Función que suma tres números enteros
int sumar(int a, int b, int c) 
{ 
    return a + b + c; 
}

// Evaluación secuencial
int main()
{
    int x = 10, y = 20, z = 30;

    // Expresión con valores descartados (Advertencia) [-Wunused-value]
    // La coma a la izquierda y a la derecha no tiene efectos.
    std::cout << sumar(x, (x, z, y), z) << '\n'                // 60
            << x << '\n'                                       // 10
            << y << '\n'                                       // 20
            << z << '\n';                                      // 30

    return EXIT_SUCCESS;
}
```

En este ejemplo se invoca la función `sumar` con tres argumentos
separados por comas `x`, `(x, z, y)`, y `z`. La coma obliga al
compilador a interpretar el segundo argumento usando la evaluación
secuencial. En una evaluación secuencial, la expresión del lado
izquierdo siempre es evaluada, pero su valor es descartado, lo cual
quiere decir que sólo tiene sentido utilizar el operador coma, cuando la
expresión de la izquierda tenga efectos secundarios.

:::: tip
::: title
Tip
:::

**Expresión con valores descartados**

Una expresión con valor descartado se puede utilizar para crear efectos
secundarios, ya que el valor calculado de la expresión se descarta.
Estas expresiones incluyen expresiones completas de cualquier sentencia
de expresión, argumentos a la izquierda del operador coma, o el
argumento de una expresión de conversión a tipo `void`
::::

Un efecto secundario se produce cuando se evalúa una expresión que
afecta una variable definida en otro ámbito.

``` C++
std::cout << sumar(++x, y, z) << '\n';                         // 61
```

En este caso, la evaluación secuencial cambia el valor de `x`, lo que a
su vez cambia el resultado de la suma. Esto es un efecto secundario ---
cuando una variable es modificada como producto de la evaluación de una
expresión ---. Por cierto, el operador `<<` no garantiza el orden de
evaluación de las expresiones evaluadas. Por lo tanto esto produce un
comportamiento indefinido:

``` C++
// Comportamiento indefinido
std::cout << sumar(x, (x += y, z += x, y), z) << '\n'          // ¿110?
          << x << '\n'  
          << y << '\n'  
          << z << '\n'; 
```

Para operadores que no especifican un orden de evaluación especifico es
un error hacer referencia o cambiar el mismo objeto en una misma
expresión, como el operador `<<` o el operador `>>`.

## Constantes y efectos secundarios

Conceptualmente hablando, podemos distinguir dos tipos de expresiones:
las que tienen efectos secundarios, por ejemplo: las que asignan y
cambian el valor de una variable y las que son evaluadas y calculan un
valor constante.

La expresión:

``` C++
// Efectos secundarios
int x = 10;
x += 20; 
```

Es un ejemplo de una expresión que produce efectos secundarios. La
expresión en sí misma se evalúa como `30`.

Mientras que las expresiones:

``` C++
// Expresiones constantes
const int x = 10 * 10;                        // 100
constexpr int y = x + (5 + 6);                // 111
```

Son expresiones constantes. Una expresión constante siempre produce el
mismo resultado. El compilador garantiza que la evaluación de una
expresión constante no puede cambiar su valor. Una expresión calificada
como `constexpr` se pueden evaluar en tiempos de compilación. Y al igual
que `const` define un objeto inmutable.

### Efectos secundarios

Cuando una expresión dentro de un bloque afecta el valor de una variable
definida en otro bloque, genera lo que comúnmente se conoce como efecto
secundario.

``` C++
int i = 0;                             // Una variable global 

// Un bucle que cambia el valor de i de forma global.
for (i = 0; i < 10; ++i) {
    std::cout << i << ' ';             // 0 1 2 3 4 5 6 7 8 9 
}  

// La variable i tiene un nuevo valor (Esto es un efecto secundario)
std::cout << i;                        // 10
```

Una expresión genera un efecto secundario cuando cambia el valor de un
objeto declarado en un ámbito diferente al que se evalúa. Aunque los
efectos secundarios son útiles en ciertos contextos, se pueden evitar
restringiendo el uso de variables globales y nombres a nivel de bloques:

``` C++
int i = 0;                             // Una variable global 

// Un bucle que cambia el valor de i de forma local.
for (int i = 0; i < 10; ++i) {
    std::cout << i << ' ';             // 0 1 2 3 4 5 6 7 8 9
}

// El valor de i no ha sido modificado.
std::cout << i;                        // 0
```

Las llamadas a funciones, declaraciones de asignación anidadas, y los
operadores de incremento y decremento provocan \"efectos secundarios\".

### Expresiones constantes

La palabra clave `constexpr` declara una expresión (o un objeto) como
constante. Una expresión constante es una operación que puede ser
evaluada en tiempos de compilación.

``` C++
Tipo Nombre  
 |     |     
 v     v
constexpr double dx = 0.01;  // <- Termina en punto y coma
^                  ^
|                  |
Calificador       Expresión
```

Todos los tipos básicos se pueden inicializar como valores `constexpr`.
Al igual que `const`, la palabra clave `constexpr` se puede aplicar a
funciones, pero a diferencia de `const`, `constexpr` se puede aplicar a
constructores de clases.

``` C++
constexpr bool verdadero = true;       // La variable booleana es constexpr
constexpr int total = 10;              // La variable es constexpr 
constexpr double log_10 = 0.434294481; // La variable es constexpr
constexpr float x = 1.4f + 2.2f;       // La variable float es constexpr
constexpr double y{2.5};               // Inicialización uniforme constante
constexpr float z = exp(5, 3);         // La función es constexpr
int arreglo[total];                    // Bien: total es constexpr

constexpr int i;                       // Error: variable no inicializada
int j = 0;                             // La variable j no es constexpr
constexpr int k = j + 1;               // Error: j no es una constante
```

Para que un objeto pueda ser declarado como `constexpr` debe seguir
algunas reglas:

- Una variable calificada como `constexpr` debe tener un valor
  inicializador.
- El valor usado por el objeto debe ser implícitamente constante.
- Se requiere una expresión constante para la inicialización de un
  objeto declarado como `constexpr`.

Es un error usar la palabra `constexpr` en una variable no inicializada.

:::: tip
::: title
Tip
:::

**Objetos inmutables**

En C++ se pueden declarar objetos inmutables usando las palabras clave
`const` y `constexpr`. El valor de un objeto inmutable \"no puede\" ser
modificado. La principal diferencia entre estas palabras clave, es que
la inicialización de una variable de tipo `const` se puede diferir hasta
el tiempo de ejecución del programa. Mientras que una variable
calificada como `constexpr` puede ser evaluada en tiempos de compilación
o en tiempo de ejecución (dependiendo de su uso).
::::

Una expresión constante sólo incluye valores constantes. Es decir
valores que son conocidos en tiempos de compilación como valores
literales y valores declarados con las palabra clave `const` o
`constexpr`.

``` C++
#include <iostream>                    // cout
#include <iomanip>                     // setw, setfill, right

// Imprime la tabla de logaritmos base 10 con 4 decimales
int main()
{
    double x{1.0};                      // Variable de inicio (mutable)
    double log_n{0.0};                  // Variable de incremento (mutable)
    constexpr std::size_t limite{1000}; // El valor del límite (inmutable) 

    constexpr double dx{0.01};          // Una expresión calificada
    constexpr double dx2 =  dx * dx;    // como constexpr puede ser
    constexpr double dx3 = dx2 * dx;    // evaluada en tiempos
    constexpr double dx4 = dx3 * dx;    // de compilación

    for (std::size_t i = 1; i <= limite; ++i) {
        const double x2 =  x * x;       // Una expresión calificada
        const double x3 = x2 * x;       // como const, puede ser evaluada
                                        // en tiempos de ejecución

        // Convierte el logaritmo natural a logaritmo base 10
        const double log_10 = log_n * 0.434294482;

        // Aproximación al logaritmo natural usando la serie de Taylor
        log_n += dx / x - dx2 / (2 * x2) + dx3 / (3 * x3) - dx4;

        x += dx;                        // Incrementa el valor de inicio

        // Formatea la salida con 4 decimales de precisión
        std::cout << std::setprecision(4) << std::fixed << std::setw(7) 
                  << std::right << std::setfill(' ') << log_10;

        // Agrega un espacio cada 10 líneas
        if ((i % 10) == 0) {
            std::cout << '\n';
        }
    }

    return EXIT_SUCCESS;
}
```

Una expresión calificada como `constexpr` puede evaluar valores en
tiempo de compilación, pero para que los valores se puedan evaluar,
deben ser conocidos en tiempo de compilación.

#### Funciones constexpr

El calificador `constexpr` puede usarse en cualquier objeto que pueda
ser evaluado en tiempos de compilación. El caso más común son las
funciones, sin embargo también se puede usar en clases, funciones
miembro y contenedores.

``` C++
#include <cmath>                       // pow
#include <print>                       // print

// Calcula iterativamente log_10 usando la serie de Taylor
// Requiere de un valor decimal, el valor de incremento y un limite 
constexpr double Taylor(double x, double dx, std::size_t iteraciones);

// Imprime la tabla de logaritmos base 10 con 4 decimales
int main()
{
    double x{1.0};                     // El valor inicial 
    double log_n{0.0};                 // El valor del logaritmo
    constexpr double dx{0.01};         // El incremento
    constexpr unsigned limite{900};    // El limite
    constexpr unsigned decimales{4};   // El número de decimales

    // Formatea la salida usando print. Compilar con: -std=c++23
    std::print("\tTabla de logaritmos con cuatro cifras decimales\n\n");
    std::print(" n{0}0{0}1{0}2{0}3{0}4{0}5{0}6{0}7{0}8{0}9", "      ");

    // Convierte el logaritmo natural a logaritmo base 10 y lo imprime
    for (std::size_t i = 0; i < limite; ++i) {
        const double log_10 = log_n * 0.434294482;
        log_n += taylor(x, dx, decimales);
        x += dx;
        if ((i % 10) == 0) {
            std::print("\n{:3} ", i + 10); // El número de cada fila
        }
        std::print("{:.4f} ", log_10);
    }

    return EXIT_SUCCESS;
}

// Función que calcula iterativamente log_10 usando la serie de Taylor
constexpr double taylor(double x, double dx, std::size_t iteraciones)
{
    double log_n{0.0};
    for (std::size_t i = 1; i <= iteraciones; ++i) {
        log_n += (i % 2 ? 1 : -1) * (pow(dx, i) / (i * pow(x, i)));
    }

    return log_n;
}
```

En algunos casos, las expresiones constantes son requeridas por las
reglas del lenguaje (por ejemplo en arreglos, valores para argumentos de
plantilla, inicializadores de variables estáticas, etc). En otros casos
la evaluación en tiempo de compilación mejora el rendimiento de un
programa. Independientemente de estos motivos, la inmutabilidad de un
objeto es una cuestión de diseño importante y siempre se deben declarar
como constantes los objetos que no cambian.

#### Aserciones estáticas

Para comprobar que un objeto se evalúa en tiempos de compilación se usa
la declaración `static_assert`. Una declaración `static_assert` define
una aserción. Una aserción especifica una condición que se espera que
sea verdadera en un punto determinado de un programa.

``` C++
#include <iostream>             // cout
#include <string_view>          // string_view

// Usando una aserción estática
int main()
{
    // Si todo sale bien, el programa se ejecuta sin errores
    constexpr std::string_view texto = "Hola mundo";
    static_assert(texto == "Hola mundo");

    return EXIT_SUCCESS;
}
```

Si la condición devuelve `true`, la declaración `static_assert` no
produce ningún efecto. Si la condición devuelve `false` se produce un
error en la aserción. En estos casos el compilador muestra un mensaje de
error y detiene el proceso de compilación.

La declaración `static_assert` solo funciona con valores inmutables:

``` C++
int valor = 1;                  // Error: el valor no es constante 
static_assert(valor == 1);  
```

Sin embargo esto si funciona,

``` C++
const int valor = 1;            // Bien: el valor es const
static_assert(valor == 1);    
```

y esto:

``` C++
constexpr int valor = 1;        // Bien: el valor es constexpr 
static_assert(valor == 1);     
```

Una aserción se usa para detectar condiciones esperadas y muestra un
error, si no se cumplen las condiciónes.

``` C++
#include <iostream>             // cout

// Aserción estática que muestra un mensaje
int main()
{
    constexpr int valor = -1;    
    static_assert(valor > 0 && valor < 10, "El valor deve ser 1-9");  

    return EXIT_SUCCESS;
}
```

Si la expresión evaluada es falsa, se muestra el mensaje pasado como
segundo parámetro y se produce un error en la compilación. Si la
expresión es verdadera, la declaración `static_assert` no tiene ningún
efecto.

## Categorías de valor

El valor de una expresión, se evalúa al crear, copiar y mover un objeto.
Sin embargo antes de crear, copiar o mover un objeto este debe estar en
alguna parte. Algunos valores se almacenan permanentemente en la memoria
mientras que otros existen unicamente cuando se evalua una expresión.
Los objetos almacenados en la memoria tienen una dirección a la que se
puede acceder en todo momento, sin embargo los objetos temporales no
tienen dirección se crean y se destruyen cuando el flujo de control
entra o sale de su ámbito. Para que el compilador pueda optimizar la
creación y destrucción de objetos se usan las categorías de valor.

Las categorías de valor son clasificaciones que describen cómo se accede
y se utiliza un objeto o una expresión en un programa. Las categorías
son clave para entender el comportamiento del lenguaje en términos de
memoria, eficiencia y semántica.

### Clasificación de expresiones

Existen dos propiedades que son importantes para un objeto cuando es
necesario obtener su dirección, moverlo o copiarlo:

1.  **Identidad** cuando el objeto tiene un nombre, es un puntero o una
    referencia a un objeto, es posible determinar si la expresión se
    refiere a la misma entidad como otra expresión, al comparar las
    direcciones de los objetos o funciones obtenidas de forma directa o
    indirecta.
2.  **Movilidad** cuando el objeto puede ser movido (por ejemplo es
    posible cambiar el valor a otra ubicación y dejar el objeto en un
    estado valido aunque no especificado en lugar de solo copiar el
    valor). En este caso, el constructor de movimiento, el operador de
    asignación de movimiento, o alguna función que implemente la
    semántica de movimiento puede vincularse a la expresión.

El compilador puede optimizar la creación de objetos al copiar o mover
objetos que estan a punto de ser destruidos, usando lo que el estándar
llama categoria de valor.

``` C++
int a = 10;          // 'a' es un valor-l
int b = 20;          // 'b' es un valor-l
int suma = a + b;    // 'a + b' es un valor-r
```

Tanto, `a` como `b` son valores-l porque tienen una dirección en memoria
y se les puede asignar un valor. Sin embargo, la expresión `a + b` es un
valor-r porque es el resultado temporal de sumar `a` y `b`.

Los valores-r se vuelven especialmente útiles al trabajar con objetos
grandes o pesados, o cuando se necesita optimizar el rendimiento, ya que
permiten capturar valores temporales y aprovechar la semántica de
movimiento en lugar de realizar copias.

``` C++
#include <iostream>        // cout, endl  
#include <utility>         // move    
#include <string>          // string

// Función que acepta como entrada un valor-r
void procesar_valor(std::string&& texto) 
{
    std::cout << "Procesando valor-r: " << texto << std::endl;
}

// Procesando valores-r
int main() 
{
    // La variable texto es un valor-l (tiene una dirección en memoria)
    std::string texto{"Una cadena de texto en memoria"};

    // La función 'move()' convierte el valor-l en un valor-r (lo mueve)
    procesar_valor(std::move(texto)); 

    // Sin embargo esto NO compila: 
    procesar_valor(texto); 
    // porque 'texto' es un valor-l y la función espera un valor-r.

    // 'Texto temporal' es un valor-r
    procesar_valor("Un valor temporal"); 

    return EXIT_SUCCESS;
}
```

En este caso, la función `move()` mueve o convierte un valor-l (texto)
en un valor-r para que la función `procesar_valor()` la acepte
(permitiendo que se use el constructor de movimiento.), mientras que
\"Un valor temporal\" es un valor-r literal que se puede pasar
directamente a la función.

A partir de C++ 20 el estándar divide el tipo de valor de una expresión
en cinco categorías diferentes llamadas **categorías de valor**.

::: parsed-literal
+-----------------+
| > Expresión     |
+-----------------+

> | 
>
> v

+\-\-\-\-\-\-\--+\-\-\-\-\-\-\--+ \| \| v v +\-\-\-\-\-\-\-\-\-\-\--+
+\-\-\-\-\-\-\-\-\-\--+ \| valor-gl \| \| valor-r \|
+\-\-\-\-\-\-\-\-\-\-\--+ +\-\-\-\-\-\-\-\-\-\--+ \| \| \| \| v v v v
+\-\-\-\-\-\-\-\--+ +\-\-\-\-\-\-\-\-\-\--+ +\-\-\-\-\-\-\-\-\--+ \|
valor-l \| \| valor-x \| \| valor-pr \| +\-\-\-\-\-\-\-\--+
+\-\-\-\-\-\-\-\-\-\--+ +\-\-\-\-\-\-\-\-\--+
:::

Estas categorías rigen la forma en que el compilador crea, copia y mueve
objetos temporales durante la evaluación de expresiones.

1.  Un **valor-gl** es un valor generalizado. Un valor generalizado es
    una expresión cuya evaluación determina la identidad de un objeto.

    ``` C++
    std::string s;
    s = "Hola";             // Tiene un nombre. Es un valor-gl.
    ```

    Esta categoría incluye valores-l (expresiones que tienen identidad),
    objetos constantes (que no se pueden mover) y valores temporales
    (expresiones que tienen identidad y se pueden mover), pero excluye
    valores puros (o expresiones sin identidad).

2.  Un **valor-pr** es un valor puro. Un valor puro es una expresión que
    inicializa un objeto o calcula un valor, pero no tiene identidad.

    ``` C++
    4 * 10;                 // La expresión no tiene identidad
    sumar(10, 10, 10);      // Invocar una función produce un valor-pr     
    i++;                    // Las expresiones post y pre fijo de 
    --i;                    // incremento y decremento son valores-pr.
    ```

    Un valor puro es una expresión que representa objetos temporales,
    como `4 * 10`, o la cadena \"Un valor temporal\".

3.  Un **valor-x** es un valor temporal que pronto va a expirar. Un
    valor que va a expirar es un valor generalizado que denota un objeto
    cuyos recursos se pueden reutilizar antes de que se destruya o salga
    de su ámbito.

    ``` C++
    std::string a;          // Tiene un nombre, así que es un valor-gl.
    a = "Hola";             // pero puede ser movido de "a".  
    auto b = std::move(a);  // Entonces es un valor-x, no un valor-l
    ```

    Un valor que expira se puede mover. El objeto que representan estas
    expresiones es un objeto temporal que está a punto de expirar o esta
    a punto de ser destruido o salir de su ámbito.

4.  Un **valor-l** es un valor a la izquierda que tiene identidad. Un
    valor con identidad es un valor generalizado que no expira. Entre
    los valores-l se encuentran las expresiones que tienen un nombre de
    variable, un nombre de función, punteros, etc.

    ``` C++
    int a = 10;            // 'a' es un valor-l porque tiene un nombre y 
    a = 20;                // ocupa una ubicación en memoria. Se le puede
                           // asignar un nuevo valor.
    ```

5.  Un **valor-r** es un valor a la derecha. Un valor-r es cualquier
    expresión que se pueda mover implícitamente, independientemente de
    que tenga identidad o no. Un valor-r puedes ser un valor puro o un
    valor temporal.

    ``` C++
    int b = a + 5;         // 'a + 5' es un valor-r, ya que es un resultado 
                           // temporal sin dirección en memoria.
    int c = 15;            // '15' es un valor-r porque es un valor literal
    ```

Históricamente los términos valor-l y valor-r (value-l y value-r) fueron
nombrados así, porque estos valores solo podían aparecer a la derecha o
a la izquierda de una asignación. Aunque los términos aún se siguen
usando, su significado ha cambiado. En C++, un valor a la izquierda es
\"algo\" que tiene identidad y no puede moverse y un valor a la derecha
es cualquier \"cosa\" que se pueda mover de una dirección de memoria a
otra. Más allá del significado de los nombres, estos términos clasifican
expresiones no valores.

Las categorías de valor juegan un papel crucial en la semántica de
movimiento y copia de objetos en C++, especialmente con la introducción
de referencias a valores-r (`&&`) introducidas en C++ 11. En la mayoría
de los casos basta con pensar en términos de valores izquierdos y
valores derechos. En una expresión un valor puede ser izquierdo o
derecho, pero nunca, puede ser ambos.
