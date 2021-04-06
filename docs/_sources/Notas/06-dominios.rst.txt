.. role:: underline
    :class: underline

Dominios
==================

Dominio es el nombre que otorga **GAP** a los conjuntos estructurados (ver). Ya vimos ejemplos de dominios en los ``Capítulos 5`` y ``6``: los grupos ``s8`` y ``a8`` en la ``Sección 5.1`` son dominios, de la misma manera el campo ``f`` y el espacio vectorial ``v`` en la ``Sección 6.1`` son dominios. Fueron construidos por funciones como ``Group`` y ``GF``, y podrían pasarse como argumentos a otras funciones como ``DerivedSubgroup`` y ``Dimension``.

Dominios como conjuntos
--------------------------------

En primer lugar, un dominio :math:`D` es un conjunto.
    
    - Si :math:`D` es finito, entonces se puede calcular una lista con los elementos de este conjunto con las funciones ``AsList`` y ``AsSortedList``. 
    - Para :math:`D` infinito, ``Enumerator`` y ``EnumeratorSorted`` pueden funcionar, pero también es posible que se obtengamos un mensaje de error.

Los dominios se pueden utilizar como argumentos de funciones de conjunto como ``Intersection`` y ``Union``. **GAP** intenta devolver un dominio en estos casos, además intenta devolver un dominio con la mayor estructura posible. Por ejemplo, la intersección de dos grupos está (ya sea vacía o) nuevamente un grupo, y **GAP** intentará devolverlo como un grupo.

Para ``Union``, la situación es diferente porque la unión de dos grupos en general no es un grupo.

.. code-block:: gap
    :caption: función Intersection
    :name: funcion_Intersection
    
    gap> g:= Group( (1,2), (3,4) );;
    gap> h:= Group( (3,4), (5,6) );;
    gap> Intersection( g, h );
    Group([ (3,4) ])

.. important::

    Dos dominios se consideran igual ``w.r.t.`` según el operador "``=``" si y solo si son iguales como conjuntos, independientemente de la estructura adicional de los dominios.

.. code-block:: gap
    :caption: igualdad de Dominios
    :name: igualdad_de_Dominios

    gap> mats:= [ [ [ 0*Z(2), Z(2)^0 ], [ Z(2)^0, 0*Z(2) ] ],
    >             [ [ Z(2)^0, 0*Z(2) ], [ 0*Z(2), Z(2)^0 ] ] ];;
    gap> Ring( mats ) = VectorSpace( GF(2), mats );
    true

.. important::

    Un dominio se considera igual a la lista ordenada de sus elementos.

.. code-block:: gap
    :caption: Dominios según la lista ordenada de sus elementos
    :name: Dominios_segun_la_lista_ordenada_de_sus_elementos

    gap> g:= Group( (1,2) );;
    gap> l:= AsSortedList( g );
    [ (), (1,2) ]
    gap> g = l;
    true
    gap> IsGroup( l ); IsList( g );
    false
    false

Estructura Algebraica
--------------------------------

La estructura adicional de :math:`D` está constituida por los hechos de que se sabe que :math:`D` está cerrado bajo ciertas operaciones como la suma o la multiplicación, y que estas operaciones tienen propiedades adicionales. Por ejemplo, si :math:`D` es un grupo, entonces se cierra al multiplicar (:math:`D \times D \to D`, :math:`(g, h) \mapsto g \ast h`), al tomar inversas (:math:`D \to D`, :math:`g \mapsto g^{-1}`) y tomando la identidad :math:`g^{0}` de cada elemento :math:`g` en :math:`D`; además, la multiplicación en :math:`D` es asociativa.

El mismo conjunto de elementos puede contener diferentes estructuras algebraicas. Por ejemplo, un semigrupo se define como cerrado bajo una multiplicación asociativa, por lo que cada grupo es también un semigrupo. Asimismo, un monoide se define como un semigrupo :math:`D` en el que la identidad :math:`g^{0}` se define para cada elemento :math:`g`, por lo que cada grupo es un monoide y cada monoide es un semigrupo.

