.. role:: underline
    :class: underline

Espacios Vectoriales y Álgebras
====================================

Este capítulo contiene una introducción a los **espacios vectoriales** y **álgebras** en **GAP**.

Espacios Vectoriales
----------------------

Un espacio vectorial sobre el campo F es un grupo aditivo que se cierra bajo multiplicación escalar con elementos en :math:`\mathbb{F}`. En **GAP**, solo aquellos dominios que se construyen como espacios vectoriales se consideran espacios vectoriales. En particular, un grupo aditivo que no conoce un dominio activo de escalares no se considera un espacio vectorial en **GAP**.

Probablemente, los :math:`\mathbb{F}`-espacios vectoriales  más comunes en **GAP** son los llamados espacios de fila. Consisten en vectores de fila, es decir, listas cuyos elementos se encuentran en :math:`\mathbb{F}`. En el siguiente ejemplo calculamos el espacio vectorial generado por los vectores fila ``[1, 1, 1]`` y ``[1, 0, 2]`` sobre los racionales.

.. code-block:: gap
    :caption: vectores fila 
    :name: vectores_fila
    
    gap> F:= Rationals;;
    gap> V:= VectorSpace( F, [ [ 1, 1, 1 ], [ 1, 0, 2 ] ] );
    <vector space over Rationals, with 2 generators>
    gap> [ 2, 1, 3 ] in V;
    true

El espacio de fila completo :math:`\mathbb{F}^{n}` se crea mediante comandos como:

.. code-block:: gap
    :caption: El espacio de fila completo :math:`\mathbb{F}^{n}`
    :name: espacio_de_fila

    gap> F:= GF( 7 );;
    gap> V:= F^3; # The full row space over F of dimension 3.
    ( GF(7)^3 )
    gap> [ 1, 2, 3 ] * One( F ) in V;
    true

De la misma forma también podemos crear espacios matriciales. Aquí se puede utilizar el campo de notación ``field^[dim1 , dim2]``

.. code-block:: gap
    :caption: El espacio de fila completo :math:`\mathbb{F}^{nxm}`
    :name: espacio_de_fila_en_el_espacio_de_matrices

    gap> m1:= [ [ 1, 2 ], [ 3, 4 ] ];; m2:= [ [ 0, 1 ], [ 1, 0 ] ];;
    gap> V:= VectorSpace( Rationals, [ m1, m2 ] );
    <vector space over Rationals, with 2 generators>
    gap> m1+m2 in V;
    true
    gap> W:= Rationals^[3,2];
    ( Rationals^[ 3, 2 ] )
    gap> [ [ 1, 1 ], [ 2, 2 ], [ 3, 3 ] ] in W;
    true

Un campo es, naturalmente, un espacio vectorial sobre sí mismo.

.. code-block:: gap
    :caption: función IsVectorSpace
    :name: funcion_IsVectorSpace

    gap> IsVectorSpace( Rationals );
    true

Si :math:`\Phi` es una extensión algebraica de :math:`\mathbb{F}`, entonces :math:`\Phi` también es un espacio vectorial sobre :math:`\mathbb{F}` (y de hecho sobre cualquier subcampo de :math:`\Phi` que contenga :math:`\mathbb{F}`). Este campo :math:`\mathbb{F}` se almacena en el atributo ``LeftActingDomain``. En **GAP**, el valor predeterminado es ver los campos como espacios de vectores sobre sus campos principales. Mediante la función ``AsVectorSpace``, podemos ver campos como espacios vectoriales sobre campos distintos del campo principal.

.. code-block:: gap
    :caption: función LeftActingDomain
    :name: funcion_LeftActingDomain

    gap> F:= GF( 16 );;
    gap> LeftActingDomain( F );
    GF(2)
    gap> G:= AsVectorSpace( GF( 4 ), F );
    AsField( GF(2^2), GF(2^4) )
    gap> F = G;
    true
    gap> LeftActingDomain( G );
    GF(2^2)

