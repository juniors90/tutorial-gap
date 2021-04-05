.. role:: underline
    :class: underline

Grupos y Homomorfismos
=======================

En este capítulo mostraremos algunos cálculos con grupos. Los ejemplos tratan principalmente con grupos de permutación, porque son los más fáciles de ingresar. Las funciones mencionadas aquí, como
    
    - ``Group``,
    - ``Size``,
    - ``SylowSubgroup``,
    - ``IsPerfect``,
    - ``IsAbelian``, etc.

Sin embargo, son las mismas para todo tipo de grupos, aunque los algoritmos que computan la información serán diferentes en la mayoría de los casos.

Grupo de Permutaciones
----------------------------

Los *grupos de permutación* son muy fáciles de ingresar porque sus elementos, es decir, las permutaciones, son muy fáciles de escribir: :underline:`se ingresan y se muestran en notación de ciclo disjunto`. Así que construyamos un grupo de permutación:

.. code-block:: gap
    :caption: grupo simétrico :math:`\mathbb{S}_{8}`
    :name: grupo_simetrico_en_ocho_puntos*
        
    gap> s8 := Group( (1,2), (1,2,3,4,5,6,7,8) );
    Group([ (1,2), (1,2,3,4,5,6,7,8) ])

Formamos el grupo generado por las permutaciones ``(1,2)`` y ``(1,2,3,4,5,6,7,8)``, que es bien conocido por ser el *grupo simétrico en ocho puntos* :math:`\mathbb{S}_{8}`, y lo asignamos al identificador ``s8``. Ahora el grupo :math:`\mathbb{S}_{8}` contiene el grupo alterno en ocho puntos que pueden describirse de varias formas, por ejemplo, como el grupo de todas las permutaciones pares en ``s8``, o como su subgrupo derivado.

.. code-block:: gap
    :caption: subgrupo derivado, Size, etc  
    :name: subgrupo_derivado
        
    gap> a8 := DerivedSubgroup( s8 );
    Group([ (1,2,3), (2,3,4), (2,4)(3,5), (2,6,4), (2,4)(5,7), (2,8,6,4)(3,5) ])
    gap> Size( a8 ); IsAbelian( a8 ); IsPerfect( a8 );
    20160
    false
    true

Una vez que se ha calculado la información sobre un grupo como ``s8`` o ``a8``, se almacena en el grupo de modo que simplemente se pueda buscar cuando se necesite nuevamente. Esto es válido para todos los datos del ejemplo anterior.

Es decir, ``a8`` almacena su orden y que no es abeliano y perfecto, y ``s8`` almacena su subgrupo derivado ``a8``. Sin embargo, si hubiéramos calculado ``a8`` como ``CommutatorSubgroup(s8, s8)``, no se habría almacenado, porque entonces se habría calculado como una función de dos argumentos y, por lo tanto, no se podría atribuir solo a uno de ellos. (Por supuesto, la función ``CommutatorSubgroup`` puede calcular el subgrupo del conmutador de dos subgrupos arbitrarios). La situación es un poco diferente para los :math:`p`-subgrupos de *Sylow*: la función ``SylowSubgroup`` también requiere dos argumentos, a saber, un grupo y un ``p`` primo, pero el resultado se almacena en el grupo, es decir, junto con el primer ``p`` en una lista llamada ``ComputedSylowSubgroups``, pero no nos detendremos en los detalles aquí.

.. code-block:: gap
    :caption: subgrupos de Sylow, Normalizador, etc 
    :name: subgrupos_de_Sylow
    
    gap> syl2 := SylowSubgroup( a8, 2 );; Size( syl2 );
    64
    gap> Normalizer( a8, syl2 ) = syl2;
    true
    gap> cent := Centralizer( a8, Centre( syl2 ) );; Size( cent );
    192
    gap> DerivedSeries( cent );; List( last, Size );
    [ 192, 96, 32, 2, 1 ]

Hemos escrito doble punto y coma después de algunos comandos para evitar la salida de los grupos (que serían impresos por sus listas generadoras). Sin embargo, se recomienda al principiante que escriba un punto y coma y estudie el resultado completo. Esta observación también se aplica al resto de este tutorial.

Con los siguientes ejemplos, queremos calcular:

    - un subgrupo de ``a8``, 
    - su normalizador y
    - finalmente determinar la estructura de la extensión.
    
Comenzamos formando un subgrupo generado por tres involuciones de conmutación, es decir, :underline:`un subgrupo isomorfo al grupo aditivo del espacio vectorial` :math:`2^{3}`.

