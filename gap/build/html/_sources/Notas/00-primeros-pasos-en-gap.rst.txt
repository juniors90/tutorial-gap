.. role:: underline
    :class: underline

Inicializando y cerrando GAP
==================================

Si el programa está instalado correctamente, generalmente inicia **GAP** simplemente escribiendo ``gap`` en el ``prompt`` o ``cmd`` de su sistema operativo seguido de la tecla de retorno, a veces también se denomina tecla de nueva línea.

.. code-block::
    :caption: Inicializando GAP
    :name: inicializando_GAP

    $> gap

**GAP** responde a este comando abriendo una nueva ventana de comandos y luego muestra su propia entrada de linea de comandos ``gap>``. (Puede evitar el banner con la opción de línea de comando ``-b``; hay más opciones de línea de comando descrito en la ``Sección 3.1`` del manual de referencia).

.. code-block:: gap
    :caption: ventana de comandos de GAP
    :name: ventana_de_comandos_de_GAP

    ┌───────┐   GAP, Version 4.7.8 of 09-Jun-2015 (free software, GPL)
    │  GAP  │   http://www.gap-system.org
    └───────┘   Architecture: i686-pc-cygwin-gcc-default32
    Libs used:  gmp, readline
    Loading the library and packages ...
    Components: trans 1.0, prim 2.1, small* 1.0, id* 1.0
    Packages:   AClib 1.2, Alnuth 3.0.0, AtlasRep 1.5.0, AutPGrp 1.6, Browse 1.8.6, CRISP 1.3.8,
                Cryst 4.1.12, CrystCat 1.1.6, CTblLib 1.2.2, FactInt 1.5.3, FGA 1.2.0, GAPDoc 1.5.1,
                IO 4.4.4, IRREDSOL 1.2.4, LAGUNA 3.7.0, Polenta 1.3.2, Polycyclic 2.11, RadiRoot 2.7,
                ResClasses 3.4.0, Sophus 1.23, SpinSym 1.5, TomLib 1.2.5
    Try '?help' for help. See also  '?copyright' and  '?authors'
    gap>

La forma habitual de :underline:`finalizar una sesión` **GAP** es escribir ``quit;`` en el indicador ``gap>``. 

.. warning::
    
    ¡No omitir el punto y coma!

.. code-block:: gap
    
    gap> quit;
    $

En algunos sistemas, puede escribir ``ctrl-D`` para obtener el mismo efecto. En cualquier situación, **GAP** finaliza escribiendo ``ctrl-C`` dos veces en un segundo. Aquí, como siempre, una combinación como ``ctrl-D`` significa que debe presionar la tecla ``D`` mientras mantiene presionada la tecla ``ctrl``.

En algunos sistemas (por ejemplo, Apple Macintosh) pueden ser necesarios cambios menores. Esto se explica
en el ``Capítulo 7.3`` del manual de referencia.

En la mayoría de los lugares, los caracteres de **espacio en blanco** (es decir, espacios, tabulaciones y retornos) son insignificantes para el significado de la entrada **GAP**. Sin embargo, los identificadores y las palabras clave no deben contener espacios en blanco. Por otro lado, a veces debe haber espacios en blanco alrededor de los identificadores y palabras clave para separarlos entre sí y de los números. Usaremos espacios en blanco para formatear comandos más complicados para una mejor legibilidad.

.. note::
    
    Un comentario en **GAP** comienza con el símbolo ``#`` y continúa hasta el final de la línea. Los comentarios son tratados como espacios en blanco por **GAP**. Usamos comentarios en los ejemplos impresos en este tutorial para explicar ciertas líneas de entrada o salida.

Debería poder reproducir los resultados de los ejemplos de sesiones de **GAP** en este manual, en el siguiente sentido. Si inicia la sesión **GAP** con los dos comandos

.. code-block:: gap
    
    gap> SizeScreen( [ 80, ] ); LogTo( "logfile1" );

(que se utilizan para establecer la longitud de línea en ``80`` si esta no es ya la longitud de línea predeterminada y para guardar una lista de la sesión en algún archivo), luego elija cualquier capítulo y vuelva a ejecutar sus ejemplos en una sesión continua y en el orden dado , la salida **GAP** debe verse como la salida que se muestra en el manual, excepto por algunas líneas de salida que hemos editado un poco con respecto a espacios en blanco o saltos de línea para mejorar la legibilidad. Sin embargo, cuando se involucran procesos aleatorios, puede obtener resultados diferentes si extrae ejemplos únicos y los ejecuta por separado.