Un espacio vectorial tiene tres atributos importantes:
    
    - su campo de definición,
    - su dimensión y
    - una base.

Ya encontramos la función ``LeftActingDomain`` en el ejemplo anterior. Extrae el campo de definición de un espacio vectorial. La función ``Dimension`` proporciona la dimensión del espacio. He aquí un ejemplo más.

.. code-block:: gap
    :caption: función Dimension
    :name: funcion_Dimension

    gap> F:= GF( 9 );;
    gap> m:= [ [ Z(3)^0, 0*Z(3), 0*Z(3) ], [ 0*Z(3), Z(3)^0, Z(3)^0 ] ];;
    gap> V:= VectorSpace( F, m );
    <vector space over GF(3^2), with 2 generators>
    gap> Dimension( V );
    2
    gap> W:= AsVectorSpace( GF( 3 ), V );
    <vector space over GF(3), with 4 generators>
    gap> V = W;
    true
    gap> Dimension( W );
    4
    gap> LeftActingDomain( W );
    GF(3)

Uno de los atributos más importantes es la **base**. Para una base dada :math:`\mathcal{B}` de :math:`V`, cada vector :math:`v` en :math:`V` se puede expresar de forma única como :math:`v = \displaystyle \sum_{b\in\mathcal{B}} c_{b}b`, con coeficientes :math:`c_{b}\in\mathbb{F}`.

En **GAP**, las bases son listas especiales de vectores. Se utilizan principalmente para el cálculo de coeficientes y combinaciones lineales.

Dado un espacio vectorial :math:`V`, se obtiene una base de V simplemente aplicando la función ``Basis`` a :math:`V`. Los vectores que forman la base se extraen de la base mediante ``BasisVectors``.

.. code-block:: gap
    :caption: función BasisVectors
    :name: funcion_BasisVectors

    gap> m1:= [ [ 1, 2 ], [ 3, 4 ] ];; m2:= [ [ 1, 1 ], [ 1, 0 ] ];;
    gap> V:= VectorSpace( Rationals, [ m1, m2 ] );
    <vector space over Rationals, with 2 generators>
    gap> B:= Basis( V );
    SemiEchelonBasis( <vector space over Rationals, with 2 generators>, ... )
    gap> BasisVectors( Basis( V ) );
    [ [ [ 1, 2 ], [ 3, 4 ] ], [ [ 0, 1 ], [ 2, 4 ] ] ]

Los coeficientes de un vector con respecto a una base dada se encuentran mediante la función ``Coefficients``. Además, las combinaciones lineales de los vectores básicos se construyen utilizando ``LinearCombination``.

.. code-block:: gap
    :caption: función LinearCombination
    :name: funcion_LinearCombination

    gap> V:= VectorSpace( Rationals, [ [ 1, 2 ], [ 3, 4 ] ] );
    <vector space over Rationals, with 2 generators>
    gap> B:= Basis( V );
    SemiEchelonBasis( <vector space over Rationals, with 2 generators>, ... )
    gap> BasisVectors( Basis( V ) );
    [ [ 1, 2 ], [ 0, 1 ] ]
    gap> Coefficients( B, [ 1, 0 ] );
    [ 1, -2 ]
    gap> LinearCombination( B, [ 1, -2 ] );
    [ 1, 0 ]

En los ejemplos anteriores, hemos visto que **GAP** a menudo elige la base con la que quiere trabajar. También es posible construir bases con vectores de base prescritos dando una lista de estos vectores como segundo argumento de ``Basis``.

.. code-block:: gap
    :caption: Coefficients
    :name: funcion_Coefficients

    gap> V:= VectorSpace( Rationals, [ [ 1, 2 ], [ 3, 4 ] ] );;
    gap> B:= Basis( V, [ [ 1, 0 ], [ 0, 1 ] ] );
    SemiEchelonBasis( <vector space over Rationals, with 2 generators>,
    [ [ 1, 0 ], [ 0, 1 ] ] )
    gap> Coefficients( B, [ 1, 2 ] );
    [ 1, 2 ]

