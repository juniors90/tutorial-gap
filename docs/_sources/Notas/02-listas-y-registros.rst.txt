.. role:: underline
    :class: underline

Listas y Registros
====================

La matemática moderna, especialmente el álgebra, se basa en la teoría de conjuntos. Cuando los conjuntos se representan en una computadora, inadvertidamente se convierten en listas. Es por eso que comenzamos nuestro estudio de los diversos objetos que **GAP** puede manejar con una descripción de las listas y su manipulación. **GAP** considera los conjuntos como un tipo especial de listas, es decir, listas sin huecos o duplicados cuyas entradas están ordenadas con respecto a la relación de precedencia ``<``.

Después de la introducción de las manipulaciones básicas con listas en ``3.1``, algunas dificultades relacionadas con la identidad y mutabilidad de listas se discuten en ``3.2`` y ``3.3``. Los conjuntos, rangos, vectores de fila y matrices se presentan como tipos especiales de listas en ``3.4``, ``3.5``, ``3.8``. Las operaciones de lista útiles se muestran en ``3.7``. Finalmente explicamos cómo usar registros en ``3.9``.

Listas Planas
------------------

Una :underline:`lista` es una colección de objetos separados por comas y entre corchetes. Construyamos, por ejemplo, la lista de números primos de los primeros :math:`10` números primos.

.. code-block:: gap

    gap> primes:= [2, 3, 5, 7, 11, 13, 17, 19, 23, 29];
    [ 2, 3, 5, 7, 11, 13, 17, 19, 23, 29 ]

Los dos números primos siguientes son :math:`31` y :math:`37`. Pueden agregarse a la lista existente mediante la función ``Append``, que toma la lista existente como su primera lista y otra lista como segundo argumento. El segundo argumento se agrega a los números de la lista ``primes`` y no se devuelve ningún valor. Tengamos en cuenta que al agregar otra lista, se los numeros del objeto ``primes`` son cambiados.

.. code-block:: gap

    gap> Append(primes, [31, 37]);
    gap> primes;
    [ 2, 3, 5, 7, 11, 13, 17, 19, 23, 29, 31, 37 ]

También puede agregar elementos nuevos individuales a las listas existentes mediante la función ``Add``, que toma la lista existente como su primer argumento y un nuevo elemento como su segundo argumento. El nuevo elemento se agrega a los números primos de la lista y nuevamente no se devuelve ningún valor, pero se cambian los números la lista ``primes``.

.. code-block:: gap

    gap> Add(primes, 41);
    gap> primes;
    [ 2, 3, 5, 7, 11, 13, 17, 19, 23, 29, 31, 37, 41 ]

Los elementos individuales de una lista se denominan por su posición en la lista. Para obtener el valor del séptimo primo, que es la séptima entrada en nuestra lista de números primos, simplemente escriba

.. code-block:: gap

    gap> primes[7];
    17

Este valor puede manejarse como cualquier otro valor, por ejemplo multiplicado por :math:`2` o asignado a una variable. Por otro lado, este mecanismo permite asignar un valor a una posición en una lista. Por lo tanto, el siguiente número primo :math:`43` puede insertarse en la lista directamente después de la última posición ocupada de números primos en la lista ``primes``. Esta última posición ocupada es devuelta por la función ``Length``.

.. code-block:: gap
    
    gap> Length(primes);
    13
    gap> primes[14]:= 43;
    43
    gap> primes;
    [ 2, 3, 5, 7, 11, 13, 17, 19, 23, 29, 31, 37, 41, 43 ]

Tenga en cuenta que esta operación ha cambiado de nuevo los números primos de los objetos. La siguiente posición después del final de una lista no es la única posición capaz de tomar un nuevo valor. Si sabe que :math:`71` es el vigésimo número primo, puede ingresarlo ahora mismo en la vigésima posición de los números primos. Esto dará como resultado una lista con agujeros que, sin embargo, sigue siendo una lista y ahora tiene una longitud de :math:`20`.


.. code-block:: gap

    gap> primes[20]:= 71;
    71
    gap> primes;
    [ 2, 3, 5, 7, 11, 13, 17, 19, 23, 29, 31, 37, 41, 43,,,,,, 71 ]
    gap> Length(primes);
    20

Sin embargo, la lista en sí debe existir antes de que se pueda asignar un valor a una posición de la lista. Esta lista puede ser la lista vacía ``[ ]``.

.. code-block:: gap

    gap> lll[1]:= 2;
    Variable: 'lll' must have a value
    
    gap> lll:= []; lll[1]:= 2;
    [ ]
    2

Por supuesto, este mecanismo también puede modificar las entradas existentes de una lista. No lo haremos aquí porque es posible que los números primos ya no sean una lista de números primos. Intente cambiar el :math:`17` de la lista por un :math:`9`.

