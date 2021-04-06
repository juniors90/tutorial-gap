.. role:: underline
    :class: underline

Operaciones y Métodos
========================

Atributos
---------------------------------------------

En los capítulos anteriores, hemos visto cómo obtener información sobre objetos matemáticos en **GAP**: :underline:`tenemos que pasar el objeto como argumento a una función`. **Por ejemplo**, si :math:`G` es un grupo, se puede llamar a ``Size( G )``, y la función devolverá un valor, en nuestro ejemplo un número entero que es del tamaño de :math:`G`. Calcular el tamaño de un grupo generalmente requiere una cantidad sustancial de trabajo, por lo tanto, parece deseable almacenar el tamaño en algún lugar una vez que se haya calculado. Debe imaginar que **GAP** almacena el tamaño en algún lugar asociado con el objeto :math:`G` cuando ``Size( G )`` se ejecuta por primera vez, y si esta llamada a la función se ejecuta nuevamente más tarde, el tamaño simplemente se busca y se devuelve, sin más cálculos.

Esto significa que el comportamiento de la función Tamaño debe depender de si el tamaño del argumento :math:`G` ya se conoce y, en caso contrario, el tamaño debe almacenarse después de que se haya calculado. Estas dos tareas adicionales se realizan mediante otras dos funciones que acompañan a ``Size( G )``, a saber, el probador ``HasSize( G )`` y el configurador ``SetSize( G, size )``. El probador devuelve ``true`` o ``false`` según :math:`G` ya haya almacenado su tamaño, y el colocador coloca el tamaño en un lugar desde donde :math:`G` puede buscarlo directamente. La función ``Size`` en sí se llama ``getter``, y de la discusión anterior vemos que realmente debe haber al menos dos métodos para el ``getter``:
    
    - Un método se usa cuando el probador devuelve ``false``; es el método que primero hace el cálculo real y luego ejecuta el ``setter`` con el valor calculado.
    - Se utiliza un segundo método cuando el probador devuelve ``true``; simplemente devuelve el valor almacenado. Este segundo método también se denomina ``getter`` del sistema. Las funciones **GAP** para las que pueden estar disponibles varios métodos se denominan operaciones, por lo que ``Size`` es un ejemplo de una operación.

Lo conveniente para el usuario es que **GAP** elige automáticamente el método correcto para el ``getter``, es decir, llama a un ``getter`` de trabajo real como máximo una vez y al ``getter`` del sistema en todas las ocurrencias posteriores. A lo sumo una vez porque el valor de una llamada de función como ``Size( G )`` también se puede establecer para :math:`G` antes de que se llame al ``getter``; por ejemplo, se puede llamar directamente al colocador si se conoce el tamaño.

El tamaño de un grupo es un ejemplo de una clase de cosas que en **GAP** se denominan :underline:`atributos`. Cada atributo en GAP está representado por un triple de un ``getter``, un ``setter`` y un ``tester``. Cuando se declara un nuevo atributo, las tres funciones se crean juntas y el ``getter`` contiene referencias a las otras dos. Esto es necesario porque cuando se llama al ``getter``, primero debe consultar al probador y tal vez ejecutar el ``setter`` al final.

Por lo tanto, el ``getter`` podría implementarse de la siguiente manera:

.. code-block:: gap
    :caption: implementación de getter
    :linenos:
    :name: getter
    
    getter := function( obj )
        local value;
        if tester( obj ) then
            value := system_getter( obj );
        else
            value := real_work_getter( obj );
            setter( obj, value );
        fi;
        return value;
    end;

La única función que depende de la naturaleza matemática del atributo es el ``getter`` de trabajo real, y esto es, por supuesto, lo que debe instalar el programador de un atributo. En ambos casos, el ``getter`` devuelve el mismo valor, que también llamamos el valor del atributo (correctamente: el valor del atributo para el objeto ``obj``).

.. important::

    Los nombres para ``setter`` y probador de un atributo siempre se componen del prefijo ``Set`` resp. Tiene y el nombre del ``getter``.


Como ejemplo (no típico), tenga en cuenta que la función ``Random`` de **GAP**, aunque solo toma un argumento, por supuesto no es un atributo, porque de lo contrario el primer elemento aleatorio de un grupo sería almacenado por el establecedor y devuelto una y otra vez. por el captador del sistema cada vez que se llama a ``Random`` en la secuela).