Podemos construir subespacios y espacios de cociente de espacios vectoriales. El mapa de proyección natural (construido por ``NaturalHomomorphismBySubspace``) conecta un espacio vectorial con su espacio cociente.

.. code-block:: gap
    :caption: función PreImagesRepresentative
    :name: funcion_PreImagesRepresentative

    gap> V:= Rationals^4;
    ( Rationals^4 )
    gap> W:= Subspace( V, [ [ 1, 2, 3, 4 ], [ 0, 9, 8, 7 ] ] );
    <vector space over Rationals, with 2 generators>
    gap> VmodW:= V/W;
    ( Rationals^2 )
    gap> h:= NaturalHomomorphismBySubspace( V, W );
    <linear mapping by matrix, ( Rationals^4 ) -> ( Rationals^2 )>
    gap> Image( h, [ 1, 2, 3, 4 ] );
    [ 0, 0 ]
    gap> PreImagesRepresentative( h, [ 1, 0 ] );
    [ 1, 0, 0, 0 ]

Álgebras
----------------

Si se define una multiplicación para los elementos de un espacio vectorial, y si el espacio vectorial se cierra bajo esta multiplicación, entonces se llama **Álgebra**. Por ejemplo, cada campo es un álgebra:

.. code-block:: gap
    :caption: función IsAlgebra
    :name: funcion_IsAlgebra

    gap> f:= GF(8); IsAlgebra( f );
    GF(2^3)
    true

Una de las clases más importantes de álgebras son :underline:`las subálgebras de álgebras matriciales`. En el conjunto de todas las matrices :math:`n \times n` sobre un campo :math:`\mathbb{F}`, es posible definir una multiplicación de muchas formas. Las más frecuentes son la multiplicación de matrices ordinaria y la multiplicación de Lie.

Cada matriz construida como ``[fila1, fila2, ...]`` es considerada por **GAP** como una matriz ordinaria, su multiplicación es la multiplicación de matriz asociativa ordinaria. La suma y el producto de dos matrices ordinarias son nuevamente matrices ordinarias.

El álgebra asociativa matricial completa se puede crear de la siguiente manera:

.. code-block:: gap
    :caption: El álgebra asociativa matricial completa
    :name: el_algebra_asociativa_matricial_completa
    
    gap> F:= GF( 9 );;
    gap> A:= F^[3,3];
    ( GF(3^2)^[ 3, 3 ] )

Se puede construir un álgebra a partir de generadores usando la función ``Algebra``. Toma como argumentos el campo de coeficientes y una lista de generadores. Por supuesto, el campo de coeficientes y los generadores deben encajar; si queremos construir un álgebra de matrices ordinarias, podemos tomar el campo generado por las entradas de las matrices generadoras, o un subcampo o campo de extensión.

.. code-block:: gap
    :caption: función Algebra
    :name: funcion_Algebra

    gap> m1:= [ [ 1, 1 ], [ 0, 0 ] ];; m2:= [ [ 0, 0 ], [ 0, 1 ] ];;
    gap> A:= Algebra( Rationals, [ m1, m2 ] );
    <algebra over Rationals, with 2 generators>

Una clase interesante de álgebras para la que se implementan muchos algoritmos especiales es la clase de álgebras de Lie. Surgen, por ejemplo, como álgebras de matrices cuyo producto está definido por el corchete de Lie :math:`[A, B] = A \ast B - B \ast A`, donde ∗ denota el producto matricial ordinario.

Dado que la multiplicación de objetos en GAP siempre se supone que es la operación ``\*`` (resp. El operador infijo ``*``), y dado que ya existe el producto matricial "ordinario" definido para matrices ordinarias, como se mencionó anteriormente, debemos usar un construcción de matrices que ocurren como elementos de las álgebras de Lie. ``LieObject`` puede construir tales matrices de Lie a partir de matrices ordinarias, la suma y el producto de las matrices de Lie son nuevamente matrices de Lie.