Cargar código fuente desde un archivo externo
-----------------------------------------------

La forma más conveniente de crear piezas más grandes de código **GAP** es escribirlas en algún archivo de texto; para este propósito, simplemente puede usar su editor de texto favorito. Puede cargar un archivo de este tipo en **GAP** usando la función ``Read``:

.. code-block:: gap

    gap> Read("../../GAPProgs/Example.g");

Puede dar el nombre de ruta de acceso absoluta del archivo de origen o su nombre de ruta de acceso relativa desde el directorio raíz de **GAP** (el directorio que contiene ``bin/``, ``doc/``, ``lib/``, etc.).

.. note::

    observemos que todos los archivos se guardan on la extension ``.g``


El ciclo de impresión de lectura y evaluación
----------------------------------------------

**GAP** es un sistema interactivo. Ejecuta continuamente un ciclo de impresión de evaluación de lectura. Cada expresión que escribe en el teclado es leída por **GAP**, evaluada y luego se muestra el resultado.

La naturaleza interactiva de **GAP** le permite escribir una expresión en el teclado y ver su valor inmediatamente. Puede definir una función y aplicarla a argumentos para ver cómo funciona. Incluso puede escribir programas completos que contengan muchas funciones y probarlos sin salir del programa.

Cuando su programa sea grande, será más conveniente escribirlo en un archivo y luego leer ese archivo en **GAP**. Preparar sus funciones en un archivo tiene varias ventajas.
    - Puede componer sus funciones con más cuidado en un archivo (con su editor de texto favorito),
    - Puede corregir errores sin volver a escribir toda la función,
    - Puede guardar una copia para su uso posterior,
    - Puede subir su proyecto a un repositorio de GitHub, GitLab, etc.
    - Además, puede escribir muchos comentarios en el texto del programa, que **GAP** ignora, pero que son muy útiles para los desarrolladores del programa.

.. important::

    **GAP** trata la entrada de un archivo de la misma manera que trata la entrada del teclado. Se pueden encontrar más detalles en la ``Sección 9.7.1`` del :underline:`Manual de referencia`.


Un cálculo simple con **GAP** es tan fácil como uno se puede imaginar. Escribe el problema justo después de la indicación, lo termina con un punto y coma y luego pasa el problema al programa con la tecla de retorno. Por ejemplo, para multiplicar la diferencia entre :math:`9` y :math:`7` por la suma de :math:`5` y :math:`6`, es decir para calcular :math:`(9 - 7) \ast (5 + 6)`, se escribe exactamente esta última secuencia de símbolos finalizada con ``;`` y retorna.

.. code-block:: gap

    gap> (9 - 7) * (5 + 6);
    22
    gap>

Luego, **GAP** nos retorna el resultado :math:`22` en la siguiente línea y muestra con el *prompt* que está listo para una siguiente entrada. De ahora en adelante, ya no imprimiremos este mensaje adicional.

Si cometemos un error al escribir la línea, pero antes de escribir la devolución final, podemos usar la tecla de *eliminar* (o, a veces, la tecla de *retroceso*) para eliminar el último carácter escrito. También podemos mover el cursor hacia atrás y hacia adelante en la línea con ``ctrl-B`` y ``ctrl-F`` e insertar o eliminar caracteres en cualquier lugar de la línea. Los comandos de edición de línea se describen en su totalidad en la ``Sección 6.9`` del **Manual de referencia**.

Entonces, la entrada puede constar de varias líneas, y **GAP** imprime un indicador ``>`` en cada línea de entrada excepto la primera, hasta que el comando se completa con un punto y coma. (Es posible que **GAP** ya evalúe parte de la entrada cuando se escribe *return*, por lo que para cálculos largos puede tomar algún tiempo hasta que aparezca la solicitud parcial). Siempre que vea la solicitud parcial y no pueda decidir qué **GAP** todavía está esperando, para escribir punto y coma hasta que vuelva el indicador normal. En cada situación, el significado exacto de la brecha de aviso es que el programa está esperando un nuevo problema.