Existe una regla general importante sobre los atributos: una vez que se ha establecido el valor de un atributo para un objeto, no se puede restablecer, es decir, no se puede cambiar más. Esto se logra al tener dos métodos no solo para el captador sino también para el configurador: si un objeto ya tiene un valor de atributo almacenado, es decir, si el probador devuelve ``true``, el configurador simplemente no hace nada.


.. code-block:: gap
    :caption: función SetSize
    :name: funcion_SetSize
    
    gap> G := SymmetricGroup(8);; Size(G);
    40320
    gap> SetSize( G, 0 ); Size( G );
    40320

Resumen
~~~~~~~~~~~~

En esta sección hemos introducido atributos como ternas de ``getter``, ``setter`` y ``tester`` y tenemos
explicó cómo estas tres funciones trabajan juntas detrás de escena para proporcionar almacenamiento automático y búsqueda de valores que se han calculado una vez. Hemos visto que pueden existir varios métodos para el mismo
función entre las cuales **GAP** selecciona automáticamente una apropiada.

Propiedades y Filtros
---------------------------------------------

Ciertos atributos, como ``IsAbelian``, tienen un valor booleano. Dichos atributos se conocen en **GAP** como propiedades, porque sus valores se almacenan de una manera ligeramente diferente. Una propiedad también tiene un ``getter``, un ``setter`` y un ``tester``, pero en este caso, tanto el ``getter`` como el ``tester`` devuelven un valor booleano. Por lo tanto, **GAP** almacena ambos valores de la misma manera, es decir, como bits en una lista booleana, tratando así a los ``getter``'s de propiedades y a todos los ``tester``'s (de atributos o propiedades) de manera uniforme. :underline:`Estas funciones con valores booleanos se denominan filtros` (``filters``). Puede imaginar un filtro como un conmutador que se establece en ``true`` o ``false``. Para cada objeto **GAP** hay una lista booleana que ha reservado un bit para cada filtro que conoce **GAP**. Estrictamente hablando, hay un bit para cada **filtro simple**, y estos filtros simples se pueden combinar con ``and`` para formar otros filtros (que son ``true`` si y solo si todos los bits correspondientes se establecen en ``true``). Por ejemplo, el filtro ``IsPermGroup`` e ``IsSolvableGroup`` se compone de varios filtros simples.

Dado que solo permiten dos valores, los bits que representan filtros se pueden comparar muy rápidamente, y el esquema por el cual **GAP** elige el método, por ejemplo, para un ``getter`` o un ``setter`` (como hemos visto en la sección anterior), se basa principalmente en en el examen de filtros, no en el examen de otros valores de atributo.

Los detalles de esta selección de método se describen en el ``Capítulo 2`` de "Programación en GAP".

Aquí solo presentamos la siguiente regla empírica: Cada método instalado para un atributo, digamos ``Size``, tiene un "filtro requerido", que se compone de ciertos filtros simples que deben ceder verdadero para que el argumento ``obj`` para que este método sea aplicable. Para ejecutar una llamada de ``Size( obj )``, **GAP** selecciona entre todos los métodos aplicables aquel cuyo filtro requerido combina los filtros más simples; la idea detrás es que cuanto más requiere un algoritmo de ``obj``, más eficiente se espera que sea. Por ejemplo, si ``obj`` es un grupo de permutación que no es (se sabe que es) solucionable, no es aplicable un método con el filtro requerido ``IsPermGroup`` e ``IsSolvableGroup``, mientras que se puede elegir un método con el filtro requerido ``IsPermGroup``. Por otro lado, si se supiera que ``obj`` se puede resolver, el método con el filtro requerido ``IsPermGroup`` e ``IsSolvableGroup`` sería preferible al método con el filtro requerido ``IsPermGroup``.

Puede suceder que un método sea aplicable para un argumento dado pero no pueda calcular el valor deseado. En tales casos, el método ejecutará la instrucción ``TryNextMethod()``; y **GAP** llama al siguiente método aplicable. Por ejemplo, `[Sim90] <https://core.ac.uk/download/pdf/82747608.pdf>`_ describe un algoritmo para calcular el tamaño de un grupo de permutación resoluble, que también puede usarse para decidir si un grupo de permutación es resoluble o no. Supongamos que la función ``size solvable`` implementa este algoritmo, y que devuelve el orden del grupo si se puede resolver y falla en caso contrario. Luego, podemos instalar el siguiente método para Tamaño con el filtro requerido ``IsPermGroup``.