Para obtener la posición de :math:`17` en los números primos de la lista ``primes``, utilice la función ``Position``, que toma la lista como primer argumento y el elemento como segundo argumento y devuelve la posición de la primera aparición del elemento :math:`17` en los números primos de la lista ``primes``. Si el elemento no está incluido en la lista, ``Position`` devolverá el objeto especial ``fail``.

.. code-block:: gap

    gap> Position(primes, 17);
    7
    gap> Position(primes, 20);
    fail

En todos los cambios anteriores a los números primos de la lista ``primes``, la lista se ha redimensionado automáticamente. No es necesario que le digas a **GAP** qué tan grande quieres que sea una lista. Todo esto se hace de forma dinámica.

No es necesario que los objetos recopilados en una lista sean del mismo tipo.

.. code-block:: gap

    gap> lll:= [true, "This is a String",,, 3];
    [ true, "This is a String",,, 3 ]

Del mismo modo, una lista puede formar parte de otra lista. Una lista puede incluso ser parte de sí misma.


.. code-block:: gap

    gap> lll[3]:= [4,5,6];; lll;
    [ true, "This is a String", [ 4, 5, 6 ],, 3 ]
    gap> lll[4]:= lll;
    [ true, "This is a String", [ 4, 5, 6 ], ~, 3 ]

Ahora, la tilde en la cuarta posición de ``lll`` denota el objeto que se está imprimiendo actualmente. Tenga en cuenta que el resultado de la última operación es el valor real del objeto ``lll`` en el lado derecho de la asignación. De hecho, es idéntico al valor de toda la lista ``lll`` en el lado izquierdo de la tarea.

Una cadena es un tipo especial de lista, es decir, una lista densa de caracteres, donde densa significa que la lista no tiene huecos. Aquí, los caracteres son objetos **GAP** especiales que representan un elemento del juego de caracteres del sistema operativo. La entrada de caracteres imprimibles es encerrándolos entre comillas simples ". Un literal de cadena puede ingresarse como la lista de caracteres o escribiendo los caracteres entre comillas dobles ``"``. Las cadenas son manejadas especialmente por ``Print``. Puede aprender mucho más sobre cadenas en el manual de referencia.

.. code-block:: gap

    gap> s1 := [’H’,’a’,’l’,’l’,’o’,’ ’,’w’,’o’,’r’,’l’,’d’,’.’];
    "Hallo world."
    gap> s1 = "Hallo world.";
    true
    gap> s1[7];
    'w'


Las sublistas de listas se pueden extraer y asignar fácilmente utilizando la lista de operadores ``list{ positions }``.

.. code-block:: gap

    gap> sl := lll{ [ 1, 2, 3 ] };
    [ true, "This is a String", [ 4, 5, 6 ] ]
    gap> sl{ [ 2, 3 ] } := [ "New String", false ];
    [ "New String", false ]
    gap> sl;
    [ true, "New String", false ]

De esta manera obtienes una nueva lista cuya :math:`i`-ésima entrada es ese elemento de la lista original cuya posición es la :math:`i`-ésima entrada del argumento entre llaves.


Listas Identicas
-----------------

Esta segunda sección sobre listas está dedicada a la sutil diferencia entre **igualdad** e **identidad** de listas. Es realmente importante comprender esta diferencia para comprender cómo se realizan las estructuras de datos complejas en **GAP**. Esta sección se aplica a todos los objetos **GAP** que tienen subobjetos, por ejemplo, a listas y registros. Después de leer la ``Sección 3.9`` sobre registros, debe volver a esta sección y traducirla al contexto del registro.

Dos listas son **iguales** si todas sus entradas son iguales. Esto significa que el operador de igualdad ``=`` devuelve ``true`` para la comparación de dos listas si y solo si estas dos listas tienen la misma longitud y para cada posición los valores en las listas respectivas son iguales.

.. code-block:: gap

    gap> numbers := primes;; numbers = primes;
    true

Asignamos los números primos de la lista a los números de variable ``numbers`` y, por supuesto, son iguales ya que tienen la misma longitud y las mismas entradas. Ahora cambiaremos el tercer número a :math:`4` y volveremos a comparar el resultado con los números primos de ``primes``.

.. code-block:: gap

    gap> numbers[3]:= 4;; numbers = primes;
    true

Si ve que los números y los números primos siguen siendo iguales, verifique esto imprimiendo el valor de ``primes``. ¡La lista de números primos ya no es una lista de números primos! ¿Lo que ha sucedido? Lo cierto es que las listas ``primes`` y ``numbers`` no solo son iguales sino que también son **idénticos**. ``primes`` y ``numbers`` son dos variables que apuntan a la misma lista.