Pero incluso si escribimos mal una linea de comandos, no tenemos que volver a escribirlo nuevamente todo. Supongamos que escribimos mal u olvidamos el último paréntesis de cierre. Entonces su comando es sintácticamente incorrecto y **GAP** lo notará, incapaz de calcular el resultado deseado.

.. code-block:: gap
    :caption: Syntax error
    :name: Syntax error

    gap> (9 - 7) * (5 + 6;
    Syntax error: ) expected
    (9 - 7) * (5 + 6;
                    ^
    gap>

En lugar del resultado, aparece un mensaje de error que indica el lugar donde ocurrió un símbolo inesperado con un signo de flecha ``^`` debajo. Como un programa de computadora no puede saber cuáles eran realmente sus intenciones, esto es solo una pista. Pero en este caso, **GAP** tiene razón al afirmar que debería haber un paréntesis de cierre antes del punto y coma. Ahora puede escribir ``ctrl-P`` para recuperar la última línea de entrada. Se escribirá después del mensaje con el cursor en la primera posición. Escriba ``ctrl-E`` para llevar el cursor al final de la línea, luego ``ctrl-B`` para mover el cursor un carácter atrás. El cursor ahora está en la posición del punto y coma. Ingrese el paréntesis que falta simplemente escribiendo ``)``. Ahora la línea es correcta y se puede pasar a **GAP** presionando la tecla de retorno. Tenga en cuenta que para esta acción no es necesario mover el cursor más allá del último carácter de la línea de entrada.

Cada línea de comandos que escribe se envía a **GAP** para su evaluación presionando retorno independientemente de la posición del cursor en esa línea. Ya no mencionaremos la clave de retorno a partir de ahora.

A veces, un error de sintaxis hará que **GAP** entre en un bucle de interrupción. Esto se indica mediante el mensaje especial ``brk>``. Si se produce otro error de sintaxis mientras **GAP** está en un bucle de interrupción, el indicador cambiará a ``brk_02>``, ``brk_03>`` y así sucesivamente. Podemos dejar el ciclo de interrupción actual y salir al siguiente exterior escribiendo ``quit;`` o presionando ``ctrl-D``. Con el tiempo, **GAP** volverá a su estado normal y mostrará su intervalo de aviso normal ``gap>`` nuevamente.

Constantes y Operadores
------------------------------

Operadores Aritméticos
~~~~~~~~~~~~~~~~~~~~~~~~~~~

En una expresión como :math:`(9 - 7) \ast (5 + 6)` las constantes :math:`5`, :math:`6`, :math:`7` y :math:`9` están compuestas por los operadores ``+``, ``*`` y ``-`` para dar como resultado un nuevo valor.

Hay tres tipos de operadores en GAP, operadores aritméticos, operadores de comparación y operadores lógicos. Ya has visto que es posible formar la suma, la diferencia y el producto de dos valores enteros. Hay algunos operadores más aplicables a los números enteros en **GAP**. Por supuesto, los números enteros pueden dividirse entre sí, lo que posiblemente resulte en valores racionales no enteros.

.. code-block:: gap

    gap> 12345/25;
    2469/5

Tengamos en cuenta que el numerador y el denominador se dividen por su máximo común divisor y que el resultado se representa de forma única como una instrucción de división.

Todavía no tenemos cifras negativas. Así que considere los siguientes ejemplos que se explican por sí mismos.

.. code-block:: gap

    gap> -3; 17 - 23;
    -3
    -6

El operador de exponenciación se escribe ``^``. Esta operación en particular puede dar lugar a un gran número de personas. Esto no es un problema para **GAP**, ya que puede manejar números de (casi) cualquier tamaño.

.. code-block::
    
    gap> 3^132;
    955004950796825236893190701774414011919935138974343129836853841


=========== ==============================
Los operadores aritméticos son:
------------------------------------------
``+``       Suma
``-``       Resta
``*``       Multiplicación
``/``       División
=========== ==============================

El operador Módulo
~~~~~~~~~~~~~~~~~~~~~~~~~~~

El operador ``mod`` le permite calcular un valor módulo otro.

.. code-block:: gap

    gap> 17 mod 3;
    2

Tengamos en cuenta que debe haber espacios en blanco alrededor de la palabra reservada ``mod`` en este ejemplo, ya que ``17mod3`` o ``17mod`` se interpretarían como identificadores. El espacio en blanco alrededor de los operadores que no constan de letras, por ejemplo, los operadores ``*`` y ``-``, no es necesario.