Otros ejemplos de dominios son los espacios vectoriales, que se definen como grupos aditivos que se cierran bajo la multiplicación (izquierda) con elementos en un cierto dominio de escalares. También las clases de conjugación en un grupo :math:`D` son dominios, están cerrados bajo la acción de conjugación de :math:`D`.

Nociones de Generación
--------------------------------

Hemos visto que un dominio se cierra bajo determinadas operaciones. Por lo general, un dominio se construye como el cierre de algunos elementos bajo estas operaciones. En esta situación, decimos que los elementos generan el dominio.

Por ejemplo, se puede utilizar una lista de matrices de la misma forma sobre un campo común para generar un grupo aditivo o un espacio vectorial sobre un campo adecuado.

.. note:: 
    
    Si las matrices son cuadradas, también podemos usar las matrices como

        - generadores de un semigrupo,
        - generadores de un anillo o
        - generadores de un álgebra.
    
Ilustramos algunas de estas posibilidades:

.. code-block:: gap
    :caption: Elementos que generan el dominio 
    :name: elementos_que_generan_el_dominio
    
    gap> mats:= [ [ [ 0*Z(2), Z(2)^0 ],
    >               [ Z(2)^0, 0*Z(2) ] ],
    >             [ [ Z(2)^0, 0*Z(2) ],
    >               [ 0*Z(2), Z(2)^0 ] ] ];;
    gap> Size( AdditiveMagma( mats ) );
    4
    gap> Size( VectorSpace( GF(8), mats ) );
    64
    gap> Size( Algebra( GF(2), mats ) );
    4
    gap> Size( Group( mats ) );
    2

Cada combinación de operaciones bajo las cuales se podría cerrar un dominio da una noción de generación. Entonces, cada grupo tiene generadores de grupo, y dado que es un monoide, también se pueden solicitar generadores de monoide de un grupo.

Nótese que no se puede simplemente preguntar por "los generadores de un dominio", siempre es necesario especificar qué noción de generación se entiende. El acceso a los diferentes generadores se proporciona mediante funciones con nombres de la forma ``GeneratorsOfSomething``. Por ejemplo, ``GeneratorsOfGroup`` denota generadores de grupo, ``GeneratorsOfMonoid`` denota generadores monoide, etc. El resultado de ``GeneratorsOfVectorSpace``, por supuesto, debe entenderse en relación con el campo de escalares del espacio vectorial en cuestión.

.. code-block:: gap
    :caption: función GeneratorsOfVectorSpace 
    :name: funcion_GeneratorsOfVectorSpace
    
    gap> GeneratorsOfVectorSpace( GF(4)^2 );
    [ [ Z(2)^0, 0*Z(2) ], [ 0*Z(2), Z(2)^0 ] ]
    gap> v:= AsVectorSpace( GF(2), GF(4)^2 );;
    gap> GeneratorsOfVectorSpace( v );
    [ [ Z(2)^0, 0*Z(2) ], [ 0*Z(2), Z(2)^0 ], [ Z(2^2), 0*Z(2) ],
    [ 0*Z(2), Z(2^2) ] ]

Constructores de dominio
--------------------------------

Un grupo se puede construir a partir de una lista de generadores de grupos gens por ``Group( gens )``, así mismo se pueden construir anillos y álgebras con las funciones ``Ring`` y ``Algebra``.

Tenga en cuenta que no siempre o completamente se comprueba que gens sea de hecho una lista válida de generadores de grupo, por ejemplo, si los elementos de gens se pueden multiplicar o si son invertibles. Esto significa que **GAP** confía en usted, al menos hasta cierto punto, que el dominio deseado ``Something( gens )`` existe.

Formación de cierres de dominios
-----------------------------------

Además de construir dominios a partir de generadores, también se puede formar el cierre de un dominio dado con un elemento u otro dominio. Existen diferentes nociones de cierre, uno tiene que especificar una de acuerdo con el resultado deseado y la estructura del dominio dado. Las funciones para calcular cierres tienen nombres como ``ClosureSomething``. Por ejemplo, si :math:`D` es un grupo y se quiere construir el grupo generado por :math:`D` y un elemento :math:`g`, entonces se puede usar ``ClosureGroup( D, g )``.