.. code-block:: gap
    :caption: función LieObject
    :name: funcion_LieObject
    
    gap> m:= LieObject( [ [ 1, 1 ], [ 1, 1 ] ] );
    LieObject( [ [ 1, 1 ], [ 1, 1 ] ] )
    gap> m*m;
    LieObject( [ [ 0, 0 ], [ 0, 0 ] ] )
    gap> IsOrdinaryMatrix( m1 ); IsOrdinaryMatrix( m );
    true
    false
    gap> IsLieMatrix( m1 ); IsLieMatrix( m );
    false
    true

Dado un campo :math:`\mathbb{F}` y una lista de mats de objetos de Lie sobre :math:`\mathbb{F}`, podemos construir el álgebra de Lie generada por mats usando la función ``Algebra``. Alternativamente, si no queremos molestarnos con la función ``LieObject``, podemos usar la función ``LieAlgebra`` que toma un campo y una lista de matrices ordinarias, y construye el álgebra de Lie generada por las matrices de Lie correspondientes. Tenga en cuenta que esto significa que las matrices ordinarias utilizadas en la llamada de ``LieAlgebra`` no están contenidas en el álgebra de Lie devuelta.

.. code-block:: gap
    :caption: función LieAlgebra
    :name: funcion_LieAlgebra

    gap> m1:= [ [ 0, 1 ], [ 0, 0 ] ];;
    gap> m2:= [ [ 0, 0 ], [ 1, 0 ] ];;
    gap> L:= LieAlgebra( Rationals, [ m1, m2 ] );
    <Lie algebra over Rationals, with 2 generators>
    gap> m1 in L;
    false

Una segunda forma de crear un álgebra es especificando una tabla de multiplicar. Sea :math:`A` un álgebra de dimensión finita con base :math:`\{x_{1},\dots , x_{n}\}`, entonces para :math:`1 \leq i`, :math:`j \leq n` el producto :math:`x_{i}x_{j}` es una combinación lineal de elementos básicos, es decir, hay :math:`c_{ij}^{k}` en el campo tal que

.. math::

    \begin{equation}
        x_{i}x_{j} = \displaystyle \sum_{k=1}^{n} c_{ij}^{k}x_{k}
    \end{equation}

No es difícil demostrar que las constantes :math:`c_{ij}^{k}` determinan la multiplicación por completo. Por lo tanto, las :math:`c_{ij}^{k}` se denominan constantes de estructura. En **GAP** podemos crear un álgebra de dimensión finita especificando una matriz de constantes de estructura.

En **GAP**, dicha tabla de constantes de estructura se representa mediante listas. La forma obvia de hacer esto sería construir una lista "tridimensional" ``T`` tal que ``T[i][j][k]`` sea igual a :math:`c_{ij}^{k}`. Pero sucede a menudo que muchas de estas constantes desaparecen. Por tanto, se utiliza una estructura más complicada para poder omitir los ceros. Una tabla de multiplicar de un álgebra :math:`n`-dimensional es una matriz ``T`` de :math:`n\times n` tal que ``T[i][j]`` describe el producto del :math:`i`-ésimo y el :math:`j`-ésimo elemento base. Este producto está codificado de la siguiente manera. La entrada ``T[i][j]`` es una lista de dos elementos.
    
    - El primero de ellos es una lista de índices :math:`k` tal que :math:`c_{ij}^{k}` es distinto de cero.
    - La segunda lista contiene las constantes correspondientes :math:`c_{ij}^{k}`. Supongamos, por ejemplo, que ``S`` es la tabla de un álgebra con base :math:`\{x_{1},\dots, x_{8}\}` y que ``S[3][7]`` es igual a ``[ [2, 4, 6], [1/2, 2, 2/3] ]``. Entonces en el álgebra tenemos la relación