.. important::

    **GAP** conoce una precedencia entre operadores que puede ser reemplazada por paréntesis.

    .. code-block:: gap

        gap> (9 - 7) * 5 = 9 - 7 * 5;
        false


Operadores de Comparación
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Además de estos operadores aritméticos, existen :underline:`operadores de comparación` en **GAP**. Una comparación da como resultado un **valor booleano** que es otro *tipo de constante*.

=========== ==============================
Los operadores de comparación son:
------------------------------------------
``=``       igualdad
``<>``      desigualdad
``<``       menor
``<=``      menor o igual
``>``       mayor 
``>=``      mayor o igual
=========== ==============================

.. code-block:: gap

    gap> 10^5 < 10^4;
    false

Operadores Lógicos
~~~~~~~~~~~~~~~~~~~~~

Los valores booleanos ``true`` y ``false`` se pueden manipular mediante :underline:`operadores lógicos`, i. e., el :underline:`operador unitario` ``not`` y los :underline:`operadores binarios` ``and`` y ``or``. Por supuesto, los valores booleanos también se pueden comparar.

.. code-block:: gap

    gap> not true; true and false; true or false;
    false
    false
    true
    gap> 10 > 0 and 10 < 100;
    true

En resumen,

=========== ==============================
Los operadores de lógicos son:
------------------------------------------
``not``     negación
``and``     conjunción
``or``      disjunción
=========== ==============================
Los valores de lógicos son:
------------------------------------------
``true``    Verdadero
``false``   Falso
=========== ==============================

Permutaciones
~~~~~~~~~~~~~~~~~~

Otro tipo importante de constantes en **GAP** son las **permutaciones**. :underline:`Están escritos en notación cíclica y se pueden multiplicar`.

.. code-block:: gap

    gap> (1,2,3);
    (1,2,3)
    gap> (1,2,3) * (1,2);
    (2,3)

:underline:`La inversa de la permutación` ``(1,2,3)`` se denota por ``(1,2,3)^-1``. Además, el operador de exponenciación ``^`` se utiliza para determinar la imagen de un punto bajo una permutación y para conjugar una permutación con otra.

.. code-block:: gap

    gap> (1,2,3)^-1;
    (1,3,2)
    gap> 2^(1,2,3);
    3
    gap> (1,2,3)^(1,2);
    (1,3,2)

.. note::
    Las otras constantes con las que puede lidiar **GAP** se introducirán cuando se utilicen, por ejemplo, hay :underline:`elementos de campos finitos` como ``Z(8)`` y :underline:`raíces complejas de unidad` como ``E(4)``.

Caracteres
~~~~~~~~~~~~~~~~~~~~

El último **tipo de constantes** que queremos mencionar aquí son los ``caracteres``, que son simplemente objetos en **GAP** que representan caracteres arbitrarios del conjunto de caracteres del sistema operativo. Los literales de caracteres se pueden ingresar en **GAP** encerrando el carácter entre **comillas simples** ``'``.

.. code-block:: gap

    gap>'a';
    'a'
    gap> '*';
    '*'

No hay operadores definidos para los caracteres, excepto que los caracteres se pueden comparar. En esta sección, hemos visto que los valores pueden estar precedidos por operadores unarios y combinados por operadores binarios colocados entre los operandos. Hay reglas de precedencia que pueden anularse con paréntesis.

Una comparación da como resultado un valor booleano. Los valores booleanos se combinan mediante operadores lógicos. Además, hemos visto que **GAP** maneja números de tamaño arbitrario. Los números y los valores booleanos son constantes. Hay otros tipos de constantes en **GAP** como permutaciones. Ahora estamos en condiciones de usar **GAP** como una simple calculadora de escritorio.

Diferencia entre Variables y Objetos
---------------------------------------

Las constantes descritas en la última sección se especifican mediante ciertas combinaciones de dígitos y signos menos (en el caso de números enteros) o dígitos, comas y paréntesis (en el caso de permutaciones). Estas secuencias de caracteres siempre tienen el mismo significado para **GAP**. Por otro lado, existen variables, especificadas por una secuencia de letras y dígitos (incluyendo al menos una letra), y su significado depende de lo que se les haya asignado. Una asignación se realiza mediante una secuencia de comandos GAP de letras y dígitos igual a un significado, es decir,

