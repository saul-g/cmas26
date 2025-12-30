# Un poco de historia

::: {.meta description="Historia de los lenguajes de programación C y C++" keywords="historia, lenguajes, programación, El lenguaje C, El lenguaje C++"}
:::

A principios de 1960, los programas para computadora se escribían y
ejecutaban de forma diferente. Un investigador escribía un programa, lo
convertía al formato de entrada que aceptaba el *mainframe*
(computadora), un operador ponía en cola el programa, lo ejecutaba y
entregaba los resultados impresos al investigador. Si había un error en
el código, después de imprimir las tarjetas perforadas, el investigador
obtenía como resultado una impresión que decía \"Error de sintaxis\".

A medida que los programas se volvieron más complejos, este método de
error y prueba se volvió complicado y frustrante. Entre los
investigadores había un interés generalizado en crear un sistema de
tiempo compartido, que les permitiera ejecutar sus programas en el
*mainframe* de forma simultánea, y obtener los resultados de inmediato
en sus terminales remotas. Para esto se buscaba la forma de escribir y
almacenar los programas en el propio *mainframe*, en vez de usar
tarjetas perforadas. En teoría, los investigadores buscaban la forma de
poder escribir, editar y ejecutar sus programas de forma simultánea y
sin salir de sus oficinas.

Bajo estas ideas, se creó el proyecto Multics (Sistema de computación e
información multiplexado) con el objetivo de crear un sistema operativo
(SO) de tiempo compartido, que tomara en cuenta conceptos innovadores,
como la gestión de archivos, la interacción de usuarios, soporte para el
intercambio de información, estructuras jerárquicas para la
administración del sistema, soporte de terminal remoto, además de
aplicaciones. El proyecto estaba formado por el Instituto Tecnológico de
Massachusetts (MIT), el cual había desarrollado un SO de tiempo
compartido llamado CTSS que estaba en uso. MIT proporcionaría las
especificaciones. La General Electric (GE) proporcionaría el hardware:
una computadora GE-645 y los Laboratorios Bell de AT&T se encargarían de
las tareas de programación. El proyecto inicio sus operaciones en 1964.
Ken Thompson, Dennis Ritchie, Rudd Canaday, M. D. Mcllroy y Brian
Kernighan formaron parte del equipo de programadores.

Multics se desarrolló inicialmente para el *mainframe* GE-645, un
costoso sistema de 36 bits que usaba un multiprocesador simétrico con
memoria física y virtual compartida. El sistema era grande y complejo
con 135 KB de memoria. El SO se escribió en PL/I (Lenguaje de
Programación Uno) y se ejecutó en el hardware de la GE. Aunque el equipo
de desarrollo se puso en acción, pronto cayó en la misma trampa que
atrapo a IBM con el SO/360. Intentaban crear un sistema operativo
demasiado grande para un hardware demasiado pequeño. En 1969, los
laboratorios Bell se retiraron del proyecto y en 1970 Honeywell, compro
la división de computadoras de la GE, y continuó como proveedor de
hardware.

## El nacimiento de Unix

Los laboratorios Bell se retiraron del proyecto debido a los altos
costos de mantenimiento del equipo y a la falta de resultados y
decidieron utilizar GECOS (El Sistema Operativo de la General Electric)
en lugar de Multics. Entre tanto, Thompson y su compañero de trabajo
Dennis Ritchie se divertían portando el software de Thompson \"Viaje
espacial\" a la PDP-7. El juego permitía volar una nave espacial que le
permitía al piloto aterrizar en los planetas y lunas del sistema solar.
Cuando el acceso de Thompson a Multics terminó, Thompson tradujo el
juego a Fortran, para el sistema operativo GECOS de la computadora
GE-645. Sin embargo, el movimiento de la pantalla era irregular y el
acceso costaba \$75 la hora. Tiempo despues encontró una computadora
PDP-7 poco usada con un buen procesador de pantalla en los laboratorios
Bell. Thompson y Ritchie portaron el juego a lenguaje ensamblador usando
GECOS y luego transfirieron el programa a la PDP-7 usando cintas de
papel perforadas.