Si cambia el valor del subobjeto ``numbers[3]`` de ``numbers``, esto también cambiará ``primes``. Las variables **no apuntan** a un determinado bloque de memoria de almacenamiento, pero sí apuntan a un objeto que ocupa la memoria de almacenamiento. Entonces, los números de asignación ``numbers := primes`` **no crearon** una nueva lista en un lugar diferente de la memoria, sino que solo crearon los nuevos números de nombre para la misma lista anterior de primos.

De esto vemos que **un mismo objeto puede tener varios nombres**.

Si desea cambiar una lista con el contenido de ``primes`` independientemente de los números primos ``primes``, tendrá que hacer una copia de ``primes`` mediante la función ``ShallowCopy``, que toma un objeto como argumento y devuelve una copia del argumento. (Primero restauraremos el antiguo valor de los números primos ``primes``).

.. code-block:: gap

    gap> primes[3]:= 5;; primes;
    [ 2, 3, 5, 7, 11, 13, 17, 19, 23, 29, 31, 37, 41, 43,,,,,, 71 ]
    gap> numbers:= ShallowCopy(primes);; numbers = primes;
    true
    gap> numbers[3]:= 4;; numbers = primes;
    false

Ahora ``numbers`` ya no son iguales a los números ``primes`` y ``primes`` siguen siendo una lista de números primos. Verifique esto imprimiendo los valores de ``numbers`` y ``primes``.

Las listas y los registros se pueden cambiar de esta manera porque los objetos **GAP** de estos tipos tienen subobjetos. Para aclarar esta afirmación, considere el siguiente ejemplo.

.. code-block:: gap

    gap> i:= 1;; j:= i;; i:= i+1;;


Al sumar :math:`1` a :math:`i`, el valor de i ha cambiado. ¿Qué le pasa a :math:`j`?. Después de la segunda instrucción :math:`j` apunta al mismo objeto que :math:`i`, es decir, al entero :math:`1`. La suma no cambia el objeto :math:`1` sino que crea un nuevo objeto de acuerdo con la instrucción :math:`i+1`. En realidad, es la asignación la que cambia el valor de :math:`i`. Por lo tanto, :math:`j` todavía apunta al objeto :math:`1`. Los enteros (como permutaciones y booleanos) no tienen subobjetos. Los objetos de este tipo no se pueden cambiar, solo se pueden reemplazar por otros objetos. Y un reemplazo no cambia los valores de otras variables. En el ejemplo anterior, una asignación de un nuevo valor a la variable ``numbers`` tampoco cambiaría el valor de ``primes``.

Por último, pruebe los siguientes ejemplos y explique los resultados.

.. code-block:: gap

    gap> l:= [];; l:= [l];
    [ [ ] ]
    gap> l[1]:= l;
    [ ~ ]

Ahora regrese a la ``Sección 3.1`` y averigüe si las funciones ``Add`` y ``Append`` cambian sus argumentos.

Inmutabilidad
------------------

**GAP** tiene un mecanismo que protege las listas contra cambios como los que nos han molestado en la ``Sección 3.2``. La función ``Immutable`` toma como argumento una lista y devuelve una copia inmutable de la misma, es decir, una lista que se ve exactamente como la anterior, pero tiene dos propiedades adicionales:

    1. La nueva lista es inmutable, es decir, la lista misma y su los subobjetos no se pueden cambiar.
    2. Al construir la copia, se ha copiado cada parte de la lista que se puede cambiar, de modo que los cambios en la lista anterior no afectarán a la nueva. En otras palabras, la nueva lista no tiene subobjetos mutables en común con la lista anterior.

.. code-block:: gap

    gap> list := [ 1, 2, "three", [ 4 ] ];; copy := Immutable( list );;
    gap> list[3][5] := 'w';; list; copy;
    [ 1, 2, "threw", [ 4 ] ]
    [ 1, 2, "three", [ 4 ] ]
    gap> copy[3][5] := 'w';
    Lists Assignment: <list> must be a mutable list
    not in any function
    Entering break read-eval-print loop ...
    you can 'quit;' to quit to outer loop, or
    you can 'return;' and ignore the assignment to continue
    brk> quit;

Como consecuencia de estas reglas, en la copia inmutable de una lista que contiene una lista ya inmutable como subobjeto, este subobjeto inmutable no necesita ser copiado, porque es inmutable. Las listas inmutables son útiles en muchos objetos **GAP** complejos, por ejemplo, como generadores de listas de grupos. Al hacerlos inmutables, **GAP** garantiza que no se puedan agregar, eliminar o intercambiar generadores a la lista. Por supuesto, tales cambios conducirían a serias inconsistencias con otros conocimientos que pueden haber sido ya calculados para el grupo.

