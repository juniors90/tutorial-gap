.. role:: underline
    :class: underline

Funciones
============================

Ya ha visto cómo usar funciones en la librería **GAP**, es decir, cómo aplicarlas a argumentos.

En esta sección verá cómo escribir funciones en el lenguaje **GAP**. También verá cómo usar la instrucción ``if`` y declarar variables locales con la instrucción local en la definición de la función. Las construcciones de bucle a través de ``while`` y ``for`` se analizan con más detalle, al igual que las funciones recursivas.

Escribiendo Funciones
----------------------

Escribiendo una función que imprima *hola, mundo*. en la pantalla es un ejercicio simple en **GAP**.

.. code-block:: gap
    :caption: Escribiendo una función
    :name: funcion_sayhello
    
    gap> sayhello:= function()
    > Print("hello, world.\n");
    > end;
    function( ) ... end

Esta función, cuando se llama, solo ejecutará la instrucción ``Print`` en la segunda línea. Esto imprimirá la cadena hola, mundo. en la pantalla seguido de un carácter de nueva línea ``\n`` que hace que el mensaje **GAP** aparezca en la siguiente línea en lugar de seguir inmediatamente a los caracteres impresos.

Sintaxis de una Función en GAP
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

    - La definición de función tiene la siguiente sintaxis.
    
    .. code-block:: gap
        :caption: Sintaxis de una función
        :name: sintaxis_de_una_funcion
        
        function( arguments ) statements end

    La definición de una función comienza con la palabra reservada ``function`` seguida de los argumentos de la lista de parámetros formales entre paréntesis "``( )``". La lista de parámetros formales puede estar vacía como en el ejemplo. Varios parámetros están separados por comas. Tenga en cuenta que no debe haber un punto y coma detrás del paréntesis de cierre. La definición de la función termina con la palabra reservada ``end``.
    
    Una función **GAP** es una expresión como un número entero, una suma o una lista. Por tanto, puede asignarse a una variable.
    
    El punto y coma de terminación en el ejemplo no pertenece a la definición de función, pero termina la asignación de la función al nombre ``sayhello``. A diferencia del caso de los números enteros, sumas y listas, el valor de la función ``sayhello`` se repite en la función abreviada ``fashion ( ) ... end``. Esto muestra la parte más interesante de una función: su lista de parámetros formales (que está vacía en este ejemplo). El valor completo de ``sayhello`` se devuelve si usa la función ``Print``.
    
    .. code-block:: gap
        :caption: Escribiendo una función personalizada
        :name: funcion_print_sayhello
    
        gap> Print(sayhello, "\n");
        function ( )
            Print( "hello, world.\n" );
            return;
        end

    Tenga en cuenta el carácter de nueva línea adicional "``\n``" en la declaración de impresión. Se imprime después del objeto ``sayhello`` para comenzar una nueva línea. **GAP** inserta la declaración de retorno adicional para simplificar el proceso de ejecución de la función.
    
    La función recién definida ``sayhello`` se ejecuta llamando a ``sayhello()`` con una lista de argumentos vacía.
    
    .. code-block:: gap
        :caption: Llamando una función sayhello
        :name: Llamando_a_sayhello
    
        gap> sayhello();
        hello, world.
        
    Sin embargo, este no es un ejemplo típico, ya que no se devuelve ningún valor, sino que solo se imprime una cadena.

Condicional If
----------------------

En el siguiente ejemplo definimos un signo de función que determina el signo de un número.

.. code-block:: gap
    :caption: ejemplo de uso del condicional if
    :name: ejemplo_de_uso_del_condicional_if
    
    gap> sign:= function(n)
    >        if n < 0 then
    >            return -1;
    >        elif n = 0 then
    >            return 0;
    >        else
    >            return 1;
    >        fi;
    >    end;
    function( n ) ... end
    gap> sign(0); sign(-99); sign(11);
    0
    -1
    1