Cambiar la estructura
-----------------------------------

El mismo conjunto de elementos puede tener diferentes estructuras algebraicas. Por ejemplo, puede suceder que un monoide :math:`M` contenga de hecho las inversas de todos sus elementos y, por tanto, :math:`M` sea igual al grupo formado por los elementos de :math:`M`.

.. code-block:: gap
    :caption: ejemplo de diferentes estructuras algebraicas 
    :name: ejemplo_de_diferentes_estructuras_algebraicas
    
    gap> m:= Monoid( mats );;
    gap> m = Group( mats );
    true
    gap> IsGroup( m );
    false

El último resultado del ejemplo anterior puede resultar sorprendente. Pero el monoide m no se considera un grupo en **GAP** y, además, no hay forma de convertir :math:`m` en un grupo. Formulemos esto como regla:

    - **El conjunto de operaciones bajo las cuales se cierra el dominio se fija en la construcción de un dominio y no puede ser modificado posteriormente.**

(Contrario a esto, un dominio puede adquirir conocimiento sobre propiedades tales como si la multiplicación es asociativa o conmutativa).

Si se necesita un dominio con una estructura diferente a la dada, se puede construir un nuevo dominio con la estructura requerida. Las funciones que hacen estas construcciones tienen nombres como ``AsSomething``, devuelven un dominio que tiene los mismos elementos que el argumento en cuestión pero la estructura ``Something``. En la situación anterior, se puede usar ``AsGroup``.

.. code-block:: gap
    :caption: función AsGroup 
    :name: ejemplo_de_funcion_AsGroup
    
    gap> g:= AsGroup( m );;
    gap> m = g;
    true
    gap> IsGroup( g );
    true

Si es imposible construir el dominio deseado, el retorno de las funciones ``AsSomething`` falla.

.. code-block:: gap
    :caption: función AsVectorSpace
    :name: ejemplo_de_funcion_AsVectorSpace
    
    gap> AsVectorSpace( GF(4), GF(2)^2 );
    fail

Las funciones ``AsList`` y ``AsSortedList`` mencionadas anteriormente no devuelven dominios, pero encajan en el patrón general en el sentido de que olvidan toda la estructura del argumento, incluido el hecho de que es un dominio, y devuelven una lista con los mismos elementos que el argumento tiene.

Subdominios
-----------------------------------

Es posible construir un dominio como un subconjunto de un dominio existente. Las funciones respectivas tienen nombres como ``Subsomething``, devuelven dominios con la estructura ``Something``. (Tenga en cuenta que la segunda :math:`s` en ``Subsomething`` no está en mayúscula). Por ejemplo, si uno quiere tratar con el subgrupo del dominio :math:`D` que es generado por los elementos en la lista gens, podemos usar ``Subgroup( D, gens )``. No se requiere que :math:`D` sea en sí mismo un grupo, solo que el grupo generado por gens debe ser un subconjunto de :math:`D`.

Se puede acceder al superconjunto de un dominio :math:`S` que fue construido por una función ``Subsomething`` como ``Parent( S )``.

.. code-block:: gap
    :caption: función Parent
    :name: ejemplo_de_funcion_Parent
    
    gap> g:= SymmetricGroup( 5 );;
    gap> gens:= [ (1,2), (1,2,3,4) ];;
    gap> s:= Subgroup( g, gens );;
    gap> h:= Group( gens );;
    gap> s = h;
    true
    gap> Parent( s ) = g;
    true

Muchas funciones devuelven subdominios de sus argumentos, por ejemplo, el resultado de ``SylowSubgroup( G )`` es un grupo con el grupo padre :math:`G`.

Si está seguro de que el dominio ``Something( gens )`` está contenido en el dominio :math:`D`, también puede llamar a ``SubsomethingNC( D, gens )`` en lugar de ``Subsomething( D, gens )``. ``NC`` significa "sin verificación" y las funciones cuyos nombres terminan con ``NC`` omiten la verificación de contención.