Una función inversa a ``Immutable`` es ``ShallowCopy``, que produce una nueva lista mutable cuya :math:`i`-ésima entrada es la :math:`i`-ésima entrada de la lista anterior. Las entradas individuales no se copian, simplemente se colocan en la nueva lista. Si la lista anterior es inmutable y, por lo tanto, las entradas de la lista son inmutables en sí mismas, el resultado de ``ShallowCopy`` es mutable solo en el nivel superior.

Cabe señalar que también otros objetos que no sean listas pueden aparecer en forma mutable o inmutable. Los registros (consulte la ``Sección 3.9``) proporcionan otro ejemplo.

Sets
-----------------

**GAP** conoce varios tipos especiales de listas. Un **set** en **GAP** es una lista que no contiene huecos (tal lista se llama densa) y cuyos elementos están estrictamente ordenados ``w.r.t. <;`` en particular, un conjunto no puede contener duplicados. (Más precisamente, se requiere que los elementos de un conjunto en **GAP** pertenezcan a la misma familia, pero aproximadamente esto significa que se pueden comparar usando el operador ``<``).

Esto proporciona un modelo natural para conjuntos matemáticos cuyos elementos vienen dados por una enumeración explícita. **GAP** también llama a un conjunto una lista estrictamente ordenada, y la función ``IsSSortedList`` prueba si una lista dada es un conjunto. Devuelve un valor booleano. Para casi cualquier lista cuyos elementos estén contenidos en la misma familia, existe un conjunto correspondiente. Este conjunto es construido por la función ``Set`` que toma la lista como argumento y devuelve un conjunto obtenido de esta lista ignorando huecos y duplicados y ordenando los elementos.

Los elementos de los conjuntos utilizados en los ejemplos de esta sección son cadenas.

.. code-block:: gap

    gap> fruits:= ["apple", "strawberry", "cherry", "plum"];
    [ "apple", "strawberry", "cherry", "plum" ]
    gap> IsSSortedList(fruits);
    false
    gap> fruits:= Set(fruits);
    [ "apple", "cherry", "plum", "strawberry" ]

Tenga en cuenta que las ``fruits`` de la lista original no es modificado por la función ``Set``. Tenemos que hacer una nueva asignación a la variable ``fruits`` para convertirla en un conjunto.

El operador ``in`` se usa para probar si un objeto es un elemento de un conjunto. Devuelve un valor booleano ``true`` o ``false``.

.. code-block:: gap

    gap> "apple" in fruits;
    true
    gap> "banana" in fruits;
    false

El operador en también se puede aplicar a listas ordinarias. Sin embargo, es mucho más rápido realizar una prueba de pertenencia para conjuntos, ya que los conjuntos siempre se ordenan y se puede utilizar una búsqueda binaria en lugar de una búsqueda lineal. Se pueden agregar nuevos elementos a un conjunto mediante la función ``AddSet`` que toma el conjunto ``fruits`` como su primer argumento y un elemento como su segundo argumento y agrega el elemento al conjunto si aún no estaba allí. Tenga en cuenta que el objeto ``fruits`` es cambiado.

.. code-block:: gap

    gap> AddSet(fruits, "banana");
    gap> fruits; # The banana is inserted in the right place.
    [ "apple", "banana", "cherry", "plum", "strawberry" ]
    gap> AddSet(fruits, "apple");
    gap> fruits; # fruits has not changed.
    [ "apple", "banana", "cherry", "plum", "strawberry" ]

Tenga en cuenta que insertar elementos nuevos en un conjunto con ``AddSet`` suele ser más caro que simplemente agregar elementos nuevos al final de una lista.

Los conjuntos pueden ser intersecados por la función ``Intersection`` y unidos por la función ``Union``, que toman dos conjuntos como argumentos y devuelven la intersección resp. unión de los dos conjuntos como un nuevo objeto.

.. code-block:: gap

    gap> breakfast:= ["tea", "apple", "egg"];
    [ "tea", "apple", "egg" ]
    gap> Intersection(breakfast, fruits);
    [ "apple" ]


Los argumentos de las funciones ``Intersection`` y ``Union`` pueden ser listas ordinarias, mientras que su resultado es siempre un conjunto. Tenga en cuenta que en el ejemplo anterior al menos un argumento de ``Intersection`` no era un conjunto. Las funciones ``IntersectSet`` y ``UniteSet`` también forman la intersección resp. unión de dos conjuntos. Sin embargo, no devolverán el resultado, sino que cambiarán su primer argumento para que sea el resultado. Pruébelos con cuidado.

Ranges
-----------------

Un *range* es una progresión aritmética finita de números enteros. Este es otro tipo especial de lista. Un rango se describe mediante los dos primeros valores y el último valor de la progresión aritmética que se dan en la forma *[primero, segundo ... último]*. En el caso habitual de una lista ascendente de enteros consecutivos, la segunda entrada puede omitirse