Basándose en su experiencia con el CTSS y al ineficiente pero funcional
Multics, Thompson lideró el diseño de un nuevo sistema operativo en una
serie de sesiones con Ritchie y Canaday. Thompson escribió una
simulación del sistema de archivos y el sistema de paginación de Multics
para verificar su funcionamiento. Después de escribir un conjunto de
utilidades básicas, rescribió todo en ensamblador para poder programar
directamente en la PDP-7. Thompson trataba de proveer al PDP-7 con los
rudimentos de un nuevo sistema operativo, mucho más simple y ligero que
el usado por Multics, sin embargo el sistema operativo estaba escrito
completamente en lenguaje ensamblador.

Thompson y Ritchie se unieron al grupo de programadores, entre ellos
Rudd Canaday, para desarrollar el sistema de archivos, así como el
sistema multitarea. Al que agregaron un intérprete de comandos y un
conjunto limitado de programas. Brian Kernighan lo llamó en broma
\"Unics\", Sistema de Computación e Información Uniplexado, como un
juego de palabras relacionado con su experiencia en Multics. Poco tiempo
después cuando se le agregó la funcionalidad de multiprocesamiento, el
nombre se cambió a \"Unix\", el cual curiosamente no es un acrónimo de
nada.

## El nacimiento de C

Escribir código en lenguaje ensamblador era un trabajo duro y
complicado. Las estructuras de datos del código fuente eran difíciles de
entender y de corregir. Thompson pensó en usar las ventajas ofrecidas
por un lenguaje de alto nivel para crear un sistema operativo nuevo,
pero sin el problema del rendimiento ni la complejidad del lenguaje PL/I
(que había visto en Multics). Después del acercamiento y fracaso con el
lenguaje Fortran, Thompson creó el lenguaje B, simplificando las
investigaciones aplicadas al lenguaje BCPL (Lenguaje de Programación
Básico Combinado), de esta forma el intérprete cabría en la memoria de
8K de la PDP-7. Sin embargo los límites de memoria del hardware
proporcionaban espacio únicamente para el intérprete, no para el
compilador. Por otra parte, el lento rendimiento que proporcionaba B, no
permitió que fuera utilizado para la programación del (nuevo) sistema
operativo Unix.

El lenguaje B, simplificó al lenguaje BCPL creado por Martin Richard en
1967, omitiendo algunas características como procedimientos anidados y
construcciones, aunque conservo algunas ideas sobre referencias y
ausencia de tipos. Sin embargo el uso de un lenguaje sin tipos, demostró
ser una mala idea, cuando el desarrollo se cambió en 1970 a la recién
creada PDP-11. Este nuevo procesador ofreció el soporte de hardware para
tipos de datos de diferentes tamaños, que el lenguaje B no tenía forma
de expresar. El rendimiento también era un problema, esto obligo a
Thompson a reimplementar el sistema operativo en ensamblador a la PDP-11
en lugar de B. Dennis Ritchie capitalizó el gran poder de la PDP-11 y
creo el \"Nuevo B\" que soluciono ambos problemas: tipos de datos
múltiples y mayor rendimiento. El nombre de \"Nuevo B\" rápidamente
cambio a \"C\". Un lenguaje compilado en lugar de interpretado. Había
nacido C.