Este ejemplo también presenta la instrucción ``if`` que se usa para ejecutar declaraciones dependiendo de una condición.

    - La instrucción if tiene la siguiente sintaxis.
    
    .. code-block:: gap
        :caption: sintaxis del condicional if
        :name: sintaxis_condicional_if
        
        if condition then statements elif condition then statements else statements fi

    Puede haber varias partes ``elif``. La parte ``elif`` así como la parte ``else`` de la instrucción ``if`` pueden omitirse. Una instrucción ``if`` no es una expresión y, por lo tanto, no se puede asignar a una variable. Además, una instrucción ``if`` no devuelve un valor.
    
    Los números de Fibonacci se definen recursivamente por :math:`f (1) = f (2) = 1` y :math:`f (n) = f (n −1) + f (n −2)` para :math:`n \geq 3`. Dado que las funciones en **GAP** pueden llamarse a sí mismas, a la función ``fib`` que calcula los números de *Fibonacci* se puede implementar básicamente escribiendo las ecuaciones anteriores. (Sin embargo, tenga en cuenta que esta es una forma muy ineficiente de calcular :math:`f(n)`.

    .. code-block:: gap
        :caption: función (recursiva) Fibonacci
        :name: sintaxis_fibonacci
        
        gap> fib:= function(n)
        >        if n in [1, 2] then
        >            return 1;
        >        else
        >            return fib(n-1) + fib(n-2);
        >        fi;
        >    end;
        function( n ) ... end
        gap> fib(15);
        610

    Debería haber pruebas adicionales para que el argumento ``n`` sea un número entero positivo. Esta función ``fib`` puede dar lugar a resultados extraños si se llama con otros argumentos. Intente insertar las pruebas necesarias en este ejemplo.


Variables Locales
----------------------

Una función ``gcd`` que calcula el *máximo común divisor* de dos enteros mediante el **algoritmo de Euclides** necesitará una variable además de los argumentos formales.

.. code-block:: gap
    :caption: función máximo común divisor
    :name: sintaxis_maximo_comun_divisor
    
    gap> gcd:= function(a, b)
    >       local c;
    >       while b <> 0 do
    >           c:= b;
    >           b:= a mod b;
    >           a:= c;
    >       od;
    >       return c;
    >    end;
    function( a, b ) ... end
    gap> gcd(30, 63);
    3

La variable adicional ``c`` se declara como **variable local** en la declaración ``local`` de la definición de función. La declaración ``local``, si está presente, debe ser la primera declaración de una definición de función. Cuando se declaran varias variables locales en una sola declaración local, se separan por comas.

La variable ``c`` es de hecho una variable local, que es local a la función ``gcd``. Si intenta usar el valor de ``c`` en el ciclo principal, verá que ``c`` no tiene un valor asignado a menos que ya haya asignado un valor a la variable ``c`` en el ciclo principal. En este caso, la naturaleza local de ``c`` en la función ``gcd`` evita que se sobrescriba el valor de ``c`` en el bucle principal.

.. code-block:: gap
    :caption: la naturaleza local de una variable
    :name: la_naturaleza_local
    
    gap> c:= 7;;
    gap> gcd(30, 63);
    3
    gap> c;
    7

Decimos que en un ámbito dado un identificador identifica una variable única. Un **alcance** es una parte léxica de un texto de programa. Existe el ámbito global que incluye todo el texto del programa, y ​​hay ámbitos locales que van desde la palabra reservada ``function``, que denota el comienzo de una definición de función, hasta la palabra clave final correspondiente. Un ámbito local introduce nuevas variables, cuyos identificadores se dan en la lista de argumentos formales y la declaración local de la función. El uso de un identificador en un texto de programa se refiere a la variable en el ámbito más interno que tiene este identificador como su nombre.


Recursion
----------------------

Ya hemos visto la recursividad en la función ``fib`` en la ``Sección 4.2``. Aquí hay otro ejemplo un poco más complicado.

Ahora escribiremos una función para determinar el número de particiones de un entero positivo. Una partición de un entero positivo es una lista descendente de números cuya suma es el entero dado. Por ejemplo, ``[4, 2, 1, 1]`` es una partición de :math:`8`. Tenga en cuenta que solo hay una partición de :math:`8`, es igual a la lista vacía ``[]``. El conjunto completo de todas las particiones de un número entero :math:`n` puede dividirse en subconjuntos con respecto al elemento más grande. Por lo tanto, el número de particiones de :math:`n` es igual a la suma de los números de particiones de :math:`n-i` con elementos menores o iguales que :math:`i` para todos los :math:`i` posibles. Más generalmente, el número de particiones de :math:`n` con elementos menores que m es la suma de los números de particiones de :math:`n-i` con elementos menores que :math:`i` para :math:`i` menores que :math:`m` y :math:`n`. Esta descripción produce la siguiente función.

.. code-block:: gap
        :caption: función (recursiva) nrparts
        :name: sintaxis_nrparts
        
        gap> nrparts:= function(n)
        >    local np;
        >    np:= function(n, m)
        >        local i, res;
        >        if n = 0 then
        >            return 1;
        >        fi;
        >        res:= 0;
        >        for i in [1..Minimum(n,m)] do
        >            res:= res + np(n-i, i);
        >        od;
        >        return res;
        >    end;
        >    return np(n,n);
        > end;
        function( n ) ... end

Queríamos escribir una función que tenga un argumento. Resolvimos el problema de determinar el número de particiones en términos de un procedimiento recursivo con dos argumentos. Entonces tuvimos que escribir dos funciones. La función ``nrparts`` que se puede usar para calcular el número de particiones, de hecho, solo toma un argumento. La función ``np`` toma dos argumentos y resuelve el problema de la forma indicada. La única tarea de la función ``nrparts`` es llamar a ``np`` con dos argumentos iguales.

Hicimos ``np`` local para ``nrparts``. Esto ilustra la posibilidad de tener funciones locales en **GAP**. Sin embargo, no es necesario ponerlo allí. ``np`` también podría definirse en el nivel principal, pero entonces el identificador ``np`` estaría vinculado y no podría usarse para otros propósitos, y si se usara, la función esencial ``np`` ya no estaría disponible para nrparts.

Ahora eche un vistazo a la función ``np``. Tiene dos variables locales ``res`` e ``i``. La variable ``res`` se usa para recolectar la suma e ``i`` es una variable del bucle. En el ciclo, la función ``np`` se vuelve a llamar a sí misma con otros argumentos. Sería muy perturbador si esta llamada de ``np`` fuera a usar la misma ``res`` e ``i`` que la llamada ``np``. Dado que la nueva llamada de ``np`` crea un nuevo alcance con nuevas variables, afortunadamente, este no es el caso.

Tenga en cuenta que los parámetros formales "``n``" y "``m``" de "``np``" se tratan como variables locales. (Independientemente de la estructura recursiva de un algoritmo, a menudo es más barato (en términos de tiempo de cálculo) evitar una implementación recursiva si es posible (y es posible en este caso), porque una llamada a función no es muy barata).