.. math::

    \begin{equation}
        x_{3}x_{7} = (1/2)x_{2} + 2x_{4} + (2/3)x_{6}
    \end{equation}

Además, si ``S[6][1] = [ [ ], [ ] ]`` entonces el producto del sexto y primer elemento básico es cero.

Finalmente se agregan dos números a la tabla. El primer número puede ser :math:`1`, :math:`-1` o :math:`0`.

    - Si es :math:`1`, se sabe que la tabla es simétrica, es decir, :math:`c_{ij}^{k} = c_{ji}^{k}`.
    - Si este número es :math:`-1`, entonces se sabe que la tabla es antisimétrica (esto sucede, por ejemplo, cuando el álgebra es un álgebra de Lie).
    - El caso restante, :math:`0`, ocurre en todos los demás casos. El segundo número que se suma es el elemento cero del campo sobre el que se define el álgebra.

Las tablas de constantes de estructura vacías son creadas por la función ``EmptySCTable``, que toma una dimensión :math:`d`, un elemento cero :math:`z` y, opcionalmente, una de las cadenas "simétrica", "antisimétrica", y devuelve una tabla de constantes de estructura vacía ``T`` correspondiente a una :math:`d`-dimensional álgebra sobre un campo con elemento cero :math:`z`. Las constantes de estructura se pueden ingresar en la tabla ``T`` usando la función ``SetEntrySCTable``. Toma cuatro argumentos, a saber, ``T``, dos índices :math:`i` y :math:`j`, y una lista de la forma :math:`[c_{ij}^{k_{1}}, k_{1}, c_{ij}^{k_{2}}, k_{2},\dots]`. En esta llamada a ``SetEntrySCTable``, el producto del vector base :math:`i`-ésimo y :math:`j`-ésimo en cualquier álgebra descrita por ``T`` se establece en :math:`\displaystyle\sum_{l}c_{ij}^{k_{l}} x_{k_{l}}`. (Nótese que en la tabla vacía, este producto era cero). Si ``T`` sabe que es (anti) simétrico, entonces al mismo tiempo también el producto del :math:`j`-ésimo y el :math:`i`-ésimo vector base se establece apropiadamente.

En el siguiente ejemplo, aumentamos temporalmente el límite de longitud de línea de su valor predeterminado :math:`80` a :math:`82` para que la expresión de salida larga se ajuste a una línea.

.. code-block:: gap
    :caption: función EmptySCTable
    :name: funcion_EmptySCTable
    
    gap> SizeScreen([ 82, ]);;
    gap> T:= EmptySCTable( 2, 0, "symmetric" );
    [ [ [ [ ], [ ] ], [ [ ], [ ] ] ], [ [ [ ], [ ] ], [ [ ], [ ] ] ], 1, 0 ]
    gap> SetEntrySCTable( T, 1, 2, [1/2,1,1/3,2] ); T;
    [ [ [ [ ], [ ] ], [ [ 1, 2 ], [ 1/2, 1/3 ] ] ],
    [ [ [ 1, 2 ], [ 1/2, 1/3 ] ], [ [ ], [ ] ] ], 1, 0 ]
    gap> SizeScreen([ 80, ]);;

Si hemos definido una tabla de constantes de estructura, entonces podemos construir el álgebra correspondiente mediante ``AlgebraByStructureConstants``.

.. code-block:: gap
    :caption: función AlgebraByStructureConstant
    :name: funcion_AlgebraByStructureConstant

    gap> A:= AlgebraByStructureConstants( Rationals, T );
    <algebra of dimension 2 over Rationals>

Si sabemos que una tabla de constantes de estructura define un álgebra de Lie, entonces podemos construir el álgebra de Lie correspondiente mediante ``LieAlgebraByStructureConstants``; el álgebra devuelta por esta función sabe que es un álgebra de Lie, por lo que **GAP** no necesita verificar la identidad de Jacobi.