El sistema de tipos de C, ayudo al compilador a distinguir los tipos
enteros de los valores decimales, además de los caracteres de las
palabras en el nuevo hardware de la PDP-11. En contraste con otros
lenguajes como Pascal, donde el propósito del sistema de tipos es
proteger al programador de las restricciones entre las operaciones
validas de datos. C usa una filosofía distinta, ya que rechaza el tipado
fuerte y permite que el programador haga asignaciones entre objetos de
diferentes tipos, si esto es lo que desea. Segun Van Der Linden, el
sistema de tipos de C, nunca fue evaluado rigurosamente ni probado
extensivamente. Aun hoy en día, muchos programadores creen que
\"fuertemente tipado\" significa teclear muy fuerte. Existen otras
características de C que fueron inventadas para ayudar al compilador a
compilar programas. Aunque esto no suena bien, no es necesariamente
malo; ya que simplifica enormemente el lenguaje y evita semánticas
complicadas.

::: parsed-literal
Línea de tiempo de C

1965-67 1969 1971 1972-1973 +\-\-\-\-\--+ +\-\--+ +\-\-\-\-\-\-\-\--+
+\-\-\-\--+ \| BCPL [\|\-\-\-\-\-\-\--\>\|](##SUBST##|-------->|) B
[\|\-\-\-\-\-\-\--\>\|](##SUBST##|-------->|) Nuevo B
[\|\-\-\-\-\-\-\--\>\|](##SUBST##|-------->|) C \| +\-\-\-\-\--+ +\-\--+
+\-\-\-\-\-\-\-\--+ +\--+\--+ \| Sistema Operativo \| v
+\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\--+
+\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\--+ +\-\-\--+\-\-\-\--+ \| UNIX
escrito en \| \| UNIX escrito en \| \| UNIX en \| \| ensamblador PDP-7
[\|\-\--\>\|](##SUBST##|--->|) ensamblador PDP-11
[\|\-\--\>\|](##SUBST##|--->|) C \|
+\-\-\-\-\-\-\-\--+\-\-\-\-\-\-\-\--+
+\-\-\-\-\-\-\-\--+\-\-\-\-\-\-\-\-\--+ +\-\-\-\-\-\-\-\-\--+ \|
Hardware \| v v +\-\-\--+\-\-\--+ +\-\-\--+\-\-\--+ \| PDP-7
[\|\-\-\-\-\-\-\-\-\-\-\-\--\>\|](##SUBST##|------------->|) PDP-11 \|
+\-\-\-\-\-\-\-\--+ +\-\-\--+\-\-\--+ \| v +\-\--+\-\-\-\--+ \| IBM-360
\| +\-\--+\-\-\-\--+ \| v +\-\-\-\-\--+\-\-\-\-\-\-\--+ \| HoneyWall-635
\| +\-\-\-\-\-\-\-\-\-\-\-\-\-\--+
:::

El primer compilador de C apareció en 1972. En ese mismo año también se
crearon las bibliotecas de entrada y salida estándar (I/O), ya que el
lenguaje no las definió originalmente. Las bibliotecas de I/O fueron
escritas por Mike Lesk, aunque fueron rescritas más tarde, debido a
problemas de rendimiento. También se agregó un pre-procesador a C, a
sugerencia de Alan Snyder, el cual debía cumplir tres objetivos:

- Remplazar cadena de texto, usando nombres simbólico definidos como
  constantes.
- Incluir archivos fuente (como se hacía en BCPL). Las declaraciones se
  separan en un archivo de cabecera (con extensión `.h`), y quedan
  disponibles para un rango más amplio de archivos.
- Expandir plantillas genéricas de código. A diferencia de una función,
  una misma macro puede tomar diferentes argumentos en cada nueva
  llamada.

Inicialmente el sistema operativo Unix fue considerado como un proyecto
de investigación, hasta el punto de distribuirse de forma gratuita en
varias universidades, sin embargo con el paso del tiempo, la demanda del
producto le permitió a los laboratorios Bell conceder licencias a
distintos usuarios.

A mediados de los años setenta el lenguaje C fue refinado, poniendo en
orden algunos detalles y extendiendo los tipos básicos al nuevo
hardware. En 1978 el libro clásico de C, llamado \"El lenguaje de
programación C\", fue publicado. Y por aclamación popular, los nombres
de los autores Brian Kernighan y Dennis Ritchie se usaron en la nueva
versión del lenguaje \"C\": llamado simplemente K&R.

C evoluciono de la mano del sistema operativo Unix, su desarrollo,
implementación y uso se realizó inicialmente en este sistema. Tanto el
compilador, como la mayoría de los programas y herramientas de Unix
fueron escritas en C. Aunque C no estaba diseñado originalmente para
usarse más allá de los laboratorios de investigación de AT&T su uso se
extendió rápidamente. A partir de entonces han surgido varias ramas de
evolución que han generado otros lenguajes:

- Objective-C es el primer intento de proporcionar soporte para la
  programación orientada a objetos en C, actualmente se usa en Mac OS X,
  iOS y GNUstep.
- C++ diseñado por Bjarne Stroustrup es el segundo intento por
  proporcionar orientación a objetos y es la variante más difundida y
  aceptada. C++ combina la flexibilidad y el acceso de bajo nivel de C
  con las características de la programación orientada a objetos como
  abstracción, encapsulación y ocultación de datos.

Otros lenguajes se han inspirado en su sintaxis, aunque no son
compatibles con C:

- Java, que usa sintaxis inspirada en C++ con una orientación a objetos
  más similar a la de Smalltalk y Objective C.
- JavaScript (o ECMAScript), un lenguaje de scripts creado en Netscape e
  inspirado en la sintaxis de Java creado para dar a las páginas web
  interactividad.
- C# (C Sharp) es un lenguaje desarrollado por Microsoft derivado de
  C/C++ y Java. Con bastante popularidad en entornos Windows.

En la actualidad, existen muchos programas escritos en C, así como una
extensa variedad de bibliotecas disponibles. Muchas bibliotecas aún se
escriben en C, debido a que C genera código objeto rápido; de esta forma
es más sencillo para los programadores generar interfaces a una
biblioteca para que las rutinas puedan ser utilizadas por lenguajes
interpretados como Perl, Python o Ruby.

Hoy en día, la mayor parte de los sistemas operativos y bibliotecas aún
se escriben en C y C++.

## El nacimiento de C++

A principios de los años ochenta, Bjarne Stroustrup un investigador que
trabajaba en los laboratorios Bell, diseñó una extensión del lenguaje C
al que originalmente llamo \"C con clases\". Stroustrup implemento y
modifico una extensión del lenguaje C, porque quería usar servicios
distribuidos en un núcleo de Unix usando varios multiprocesadores y
redes de área local (lo qué ahora se conoce como multi-procesamiento y
clúster\'s). Para realizar este trabajo necesitaba realizar algunas
simulaciones sobre el manejo de eventos para los cuales \"Simula\"
habría sido el lenguaje ideal, lo cual no fue posible por razones de
rendimiento. También necesitaba tratar directamente con el hardware y
quería usar los mecanismos de alto rendimiento de la programación
concurrente para los cuales \"C\" habría sido ideal, excepto que el
lenguaje no soporta completamente la modularidad y la comprobación de
tipos. Así que lo que Stroustrup hizo fue tomar las clases de Simula y
las agrego a C, dando como resultado \"C con clases\".

C con clases fue usado en algunos proyectos de investigación en los
cuales demostró su utilidad y la facilidad para escribir programas,
aunque el lenguaje original carecía de sobrecarga de operadores,
referencias, plantillas, excepciones y funciones virtuales era usable.
El primer uso de C con clases fuera de los entornos de investigación
Bell sucedió en julio de 1983. Entre 1983 y 1984, \"C con clases\" fue
rediseñado, extendido e implementado como un lenguaje con
características propias. El lenguaje se denominó C++ y fue descrito por
Stroustrup en el libro \"Abstracción de datos en C\" publicado en
octubre de 1984.

El nombre C++ (pronunciado \"ce más más\" en Español) fue acuñado por
Rick Mascitti en el verano de 1983 y elegido como reemplazo para el
nombre \"C con clases\". El nombre daba a entender el carácter evolutivo
de C. Los símbolos `++` son el operador de incremento de C. De acuerdo a
Stroustrup, el nombre pudo haber sido más corto: C+, pero era un error
de sintaxis. También se intentó usar un nombre sin relación, como D,
pero el lenguaje era una extensión de C, y ya existía un supuesto
sucesor de C llamado D. La primera versión comercial de C++ fue liberada
en 1985 y se documentó en el libro escrito por el propio Stroustrup
titulado \"El lenguaje de programación C++\" publicado en 1986. El
nombre, C++ fue elegido como una variante del lenguaje de programación
C, ya que en esencia C++ es una extensión de C. Se eligió el nombre
debido a que el operador `++` significa \"agregar uno a la variable\" y
por lo tanto el lenguaje C++ sería una versión \"mejorada\" de `C`.

C++ fue diseñado sobre todo para evitar programar en ensamblador, en C,
y en varios lenguajes de alto nivel en ese entonces de moda. Su objetivo
principal era hacer que la escritura de programas fuera más fácil y
agradable para el programador individual. No hubo un proyecto de C++ o
un comité de diseño. El diseño, la documentación y la implementación se
dieron simultáneamente. El lenguaje se desarrolló para hacer frente a
los problemas encontrados por los usuarios y como resultado de las
discusiones entre los amigos, colegas y el propio Stroustrup.

En 1989 la ANSI estandarizo por primera vez el lenguaje. En ese mismo
año se publico el manual de anotaciones y referencia de C++ (ARM). En
1991 se publicó la segunda edición del libro \"El lenguaje de
programación C++\", en el que se presenta la programación genérica
usando plantillas y el manejo de errores usando excepciones. En 1997 se
publica la tercera edición del libro \"El lenguaje de programación C++\"
que introduce el nuevo estándar ISO C++ 98. En esta versión se agregaron
al lenguaje los espacios de nombres, las conversiones dinámicas y las
plantillas. El estándar agrego la biblioteca STL de contenedores
genéricos y algoritmos. Esta fue la primera gran innovación del lenguaje
y una de la más importantes. La STL es un framework de algoritmos y
contenedores agregados a la biblioteca estándar. La STL fue el trabajo
de Alex Stepanov (junto con Dave Musser, Meng Le y otros programadores)
basado en el trabajo de más de una década sobre programación genérica.
Andrew Koenig, Beman Dawes y el propio Stroustrup ayudaron a validar el
trabajo de Stepanov para incluirlo en el estándar.

A partir de 1998, se han liberado distintas versiones de C++. Las
versiones siempre buscan mantener compatibilidad con estándares
anteriores y (con C), al mismo tiempo que agregan nuevas características
y capacidades al lenguaje, convirtiendo a C++ en un lenguaje moderno y
en constante evolución. En 2002 se inició la revisión del nuevo
estándar, llamado C++0x en el que se corrigen varios errores. Se
introducen nuevos componentes en la librería estándar como expresiones
regulares, contenedores no ordenados (tablas hash) y punteros para la
administración de recursos.

## C++ Moderno

En 2006 se lleva a cabo un reporte técnico sobre problemas de
rendimiento para contestar preguntas sobre costo, pronosticabilidad y
problemas técnicos, en su mayor parte relacionados con programación de
sistemas embebidos. En 2009 se inicia con el trabajo de C++0x y se
libera C++11 en 2011. Este nuevo estándar proporciona la semántica de
inicialización uniforme, argumentos para plantillas, agrega expresiones
lambda, alias de tipo, y un modelo de memoria más adecuado para usar
concurrencia. La biblioteca estándar tabién agrega varios componentes,
incluyendo threads (hilos), locks (bloqueos) y muchos de los componentes
del reporte técnico de 2003. En 2012 surge la primera implementación
publica de C++11. Iniciando lo que se conoce hoy en día como **C++
Moderno**.

En 2012 el comité inicia los trabajos de los nuevos estándares
(nombrados C++14 y C++17) para incluir algunas mejoras, como funciones
lambda. En 2013 Stroustrup publica la cuarta edición del libro \"El
lenguaje de programación C++\". Que incluye por primera vez el estándar
C++11. En 2014 se libera y publica C++14. El nombre sigue la tradición
de denominar a las versiones del lenguaje con la fecha de publicación de
la especificación. Durante el desarrollo se utilizó la denominación
C++1y para referirse al borrador del estándar, análogamente a cómo la
denominación C++0x se había utilizado durante el desarrollo de C++11.

::: parsed-literal
Línea de tiempo de C++

1983 1985 1989 +\-\-\-\-\-\-\-\-\-\-\-\-\--+ +\-\-\-\--+
+\-\-\-\-\-\-\--+ \| C con clases
[\|\-\-\-\-\-\--\>\|](##SUBST##|------->|) C++
[\|\-\-\-\-\-\--\>\|](##SUBST##|------->|) C++89 \|
+\-\-\-\-\-\-\-\-\-\-\-\-\--+ +\-\-\-\--+ +\-\-\--+\-\--+ \| v
+\-\-\--+\-\--+ \| C++98 \| C, C++ +\-\-\--+\-\--+ \| v +\-\-\--+\-\--+
\| C++03 \| +\-\-\--+\-\--+ \| v +\-\-\--+\-\--+ +\-\-\-\-\-\-\--+ C++
Moderno \| C++0x [\|\-\--\>\|](##SUBST##|--->|) C++11 \| 2011
+\-\-\-\-\-\-\--+ +\-\-\--+\-\--+ \| v +\-\-\-\-\-\-\--+
+\-\-\--+\-\-\--+ \| C++14 [\|\<\-\--\|](##SUBST##|<---|) C++1y \|
+\-\-\--+\-\--+ +\-\-\-\-\-\-\-\--+ \| v +\-\-\--+\-\--+
+\-\-\-\-\-\-\-\--+ \| C++1z [\|\-\--\>\|](##SUBST##|--->|) C++17 \|
+\-\-\-\-\-\-\--+ +\-\-\--+\-\-\--+ \| +\-\-\-\-\-\-\-\--+
+\-\-\-\-\-\-\-\--+ +\-\-\-\-\-\-\-\--+ \| \| C++26
[\|\<\-\-\-\-\--\|](##SUBST##|<------|) C++23
[\|\<\-\-\-\-\--\|](##SUBST##|<------|) C++20 \|\<\-\-\-\--+
+\-\-\-\-\-\-\-\--+ +\-\-\-\-\-\-\-\--+ +\-\-\-\-\-\-\-\--+
:::

En 2017 se libera la especificación final de C++17. En Mayo del 2020 se
libera la especificación C++20. La cual agrega varios cambios
\"grandes\" al lenguaje. Se introduce la biblioteca `ranges` como una
opción para trabajar con contenedores, se introduce la biblioteca
`format` como una opción para el formateo de cadenas y se introduce el
nuevo sistema de módulos. En 2023 se libera la nueva especificación
C++23, siguiendo la tradición de liberar cada tres años una nueva
versión. Esta versión introduce pocos cambios en comparación con C++20.
Se mejora la biblioteca `ranges`, y se introduce por primera vez la
función `print()`. Además de varios tipos y utilidades que se suman a la
biblioteca estándar.

La próxima versión se espera que se libere en 2026. La cual
tentativamente llevará por nombre C++26 y se espera que incluya muchas
mejoras y utilidades requeridas por algoritmos de Inteligencia Artifical
como matrices y funciones Matématicas tipo Blas.