.. code-block:: gap
    
    sequence_of_letters_and_digits:= meaning

donde
    - ``sequence_of_letters_and_digits``, la secuencia en el lado izquierdo, se llama :underline:`el identificador de la variable` y sirve como su nombre.
    - ``meaning``, el significado del lado derecho, puede ser una constante como un número entero o una permutación, pero también puede ser casi cualquier otro objeto **GAP**.
    
.. important::
    
    De ahora en adelante, usaremos el término **objeto** para denotar algo que se puede asignar a una variable.

.. warning::

    No debe haber espacios en blanco entre ``:`` y ``=`` en el operador de asignación. Además, no confunda el operador de asignación con el signo de igualdad único ``=`` que en **GAP** solo se usa para la prueba de igualdad.

.. code-block:: gap

    gap> a:= (9 - 7) * (5 + 6);
    22
    gap> a;
    22
    gap> a * (a + 1);
    506
    gap> a = 10;
    false
    gap> a:= 10;
    10
    gap> a * (a + 1);
    110

Después de una asignación, el objeto asignado se repite en la siguiente línea. La impresión del objeto de una declaración puede evitarse en todos los casos escribiendo un punto y coma doble.

.. code-block:: gap
    
    gap> w:= 2;;

Después de la asignación, la variable se evalúa como ese objeto si se evalúa. Por tanto, es posible referirse a ese objeto por el nombre de la variable en cualquier situación.

Este es, de hecho, todo el secreto de una asignación. Un identificador está ligado a un objeto y desde este momento apunta a ese objeto. Nada más. Esta vinculación se cambia con la siguiente asignación a ese identificador. Un identificador no denota un bloque de memoria como en algunos otros lenguajes de programación. Simplemente apunta a un objeto, al que el administrador de almacenamiento **GAP** le ha asignado su lugar en la memoria. Este lugar puede cambiar durante una sesión de **GAP**, pero eso no molesta al identificador. El identificador apunta al objeto, no a un lugar en la memoria.

Por la misma razón, no es el identificador el que tiene un tipo, sino el objeto. Esto significa, por otro lado, que el identificador a que ahora está vinculado a un objeto entero puede apuntar en la misma sesión a cualquier otro objeto independientemente de su tipo.

Los identificadores pueden ser secuencias de letras y dígitos que contienen al menos una letra. Por ejemplo, ``abc`` y ``a0bc1`` son identificadores válidos. Pero también ``123a`` es un identificador válido, ya que no se puede confundir con ningún número. Solo ``1234`` indica el número ``1234`` y no puede ser al mismo tiempo el nombre de una variable.

Dado que **GAP** distingue mayúsculas y minúsculas, ``a1`` y ``A1`` son identificadores diferentes. Las palabras clave como dejar de fumar no deben utilizarse como identificadores. Verá más palabras clave en las siguientes secciones.

En la parte restante de este manual ignoraremos la diferencia entre las variables, sus nombres (identificadores) y los objetos a los que apuntan. Puede ser útil pensar de vez en cuando sobre lo que realmente se entiende por términos como "el entero w".

Hay algunas variables predefinidas que vienen con **GAP**. Muchos de ellos los encontrará en los capítulos restantes de este manual, ya que las funciones también se denominan mediante identificadores.

Puede obtener una descripción general de todas las variables **GAP** ingresando ``NamesGVars()``. Muchos de estos están predefinidos. Si está interesado en las variables que ha definido usted mismo en la sesión actual de **GAP**, puede ingresar ``NamesUserGVars()``.

.. code-block:: gap
    
    gap> NamesUserGVars();
    [ "a", "w" ]

.. important::
    
    Este parece ser el lugar adecuado para establecer la siguiente regla:

        - El nombre de cada variable global en la librería **GAP** comienza con una **letra mayúscula**. Por lo tanto, si elige solo nombres que comienzan con una letra minúscula para sus propias variables, no intentará sobrescribir ninguna variable predefinida. (Tenga en cuenta que la mayoría de las variables predefinidas son de solo lectura, y si intenta cambiar sus valores, aparecerá un mensaje de error).

Hay otras variables interesantes, una de las cuales se presentará ahora.

Siempre que **GAP** devuelve un objeto imprimiéndolo en la siguiente línea, este objeto se asigna a la última variable. Entonces, si calcularamos