.. code-block:: gap

    gap> [1..999999]; # a range of almost a million numbers
    [ 1 .. 999999 ]
    gap> [1, 2..999999]; # this is equivalent
    [ 1 .. 999999 ]
    gap> [1, 3..999999]; # here the step is 2
    [ 1, 3 .. 999999 ]
    gap> Length( last );
    500000
    gap> [ 999999, 999997 .. 1 ];
    [ 999999, 999997 .. 1 ]

Esta representación impresa compacta de una lista bastante larga corresponde a una representación interna compacta. La función ``IsRange`` prueba si un objeto es un rango, la función ``ConvertToRangeRep`` cambia la representación de una lista que de hecho es un rango a esta representación interna compacta.

.. code-block:: gap
    
    gap> a:= [-2,-1,0,1,2,3,4,5];
    [ -2, -1, 0, 1, 2, 3, 4, 5 ]
    gap> IsRange( a );
    true
    gap> ConvertToRangeRep( a );; a;
    [ -2 .. 5 ]
    gap> a[1]:= 0;; IsRange( a );
    false

Tenga en cuenta que este cambio de representación **no cambia** el valor de la lista a. La lista a todavía se comporta en cualquier contexto de la misma manera que lo haría en la representación larga.


Ciclos For y While
----------------------

Dada una lista ``pp`` de permutaciones, podemos formar su producto por medio de un bucle ``for`` en lugar de escribir el producto explícitamente.

.. code-block:: gap

    gap> pp:= [ (1,3,2,6,8)(4,5,9), (1,6)(2,7,8), (1,5,7)(2,3,8,6),
    > (1,8,9)(2,3,5,6,4), (1,9,8,6,3,4,7,2)];;
    gap> prod:= ();
    ()
    gap> for p in pp do
    > prod:= prod*p;
    > od;
    gap> prod;
    (1,8,4,2,3,6,5,9)

Primero se inicializa una nueva variable prod para la permutación de identidad ``()``. Luego, la variable de ciclo ``p`` toma como valor una permutación tras otra de la lista ``pp`` y se multiplica por el valor actual de ``prod``, lo que da como resultado un nuevo valor que luego se asigna a ``prod``.


El bucle ``for``
~~~~~~~~~~~~~~~~~~

    - El bucle ``for`` tiene la siguiente sintaxis

    .. code-block:: gap
    
        for var in list do statements od;
    
    El efecto del ciclo ``for`` es ejecutar las declaraciones para cada elemento de la lista. Un bucle for es una instrucción y, por lo tanto, termina con un punto y coma. La lista de declaraciones está encerrada por las palabras clave ``do`` y ``od`` (do inverso). Un bucle ``for`` no devuelve ningún valor. Por lo tanto, tuvimos que preguntar explícitamente por el valor de ``prod`` en el ejemplo anterior.
    
    El bucle ``for`` puede recorrer cualquier tipo de lista, incluso una lista con huecos. En muchos lenguajes de programación, el bucle ``for`` tiene la forma

    .. code-block:: gap
    
        for var from first to last do statements od;
        
    En **GAP**, este es simplemente un caso especial del bucle ``for`` general como se define anteriormente, donde la lista en el cuerpo del bucle es un ``range`` (ver 3.5).

    - Recurrer una lista:
    
    .. code-block:: gap
        
        for var in [first.. last] do statements od;
    
    ¡Por ejemplo, puede recorrer un rango para calcular el factorial :math:`15`! del número :math:`15` de la siguiente manera

    .. code-block:: gap
        
        gap> ff:= 1;
        1
        gap> for i in [1..15] do
        > ff:= ff * i;
        > od;
        gap> ff;
        1307674368000