.. code-block:: gap
    :caption: función LieAlgebraByStructureConstants
    :name: funcion_LieAlgebraByStructureConstants
    
    gap> T:= EmptySCTable( 2, 0, "antisymmetric" );;
    gap> SetEntrySCTable( T, 1, 2, [2/3,1] );
    gap> L:= LieAlgebraByStructureConstants( Rationals, T );
    <Lie algebra of dimension 2 over Rationals>

En **GAP**, un álgebra es naturalmente un espacio vectorial. Por lo tanto, :underline:`toda la funcionalidad para espacios vectoriales también está disponible para álgebras`.

.. code-block:: gap
    :caption: función Dimension, LeftActingDomain, Basis para Álgebras
    :name: funcion_Dimension_LeftActingDomain_Basis
    
    gap> F:= GF(2);;
    gap> z:= Zero( F );; o:= One( F );;
    gap> T:= EmptySCTable( 3, z, "antisymmetric" );;
    gap> SetEntrySCTable( T, 1, 2, [ o, 1, o, 3 ] );
    gap> SetEntrySCTable( T, 1, 3, [ o, 1 ] );
    gap> SetEntrySCTable( T, 2, 3, [ o, 3 ] );
    gap> A:= AlgebraByStructureConstants( F, T );
    <algebra of dimension 3 over GF(2)>
    gap> Dimension( A );
    3
    gap> LeftActingDomain( A );
    GF(2)
    gap> Basis( A );
    CanonicalBasis( <algebra of dimension 3 over GF(2)> )

Las subálgebras y los ideales de un álgebra se pueden construir especificando un conjunto de generadores para la subálgebra o ideal. El espacio del cociente de un álgebra por un ideal es naturalmente un álgebra en sí.

**Observación**: En el siguiente ejemplo, aumentamos temporalmente el límite de longitud de línea de su valor predeterminado :math:`80` a :math:`81` para que la expresión de salida larga se ajuste a una línea.

.. code-block:: gap
    :caption: subálgebras e ideales
    :name: subalgebras_e_ideales

    gap> m:= [ [ 1, 2, 3 ], [ 0, 1, 6 ], [ 0, 0, 1 ] ];;
    gap> A:= Algebra( Rationals, [ m ] );;
    gap> subA:= Subalgebra( A, [ m-m^2 ] );
    <algebra over Rationals, with 1 generators>
    gap> Dimension( subA );
    2
    gap> SizeScreen([ 81, ]);;
    gap> idA:= Ideal( A, [ m-m^3 ] );
    <two-sided ideal in <algebra of dimension 3 over Rationals>, (1 generators)>
    gap> SizeScreen([ 80, ]);;
    gap> Dimension( idA );
    2
    gap> B:= A/idA;
    <algebra of dimension 1 over Rationals>

La llamada ``B:= A/idA;`` crea un nuevo álgebra que no "sabe" acerca de su conexión con :math:`A`. Si queremos conectar un álgebra con su factor a través de un homomorfismo, primero tenemos que crear el homomorfismo (``NaturalHomomorphismByIdeal``). Después de esto creamos el álgebra de factores a partir del homomorfismo mediante la función ``ImagesSource``. En el siguiente ejemplo dividimos un álgebra :math:`A` por su radical y elevamos los idempotentes centrales del factor al álgebra original :math:`A`.