.. code-block:: gap
    
    gap> (9 - 7) * (5 + 6);
    22

y olvidamos asignar el objeto a la variable a para su uso posterior, aún puede hacerlo mediante la siguiente asignación.

.. code-block:: gap
    
    gap> a:= last;
    22

Además, hay variables ``last2`` y ``last3``, puedes adivinar sus valores.

En esta sección ha visto cómo asignar objetos a variables. Posteriormente se puede acceder a estos objetos a través del nombre de la variable, su identificador. También ha encontrado el concepto útil de las últimas variables que almacenan los últimos objetos devueltos. Y ha aprendido que un punto y coma doble evita que se imprima el resultado de una declaración.


Diferencia entre Objetos y Elementos
--------------------------------------

En la última sección mencionamos que el administrador de almacenamiento de **GAP** le asigna a cada objeto un lugar determinado en la memoria (aunque ese lugar puede cambiar en el transcurso de una sesión de **GAP**). En este sentido, los objetos en diferentes lugares de la memoria nunca son iguales, y si el objeto apuntado por la variable ``a`` (para ser más precisos, la variable con el identificador ``a``) es igual al objeto apuntado por la variable ``b``, entonces Mejor debería decir que no solo son iguales sino idénticos. **GAP** proporciona la función IsIdenticalObj para probar si este es el caso.

.. code-block:: gap

    gap> a:= (1,2);; IsIdenticalObj( a, a );
    true
    gap> b:= (1,2);; IsIdenticalObj( a, b );
    false
    gap> b:= a;; IsIdenticalObj( a, b );
    true

Como indica el ejemplo anterior, los objetos **GAP** ``a`` y ``b`` pueden ser distintos aunque son iguales desde un punto de vista matemático, es decir, aunque deberíamos tener ``a = b``. Puede ser que los objetos ``a`` y ``b`` estén almacenados en diferentes lugares en la memoria, o puede ser que tengamos una relación de equivalencia definida en el conjunto de objetos bajo los cuales ``a`` y ``b`` pertenecen a la misma clase de equivalencia. Por ejemplo, si :math:`a=x^{3}` y :math:`b=x^{-5}` son palabras en el grupo presentado de forma finita :math:`\langle x | x^{2}=1 \rangle`, tendríamos :math:`a=b` en ese grupo.

**GAP** usa el :underline:`operador de igualdad` ``=`` para denotar tal igualdad matemática, no la identidad de los objetos. Por lo tanto, a menudo tenemos ``a = b`` aunque ``IsIdenticalObj(a, b) = false``. El operador ``=`` define una relación de equivalencia en el conjunto de todos los objetos **GAP**, y llamamos elementos de clases de equivalencia correspondientes. Expresándolo de manera diferente, el mismo elemento puede estar representado por varios objetos **GAP**.

Los ejemplos no triviales de elementos que están representados por diferentes objetos (objetos que realmente se ven diferentes, no aquellos que simplemente están almacenados en diferentes lugares de memoria) ocurrirán solo cuando consideremos objetos compuestos como listas o dominios.

Funciones
----------------

Un programa escrito en el lenguaje GAP se llama función. Las funciones son objetos **GAP** especiales. La mayoría de ellos se comportan como funciones matemáticas. Se aplican a los objetos y devolverán un nuevo objeto según la entrada. La función ``Factorial``, por ejemplo, se puede aplicar a un entero y devolverá el *factorial* de este entero.

.. code-block:: gap

    gap> Factorial(17);
    355687428096000

Aplicar una función a argumentos significa escribir los argumentos entre paréntesis después de la función. Varios argumentos están separados por comas, como en la función ``Gcd``, que calcula el *máximo común divisor* de dos números enteros.

.. code-block:: gap

    gap> Gcd(1234, 5678);
    2

Hay otras funciones que no devuelven un objeto, sino que solo producen un efecto secundario, por ejemplo, cambiar uno de sus argumentos. Estas funciones a veces se denominan procedimientos. La función ``Print`` solo se llama por el efecto secundario de imprimir algo en la pantalla.

.. code-block:: gap

    gap> Print(1234, "\n");
    1234