El bucle ``while``
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

    - El bucle ``while`` tiene la siguiente sintaxis
    
    .. code-block:: gap
        
        while condition do statements od;

    El ciclo ``while`` recorre las declaraciones siempre que la condición se evalúe como verdadera. Al igual que el ciclo ``for``, el ciclo ``while`` termina con la palabra clave ``od`` seguida de un punto y coma.
    
    Podemos usar nuestra lista de números primos ``primes`` para realizar una factorización muy simple. Comenzamos inicializando una lista de factores a la lista vacía. En esta lista queremos recopilar los factores primos del número :math:`1333`. Recuerde que debe existir una lista antes de que se puedan asignar valores a las posiciones de la lista. Luego recorreremos los números primos de la lista ``primes`` y probaremos para cada primo si divide el número. Si es así, dividiremos el número por ese primo, lo sumaremos a la lista de factores ``factors`` y continuaremos.

    .. code-block:: gap

        gap> n:= 1333;;
        gap> factors:= [];;
        gap> for p in primes do
        >        while n mod p = 0 do
        >            n:= n/p;
        >            Add(factors, p);
        >        od;
        >    od;
        gap> factors;
        [ 31, 43 ]
        gap> n;
        1

    Como ``n`` ahora tiene el valor ``1``, se han encontrado todos los factores primos de ``1333`` y ``factors`` contienen una la factorización completa de ``1333``. Esto, por supuesto, puede verificarse multiplicando ``31`` por ``43``.
    
    Este ciclo se puede aplicar a números arbitrarios para encontrar factores primos. Pero como ``primes`` no es una lista completa de todos los números primos, este ciclo puede fallar en encontrar todos los factores primos de un número mayor que ``2000``, digamos. Puede intentar mejorarlo de tal manera que se agreguen nuevos números primos a la lista si es necesario.
    
    Ya ha visto que los objetos de la lista se pueden cambiar. Por supuesto, esto también se aplica a la lista en un cuerpo de bucle. En la mayoría de los casos, debe tener cuidado de no cambiar esta lista, pero hay situaciones en las que esto es bastante útil. El siguiente ejemplo muestra una forma rápida de determinar los primos menores de 1000 mediante un método de tamiz. Aquí haremos uso de la función ``Unbind`` para eliminar entradas de una lista, y la declaración "``if``" cubierta en 4.2.
    
    .. code-block:: gap
        
        gap> primes:= [];;
        gap> numbers:= [2..1000];;
        gap> for p in numbers do
        >        Add(primes, p);
        >        for n in numbers do
        >            if n mod p = 0 then
        >                Unbind(numbers[n-1]);
        >            fi;
        >        od;
        >    od;

    El bucle interno elimina todas las entradas de ``numbers`` que son divisibles por el último primo detectado ``p``. Esto se realiza mediante la función ``Unbind``, que elimina la vinculación de la posición de la lista ``numbers[n-1]`` al valor ``n``, de modo que posteriormente ``numbers[n-1]`` ya no tengan un valor asignado. El siguiente elemento encontrado en ``numbers`` por el bucle exterior es necesariamente el siguiente primo.
    
    De manera similar, es posible ampliar la lista que se repite. Esto produce un algoritmo de órbita agradable y corto para la acción de un grupo, por ejemplo.
    
    Puede encontrar más información sobre los bucles ``for`` y ``while`` en las ``Secciones 4.17`` y ``4.19`` del manual de referencia.

Listas de Operaciones
----------------------

Existe una forma más cómoda que la que se dio en la sección anterior para calcular el producto de una lista de números o permutaciones.

.. code-block:: gap

    gap> Product([1..15]);
    1307674368000
    gap> Product(pp);
    (1,8,4,2,3,6,5,9)

La función ``Product`` toma una lista como argumento y calcula el producto de los elementos de la lista. Esto es posible siempre que se defina una multiplicación de los elementos de la lista. Entonces ``Product`` ejecuta un ciclo sobre todos los elementos de la lista.

There are other often used loops available as functions. Guess what the function Sum does. The function List may take a list and a function as its arguments. It will then apply the function to each element of the list and return the corresponding list of results. A list of cubes is produced as follows with the function cubed from Section 4.

Hay otros bucles de uso frecuente disponibles como funciones. Adivina qué hace la función ``Sum``. La función ``List`` puede tomar una lista y una función como argumentos. Luego aplicará la función a cada elemento de la lista y devolverá la lista de resultados correspondiente. Se produce una lista de cubos de la siguiente manera con la función al cubo de la ``Sección 4``.

.. code-block:: gap

    gap> cubed:= x -> x^3;;
    gap> List([2..10], cubed);
    [ 8, 27, 64, 125, 216, 343, 512, 729, 1000 ]

Para agregar todos estos cubos, podríamos aplicar la función ``Sum`` a la última lista. Pero también podemos dar la función ``cubed`` de ``Sum`` como argumento adicional.

.. code-block:: gap
    
    gap> Sum(last) = Sum([2..10], cubed);
    true

Los números primos menores que :math:`30` se pueden recuperar de la lista de números primos ``primes`` de la ``Sección 3.1`` mediante la función ``Filtered``. Esta función toma la lista de números primos ``primes`` y una propiedad como sus argumentos y devolverá la lista de aquellos elementos de ``primes`` que tienen esta propiedad. Dicha propiedad estará representada por una función que devuelve un valor booleano. En este ejemplo, la propiedad de ser menor que :math:`30` se puede representar mediante la función ``x -> x < 30`` ya que ``x < 30`` se evaluará como ``true`` para valores :math:`x` menores que :math:`30` y ``false`` en caso contrario.

.. code-block:: gap

    gap> Filtered(primes, x-> x < 30);
    [ 2, 3, 5, 7, 11, 13, 17, 19, 23, 29 ]