.. code-block:: gap
    :caption: función NaturalHomomorphismByIdeal
    :name: funcion_NaturalHomomorphismByIdeal

    gap> m1:=[[1,0,0],[0,2,0],[0,0,3]];;
    gap> m2:=[[0,1,0],[0,0,2],[0,0,0]];;
    gap> A:= Algebra( Rationals, [ m1, m2 ] );;
    gap> Dimension( A );
    6
    gap> R:= RadicalOfAlgebra( A );
    <algebra of dimension 3 over Rationals>
    gap> h:= NaturalHomomorphismByIdeal( A, R );
    <linear mapping by matrix, <algebra of dimension
    6 over Rationals> -> <algebra of dimension 3 over Rationals>>
    gap> AmodR:= ImagesSource( h );
    <algebra of dimension 3 over Rationals>
    gap> id:= CentralIdempotentsOfAlgebra( AmodR );
    [ v.3, v.2+(-3)*v.3, v.1+(-2)*v.2+(3)*v.3 ]
    gap> PreImagesRepresentative( h, id[1] );
    [ [ 0, 0, 0 ], [ 0, 0, 0 ], [ 0, 0, 1 ] ]
    gap> PreImagesRepresentative( h, id[2] );
    [ [ 0, 0, 0 ], [ 0, 1, 0 ], [ 0, 0, 0 ] ]
    gap> PreImagesRepresentative( h, id[3] );
    [ [ 1, 0, 0 ], [ 0, 0, 0 ], [ 0, 0, 0 ] ]

Las tablas de constantes de estructura para las álgebras de Lie simples están presentes en **GAP**. Se pueden construir usando la función ``SimpleLieAlgebra``. Las álgebras de Lie construidas por esta función vienen con un sistema de raíces adjunto.

.. code-block:: gap
    :caption: función SimpleLieAlgebra
    :name: funcion_SimpleLieAlgebra

    gap> L:= SimpleLieAlgebra( "G", 2, Rationals );
    <Lie algebra of dimension 14 over Rationals>
    gap> R:= RootSystem( L );
    <root system of rank 2>
    gap> PositiveRoots( R );
    [ [ 2, -1 ], [ -3, 2 ], [ -1, 1 ], [ 1, 0 ], [ 3, -1 ], [ 0, 1 ] ]
    gap> CartanMatrix( R );
    [ [ 2, -1 ], [ -3, 2 ] ]

Otro ejemplo de álgebras lo proporcionan las **álgebras de cuaterniones**. Definimos un álgebra de cuaternión sobre un campo de extensión de los racionales, es decir, el campo generado por :math:`\sqrt{5}`. (El número ``EB( 5 )`` es igual a :math:`1/2 (−1 + \sqrt{5})`. El campo se imprime como ``NF( 5, [1, 4] )``.)

.. code-block:: gap
    :caption: función QuaternionAlgebra
    :name: funcion_QuaternionAlgebra

    gap> b5:= EB(5);
    E(5)+E(5)^4
    gap> q:= QuaternionAlgebra( FieldByGenerators( [ b5 ] ) );
    <algebra-with-one of dimension 4 over NF(5,[ 1, 4 ])>
    gap> gens:= GeneratorsOfAlgebra( q );
    [ e, i, j, k ]
    gap> e:= gens[1];; i:= gens[2];; j:= gens[3];; k:= gens[4];;
    gap> IsAssociative( q );
    true
    gap> IsCommutative( q );
    false
    gap> i*j; j*i;
    k
    (-1)*k
    gap> One( q );
    e

Si el campo de coeficientes es un subcampo real de los números complejos, entonces el álgebra de cuaterniones es de hecho un anillo de división.

.. code-block:: gap
    :caption: funciones IsDivisionRing e Inverse
    :name: funcion_IsDivisionRing_e_Inverse

    gap> IsDivisionRing( q );
    true
    gap> Inverse( e+i+j );
    (1/3)*e+(-1/3)*i+(-1/3)*j

Entonces **GAP** conoce este hecho. Como en cualquier anillo, podemos mirar grupos de unidades. 

.. note::
    
    La función ``StarCyc`` que se usa a continuación calcula el conjugado algebraico único de un elemento en un subcampo cuadrático de un campo ciclotómico.

.. existe la palabra ciclotómico? 