Para poder componer texto arbitrario con Imprimir, esta función en sí misma no producirá un salto de línea después de la impresión. Por lo tanto, teníamos otro carácter de nueva línea "``\n``" impreso para comenzar una nueva línea. Algunas funciones cambiarán un argumento y devolverán un objeto como la función ``Sortex`` que ordena una lista y devuelve la permutación de los elementos de la lista que ha realizado. No comprenderá ahora mismo lo que significa cambiar un objeto. Volveremos a este tema varias veces en las próximas secciones.

Una forma cómoda de definir una función nosotros mismos es el operador ``maps-to`` ``->`` que consta de un signo menos y un signo mayor sin espacios en blanco entre ellos. La función al cubo que asigna un número a su cubo se define en la siguiente línea.

.. code-block:: gap
    
    gap> cubed:= x -> x^3;
    function( x ) ... end

Una vez definida la función, ahora se puede aplicar

.. code-block:: gap

    gap> cubed(5);
    125

Las funciones más complicadas, especialmente las funciones con más de un argumento, no se pueden definir de esta manera. Verá cómo escribir sus propias funciones **GAP** en la ``Sección 4.1``.

En esta sección ha visto objetos **GAP** de tipo función. Ha aprendido a aplicar una función a argumentos. Esto produce como resultado un nuevo objeto o un efecto secundario. Un efecto secundario puede cambiar un argumento de la función. Además, ha visto una manera fácil de definir una función en **GAP** con el operador de mapas.

Help
----------------

El contenido de los manuales de **GAP** también está disponible como ayuda en línea. Una sesión **GAP** carga una larga lista de entradas de índice. Por lo general, contiene todos los encabezados de capítulos y secciones, todos los nombres de funciones documentadas, operaciones, etc., así como algunas entradas de índice explícitas definidas en los manuales.

El formato de una consulta es el siguiente.

.. code-block:: gap

    ?[book:][?] topic

Un ejemplo simple sería escribir ayuda en el mensaje de GAP. Si hay una sola sección con un tema de entrada de índice, esto se muestra directamente.

Si hay varias coincidencias, obtendrá una descripción general como en el siguiente ejemplo.

.. code-block:: gap

    gap> ?sets
    Help: several entries match this topic - type ?2 to get match [2]
    [1] Tutorial: Sets
    [2] Reference: Sets
    [3] Reference: sets
    [4] Reference: Sets of Subgroups
    [5] Reference: setstabilizer

Los manuales de **GAP** constan de varios libros, que se indican antes de los dos puntos en la lista anterior. Una consulta de ayuda se puede restringir a un libro usando el libro opcional: ``part``. Por ejemplo, ``? Tut: sets`` mostrará la primera de estas secciones de ayuda. Más precisamente, las partes del libro de cuerdas que están separadas por espacios en blanco se interpretan como comienzos de las primeras palabras del nombre del libro. Pruebe ``?books`` para ver la lista de libros disponibles y sus nombres.

La búsqueda de un *tema* coincidente (y un libro opcional) se realiza **sin distinción entre mayúsculas y minúsculas**. ¿Si hay otro? antes del *tema*, se realiza una búsqueda de subcadena para el *tema* en todas las entradas del índice. De lo contrario, las partes del *tema* que están separadas por espacios en blanco se consideran comienzos de las primeras palabras en una entrada de índice.

El espacio en blanco se normaliza en la cadena de búsqueda (y las entradas del índice).

**Ejemplos**. Todas las consultas siguientes conducen al capítulo del manual de referencia que explica el uso del sistema de ayuda de GAP con más detalle.

.. code-block:: gap

    gap> ?Reference: The Help System
    gap> ? REF : t h s
    gap> ?ref:? help system

La consulta ``??sets`` muestran todas las secciones de ayuda en todos los libros cuyas entradas de índice contienen las subcadenas ``sets``.

Como se mencionó en el ejemplo anterior, una lista completa de comandos para el sistema de ayuda está disponible en la ``?Ref: The Help System`` - El Sistema de Ayuda - del manual de referencia. En particular, hay comandos para navegar a través de las secciones de ayuda, ``?Ref: Browsing through the Sections`` - Navegación por las secciones - y hay una manera de influir en la forma en que se muestran las secciones de ayuda, ``?Ref: SetHelpViewer``. Por ejemplo, puede utilizar un programa de buscapersonas externo, un navegador web, dvi-previewer y/o pdf-viewer para leer la ayuda en línea de **GAP**.