.. code-block:: gap
    :caption: función TryNextMethod()
    :linenos:
    :name: funcion_TryNextMethod
    
    function( G )
        local value;
        value := size_solvable( G );
        if value <> fail then
            return value;
        else
            TryNextMethod();
        fi;
    end;


Este método se puede probar en cada grupo de permutación (ya sea que se sepa que se puede resolver o no), e incluiría una prueba de solubilidad obligatoria.

Si no se encuentra ningún método aplicable (o ningún método aplicable siguiente), **GAP** se detiene con un mensaje de error de la forma

.. code-block:: gap
    :caption: mensaje de error
    :name: mensaje_de_error
    
    Error, no method found! For debugging hints type ?Recovery from NoMethodFound
    Error, no 1st choice method found for ‘Size’ on 1 arguments called from
    ... lines deleted here ...

Recibirá un mensaje de error como el anterior si solicita ``Size( 1 )``. El mensaje simplemente dice que no hay ningún método instalado para calcular el tamaño de :math:`1`. La ``Sección 7.1`` del manual de referencia contiene más información sobre cómo tratar estos mensajes.

Resumen
~~~~~~~~~~~

En esta sección hemos introducido propiedades como atributos especiales y filtros como el concepto general detrás de las propiedades de getters y de los atributos testers. Los valores de los filtros de un objeto gobiernan cómo se trata el objeto en la selección de métodos para las operaciones.

Métodos Immediate y True
---------------------------------------------

En el ejemplo de la ``Sección 8.2``, hemos mencionado que la operación ``Size`` tiene un método para grupos de permutación resolubles que es tan superior al método para grupos de permutación generales que parece que vale la pena intentarlo incluso si no se sabe nada sobre la solubilidad del grupo. del cual se calculará el ``Size``. Hay otros ejemplos en los que ciertos métodos son incluso "más económicos" de ejecutar. Por ejemplo, si se conoce el tamaño de un grupo, es fácil comprobar si es impar y, de ser así, el teorema de *Feit-Thompson* nos permite establecer ``IsSolvableGroup`` en ``true`` para este grupo. **GAP** utiliza este celebrado teorema al tener un método inmediato para ``IsSolvableGroup`` con el filtro requerido HasSize que verifica la paridad del tamaño y establece ``IsSolvableGroup`` o no hace nada, es decir, llama a ``TryNextMethod()``. Estos métodos inmediatos se ejecutan automáticamente para un objeto cada vez que cambia el valor de un filtro, por lo que la capacidad de solución de un grupo se detectará automáticamente cuando se haya calculado un tamaño impar para él (y, por lo tanto, el valor de HasSize para ese grupo ha cambiado a ``true``).

Algunos métodos son incluso más inmediatos, porque no requieren ningún cálculo en absoluto: permiten establecer un filtro si también se establece otro filtro. En otras palabras, modelan una implicación matemática como ``IsGroup`` e ``IsCyclic`` :math:`\Rightarrow` ``IsSolvableGroup`` y tales implicaciones se pueden instalar en **GAP** como métodos ``true``. Para ejecutar métodos ``true``, **GAP** solo necesita hacer una contabilidad con sus filtros, por lo tanto, los métodos verdaderos son mucho más rápidos que los métodos inmediatos.

Operaciones y Método de Selección
---------------------------------------------

La selección de método no solo se usa para seleccionar métodos para atributos ``getters``, sino también para operaciones arbitrarias, que pueden tener más de un argumento. En este caso, hay un filtro requerido para cada argumento (que debe dar ``true`` para los argumentos correspondientes).

Además, un método con al menos dos argumentos puede requerir una cierta relación entre los argumentos, que se expresa en términos de las familias de los argumentos. Por ejemplo, los métodos para ``ConjugateGroup( grp, elm )`` requieren que elm pertenezca a la familia de elementos a partir de la cual está hecho ``grp``, es decir, que la familia de ``elm`` sea igual a la "familia de elementos" de ``grp``.

Para los grupos de permutación, la situación es bastante fácil: todas las permutaciones forman una familia, ``PermutationsFamily``, y cada colección de permutaciones, por ejemplo, cada grupo de permutación, cada clase lateral de un grupo de permutación, o cada lista densa de permutaciones, se encuentra en ``CollectionsFamily( PermutationsFamily )``.