.. code-block:: gap
    :caption: función StarCyc
    :name: funcion_StarCyc
    
    gap> c5:= StarCyc( b5 );
    E(5)^2+E(5)^3
    gap> g1:= 1/2*( b5*e + i - c5*j );
    (1/2*E(5)+1/2*E(5)^4)*e+(1/2)*i+(-1/2*E(5)^2-1/2*E(5)^3)*j
    gap> Order( g1 );
    5
    gap> g2:= 1/2*( -c5*e + i + b5*k );
    (-1/2*E(5)^2-1/2*E(5)^3)*e+(1/2)*i+(1/2*E(5)+1/2*E(5)^4)*k
    gap> Order( g2 );
    10
    gap> g:=Group( g1, g2 );;
    #I default ‘IsGeneratorsOfMagmaWithInverses’ method returns ‘true’ for
    [ (1/2*E(5)+1/2*E(5)^4)*e+(1/2)*i+(-1/2*E(5)^2-1/2*E(5)^3)*j,
    (-1/2*E(5)^2-1/2*E(5)^3)*e+(1/2)*i+(1/2*E(5)+1/2*E(5)^4)*k ]
    gap> Size( g );
    120
    gap> IsPerfect( g );
    true

Dado que solo hay un grupo perfecto de orden :math:`120`, hasta el isomorfismo, vemos que el grupo ``g`` es isomorfo a :math:`SL_{2}(5)`. Como es habitual, se puede construir una representación de permutación del grupo utilizando una acción adecuada del grupo.

.. code-block:: gap
    :caption: función RightCosets
    :name: funcion_RightCosets
    
    gap> cos:= RightCosets( g, Subgroup( g, [ g1 ] ) );;
    gap> Length( cos );
    24
    gap> hom:= ActionHomomorphism( g, cos, OnRight );;
    gap> im:= Image( hom );
    Group([ (2,3,5,9,15)(4,7,12,8,14)(10,17,23,20,24)(11,19,22,16,13),
            (1,2,4,8,3,6,11,20,17,19)(5,10,18,7,13,22,12,21,24,15)(9,16)(14,23) ])
    gap> Size( im );
    120

Para obtener una representación matricial de ``g`` o de todo el álgebra ``q``, debemos especificar una base del espacio vectorial sobre el que actúa el álgebra y calcular la acción lineal de los elementos ``w.r.t.`` esta base.

.. code-block:: gap
    :caption: función OperationAlgebraHomomorphism
    :name: funcion_OperationAlgebraHomomorphism

    gap> bas:= CanonicalBasis( q );;
    gap> BasisVectors( bas );
    [ e, i, j, k ]
    gap> op:= OperationAlgebraHomomorphism( q, bas, OnRight );
    <op. hom. AlgebraWithOne( NF(5,[ 1, 4 ]),
    [ e, i, j, k ] ) -> matrices of dim. 4>
    gap> ImagesRepresentative( op, e );
    [ [ 1, 0, 0, 0 ], [ 0, 1, 0, 0 ], [ 0, 0, 1, 0 ], [ 0, 0, 0, 1 ] ]
    gap> ImagesRepresentative( op, i );
    [ [ 0, 1, 0, 0 ], [ -1, 0, 0, 0 ], [ 0, 0, 0, -1 ], [ 0, 0, 1, 0 ] ]
    gap> ImagesRepresentative( op, g1 );
    [ [ 1/2*E(5)+1/2*E(5)^4, 1/2, -1/2*E(5)^2-1/2*E(5)^3, 0 ],
      [ -1/2, 1/2*E(5)+1/2*E(5)^4, 0, -1/2*E(5)^2-1/2*E(5)^3 ],
      [ 1/2*E(5)^2+1/2*E(5)^3, 0, 1/2*E(5)+1/2*E(5)^4, -1/2 ],
      [ 0, 1/2*E(5)^2+1/2*E(5)^3, 1/2, 1/2*E(5)+1/2*E(5)^4 ] ]