Ya hemos mencionado el operador ``{ }`` que forma sublistas. Toma una lista de posiciones como argumento y devolverá la lista de elementos de la lista original correspondiente a estas posiciones.

.. code-block:: gap

    gap> primes{ [1 .. 10] };
    [ 2, 3, 5, 7, 11, 13, 17, 19, 23, 29 ]

Finalmente, mencionamos la función ``ForAll`` que verifica si una propiedad es válida para todos los elementos de una lista. Toma como argumentos una lista y una función que devuelve un valor booleano. ``ForAll`` comprueba si la función devuelve ``true`` para todos los elementos de la lista.

.. code-block:: gap

    gap> list:= [ 1, 2, 3, 4 ];;
    gap> ForAll( list, x -> x > 0 );
    true
    gap> ForAll( list, x -> x in primes );
    false

Encontrará más bucles ``for`` predefinidos en el ``Capítulo 2.1`` del manual de referencia.


Vectores y Matrices
-----------------------------

Esta sección describe cómo **GAP** usa listas para representar matrices y vectores de fila. Un **vector de fila** (row vector) es una lista densa de elementos de un campo común. Una **matriz** (matrix) es una lista densa de vectores de fila sobre un campo común y de igual longitud.

.. code-block:: gap

    gap> v:= [3, 6, 2, 5/2];; IsRowVector(v);
    true

Los vectores de fila se pueden sumar y multiplicar por escalares de su campo. La multiplicación de vectores de fila de igual longitud da como resultado su producto escalar.

.. code-block:: gap
    
    gap> 2 * v; v * 1/3;
    [ 6, 12, 4, 5 ]
    [ 1, 2, 2/3, 5/6 ]
    gap> v * v; # the scalar product of 'v' with itself
    221/4

Tenga en cuenta que la expresión ``v * 1/3`` se evalúa en realidad multiplicando primero :math:`v` por :math:`1` (lo que da de nuevo :math:`v`) y luego dividiendo por :math:`3`. Esta también es una operación escalar permitida. La expresión ``v / 3`` resultaría en el mismo valor.

Tales operaciones aritméticas (si los resultados son nuevamente vectores) dan como resultado vectores mutables, excepto si la operación es binaria y ambos operandos son inmutables; por tanto, los vectores mostrados en los ejemplos anteriores son todos mutables.

Entonces, si desea producir una lista mutable con :math:`100` entradas iguales a :math:`25`, simplemente puede decir ``25 + 0 * [1 .. 100]``. Tenga en cuenta que los rangos también son vectores (sobre los racionales) y que ``[1 .. 100]`` es mutable.

Una matriz es una lista densa de vectores de fila de igual longitud.

.. code-block:: gap

    gap> m:= [[1,-1, 1],
    > [2, 0,-1],
    > [1, 1, 1]];
    [ [ 1, -1, 1 ], [ 2, 0, -1 ], [ 1, 1, 1 ] ]
    gap> m[2][1];
    2

Sintácticamente, una matriz es una lista de listas. Entonces, el número :math:`2` en la segunda fila y la primera columna de la matriz :math:`2` se conoce como el primer elemento del segundo elemento de la lista :math:`m` a través de ``m[2][1]``.

Una matriz se puede multiplicar por escalares, vectores de fila y otras matrices. (Si los vectores de fila y las matrices involucradas en dicha multiplicación no tienen las dimensiones adecuadas, las entradas "faltantes" se tratan como ceros, por lo que los resultados pueden parecer inesperados en tales casos).

.. code-block:: gap

    gap> [1, 0, 0] * m;
    [ 1, -1, 1 ]
    gap> [1, 0, 0, 2] * m;
    [ 1, -1, 1 ]
    gap> m * [1, 0, 0];
    [ 1, 2, 1 ]
    gap> m * [1, 0, 0, 2];
    [ 1, 2, 1 ]

Tenga en cuenta que la multiplicación de un vector de fila con una matriz dará como resultado una combinación lineal de las filas de la matriz, mientras que la multiplicación de una matriz con un vector de fila da como resultado una combinación lineal de las columnas de la matriz. En el último caso, el vector de fila se considera un vector de columna.

Un vector o matriz de números enteros también se puede multiplicar por un escalar de campo finito y viceversa. Tales productos dan como resultado una matriz sobre el campo finito con los números enteros mapeados en el campo finito de la manera obvia.

Las matrices de campo finito son más agradables de leer cuando se muestran con un ``Displayed`` en lugar de impresas con ``Printed``. (Aquí escribimos ``Z(q)`` para denotar :underline:`una raíz primitiva del campo finito con` :math:`q` :underline:`elementos`).