Para otros tipos de elementos de grupo, la situación puede ser diferente. Cada llamada de FreeGroup construye una nueva familia de elementos de grupo libres. **GAP** se niega a calcular ``One( FreeGroup( 1 ) ) * One( FreeGroup( 1 ) )`` porque los dos operandos de la multiplicación se encuentran en familias diferentes y no se instala ningún método para este caso.

Para obtener más información sobre las relaciones familiares, consulte ``13.1`` en el manual de referencia.

Si desea saber qué propiedades ya se conocen para un objeto obj, o qué propiedades se sabe que son verdaderas, puede usar las funciones ``KnownPropertiesOfObject(obj)`` resp. ``KnownTruePropertiesOfObject( obj )``. Esto imprimirá una lista de nombres de propiedades. Estos nombres también son los identificadores de los captadores de propiedades, mediante los cuales puede recuperar el valor de las propiedades (y confirmar que son realmente ``true``). De manera análoga, existe la función ``KnownAttributesOfObject`` que enumera los nombres de los atributos conocidos, omitiendo las propiedades.

Dado que **GAP** le permite saber lo que ya sabe sobre un objeto, es natural que también le permita saber qué métodos considera aplicables para un determinado método y en qué orden los probará (en caso de que ocurra ``TryNextMethod()``). ``ApplicableMethod( opr, [arg1, arg2,...] )`` Devuelve el primer método aplicable para la llamada ``opr( arg1, arg2,... )``. De manera más general, ``ApplicableMethod( opr, [...], 0, nr )`` devuelve el :math:`n`-ésimo método aplicable (es decir, el que se elegiría después de ``nr - 1`` ``TryNextMethods``) y si ``nr = "all"``, la lista ordenada de todos se devuelven los métodos aplicables. Para obtener más información, consulte ``2.3`` en “Programación en GAP”.

Si desea ver qué métodos se eligen para ciertas operaciones mientras se ejecuta el código **GAP**, puede llamar a la función ``TraceMethods`` con una lista de estas operaciones como argumentos.

.. code-block:: gap
    :caption: TraceMethods
    :name: TraceMethods
    
    gap> TraceMethods( [ Size ] );
    gap> g:= Group( (1,2,3), (1,2) );; Size( g );
    #I Size: for a permutation group
    #I Setter(Size): system setter
    #I Size: system getter
    #I Size: system getter
    6

El ``getter`` del sistema se llama una vez para recuperar el valor recién calculado y devolverlo al usuario. La segunda llamada se activa mediante un método inmediato. Para averiguar por cuál, podemos rastrear los métodos inmediatos diciendo ``TraceImmediateMethods( true )``.


.. code-block:: gap
    :caption: TraceImmediateMethods( true )
    :name: TraceImmediateMethods
    
    gap> TraceImmediateMethods( true );
    gap> g:= Group( (1,2,3), (1,2) );;
    #I immediate: Size
    #I immediate: IsCyclic
    #I immediate: IsCommutative
    #I immediate: IsTrivial
    gap> Size( g );
    #I Size: for a permutation group
    #I immediate: IsNonTrivial
    #I immediate: Size
    #I immediate: IsNonTrivial
    #I immediate: GeneralizedPcgs
    #I Setter(Size): system setter
    #I Size: system getter
    #I immediate: IsPerfectGroup
    #I Size: system getter
    #I immediate: IsEmpty
    6
    gap> TraceImmediateMethods( false );
    gap> UntraceMethods( [ Size ] );

Las dos últimas líneas desactivan el rastreo de nuevo. Ahora vemos que el getter del sistema fue llamado por el método inmediato para ``IsPerfectGroup``. Además, el método inmediato mencionado anteriormente para ``IsSolvableGroup`` no se utilizó porque la solubilidad de :math:`g` ya se descubrió durante el cálculo del tamaño (véase el ejemplo de la ``Sección 8.2``).

Resumen
~~~~~~~~~~~~~~~~~~~~~~~~

En esta sección y en la última hemos mirado un poco más detrás de escena y hemos visto que **GAP** ejecuta automáticamente métodos inmediatos y verdaderos para deducir información sobre objetos que está disponible a bajo costo. Hemos visto cómo esto se puede supervisar rastreando los métodos.