.. code-block:: gap
    :caption: Subgrupo generado por tres involuciones de conmutación` 
    :name: subgrupo_generado_por_tres_involuciones_de_conmutación
    
    gap> elab := Group( (1,2)(3,4)(5,6)(7,8), (1,3)(2,4)(5,7)(6,8),
    > (1,5)(2,6)(3,7)(4,8) );;
    gap> Size( elab );
    8
    gap> IsElementaryAbelian( elab );
    true

Como es habitual, **GAP** imprime el grupo dándole todos sus generadores. Esto puede resultar molesto, especialmente si hay muchos de ellos o si son de gran magnitud. También dificulta el reconocimiento de un grupo en particular cuando ya hay varios alrededor. Tengamos en cuenta que aunque no es un problema para nosotros especificar un grupo en particular para **GAP**, al usar identificadores bien elegidos como ``a8`` y ``elab``, es imposible que **GAP** use estos identificadores cuando imprime un grupo para nosotros, debido a que el grupo no sabe qué identificador(es) lo señalan, de hecho puede haber varios. Para dar un nombre al grupo en sí (en lugar del identificador), debe usar la función ``SetName``. Hacemos esto con el nombre ``2^3`` aquí que refleja las propiedades matemáticas del grupo. A partir de ahora, **GAP** usará este nombre cuando imprima el grupo para nosotros, pero aún no podemos usar este nombre para especificar el grupo a **GAP**, porque el nombre no sabe a qué grupo fue asignado (después de todo, podría asignar el mismo nombre a varios grupos). Cuando hable con la computadora, siempre debe usar identificadores.

.. code-block:: gap
    :caption: función SetName
    :name: funcion_SetName
    
    gap> SetName( elab, "2^3" ); elab;
    2^3
    gap> norm := Normalizer( a8, elab );; Size( norm );
    1344

Ahora que tenemos la norma de subgrupo de orden :math:`1344` y su subgrupo ``elab``, queremos ver su grupo de factores. Pero dado que también queremos encontrar preimágenes de elementos de grupos de factores en la norma, realmente queremos mirar el **homomorfismo natural** definido en la norma con kernel ``elab`` y cuya imagen es el grupo de factores.

.. code-block:: gap
    :caption: funcion NaturalHomomorphismByNormalSubgroup
    :name: funcion_NaturalHomomorphismByNormalSubgroup
    
    gap> hom := NaturalHomomorphismByNormalSubgroup( norm, elab );
    <action epimorphism>
    gap> f := Image( hom );
    Group([ (), (), (), (4,5)(6,7), (4,6)(5,7), (2,3)(6,7), (2,4)(3,5),
    (1,2)(5,6) ])
    gap> Size( f );
    168

El grupo de factores se representa nuevamente como un grupo de permutación. Sin embargo, el dominio de acción de este grupo de factores no tiene nada que ver con el dominio de acción de la norma ``norm``. (Solo sucede que ambos son subconjuntos de los números naturales). Ahora podemos formar imágenes y preimágenes bajo el homomorfismo natural. El conjunto de preimágenes de un elemento bajo ``hom`` es una clase lateral modulo ``elab`` **(corregir la redacción)**.

.. important::

    Usamos la función ``PreImages`` aquí porque ``hom`` :underline:`no es una biyección`, por lo que un elemento del rango puede tener varias preimágenes o ninguna.

    - La pregunta sería *¿Qué función podríamos usar cuando tengamos una función biyectiva?*.

.. code-block:: gap
    :caption: funcion Kernel
    :name: funcion_Kernel
    
    gap> ker:= Kernel( hom );
    2^3
    gap> x := (1,8,3,5,7,6,2);; Image( hom, x );
    (1,7,5,6,2,3,4)
    gap> coset := PreImages( hom, last );
    RightCoset(2^3,(2,8,6,7,3,4,5))

Tengamos en cuenta que **GAP** es libre de elegir cualquier representante para la clase lateral de las preimágenes. Por supuesto, el cociente de dos representantes se encuentra en el núcleo del homomorfismo.

.. code-block:: gap
    :caption: funcion Representative
    :name: funcion_Representative
    
    gap> rep:= Representative( coset );
    (2,8,6,7,3,4,5)
    gap> x * rep^-1 in ker;
    true

El grupo de factores ``f`` es un grupo simple, es decir, no tiene subgrupos normales no triviales. **GAP** puede detectar este hecho, y luego también puede encontrar el nombre por el cual este simple grupo es conocido entre los teóricos de grupos. (Por supuesto, estos nombres no están disponibles para grupos que no sean simples).

.. code-block:: gap
    :caption: funcion IsSimple, IsomorphismTypeInfoFiniteSimpleGroup, etc
    :name: funcion_IsSimple
    
    gap> IsSimple( f ); IsomorphismTypeInfoFiniteSimpleGroup( f );
    true
    rec( series := "L", parameter := [ 2, 7 ],
    name := "A(1,7) = L(2,7) ~ B(1,7) = O(3,7) ~ C(1,7) = S(2,7) ~ 2A(1,7) = U(2\
    ,7) ~ A(2,2) = L(3,2)" )
    gap> SetName( f, "L_3(2)" );

Damos a ``f`` el nombre ``L_3(2)`` porque la última parte de la cadena de nombres revela que es isomorfa al grupo lineal simple :math:`L3 (2)`. Este grupo, sin embargo, también tiene muchos otros nombres. Los nombres que están conectados con un signo ``=`` son nombres diferentes para el mismo grupo de matrices, por ejemplo, ``A(2,2)`` es la notación de tipo Lie para la notación clásica ``L(3,2)``. Otros pares de nombres están conectados a través de ``~``, estos luego especifican otros grupos clásicos que son isomorfos a ese grupo lineal (por ejemplo, el grupo simpléctico ``S(2,7)``, cuya notación tipo Lie sería ``C(1,7)``).

La norma de grupo ``norm`` actúa sobre los ocho elementos de su subgrupo normal ``elab`` por conjugación, produciendo una representación de :math:`L_{3}(2)` en ``s8`` que deja un punto fijo (llememosle el punto :math:`1`). La imagen de esta representación se puede calcular con la función ``Action``; incluso está contenido en la norma de grupo ``norm`` y podemos mostrar que la norma ``norm`` es de hecho una extensión dividida del grupo abeliano elemental :math:`2^{3}` con esta imagen de :math:`L_{3}(2)`.

.. code-block:: gap
    :caption: funcion Action, IsSubgroup, etc
    :name: funcion_Action

    gap> op := Action( norm, elab );
    Group([ (), (), (), (5,6)(7,8), (5,7)(6,8), (3,4)(7,8), (3,5)(4,6),
    (2,3)(6,7) ])
    gap> IsSubgroup( a8, op ); IsSubgroup( norm, op );
    true
    true
    gap> IsTrivial( Intersection( elab, op ) );
    true
    gap> SetName( norm, "2^3:L_3(2)" );

.. warning::

    No deberíamos probar el operador ``<`` en lugar de la función ``IsSubgroup``. Algo como

    .. code-block:: gap
        :caption: Notas sobre la funcion IsSubgroup
        :name: funcion_IsSubgroup
    
        gap> elab < a8;
        false
        
    no causará un error, pero el resultado no significa nada sobre la inclusión de un grupo en otro; ``<`` prueba cuál de los dos grupos es menor en algún orden total. Por otro lado, el operador de igualdad ``=`` de hecho prueba la igualdad de sus argumentos.

Resumen
~~~~~~~~~~

En esta sección hemos hecho dos cosas relevantes:

    1. Utilizar las funciones de grupo elementales para determinar la estructura de un normalizador,
    2. Asignar nombres a los grupos involucrados, tales que :underline:`reflejan su estructura matemática` y **GAP** usa estos nombres al imprimir los grupos.


Acciones de Grupos
----------------------------

Para obtener otra representación de ``a8``, consideramos otra acción, es decir, la de los elementos de una determinada clase de conjugación por conjugación. **(preguntar)**

En el siguiente ejemplo, aumentamos temporalmente el límite de longitud de línea de su valor predeterminado ``80`` a ``82`` para que la expresión larga se ajuste a una línea.

.. code-block:: gap
    :caption: función ConjugacyClasses
    :name: funcion_ConjugacyClasses
    
    gap> ccl := ConjugacyClasses( a8 );; Length( ccl );
    14
    gap> List( ccl, c -> Order( Representative( c ) ) );
    [ 1, 2, 2, 3, 6, 3, 4, 4, 5, 15, 15, 6, 7, 7 ]
    gap> SizeScreen([ 82, ]);;
    gap> List( ccl, Size );
    [ 1, 210, 105, 112, 1680, 1120, 2520, 1260, 1344, 1344, 1344, 3360, 2880, 2880 ]
    gap> SizeScreen([ 80, ]);;

Note la diferencia entre ``Order`` (que significa el orden de los elementos), ``Size`` (que significa el tamaño de la clase de conjugación) y ``Length`` (que significa la longitud de una lista). Elegimos dejar que ``a8`` opere en la clase de longitud ``112``.

.. code-block:: gap
    :caption: función Size
    :name: funcion_Size

    gap> class := First( ccl, c -> Size(c) = 112 );;
    gap> op := Action( a8, AsList( class ) );;


Usamos ``AsList`` aquí para convertir la clase de conjugación en una lista de sus elementos, mientras que escribimos ``Action (norm, elab)`` directamente en la sección anterior. La razón es que el grupo abeliano elemental ``elab`` puede enumerarse rápidamente mediante **GAP**, mientras que el método de enumeración estándar para clases de conjugación es más lento que el simple cálculo explícito de los elementos. Sin embargo, **GAP** es reacio a construir listas de elementos explícitos, porque para grupos realmente grandes este método directo no es factible.

Tenga en cuenta también la función ``First``, que se utiliza para encontrar el primer elemento en una lista que pasa alguna prueba. Consulte ``21.20.20`` en el manual de referencia para obtener más detalles.

Ahora tenemos una representación de permutación ``op`` en ``112`` puntos, cuya primitividad probamos. Si no es primitivo, podemos obtener un sistema de bloques mínimo (es decir, uno donde los bloques tienen una longitud mínima) mediante la función``Blocks``.

.. code-block:: gap
    :caption: función Blocks
    :name: funcion_Blocks
    
    gap> IsPrimitive( op, [ 1 .. 112 ] );
    false
    gap> blocks := Blocks( op, [ 1 .. 112 ] );;

Tenga en cuenta que debemos especificar el dominio de la acción. Podría pensar que las funciones ``IsPrimitive`` y ``Blocks`` podrían usar ``[1..112]`` como dominio predeterminado si no se proporciona ningún dominio. Pero esto no es tan fácil, por ejemplo, ¿el dominio predeterminado del Grupo ``((2,3,4))`` sería ``[1..4]`` o ``[2..4]``? Para evitar confusiones, todas las funciones de acción requieren que especifique el dominio de acción. Si hubiéramos especificado ``[1..113]`` en la prueba de primitividad anterior, el punto ``113`` habría sido un punto fijo (y la acción ni siquiera habría sido transitiva).

Ahora ``blocks`` es una lista de bloques (es decir, una lista de listas), que no imprimimos aquí para ahorrar papel (pruébelo usted mismo). De hecho, todo lo que queremos saber es el tamaño de los bloques, o más bien cuántos hay (el producto de estos dos números, por supuesto, debe ser :math:`112`). Entonces podemos obtener un nuevo grupo de permutación del grado correspondiente dejando que ``op`` actúe sobre estos bloques de forma secuencial.

.. code-block:: gap
    :caption: función Action sobre Blocks
    :name: funcion_Action_sobre_Blocks
    
    gap> Length( blocks[1] ); Length( blocks );
    2
    56
    gap> op2 := Action( op, blocks, OnSets );;
    gap> IsPrimitive( op2, [ 1 .. 56 ] );
    true

Tengamos en cuenta que damos un tercer argumento (la función de acción ``OnSets``) para indicar que la acción no es la acción predeterminada en puntos, sino una acción en conjuntos de elementos dados como listas ordenadas. (La ``Sección 39.2`` del manual de referencia enumera todas las acciones predefinidas por **GAP**).

La acción de ``op`` en el sistema de bloques dado nos dio una nueva representación en :math:`56` puntos que es primitiva, es decir, el estabilizador de puntos es un subgrupo máximo. Calculamos su preimagen en la representación en ocho puntos usando los homomorfismos de acción asociados (que por supuesto son monomorfismos). Construimos la composición de dos homomorfismos con el operador ``*``,  leyendo de izquierda a derecha.

.. code-block:: gap
    :caption: función ActionHomomorphism
    :name: funcion_ActionHomomorphism
    
    gap> ophom := ActionHomomorphism( a8, op );;
    gap> ophom2 := ActionHomomorphism( op, op2 );;
    gap> composition := ophom * ophom2;;
    gap> stab := Stabilizer( op2, 2 );;
    gap> preim := PreImages( composition, stab );
    Group([ (1,4,2), (3,6,7), (3,8,5,7,6), (1,4)(7,8) ])

El normalizador de un elemento en la clase de clase de conjugación también es un grupo de orden :math:`360`. De hecho, es un conjugado del subgrupo máximo que habíamos encontrado antes, y un elemento de conjugación en ``a8`` se encuentra mediante la función ``RepresentativeAction``.

.. code-block:: gap
    :caption: función RepresentativeAction
    :name: funcion_RepresentativeAction
    
    gap> sgp := Normalizer( a8, Subgroup(a8,[Representative(class)]) );;
    gap> Size( sgp );
    360
    gap> RepresentativeAction( a8, sgp, preim );
    (3,4)(7,8)

Hasta ahora hemos visto algunas aplicaciones de las funciones ``Action`` y ``ActionHomomorphism``. Pero quizás aún más interesante es el hecho de que el homomorfismo natural hom construido anteriormente es también un homomorfismo de acción; esta es también la razón por la que su imagen se representa como un grupo de permutación: es :underline:`la representación natural de las acciones`. Ahora veremos este homomorfismo de acción nuevamente para averiguar sobre qué objetos opera. Estos objetos forman el llamado conjunto externo que se asocia con cada homomorfismo de acción. Mencionaremos los conjuntos externos solo de manera superficial en este tutorial; para obtener más detalles, consulte ``39.11`` en el manual de referencia. Por el momento, solo necesitamos saber que el conjunto externo se obtiene mediante la función ``UnderlyingExternalSet``.

.. code-block:: gap
    :caption: función UnderlyingExternalSet
    :name: funcion_UnderlyingExternalSet
    
    gap> t := UnderlyingExternalSet( hom );
    <xset:RightTransversal(2^3:L_3(2),Group(
        [ (1,5)(2,6)(3,7)(4,8), (1,3)(2,4)(5,7)(6,8), (1,2)(3,4)(5,6)(7,8),
        (5,6)(7,8), (5,7)(6,8), (3,4)(7,8), (3,5)(4,6) ]))>

Para el homomorfismo natural ``hom``, el conjunto externo es una **transversal derecha de un subgrupo** :math:`U` en ``norm``, y ​​la acción de la transversal derecha realmente significa acción en las clases laterales del subgrupo :math:`U`. Al ejecutar la función llamada ``NaturalHomomorphismByNormalSubgroup( norm, elab )``, **GAP** tiene la elección de un subgrupo :math:`U` para el cual el núcleo de esta acción (es decir, el núcleo de :math:`U` en ``norm``) es el subgrupo normal deseado ``elab``. Para el propósito de operar en las clases laterales, la transversal derecha ``t`` contiene un representante de cada clase lateral de :math:`U`. Visto de esta manera, una transversal es simplemente una lista de elementos de grupo, y usted puede hacer que **GAP** produzca esta lista por ``AsList( t )``. (Intentalo.)

La imagen de tal representante de ``AsList(t)`` bajo la multiplicación de la derecha con un elemento de la norma no estará en general en ``AsList(t)``, porque no volverá a estar entre los representantes elegidos. Por tanto, la multiplicación a la derecha no es una acción en ``AsList(t)``. Sin embargo, **GAP** usa un truco especial que se discutirá a continuación para hacer de esta una acción bien definida en las clases laterales representadas por los elementos de ``AsList(t)``. Por ahora, es importante saber que el conjunto externo ``t`` es más que la transversal derecha sobre la que opera la norma del grupo. En total son necesarias tres cosas para especificar una acción: un grupo :math:`G`, un conjunto :math:`D` y una función :math:`opr: D\times G\to D`. Podemos acceder a estos ingredientes con las siguientes funciones:

.. code-block:: gap
    :caption: función ActingDomain
    :name: funcion_ActingDomain

    gap> ActingDomain(t); # the group
    2^3:L_3(2)
    gap> Enumerator(t);
    RightTransversal(2^3:L_3(2),Group(
        [ (1,5)(2,6)(3,7)(4,8), (1,3)(2,4)(5,7)(6,8), (1,2)(3,4)(5,6)(7,8),
        (5,6)(7,8), (5,7)(6,8), (3,4)(7,8), (3,5)(4,6) ]))
    gap> FunctionAction(t);
    function( pnt, elm ) ... end
    gap> NameFunction( last );
    "OnRight"


La función que se llama "``OnRight``" también se asigna al identificador ``OnRight``, y significa multiplicación desde la derecha; esta es la forma habitual de operar en una transversal derecha. ``OnRight( d, g )`` se define como :math:`d * g`.

Observe que el conjunto externo ``t`` y su enumerador se imprimen de la misma manera, pero tenga en cuenta que un conjunto externo también comprende el dominio de actuación y la función de acción. El propio ``Enumerator``, es decir, la transversal derecha, a su vez comprende el conocimiento sobre la norma de grupo y el subgrupo :math:`U` y esto es lo que permite el truco especial prometido anteriormente. En cuanto a la función  ``Position``, el objeto ``Enumerator`` se comporta como una lista (inmutable) y puedes preguntar por la posición de un elemento en ella.

.. code-block:: gap
    :caption: función Position
    :name: funcion_Position
    
    gap> elm := (1,4)(2,7)(3,6)(5,8);;
    gap> Position( Enumerator(t), elm );
    fail
    gap> PositionCanonical( Enumerator(t), elm );
    5

El resultado fallido significa que el elemento no se encontró en absoluto en la lista: no está entre los representantes elegidos. La diferencia entre las funciones ``Position`` y ``PositionCanonical`` es que:

    - ``Position`` la primera simplemente mira si olmo está contenido entre los representantes que juntos forman la transversal derecha ``t``,
    - ``PositionCanonical`` realmente busca la posición de la clase lateral descrita por el olmo representativo.
    
En otras palabras, primero reemplaza ``elm`` por un representante canónico de la misma clase lateral (que debe estar contenida en ``Enumerator( t )``) y luego busca su posición, de ahí el nombre. La función ``ActionHomomorphism`` (y sus parientes) siempre usan ``PositionCanonical`` cuando calculan las imágenes de los generadores del grupo fuente (aquí, norma) bajo el homomorfismo (aquí, hom). Por lo tanto, pueden dar una acción bien definida en un enumerador, incluso si la acción no estaría bien definida en ``AsList( enumerator )``.

La imagen del homomorfismo natural es el grupo de permutación ``f`` que resulta de la acción de la norma sobre la transversal derecha. Puede calcularse mediante cualquiera de los siguientes comandos. El segundo de ellos muestra que el conjunto externo ``t`` contiene toda la información que es necesaria para que ``Action`` haga su trabajo.

.. code-block:: gap
    :caption: función Action sobre t
    :name: funcion_Action_sobre_t

    gap> Action( norm, Enumerator(t), OnRight ) = f;
    true
    gap> Action( t ) = f;
    true


Hemos especificado la función de acción ``OnRight`` en este ejemplo, pero hemos visto ejemplos como ``Action( norm, elab)`` anteriormente donde no se dio este tercer argumento. Si se omite una función de acción, **GAP** siempre asume ``OnPoints`` que se define como ``OnPoints( d, g ) = d ^ g``. Este operador de "signo de intercalación" denota la conjugación en un grupo si ambos argumentos :math:`d` y :math:`g` son elementos de grupo (contenidos en un grupo común), pero también denota la acción natural de las permutaciones en enteros positivos (y exponenciación de enteros también, por supuesto) .

Resumen
~~~~~~~~~~~~

En esta sección hemos aprendido cómo los grupos pueden operar en objetos **GAP** como números enteros y elementos de grupo. Hemos utilizado ``ActionHomomorphism``, entre otros, para construir un homomorfismo natural, en cuyo caso el grupo operó en la transversal derecha de un subgrupo adecuado. Esta transversal derecha nos dio un ejemplo para el uso de ``PositionCanonical``, que nos permitió especificar clases laterales dando representantes.

Subgrupos como Estabilizadores
--------------------------------

Las funciones de acción también se pueden utilizar sin construir conjuntos externos. Intentaremos encontrar varios subgrupos en ``a8`` como estabilizadores de tales acciones. Un subgrupo está disponible de inmediato, a saber, el estabilizador de un punto. Por supuesto, el índice del estabilizador debe ser igual a :underline:`la longitud de la órbita`, es decir, :math:`8`.

.. code-block:: gap
    :caption: función Stabilizer
    :name: funcion_Stabilizer
    
    gap> u8 := Stabilizer( a8, 1 );
    Group([ (2,3,4), (2,4)(3,5), (2,6,4), (2,4)(5,7), (2,8,6,4)(3,5) ])
    gap> Index( a8, u8 );
    8
    gap> Orbit( a8, 1 ); Length( last );
    [ 1, 3, 2, 4, 5, 6, 7, 8 ]
    8

Esto nos da una pista de cómo encontrar más subgrupos. Cada subgrupo es el estabilizador de un punto de una acción transitiva apropiada (es decir, la acción sobre las clases laterales de ese subgrupo u otra acción que sea equivalente a esta acción). Entonces la pregunta es *¿cómo encontrar otras acciones?*. Lo obvio es operar sobre pares de puntos.

Entonces, usando la función ``Tuples``, primero generamos una lista de todos los pares.

.. code-block:: gap
    :caption: función Tuples
    :name: funcion_Tuples

    gap> pairs := Tuples( [1..8], 2 );;

Ahora nos gustaría que a8 operara en este dominio. Pero no podemos usar la acción predeterminada ``OnPoints`` porque ``list ^ perm`` no está definida. Por lo tanto, debemos decirle a las funciones del paquete de acciones cómo operan los elementos del grupo en los elementos del dominio. En nuestro ejemplo, podemos hacer esto simplemente pasando ``OnPairs`` como último argumento opcional. Todas las funciones del paquete de acciones aceptan un argumento opcional que describe la acción. Un ejemplo es ``IsTransitive``.

.. code-block:: gap
    :caption: función IsTransitive
    :name: funcion_IsTransitive

    gap> IsTransitive( a8, pairs, OnPairs );
    false

La acción, por supuesto, no es transitiva, ya que los pares ``[1, 1]`` y ``[1, 2]`` no pueden encontrarse en la misma órbita. Así que queremos averiguar cuáles son las órbitas. La función ``Orbits`` lo hace por nosotros. Devuelve una lista de todas las órbitas. Observamos las longitudes de las órbitas y los representantes de las órbitas.

.. code-block:: gap
    :caption: función Orbits
    :name: funcion_Orbits

    gap> orbs := Orbits( a8, pairs, OnPairs );; Length( orbs );
    2
    gap> List( orbs, Length );
    [ 8, 56 ]
    gap> List( orbs, o -> o[1] );
    [ [ 1, 1 ], [ 1, 2 ] ]

La acción de ``a8`` en la primera órbita (esta es la que contiene ``[1,1]``, probar ``[1,1]`` en ``orbs[1]``) es por supuesto equivalente a la acción original, así que la ignoramos y trabajamos con la segunda orbita.

.. code-block:: gap
    :caption: función Stabilizer
    :name: funcion_Stabilizer.
    
    gap> u56 := Stabilizer( a8, orbs[2][1], OnPairs );; Index( a8, u56 );
    56

Entonces ahora hemos encontrado un segundo subgrupo. Para hacer los siguientes cálculos un poco más fáciles y eficientes, ahora nos gustaría trabajar en los puntos ``[1..56]`` en lugar de la lista de pares. La función ``ActionHomomorphism`` hace lo que necesitamos. Crea un homomorfismo definido en ``a8`` cuya imagen es un nuevo grupo que opera en ``[1..56]`` de la misma forma que ``a8`` opera en la segunda órbita.

.. code-block:: gap
    :caption: función ActionHomomorphism
    :name: funcion_ActionHomomorphism.

    gap> h56 := ActionHomomorphism( a8, orbs[2], OnPairs );;
    gap> a8_56 := Image( h56 );;

Ahora nos gustaría saber si el subgrupo ``u56`` del índice :math:`56` que encontramos es máximo o no. Como ya hemos usado en la ``Sección 5.2``, un subgrupo es máximo si y solo si la acción sobre las clases laterales de este subgrupo es primitiva.

.. code-block:: gap
    :caption: función IsPrimitiv
    :name: funcion_IsPrimitiv
    
    gap> IsPrimitive( a8_56, [1..56] );
    false

Recuerde que podemos omitir la función si nos referimos a ``OnPoints`` pero que tenemos que especificar el dominio de acción para todas las funciones de acción. Vemos que ``a8_56`` no es primitivo. Esto significa, por supuesto, que la acción de ``a8`` sobre ``orb[2]`` no es primitiva, porque esas dos acciones son equivalentes. Entonces el estabilizador ``u56`` no es máximo. Intentemos encontrar sus supergrupos. Usamos la función ``Blocks`` para encontrar un sistema de bloques. El tercer argumento (opcional) en el siguiente ejemplo le dice a ``Blocks`` que queremos un sistema de bloques donde :math:`1` y :math:`14` se encuentran en un bloque.

.. code-block:: gap
    :caption: función Blocks
    :name: funcion_Blocks.
    
    gap> blocks := Blocks( a8_56, [1..56], [1,14] );
    [ [ 1, 3, 4, 5, 6, 14, 31 ], [ 2, 13, 15, 16, 17, 23, 24 ],
      [ 7, 8, 22, 34, 37, 47, 49 ], [ 9, 11, 18, 20, 35, 38, 48 ],
      [ 10, 25, 26, 27, 32, 39, 50 ], [ 12, 28, 29, 30, 33, 36, 40 ],
      [ 19, 21, 42, 43, 45, 46, 55 ], [ 41, 44, 51, 52, 53, 54, 56 ] ]

El resultado es una lista de conjuntos, de modo que ``a8_56`` opera en esos conjuntos. Ahora nos gustaría el estabilizador de esta acción en los sets. Debido a que queremos operar en los conjuntos, tenemos que pasar ``OnSets`` como tercer argumento.

.. code-block:: gap
    :caption: función Stabilizer aplicada a esta acción en los sets
    :name: funcion_Stabilizer_aplicada
    
    gap> u8_56 := Stabilizer( a8_56, blocks[1], OnSets );;
    gap> Index( a8_56, u8_56 );
    8
    gap> u8b := PreImages( h56, u8_56 );; Index( a8, u8b );
    8
    gap> IsConjugate( a8, u8, u8b );
    true

Entonces, hemos encontrado un supergrupo de ``u56`` que se conjuga en ``a8`` con ``u8``. Esto no es sorprendente, ya que ``u8`` es un estabilizador de puntos y ``u56`` es un estabilizador de dos puntos en la acción natural de a8 en ocho puntos.

.. warning::
    
    Si especifica ``OnSets`` como tercer argumento para una función como ``Stabilizer``, :underline:`debe asegurarse de que el punto (es decir, el segundo argumento) sea de hecho un conjunto`. De lo contrario, obtendrá un mensaje de error desconcertante o incluso resultados incorrectos. En el ejemplo anterior, el segundo argumento ``blocks[1]`` provino de la función ``Blocks``, que devuelve una lista de conjuntos, por lo que todo estaba bien.
    
Actualmente existe un tercer sistema de bloques de ``a8_56`` que da lugar a un tercer subgrupo.

.. code-block:: gap
    :caption: Tercer sistema de bloques de ``a8_56``
    :name: tercer_sistema_de_bloques_de_a8_56

    gap> blocks := Blocks( a8_56, [1..56], [1,13] );;
    gap> u28_56 := Stabilizer( a8_56, [1,13], OnSets );;
    gap> u28 := PreImages( h56, u28_56 );;
    gap> Index( a8, u28 );
    28

Sabemos que el subgrupo ``u28`` del índice :math:`28` es máximo, porque sabemos que a8 no tiene subgrupos del índice :math:`2`, :math:`4` o :math:`7`. Sin embargo, también podemos verificar esto rápidamente comprobando que ``a8_56`` opera primitivamente en los :math:`28` bloques.

.. code-block:: gap
    :caption: comprobando el bloques máximo
    :name: comprobando_el_bloques_maximo

    gap> IsPrimitive( a8_56, blocks, OnSets );
    true

El estabilizador no solo es aplicable a grupos como ``a8`` sino también a sus subgrupos como ``u56``. Entonces, otro método para encontrar un nuevo subgrupo es calcular el estabilizador de otro punto en ``u56``. Tenga en cuenta que ``u56`` ya deja :math:`1` y :math:`2` fijos.

.. code-block:: gap
    :caption: función Stabilizer aplicada a ``u56``
    :name: funcion_Stabilizer_a_el_subgrupo
    
    gap> u336 := Stabilizer( u56, 3 );;
    gap> Index( a8, u336 );
    336

Otras funciones también son aplicables a subgrupos. A continuación mostramos que ``u336`` opera regularmente en los :math:`60` triples (o ternas) de ``[4..8]`` que no contienen ningún elemento dos veces. Construimos la lista de estos :math:`60` triples con la función ``Orbit`` (usando ``OnTuples`` como la generalización natural de ``OnPairs``) y luego la pasamos como dominio de acción a la función ``IsRegular``. El resultado positivo de la prueba de regularidad significa que esta acción es equivalente a las acciones de ``u336`` en sus :math:`60` elementos por la derecha.

.. code-block:: gap
    :caption: función IsRegular aplicada a ``u336``
    :name: funcion_IsRegular_aplicada
    
    gap> IsRegular( u336, Orbit( u336, [4,5,6], OnTuples ), OnTuples );
    true

Al igual que hicimos en el caso de la acción sobre los pares anteriores, ahora construimos un nuevo grupo de permutación que opera en ``[1..336]`` de la misma manera que ``a8`` opera en las clases laterales de ``u336``. Pero esta vez dejamos que ``a8`` opere en una transversal derecha, tal como lo hizo la norma en el homomorfismo natural anterior.

.. code-block:: gap
    :caption: función RightTransversal
    :name: funcion_RightTransversal
    
    gap> t := RightTransversal( a8, u336 );;
    gap> a8_336 := Action( a8, t, OnRight );;

Para encontrar subgrupos por encima de ``u336``, nuevamente buscamos sistemas de bloques no triviales.

.. code-block:: gap
    :caption: sistemas de bloques no triviales
    :name: sistemas_de_bloques_no_triviales

    gap> blocks := Blocks( a8_336, [1..336] );; blocks[1];
    [ 1, 43, 85 ]

Vemos que la unión de ``u336`` con su ``43ª`` y ``85ª`` clase lateral es un subgrupo en ``a8_336``, su índice es :math:`112`. Podemos obtenerlo como el cierre de ``u336`` con un representante de la ``43ª`` clase lateral, que se puede encontrar como :math:`43` elemento de la transversal ``t``. Nótese que en la representación ``a8_336`` sobre :math:`336` puntos, este subgrupo corresponde al estabilizador del bloque ``[1, 43, 85]``.

.. code-block:: gap
    :caption: función ClosureGroup
    :name: funcion_ClosureGroup

    gap> u112 := ClosureGroup( u336, t[43] );;
    gap> Index( a8, u112 );
    112

Por encima de este subgrupo del índice :math:`112` se encuentra un subgrupo del índice :math:`56`, que no está conjugado con ``u56``. De hecho, a diferencia de ``u56``, es maximal. Obtenemos este subgrupo de la misma manera que obtuvimos ``u112``, esta vez forzando dos puntos, es decir, :math:`7` y :math:`43` en el primer bloque.

.. code-block:: gap
    :caption: forzando los puntos :math:`7` y :math:`43`
    :name: forzando_los_puntos_7_y_43
    
    gap> blocks := Blocks( a8_336, [1..336], [1,7,43] );;
    gap> Length( blocks );
    56
    gap> u56b := ClosureGroup( u112, t[7] );; Index( a8, u56b );
    56
    gap> IsPrimitive( a8_336, blocks, OnSets );
    true

Ya mencionamos en la ``Sección 5.2`` que existe otra acción estándar de las permutaciones, a saber, la conjugación. Por ejemplo, dado que no se especifica ninguna otra acción en el siguiente ejemplo, ``OrbitLength`` simplemente opera a través de ``OnPoints``, y debido a que ``perm 1 ^ perm 2`` se define como la conjugación de ``perm2`` en ``perm1``, de hecho calculamos la longitud de la clase de conjugación de ``(1, 2)(3,4)(5,6)(7,8)``.

.. code-block:: gap
    :caption: función OrbitLength
    :name: funcion_OrbitLength
    
    gap> OrbitLength( a8, (1,2)(3,4)(5,6)(7,8) );
    105
    gap> orb := Orbit( a8, (1,2)(3,4)(5,6)(7,8) );;
    gap> u105 := Stabilizer( a8, (1,2)(3,4)(5,6)(7,8) );; Index( a8, u105 );
    105

.. note::

    Tengamos en cuenta que aunque la longitud de una clase de conjugación de cualquier elemento ``elm`` en cualquier *grupo finito* :math:`G` se puede calcular como ``OrbitLength( G, elm )``, el comando ``Size( ConjugacyClass( G, elm ) )`` es probablemente más eficiente.

.. code-block:: gap
    :caption: función ConjugacyClass
    :name: funcion_ConjugacyClass
    
    gap> Size( ConjugacyClass( a8, (1,2)(3,4)(5,6)(7,8) ) );
    105

Por supuesto, el estabilizador ``u105`` es de hecho el centralizador del elemento ``(1,2)(3,4)(5,6)(7,8)``. El estabilizador se da cuenta de eso y calcula el estabilizador utilizando el algoritmo del centralizador para los grupos de permutación. De la forma habitual, ahora buscamos los subgrupos por encima de ``u105``.

.. code-block:: gap
    :caption: subgrupo del índice :math:`15`
    :name: subgrupo_del_indice_15
    
    gap> blocks := Blocks( a8, orb );; Length( blocks );
    15
    gap> blocks[1];
    [ (1,2)(3,4)(5,6)(7,8), (1,3)(2,4)(5,8)(6,7), (1,4)(2,3)(5,7)(6,8),
      (1,5)(2,6)(3,8)(4,7), (1,6)(2,5)(3,7)(4,8), (1,7)(2,8)(3,6)(4,5),
      (1,8)(2,7)(3,5)(4,6) ]

Para encontrar el subgrupo del índice :math:`15`, nuevamente usamos el cierre. Ahora debemos tener un poco de cuidado para evitar confusiones. ``u105`` es el estabilizador de ``(1,2)(3,4)(5,6)(7,8)``. Sabemos que existe una correspondencia entre los puntos de la órbita y las clases laterales de ``u105``. El punto ``(1,2)(3,4)(5,6)(7,8)`` corresponde a ``u105``. Para obtener el subgrupo sobre ``u105`` que tiene el índice :math:`15` en ``a8``, debemos formar el cierre de ``u105`` con un elemento de la clase lateral que corresponda a cualquier otro punto del primer bloque. Si elegimos el punto ``(1,3)(2,4)(5,8)(6,7)``, debemos usar un elemento de ``a8`` que mapee ``(1,2)(3,4)(5,6)(7,8)`` a ``(1,3)(2,4)(5,8)(6,7)``. La función ``RepresentativeAction`` hace lo que necesitamos. Toma un grupo y dos puntos y devuelve un elemento del grupo que asigna el primer punto al segundo. De hecho, también le permite especificar la acción como un cuarto argumento opcional como de costumbre, pero no lo necesitamos aquí. Si no existe tal elemento en el grupo, es decir, si los dos puntos no se encuentran en una órbita debajo del grupo, ``RepresentativeAction`` retorna ``fail``.

.. code-block:: gap
    :caption: función RepresentativeAction
    :name: funcion_RepresentativeAction_dos
    
    gap> rep := RepresentativeAction( a8, (1,2)(3,4)(5,6)(7,8),
    >                                     (1,3)(2,4)(5,8)(6,7) );
    (2,3)(6,8)
    gap> u15 := ClosureGroup( u105, rep );; Index( a8, u15 );
    15

``u15`` es, por supuesto, un subgrupo maximal, porque ``a8`` no tiene subgrupos de índice :math:`3` o :math:`5`. De hecho, existe otra clase de subgrupos de índice :math:`15` por encima de ``u105`` que obtenemos sumando ``(2,3)(6,7)`` a ``u105``.

.. code-block:: gap
    :caption: función RepresentativeAction retornando fail
    :name: funcion_RepresentativeAction_retornando_fail
    
    gap> u15b := ClosureGroup( u105, (2,3)(6,7) );; Index( a8, u15b );
    15
    gap> RepresentativeAction( a8, u15, u15b );
    fail

``RepresentativeAction`` nos dice que no hay un elemento :math:`g` en ``a8`` tal que ``u15 ^ g = u15b``. Como ``^`` también denota la conjugación de subgrupos, esto nos dice que ``u15`` y ``u15b`` no son conjugados.

.. important::

    Un concepto muy utilizado en programación, que el de :underline:`sobrecarga de operadores`. Aquí podemos ver un claro ejemplo de esto, ya que en este contexto, ``^`` **es un operador que denota la conjugación de subgrupos**.

Resumen
~~~~~~~~~~~~~~~~

En esta sección hemos demostrado algunas funciones del paquete de acciones. Hay toda una clase de funciones que no mencionamos, es decir, aquellas que toman un solo elemento en lugar de un grupo completo como primer argumento, por ejemplo, ``Cycle`` y ``Permutation``. Estos se describen detalladamente en el ``Capítulo 39`` del manual de referencia.

Homomorfismos de Grupos por Imagenes
--------------------------------------

Ya hemos visto ejemplos de homomorfismos de grupo en las últimas secciones, es decir, homomorfismos naturales y homomorfismos de acción. En esta sección mostraremos cómo construir un homomorfismo de grupo :math:`G \to H` especificando un conjunto generador para :math:`G` y las imágenes de estos generadores en :math:`H`. Usamos la función ``GroupHomomorphismByImages( G, H, gens, imgs )`` donde

    - ``G`` es el dominio,
    - ``H`` es el codominio,
    - ``gens`` es un generador set para :math:`G` y
    - ``imgs`` es una lista cuya :math:`i`-ésima entrada es la imagen de ``gens[ i ]`` bajo el homomorfismo.

.. code-block:: gap
    :caption: función GroupHomomorphismByImages
    :name: funcion_GroupHomomorphismByImages

    gap> s4 := Group((1,2,3,4),(1,2));; s3 := Group((1,2,3),(1,2));;
    gap> hom := GroupHomomorphismByImages( s4, s3, 
    >                                      GeneratorsOfGroup(s4), [(1,2),(2,3)] );
    [ (1,2,3,4), (1,2) ] -> [ (1,2), (2,3) ]
    gap> Kernel( hom );
    Group([ (1,4)(2,3), (1,3)(2,4) ])
    gap> Image( hom, (1,2,3) );
    (1,2,3)
    gap> Image( hom, DerivedSubgroup(s4) );
    Group([ (1,3,2), (1,3,2) ])
    gap> PreImage( hom, (1,2,3) );
    Error, <map> must be an inj. and surj. mapping called from
    <function>( <arguments> ) called from read-eval-loop
    Entering break read-eval-print loop ...
    you can ’quit;’ to quit to outer loop, or
    you can ’return;’ to continue
    brk> quit;
    gap> PreImagesRepresentative( hom, (1,2,3) );
    (1,4,2)
    gap> PreImage( hom, TrivialSubgroup(s3) ); # the kernel
    Group([ (1,4)(2,3), (1,3)(2,4) ])

Este homomorfismo de :math:`\mathbb{S}_4` a :math:`\mathbb{S}_3` es bien conocido por la teoría de grupos elemental. Las imágenes de elementos y subgrupos bajo hom se pueden calcular con la función ``Image``. Pero como el mapeo ``hom`` no es biyectivo, no podemos usar la función ``PreImage`` para preimágenes de elementos (pueden tener varias preimágenes). En su lugar, tenemos que usar ``PreImagesRepresentative``, que devuelve una preimagen si existe al menos una (y devolvería fail si no existe ninguna, lo que no puede ocurrir para nuestro ``hom`` sobreyectivo). Por otro lado, podemos usar ``PreImage`` para la preimagen de un conjunto (que siempre existe, incluso si está vacío).

Supongamos que escribimos mal la entrada cuando intentamos construir un homomorfismo, como en el siguiente ejemplo.

.. code-block:: gap
    :caption: función GroupHomomorphismByImages retornando fail
    :name: funcion_GroupHomomorphismByImages_retornando_fail
    
    gap> GroupHomomorphismByImages( s4, s3,
    >                                GeneratorsOfGroup(s4), [(1,2,3),(2,3)] );
    fail

No existe tal homomorfismo, por lo que se devuelve fail. Pero tenga en cuenta que debido a esto, ``GroupHomomorphismByImages`` debe hacer algunas comprobaciones, y esto también se hizo para el mapeo hom anterior. Se pueden evitar estos controles si se está seguro de que realmente existe el homomorfismo deseado. Para eso, se puede utilizar la función ``GroupHomomorphismByImagesNC``; ``NC`` significa "no check".

Pero tenga en cuenta que pueden suceder cosas horribles si se usa ``GroupHomomorphismByImagesNC`` cuando la entrada no describe un homomorfismo.

.. code-block:: gap
    :caption: función GroupHomomorphismByImagesNC
    :name: funcion_GroupHomomorphismByImagesNC
    
    gap> hom2 := GroupHomomorphismByImagesNC( s4, s3,
    >                                         GeneratorsOfGroup(s4), [(1,2,3),(2,3)] );
    [ (1,2,3,4), (1,2) ] -> [ (1,2,3), (2,3) ]
    gap> Size( Kernel(hom2) );
    24


En otras palabras, **GAP** afirma que el kernel es ``s4`` completo, ¡pero ``hom2`` obviamente tiene algunas imágenes no triviales! Claramente no existe un homomorfismo que mapee un elemento de orden :math:`4` (es decir, ``(1,2,3,4)``) a un elemento de orden :math:`3` (es decir, ``(1,2,3)``). Pero si usa el comando ``GroupHomomorphismByImagesNC``, **GAP** confía en usted.

.. code-block:: gap
    :caption: función IsGroupHomomorphism
    :name: funcion_IsGroupHomomorphism
    
    gap> IsGroupHomomorphism( hom2 );
    true

¡Y luego produce serias tonterías si la cosa no es un homomorfismo, como se vio arriba!

Además del comando seguro ``GroupHomomorphismByImages``, que devuelve un error si el homomorfismo solicitado no existe, existe la función ``GroupGeneralMappingByImages``, que devuelve un mapeo general (es decir, un mapeo posiblemente de varios valores) que se puede probar con ``IsGroupHomomorphism``.

.. code-block:: gap
    :caption: función GroupGeneralMappingByImage
    :name: funcion_GroupGeneralMappingByImage
    
    gap> hom2 := GroupGeneralMappingByImages( s4, s3,
    >                                         GeneratorsOfGroup(s4), [(1,2,3),(2,3)] );;
    gap> IsGroupHomomorphism( hom2 );
    false

Pero la posibilidad de probar si es un homomorfismo no es la única razón por la que **GAP** ofrece mapeos generales de grupo. Otra razón (¿más importante?) es que su existencia permite la "inversión de flechas" en un homomorfismo como nuestro hom original. Con esto nos referimos al ``GroupHomomorphismByImages`` con los lados izquierdo y derecho intercambiados, en cuyo caso, por supuesto, es simplemente un ``GroupGeneralMappingByImages``.

.. code-block:: gap
    :caption: función GroupGeneralMappingByImage
    :name: funcion_GroupGeneralMappingByImage_dos
    
    gap> rev := GroupGeneralMappingByImages( s3, s4,
    >                                        [(1,2),(2,3)], GeneratorsOfGroup(s4) );;

Ahora tenemos :math:`a\stackrel{hom}{\longmapsto} b \Longleftrightarrow b\stackrel{rev}{\longmapsto} a` para :math:`a \in \mathbb{S}_{4}` y :math:`b \in \mathbb{S}_{3}`. Dado que cada :math:`b` tiene :math:`4` imágenes previas bajo ``hom``, ahora tiene :math:`4` imágenes bajo ``rev``. Así como las :math:`4` preimágenes forman una clase lateral del núcleo :math:`V_{4}\leq\mathbb{S}_{4}` de ``hom``, también forman una clase lateral del conucleo :math:`V_{4}\leq\mathbb{S}_{4}` de ``rev``. El conúcleo en sí es el conjunto de todas las imágenes de ``One( s3 )`` (es un subgrupo normal en el grupo de todas las imágenes bajo ``rev``). La operación ``One`` devuelve el elemento de identidad de un grupo, consulte ``30.10.2`` en el manual de referencia. Y es por eso que **GAP** quiere realizar tal inversión de flechas: calcula el núcleo de un homomorfismo como ``hom`` como el conúcleo del mapeo general del grupo invertido (aquí ``rev``).

.. code-block:: gap
    :caption: función CoKernel
    :name: funcion_CoKernel
    
    gap> CoKernel( rev );
    Group([ (1,4)(2,3), (1,3)(2,4) ])

La razón por la que ``rev`` no es un homomorfismo es que no tiene un solo valor (porque ``hom`` no era inyectivo). Pero hay otra condición crítica: si invertimos las flechas de un homomorfismo no sobreyectivo, obtenemos un mapeo general de grupo que no está definido en todas partes, es decir, que no es total (aunque tendrá un solo valor si el homomorfismo original es inyectable). **GAP** requiere que un homomorfismo de grupo sea tanto de un solo valor como total, por lo que fallará, mostrando un ``fail``, si dice ``GroupHomomorphismByImages( G, H, gens, imgs )`` donde ``gens`` no genera :math:`G` (incluso si esto daría un homomorfismo decente en el subgrupo generado por ``gens``). Para obtener una descripción completa, consulte el ``Capítulo 38`` del manual de referencia.

El último ejemplo de esta sección muestra que la noción de núcleo y conúcleo se extiende naturalmente incluso al caso en el que ni ``hom2`` ni su mapeo general inverso (con flechas invertidas) es un homomorfismo.

.. code-block:: gap
    :caption: función InverseGeneralMapping
    :name: funcion_InverseGeneralMapping
    
    gap> CoKernel( hom2 ); Kernel( hom2 );
    Group([ (2,3), (1,3) ])
    Group([ (3,4), (2,3,4), (1,2,4) ])
    gap> IsGroupHomomorphism( InverseGeneralMapping( hom2 ) );
    false

Resumen
~~~~~~~~~~~~~~~~~~~~~

En esta sección hemos construido homomorfismos especificando imágenes para un conjunto de generadores. Hemos visto que al invertir la dirección del mapeo, obtenemos mapeos generales de grupo, que no necesitan ser de un solo valor (a menos que el mapeo sea inyectivo) ni total (a menos que el mapeo sea sobreyectivo).


Buenos monomorfismos
----------------------------

Para algunos tipos de grupos, el mejor método para calcular en un grupo isomorfo en una "mejor" representación (por ejemplo, un grupo de permutación). A un homomorfismo inyectivo lo llamamos, que le dará a una imagen tan isomorfa un ":underline:`buen monomorfismo`".

Por ejemplo, en el caso de un grupo de matrices, podemos tomar la acción en el espacio vectorial subyacente (o un subconjunto adecuado) para obtener tal monomorfismo:

.. code-block:: gap
    :caption: función ActionHomomorphism
    :name: funcion_ActionHomomorphism_dos
    
    gap> grp:=GL(2,3);;
    gap> dom:=GF(3)^2;;
    gap> hom := ActionHomomorphism( grp, dom );; IsInjective( hom );
    true
    gap> p := Image( hom,grp );
    Group([ (4,7)(5,8)(6,9), (2,7,6)(3,4,8) ])

Para demostrar la técnica de los "buenos monomorfismos", calculamos las clases de conjugación del grupo de permutación y las volvemos a colocar en el grupo de matriz con el monomorfismo ``hom``. Recuperar una clase de conjugación significa encontrar la preimagen del representante y del centralizador; este último se llama ``StabilizerOfExternalSet`` en **GAP** (debido a que las clases de conjugación se representan como conjuntos externos, consulte la ``Sección 37.10`` del manual de referencia).

.. code-block:: gap
    :caption: función StabilizerOfExternalSet
    :name: funcion_StabilizerOfExternalSet

    gap> pcls := ConjugacyClasses( p );; gcls := [ ];;
    gap> for pc in pcls do
    >        gc:=ConjugacyClass( grp , PreImagesRepresentative(hom,Representative(pc)));
    >        SetStabilizerOfExternalSet(gc,PreImage(hom,
    >        StabilizerOfExternalSet(pc)));
    >        Add( gcls, gc );
    >    od;
    gap> List( gcls, Size );
    [ 1, 8, 12, 1, 8, 6, 6, 6 ]

Todos los pasos que hemos realizado anteriormente los realiza **GAP** automáticamente si simplemente consulta a ``ConjugacyClasses( grp )``, siempre que **GAP** ya sepa que ``grp`` es finito (por ejemplo, porque preguntó a ``IsFinite( grp )`` antes). La razón de esto es que un grupo de matrices finitas como ``grp`` es "manejado por un buen monomorfismo". Para tales grupos, **GAP** usa el comando ``NiceMonomorphism`` para construir un monomorfismo (como el ``hom`` en el ejemplo anterior) y luego procede como lo hicimos anteriormente.

.. code-block:: gap
    :caption: función NiceMonomorphis
    :name: funcion_NiceMonomorphis
    
    gap> grp:=GL(2,3);;
    gap> IsHandledByNiceMonomorphism( grp );
    true
    gap> hom := NiceMonomorphism( grp );
    <action isomorphism>
    gap> p :=Image(hom,grp);
    Group([ (4,7)(5,8)(6,9), (2,7,6)(3,4,8) ])
    gap> cc := ConjugacyClasses( grp );; ForAll(cc, x-> x in gcls);
    true
    gap> ForAll(gcls, x->x in cc); # cc and gcls might be ordered differently
    true

Tenga en cuenta que un buen monomorfismo podría definirse en un grupo más grande que ``grp``, por lo que tenemos que usar ``Image( hom, grp )`` y no solo ``Image( hom )``. Los monomorfismos agradables no solo se utilizan para grupos de matrices, sino también para otros tipos de grupos en los que no se pueden calcular con la suficiente facilidad. Como otro ejemplo, demostremos que el grupo de automorfismos del grupo de cuaterniones de orden :math:`8` es isomorfo al grupo simétrico de grado :math:`4` examinando el " buen objeto" asociado con ese grupo de automorfismos.

.. code-block:: gap
    :caption: función NiceObject
    :name: funcion_NiceObject

    gap> p:= Group((1,7,6,8)(2,5,3,4), (1,2,6,3)(4,8,5,7));;
    gap> aut:= AutomorphismGroup( p );; NiceMonomorphism(aut);;
    gap> niceaut := NiceObject( aut );
    Group([ (1,2)(3,4), (3,4)(5,6), (1,5,2,6), (1,5,3)(2,6,4) ])
    gap> IsomorphismGroups( niceaut, SymmetricGroup( 4 ) );
    [ (1,2)(3,4), (3,4)(5,6), (1,5,2,6), (1,5,3)(2,6,4) ] ->
    [ (1,4)(2,3), (1,3)(2,4), (1,4,2,3), (1,3,2) ]

El rango de un monomorfismo agradable es en la mayoría de los casos un grupo de permutación, porque los buenos monomorfismos son en su mayoría homomorfismos de acción. En algunos casos, como en nuestro último ejemplo, el grupo se puede resolver y es posible que prefiera un grupo de ``PC`` como buen objeto. No puede cambiar el buen monomorfismo del grupo de automorfismo (porque es el valor del atributo ``NiceMonomorphism``), pero puede componerlo con un isomorfismo del grupo de permutación a un grupo de ``pc`` para obtener su monomorfismo personal como el mejor de todos. Si reconstruye el grupo de automorfismo, incluso puede prescribirle este buen monomorfismo como su ``NiceMonomorphism``, porque un grupo recién construido aún no tendrá un conjunto de ``NiceMonomorphism``.

.. code-block:: gap
    :caption: función SetNiceMonomorphism
    :name: funcion_SetNiceMonomorphism

    gap> nicer := NiceMonomorphism(aut) * IsomorphismPcGroup(niceaut);;
    gap> aut2 := GroupByGenerators( GeneratorsOfGroup( aut ) );;
    gap> SetIsHandledByNiceMonomorphism( aut2, true );
    gap> SetNiceMonomorphism( aut2, nicer );
    gap> NiceObject( aut2 ); # a pc group
    Group([ f4, f3, f1*f2^2*f3*f4, f2^2*f3*f4 ])

El asterisco ``*`` denota la composición de las asignaciones de izquierda a derecha, como hemos visto en la ``Sección 5.2`` anterior. La reconstrucción del grupo de automorfismo puede, por supuesto, resultar en la pérdida de otra información que **GAP** ya había reunido, además del (no tan) buen monomorfismo.

Resumen
~~~~~~~~~~~~~~

En esta sección hemos visto cómo se pueden realizar cálculos en grupos en imágenes isomorfas en grupos más agradables. Hemos visto que **GAP** persigue esta técnica automáticamente para ciertas clases de grupos, por ejemplo, para grupos matriciales que se sabe que son finitos.