.. code-block:: gap

    gap> Display( m * One( GF(5) ) );
    1 4 1
    2 . 4
    1 1 1
    gap> Display( m^2 * Z(2) + m * Z(4) );
    z = Z(4)
    z^1 z^1 z^2
    1 1 z^2
    z^1 z^1 z^2

Las submatrices se pueden extraer fácilmente usando la expresión ``mat{rows}{columns}``. También se pueden asignar, siempre que la matriz grande sea mutable (lo que no es así si es el resultado de una operación aritmética, ver más arriba).

.. code-block:: gap
    
    gap> sm := m{ [ 1, 2 ] }{ [ 2, 3 ] };
    [ [ -1, 1 ], [ 0, -1 ] ]
    gap> sm{ [ 1, 2 ] }{ [2] } := [[-2],[0]];; sm;
    [ [ -1, -2 ], [ 0, 0 ] ]

Las primeras llaves contienen la selección de filas, la segunda la de columnas. Las matrices aparecen no solo en álgebra lineal, sino también como elementos de grupo, siempre que sean invertibles. Aquí tenemos la oportunidad de encontrar una función :underline:`teórica de grupo`, llamada ``Order``, que calcula :math:`\text{el orden de un elemento del grupo}`.

.. code-block:: gap

    gap> Order( m * One( GF(5) ) );
    8
    gap> Order( m );
    infinity

Para matrices cuyas entradas son objetos más complejos, por ejemplo, funciones racionales, es posible que los métodos ``Order`` de **GAP** no puedan probar que la matriz tiene un orden infinito, y uno recibe la siguiente advertencia.

.. code-block:: gap

    #I Order: warning, order of <mat> might be infinite

En tal caso, si :math:`\text{el orden de la matriz es realmente infinito}`, tendrá que interrumpir **GAP** presionando ``ctrl-C`` (seguido de ``ctrl-D`` o ``quit;`` para salir del ciclo de interrupción).

Para demostrar que :math:`\text{el orden de $m$ es infinito}`, también podríamos observar :math:`\text{el polinomio}` :math:`\text{mínimo de $m$ sobre los racionales}`.

.. code-block:: gap

    gap> f:= MinimalPolynomial( Rationals, m );; Factors( f );
    [ x_1-2, x_1^2+3 ]

``Factors`` devuelve una lista de factores irreducibles del polinomio ``f``. El primer factor irreductible :math:`x_1 - 2` nos dice que :math:`2` es un valor propio de ``m``, por lo que **su orden no puede ser finito**.


Registros simples
----------------------

Un registro proporciona otra forma de construir nuevas estructuras de datos. Como una lista, un registro contiene subobjetos. En un registro, los elementos, los denominados **componentes del registro** (record components), no están indexados por números sino por nombres.

En esta sección verá cómo definir y cómo utilizar registros. Los registros se cambian por asignaciones a componentes de registro.

Inicialmente, un registro se define como una lista separada por comas de asignaciones a sus componentes de registro.

.. code-block:: gap

    gap> date:= rec(year:= 1997,
    > month:= "Jul",
    > day:= 14);
    rec( year := 1997, month := "Jul", day := 14 )

El valor de un componente de registro es accesible por el nombre de registro y el nombre del componente de registro separados por un punto como selector de componente de registro.

.. code-block:: gap

    gap> date.year;
    1997

Las asignaciones a nuevos componentes de registro son posibles de la misma manera. El registro cambia de tamaño automáticamente para contener el nuevo componente.

.. code-block:: gap

    gap> date.time:= rec(hour:= 19, minute:= 23, second:= 12);
    rec( hour := 19, minute := 23, second := 12 )
    gap> date;
    rec( year := 1997, month := "Jul", day := 14,
    time := rec( hour := 19, minute := 23, second := 12 ) )


Podemos utilizar la función ``Display`` para ilustrar la jerarquía de los componentes del registro.

.. code-block:: gap

    gap> Display( date );
    rec(
        year := 1997,
        month := "Jul",
        day := 14,
        time := rec(
            hour := 19,
            minute := 23,
            second := 12 ) )

Los registros son objetos que pueden modificarse. Una asignación a un componente de registro cambia el objeto original. Las observaciones hechas en las ``Secciones 3.2`` y ``3.3`` sobre la identidad y la mutabilidad de las listas también son válidas para los registros.

A veces es interesante saber qué componentes de un determinado registro están vinculados. Esta información está disponible en la función ``RecNames``, que toma un registro como argumento y devuelve una lista de nombres de todos los componentes vinculados de este registro como una lista de cadenas.

.. code-block:: gap
    :caption: Función RecNames
    :name: funcion_RecNames

    gap> RecNames(date);
    [ "year", "month", "day", "time" ]

Ahora bien, veamos a las ``Secciones 3.2`` y ``3.3`` para ver que significan estas secciones para los registros.