.. _writing-tests:

Writing tests
=============

.. _asserters-anchor:

Asserters
---------

.. _variable-anchor:

variable
~~~~~~~~

The base asserter for all variables. It contains all the tests you would need for any kind of variable.

.. _variable-is-callable:

isCallable
^^^^^^^^^^

``isCallable`` checks that the variable call be called like a function

.. code-block:: php

   <?php
   $f = function() {
       // code
   };

   $this
       ->variable($f)
           ->isCallable()  // passes

       ->variable('\Vendor\Project\foobar')
           ->isCallable()

       ->variable(array('\Vendor\Project\Foo', 'bar'))
           ->isCallable()

       ->variable('\Vendor\Project\Foo::bar')
           ->isCallable()
   ;

.. _variable-is-equal-to:

isEqualTo
^^^^^^^^^

``isEqualTo`` checks that the variable is equal to an expected value

.. code-block:: php

   <?php
   $a = 'a';

   $this
       ->variable($a)
           ->isEqualTo('a')    // passes
   ;

.. warning::
   ``isEqualTo`` does not check the variable type. If you want to also check the type, use :ref:`isIdenticalTo <variable-is-identical-to>`.


.. _variable-is-identical-to:

isIdenticalTo
^^^^^^^^^^^^^

``isIdenticalTo`` checks that the variable is equal to the expected value, and also checks that the types are the same. With objects, ``isIdenticalTo`` checks that both values are references to the same object

.. code-block:: php

   <?php
   $a = '1';

   $this
       ->variable($a)
           ->isIdenticalTo(1)          // fails
   ;

   $stdClass1 = new \StdClass();
   $stdClass2 = new \StdClass();
   $stdClass3 = $stdClass1;

   $this
       ->variable($stdClass1)
           ->isIdenticalTo(stdClass3)  // passes
           ->isIdenticalTo(stdClass2)  // fails
   ;

.. warning::
   ``isIdenticalTo`` checks the variable type. If you do not want to check the type, use :ref:`isEqualTo <variable-is-equal-to>`.


.. _variable-is-not-callable:

isNotCallable
^^^^^^^^^^^^^

``isNotCallable`` checks the variable cannot be called like a function.

.. code-block:: php

   <?php
   $f = function() {
       // code
   };
   $int    = 1;
   $string = 'nonExistingMethod';

   $this
       ->variable($f)
           ->isNotCallable()   // fails

       ->variable($int)
           ->isNotCallable()   // passes

       ->variable($string)
           ->isNotCallable()   // passes

       ->variable(new StdClass)
           ->isNotCallable()   // passes
   ;

.. _variable-is-not-equal-to:

isNotEqualTo
^^^^^^^^^^^^

``isNotEqualTo`` checks that the variable is not the same as the given value

.. code-block:: php

   <?php
   $a       = 'a';
   $aString = '1';

   $this
       ->variable($a)
           ->isNotEqualTo('b')     // passes
           ->isNotEqualTo('a')     // fails

       ->variable($aString)
           ->isNotEqualTo($1)      // fails
   ;

.. warning::
   ``isNotEqualTo`` does not check the variable type. If you also want to check the type, use :ref:`isNotIdenticalTo <variable-is-not-identical-to>`.


.. _variable-is-not-identical-to:

isNotIdenticalTo
^^^^^^^^^^^^^^^^

``isNotIdenticalTo`` checks that the variable has neither the same type nor the same value as the given value

With objects, ``isNotIdenticalTo`` checks that both values do not reference the same instance.

.. code-block:: php

   <?php
   $a = '1';

   $this
       ->variable($a)
           ->isNotIdenticalTo(1)           // passes
   ;

   $stdClass1 = new \StdClass();
   $stdClass2 = new \StdClass();
   $stdClass3 = $stdClass1;

   $this
       ->variable($stdClass1)
           ->isNotIdenticalTo(stdClass2)   // passes
           ->isNotIdenticalTo(stdClass3)   // fails
   ;

.. warning::
   ``isNotIdenticalTo`` checks the variable type. If you do not want to check the variable type, use :ref:`isNotEqualTo <variable-is-not-equal-to>`.


.. _is-null:

isNull
^^^^^^

``isNull`` checks that the variable is null.

.. code-block:: php

   <?php
   $emptyString = '';
   $null        = null;

   $this
       ->variable($emptyString)
           ->isNull()              // fails
                                   // (it is empty but not null)

       ->variable($null)
           ->isNull()              // passes
   ;

.. _is-not-null:

isNotNull
^^^^^^^^^

``isNotNull`` checks that the variable is not null.

.. code-block:: php

   <?php
   $emptyString = '';
   $null        = null;

   $this
       ->variable($emptyString)
           ->isNotNull()           // passe (it is empty but not null)

       ->variable($null)
           ->isNotNull()           // fails
   ;



.. _boolean-anchor:

boolean
~~~~~~~

This is the asserter for booleans.

The check will fail if you pass a non boolean value.

.. note::
   ``null`` is not a boolean. You can read the PHP manual to know what `is_bool <http://php.net/is_bool>`_ considers a boolean or not.


.. _boolean-is-equal-to:

isEqualTo
^^^^^^^^^

.. hint::
   ``isEqualTo`` is an inherited method from the ``variable`` asserter.
   For more information, you can read the :ref:`variable::isEqualTo <variable-is-equal-to>` documentation


.. _is-false:

isFalse
^^^^^^^

``isFalse`` checks that the boolean is strictly equal to ``false``.

.. code-block:: php

   <?php
   $true  = true;
   $false = false;

   $this
       ->boolean($true)
           ->isFalse()     // fails

       ->boolean($false)
           ->isFalse()     // passes
   ;

.. _boolean-is-identical-to:

isIdenticalTo
^^^^^^^^^^^^^

.. hint::
   ``isIdenticalTo`` is an inherited method from the ``variable`` asserter.
   For more information, you can read the :ref:`variable::isIdenticalTo <variable-is-identical-to>` documentation


.. _boolean-is-not-equal-to:

isNotEqualTo
^^^^^^^^^^^^

.. hint::
   ``isNotEqualTo`` is an inherited method from the ``variable`` asserter.
   For more information, you can read the :ref:`variable::isNotEqualTo <variable-is-not-equal-to>` documentation


.. _boolean-is-not-identical-to:

isNotIdenticalTo
^^^^^^^^^^^^^^^^

.. hint::
   ``isNotIdenticalTo`` is an inherited method from the ``variable`` asserter.
   For more information, you can read the :ref:`variable::isNotIdenticalTo <variable-is-not-identical-to>` documentation


.. _is-true:

isTrue
^^^^^^

``isTrue`` checks that the boolean is strictly equal to ``true``.

.. code-block:: php

   <?php
   $true  = true;
   $false = false;

   $this
       ->boolean($true)
           ->isTrue()      // passes

       ->boolean($false)
           ->isTrue()      // fails
   ;



.. _integer-anchor:

integer
~~~~~~~

This is the asserter for integers.

The check will fail if pass a non integer value.

.. note::
   ``null`` is not an integer. You can read the PHP manual to know what `is_int <http://php.net/is_int>`_ considers an integer or not.


.. _integer-is-equal-to:

isEqualTo
^^^^^^^^^

.. hint::
   ``isEqualTo`` is an inherited method from the ``variable`` asserter.
   For more information, you can read the :ref:`variable::isEqualTo <variable-is-equal-to>` documentation


.. _integer-is-greater-than:

isGreaterThan
^^^^^^^^^^^^^

``isGreaterThan`` checks that the integer is strictly greater then the given value.

.. code-block:: php

   <?php
   $zero = 0;

   $this
       ->integer($zero)
           ->isGreaterThan(-1)     // passes
           ->isGreaterThan('-1')   // fails because "-1"
                                   // is not an integer (string)
           ->isGreaterThan(0)      // fails
   ;

.. _integer-is-greater-than-or-equal-to:

isGreaterThanOrEqualTo
^^^^^^^^^^^^^^^^^^^^^^

``isGreaterThanOrEqualTo`` checks that the integer is greater or equal to the given value.

.. code-block:: php

   <?php
   $zero = 0;

   $this
       ->integer($zero)
           ->isGreaterThanOrEqualTo(-1)    // passes
           ->isGreaterThanOrEqualTo(0)     // passes
           ->isGreaterThanOrEqualTo('-1')  // fails because "-1"
                                           // is not an integer (string)
   ;

.. _integer-is-identical-to:

isIdenticalTo
^^^^^^^^^^^^^

.. hint::
   ``isIdenticalTo`` is an inherited method from the ``variable`` asserter.
   For more information, you can read the :ref:`variable::isIdenticalTo <variable-is-identical-to>` documentation


.. _integer-is-less-than:

isLessThan
^^^^^^^^^^

``isLessThan`` checks that the integer is strictly lower than the given value.

.. code-block:: php

   <?php
   $zero = 0;

   $this
       ->integer($zero)
           ->isLessThan(10)    // passes
           ->isLessThan('10')  // fails because "10" is not an integer (string)
           ->isLessThan(0)     // fails
   ;

.. _integer-is-less-than-or-equal-to:

isLessThanOrEqualTo
^^^^^^^^^^^^^^^^^^^

``isLessThanOrEqualTo`` checks that the integer is less or equal than the given value.

.. code-block:: php

   <?php
   $zero = 0;

   $this
       ->integer($zero)
           ->isLessThanOrEqualTo(10)       // passes
           ->isLessThanOrEqualTo(0)        // passes
           ->isLessThanOrEqualTo('10')     // fails because "10"
                                           // is not an integer
   ;

.. _integer-is-not-equal-to:

isNotEqualTo
^^^^^^^^^^^^

.. hint::
   ``isNotEqualTo`` is an inherited method from the ``variable`` asserter.
   For more information, you can read the :ref:`variable::isNotEqualTo <variable-is-not-equal-to>` documentation


.. _integer-is-not-identical-to:

isNotIdenticalTo
^^^^^^^^^^^^^^^^

.. hint::
   ``isNotIdenticalTo`` is an inherited method from the ``variable`` asserter.
   For more information, you can read the :ref:`variable::isNotIdenticalTo <variable-is-not-identical-to>` documentation


.. _integer-is-zero:

isZero
^^^^^^

``isZero`` checks that the integer is equal to 0.

.. code-block:: php

   <?php
   $zero    = 0;
   $notZero = -1;

   $this
       ->integer($zero)
           ->isZero()          // passes

       ->integer($notZero)
           ->isZero()          // fails
   ;

.. note::
   ``isZero`` is equivalent to ``isEqualTo(0)``.




.. _float-anchor:

float
~~~~~

This is the asserter for floats.

The check will fail if you pass a non float value.

.. note::
   ``null`` is not a float. Read the PHP manual to know what `is_float <http://php.net/is_float>`_ considers a float or not.


.. _float-is-equal-to:

isEqualTo
^^^^^^^^^

.. hint::
   ``isEqualTo`` is an inherited method from the ``variable`` asserter.
   For more information, you can read the :ref:`variable::isEqualTo <variable-is-equal-to>` documentation


.. _float-is-greater-than:

isGreaterThan
^^^^^^^^^^^^^

.. hint::
   ``isGreaterThan`` is an inherited method from the ``integer`` asserter.
   For more information, you can read the :ref:`integer::isGreaterThan <integer-is-greater-than>` documentation


.. _float-is-greater-than-or-equal-to:

isGreaterThanOrEqualTo
^^^^^^^^^^^^^^^^^^^^^^

.. hint::
   ``isGreaterThanOrEqualTo`` is an inherited method from the ``integer`` asserter.
   For more information, you can read the :ref:`integer::isGreaterThanOrEqualTo <integer-is-greater-than-or-equal-to>` documentation


.. _float-is-identical-to:

isIdenticalTo
^^^^^^^^^^^^^

.. hint::
   ``isIdenticalTo`` is an inherited method from the ``variable`` asserter.
   For more information, you can read the :ref:`variable::isIdenticalTo <variable-is-identical-to>` documentation


.. _float-is-less-than:

isLessThan
^^^^^^^^^^

.. hint::
   ``isLessThan`` is an inherited method from the ``integer`` asserter.
   For more information, you can read the :ref:`integer::isLessThan <integer-is-less-than>` documentation


.. _float-is-less-than-or-equal-to:

isLessThanOrEqualTo
^^^^^^^^^^^^^^^^^^^

.. hint::
   ``isLessThanOrEqualTo`` is an inherited method from the ``integer`` asserter.
   For more information, you can read the :ref:`integer::isLessThanOrEqualoo <integer-is-less-than-or-equal-to>` documentation


.. _is-nearly-equal-to:

isNearlyEqualTo
^^^^^^^^^^^^^^^

``isNearlyEqualTo`` checks that the float is approximately equal to the given value.

Computers handle floats in a way that makes precise comparisons impossible without using advanced tools. Try for example the following command:

.. code-block:: shell

   $ php -r 'var_dump(1 - 0.97 === 0.03);'
   bool(false)

The result should be ``true`` though.

.. note::
   For more information about this behavior, read `the PHP manual <http://php.net/types.float>`_


This method tries to avoid this issue.

.. code-block:: php

   <?php
   $float = 1 - 0.97;

   $this
       ->float($float)
           ->isNearlyEqualTo(0.03) // passes
           ->isEqualTo(0.03)       // fails
   ;

.. note::
   For more information about the algorithm used, read the `floating point guide <http://www.floating-point-gui.de/errors/comparison/>`_.


.. _float-is-not-equal-to:

isNotEqualTo
^^^^^^^^^^^^

.. hint::
   ``isNotEqualTo`` is an inherited method from the ``variable`` asserter.
   For more information, you can read the :ref:`variable::isNotEqualTo <variable-is-not-equal-to>` documentation


.. _float-is-not-identical-to:

isNotIdenticalTo
^^^^^^^^^^^^^^^^

.. hint::
   ``isNotIdenticalTo`` is an inherited method from the ``variable`` asserter.
   For more information, you can read the :ref:`variable::isNotIdenticalTo <variable-is-not-identical-to>` documentation


.. _float-is-zero:

isZero
^^^^^^

.. hint::
   ``isZero`` is an inherited method from the ``integer`` asserter.
   For more information, you can read the :ref:`integer::isZero <integer-is-zero>` documentation




.. _size-of:

sizeOf
~~~~~~

This is the asserter for array sizes and objects that implements the ``Countable`` interface.

.. code-block:: php

   <?php
   $array           = array(1, 2, 3);
   $countableObject = new GlobIterator('*');

   $this
       ->sizeOf($array)
           ->isEqualTo(3)

       ->sizeOf($countableObject)
           ->isGreaterThan(0)
   ;

.. _size-of-is-equal-to:

isEqualTo
^^^^^^^^^

.. hint::
   ``isEqualTo`` is an inherited method from the ``variable`` asserter.
   For more information, you can read the :ref:`variable::isEqualTo <variable-is-equal-to>` documentation


.. _size-of-is-greater-than:

isGreaterThan
^^^^^^^^^^^^^

.. hint::
   ``isGreaterThan`` is an inherited method from the ``integer`` asserter.
   For more information, you can read the :ref:`integer::isGreaterThan <integer-is-greater-than>` documentation


.. _size-of-is-greater-than-or-equal-to:

isGreaterThanOrEqualTo
^^^^^^^^^^^^^^^^^^^^^^

.. hint::
   ``isGreaterThanOrEqualTo`` is an inherited method from the ``integer`` asserter.
   For more information, you can read the :ref:`integer::isGreaterThanOrEqualTo <integer-is-greater-than-or-equal-to>` documentation


.. _size-of-is-identical-to:

isIdenticalTo
^^^^^^^^^^^^^

.. hint::
   ``isIdenticalTo`` is an inherited method from the ``variable`` asserter.
   For more information, you can read the :ref:`variable::isIdenticalTo <variable-is-identical-to>` documentation


.. _size-of-is-less-than:

isLessThan
^^^^^^^^^^

.. hint::
   ``isLessThan`` is an inherited method from the ``integer`` asserter.
   For more information, you can read the :ref:`integer::isLessThan <integer-is-less-than>` documentation


.. _size-of-is-less-than-or-equal-to:

isLessThanOrEqualTo
^^^^^^^^^^^^^^^^^^^

.. hint::
   ``isLessThanOrEqualTo`` is an inherited method from the ``integer`` asserter.
   For more information, you can read the :ref:`integer::isLessThanOrEqualoo <integer-is-less-than-or-equal-to>` documentation


.. _size-of-is-not-equal-to:

isNotEqualTo
^^^^^^^^^^^^

.. hint::
   ``isNotEqualTo`` is an inherited method from the ``variable`` asserter.
   For more information, you can read the :ref:`variable::isNotEqualTo <variable-is-not-equal-to>` documentation


.. _size-of-is-not-identical-to:

isNotIdenticalTo
^^^^^^^^^^^^^^^^

.. hint::
   ``isNotIdenticalTo`` is an inherited method from the ``variable`` asserter.
   For more information, you can read the :ref:`variable::isNotIdenticalTo <variable-is-not-identical-to>` documentation


.. _size-of-is-zero:

isZero
^^^^^^

.. hint::
   ``isZero`` is an inherited method from the ``integer`` asserter.
   For more information, you can read the :ref:`integer::isZero <integer-is-zero>` documentation




.. _object-anchor:

object
~~~~~~

This is the asserter for objects.

The check will fail if you pass a non object.

.. note::
   ``null`` is not an object. Read the PHP manual to know what `is_object <http://php.net/is_object>`_ considers an object or not.


.. _object-has-size:

hasSize
^^^^^^^

``hasSize`` checks the size of objects that implement the ``Countable`` interface.

.. code-block:: php

   <?php
   $countableObject = new GlobIterator('*');

   $this
       ->object($countableObject)
           ->hasSize(3)
   ;

.. _object-is-callable:

isCallable
^^^^^^^^^^

.. code-block:: php

   <?php
   class foo
   {
       public function __invoke()
       {
           // code
       }
   }

   $this
       ->object(new foo)
           ->isCallable()  // passes

       ->object(new StdClass)
           ->isCallable()  // fails
   ;

.. note::
   To be ``callable``, your objects must be instantiated from classes that implement the `__invoke <http://www.php.net/manual/fr/language.oop5.magic.php#object.invoke>`_ magic method.


.. hint::
   ``isCallable`` is an inherited method from the ``variable`` asserter.
   For more information, you can read the :ref:`variable::isCallable <variable-is-callable>` documentation


.. _object-is-clone-of:

isCloneOf
^^^^^^^^^

``isCloneOf`` checks that the object is the clone of the given object, that is to say the objects are equal, but are not the same instance.

.. code-block:: php

   <?php
   $object1 = new \StdClass;
   $object2 = new \StdClass;
   $object3 = clone($object1);
   $object4 = new \StdClass;
   $object4->foo = 'bar';

   $this
       ->object($object1)
           ->isCloneOf($object2)   // passes
           ->isCloneOf($object3)   // passes
           ->isCloneOf($object4)   // fails
   ;

.. note::
   For more information on object comparison, read `the PHP manual <http://php.net/language.oop5.object-comparison>`_.


.. _object-is-empty:

isEmpty
^^^^^^^

``isEmpty`` checks that the size of an object that implements the ``Countable`` interface is equal to 0.

.. code-block:: php

   <?php
   $countableObject = new GlobIterator('atoum.php');

   $this
       ->object($countableObject)
           ->isEmpty()
   ;

.. note::
   ``isEmpty`` is equivalent to ``hasSize(0)``.


.. _object-is-equal-to:

isEqualTo
^^^^^^^^^

``isEqualTo`` checks that the object is equal to the given object.
Two objects are considered equal when they have the same attributes and attributes values, and that they are instances of the same class.

.. note::
   For more information on object comparison, read `the PHP manual <http://php.net/language.oop5.object-comparison>`_.


.. hint::
   ``isEqualTo`` is an inherited method from the ``variable`` asserter.
   For more information, you can read the :ref:`variable::isEqualTo <variable-is-equal-to>` documentation


.. _object-is-identical-to:

isIdenticalTo
^^^^^^^^^^^^^

``isIdenticalTo`` checks that the objects are identical.
Two objects are considered identical when they are references to the same instance of the same class.

.. note::
   For more information on object comparison, read `the PHP manual <http://php.net/language.oop5.object-comparison>`_.


.. hint::
   ``isIdenticalTo`` is an inherited method from the ``variable`` asserter.
   For more information, you can read the :ref:`variable::isIdenticalTo <variable-is-identical-to>` documentation


.. _object-is-instance-of:

isInstanceOf
^^^^^^^^^^^^
``isInstanceOf`` checks that an object is :

* an instance of the given class,
* a subclass of the given class (abstract or not),
* an instance of a class that implements the given interface.

.. code-block:: php

   <?php
   $object = new \StdClass();

   $this
       ->object($object)
           ->isInstanceOf('\StdClass')     // passes
           ->isInstanceOf('\Iterator')     // fails
   ;


   interface FooInterface
   {
       public function foo();
   }

   class FooClass implements FooInterface
   {
       public function foo()
       {
           echo "foo";
       }
   }

   class BarClass extends FooClass
   {
   }

   $foo = new FooClass;
   $bar = new BarClass;

   $this
       ->object($foo)
           ->isInstanceOf('\FooClass')     // passes
           ->isInstanceOf('\FooInterface') // passes
           ->isInstanceOf('\BarClass')     // fails
           ->isInstanceOf('\StdClass')     // fails

       ->object($bar)
           ->isInstanceOf('\FooClass')     // passes
           ->isInstanceOf('\FooInterface') // passes
           ->isInstanceOf('\BarClass')     // passes
           ->isInstanceOf('\StdClass')     // fails
   ;

.. note::
   Classes and interfaces names have to be absolute, because namespace import are not taken into account.

.. hint::
   Note that on PHP ``>= 5.5`` you can use the ``class`` keyword to get fully qualified class names, for example ``$this->object($foo)->isInstanceOf(FooClass::class)``.


.. _object-is-not-callable:

isNotCallable
^^^^^^^^^^^^^

.. code-block:: php

   <?php
   class foo
   {
       public function __invoke()
       {
           // code
       }
   }

   $this
       ->variable(new foo)
           ->isNotCallable()   // fails

       ->variable(new StdClass)
           ->isNotCallable()   // passes
   ;

.. hint::
   ``isNotCallable`` is an inherited method from the ``variable`` asserter.
   For more information, you can read the :ref:`variable::isNotCallable <variable-is-not-callable>` documentation


.. _object-is-not-equal-to:

isNotEqualTo
^^^^^^^^^^^^

``isEqualTo`` checks that the object is not equal to the given object.
Two objects are considered equal when they have the same attributes and attributes values, and that they are instances of the same class.

.. note::
   For more information on object comparison, read `the PHP manual <http://php.net/language.oop5.object-comparison>`_.


.. hint::
   ``isNotEqualTo`` is an inherited method from the ``variable`` asserter.
   For more information, you can read the :ref:`variable::isNotEqualTo <variable-is-not-equal-to>` documentation


.. _object-is-not-identical-to:

isNotIdenticalTo
^^^^^^^^^^^^^^^^

``isIdenticalTo`` checks that the object is not identical to the given object.
Two objects are considered identical when they are references to the same instance of the same class.

.. note::
   For more information on object comparison, read `the PHP manual <http://php.net/language.oop5.object-comparison>`_.


.. hint::
   ``isNotIdenticalTo`` is an inherited method from the ``variable`` asserter.
   For more information, you can read the :ref:`variable::isNotIdenticalTo <variable-is-not-identical-to>` documentation


.. _date-interval:

dateInterval
~~~~~~~~~~~~

This is the asserter for the `DateInterval <http://php.net/dateinterval>`_ object.

The check will fail if you pass a value that is not a ``DateInterval`` instance (or an instance of a class that extends it).

.. _date-interval-is-clone-of:

isCloneOf
^^^^^^^^^

.. hint::
   ``isCloneOf`` is an inherited method from the ``object`` asserter.
   For more information, you can read the :ref:`object::isCloneOf <object-is-clone-of>` documentation


.. _date-interval-is-equal-to:

isEqualTo
^^^^^^^^^

``isEqualTo`` checks that the duration of the ``DateInterval`` object is equal to the duration of the given ``DateInterval`` object.

.. code-block:: php

   <?php
   $di = new DateInterval('P1D');

   $this
       ->dateInterval($di)
           ->isEqualTo(                // passes
               new DateInterval('P1D')
           )
           ->isEqualTo(                // fails
               new DateInterval('P2D')
           )
   ;

.. _date-interval-is-greater-than:

isGreaterThan
^^^^^^^^^^^^^

``isGreaterThan`` checks that the duration of the ``DateInterval`` object is greater than the duration of the given ``DateInterval`` object.

.. code-block:: php

   <?php
   $di = new DateInterval('P2D');

   $this
       ->dateInterval($di)
           ->isGreaterThan(            // passes
               new DateInterval('P1D')
           )
           ->isGreaterThan(            // fails
               new DateInterval('P2D')
           )
   ;

.. _date-interval-is-greater-than-or-equal-to:

isGreaterThanOrEqualTo
^^^^^^^^^^^^^^^^^^^^^^

``isGreaterThanOrEqualTo`` checks that the duration of the ``DateInterval`` object is greater or equal to the duration of the given ``DateInterval`` object.

.. code-block:: php

   <?php
   $di = new DateInterval('P2D');

   $this
       ->dateInterval($di)
           ->isGreaterThanOrEqualTo(   // passes
               new DateInterval('P1D')
           )
           ->isGreaterThanOrEqualTo(   // passes
               new DateInterval('P2D')
           )
           ->isGreaterThanOrEqualTo(   // fails
               new DateInterval('P3D')
           )
   ;

.. _date-interval-is-identical-to:

isIdenticalTo
^^^^^^^^^^^^^

.. hint::
   ``isIdenticalTo`` is an inherited method from the ``object`` asserter.
   For more information, you can read the :ref:`object::isIdenticalTo <object-is-identical-to>` documentation


.. _date-interval-is-instance-of:

isInstanceOf
^^^^^^^^^^^^

.. hint::
   ``isInstanceOf`` is an inherited method from the ``object`` asserter.
   For more information, you can read the :ref:`object::isInstanceOf <object-is-instance-of>` documentation


.. _date-interval-is-less-than:

isLessThan
^^^^^^^^^^

``isLessThan`` checks that the duration of the ``DateInterval`` object is less than the duration of the given ``DateInterval`` object.

.. code-block:: php

   <?php
   $di = new DateInterval('P1D');

   $this
       ->dateInterval($di)
           ->isLessThan(               // passes
               new DateInterval('P2D')
           )
           ->isLessThan(               // fails
               new DateInterval('P1D')
           )
   ;

.. _date-interval-is-less-than-or-equal-to:

isLessThanOrEqualTo
^^^^^^^^^^^^^^^^^^^

``isLessThanOrEqualTo`` checks that the duration of the ``DateInterval`` object is less or equal than the duration of the given ``DateInterval`` object.

.. code-block:: php

   <?php
   $di = new DateInterval('P2D');

   $this
       ->dateInterval($di)
           ->isLessThanOrEqualTo(      // passes
               new DateInterval('P3D')
           )
           ->isLessThanOrEqualTo(      // passes
               new DateInterval('P2D')
           )
           ->isLessThanOrEqualTo(      // fails
               new DateInterval('P1D')
           )
   ;

.. _date-interval-is-not-equal-to:

isNotEqualTo
^^^^^^^^^^^^

.. hint::
   ``isNotEqualTo`` is an inherited method from the ``object`` asserter.
   For more information, you can read the :ref:`object::isNotEqualTo <object-is-not-equal-to>` documentation


.. _date-interval-is-not-identical-to:

isNotIdenticalTo
^^^^^^^^^^^^^^^^

.. hint::
   ``isNotIdenticalTo`` is an inherited method from the ``object`` asserter.
   For more information, you can read the :ref:`object::isNotIdenticalTo <object-is-not-identical-to>` documentation


.. _date-interval-is-zero:

isZero
^^^^^^

``isZero`` checks that the duration of the ``DateInterval`` is equal to 0.

.. code-block:: php

   <?php
   $di1 = new DateInterval('P0D');
   $di2 = new DateInterval('P1D');

   $this
       ->dateInterval($di1)
           ->isZero()      // passes
       ->dateInterval($di2)
           ->isZero()      // fails
   ;


.. _date-time:

dateTime
~~~~~~~~

This is the asserter for the `DateTime <http://php.net/datetime>`_ object.

The check will fail if you pass a value that is not an instance of ``DateTime`` (or an instance of a class that extends it).

.. _date-time-has-date:

hasDate
^^^^^^^

``hasDate`` checks the date part of the ``DateTime`` object.

.. code-block:: php

   <?php
   $dt = new DateTime('1981-02-13');

   $this
       ->dateTime($dt)
           ->hasDate('1981', '02', '13')   // passes
           ->hasDate('1981', '2',  '13')   // passes
           ->hasDate(1981,   2,    13)     // passes
   ;

.. _date-time-has-date-and-time:

hasDateAndTime
^^^^^^^^^^^^^^

``hasDateAndTime`` check the date and time of the ``DateTime`` object.

.. code-block:: php

   <?php
   $dt = new DateTime('1981-02-13 01:02:03');

   $this
       ->dateTime($dt)
           // passes
           ->hasDateAndTime('1981', '02', '13', '01', '02', '03')
           // passes
           ->hasDateAndTime('1981', '2',  '13', '1',  '2',  '3')
           // passes
           ->hasDateAndTime(1981,   2,    13,   1,    2,    3)
   ;

.. _date-time-has-day:

hasDay
^^^^^^

``hasDay`` checks the day of the ``DateTime`` object.

.. code-block:: php

   <?php
   $dt = new DateTime('1981-02-13');

   $this
       ->dateTime($dt)
           ->hasDay(13)        // passes
   ;

.. _date-time-has-hours:

hasHours
^^^^^^^^

``hasHours`` checks the hours of the ``DateTime`` object.

.. code-block:: php

   <?php
   $dt = new DateTime('01:02:03');

   $this
       ->dateTime($dt)
           ->hasHours('01')    // passes
           ->hasHours('1')     // passes
           ->hasHours(1)       // passes
   ;

.. _date-time-has-minutes:

hasMinutes
^^^^^^^^^^

``hasMinutes`` checks the minutes of the ``DateTime`` object.

.. code-block:: php

   <?php
   $dt = new DateTime('01:02:03');

   $this
       ->dateTime($dt)
           ->hasMinutes('02')  // passes
           ->hasMinutes('2')   // passes
           ->hasMinutes(2)     // passes
   ;

.. _date-time-has-month:

hasMonth
^^^^^^^^

``hasMonth`` checks the month of the ``DateTime`` object.

.. code-block:: php

   <?php
   $dt = new DateTime('1981-02-13');

   $this
       ->dateTime($dt)
           ->hasMonth(2)       // passes
   ;

.. _date-time-has-seconds:

hasSeconds
^^^^^^^^^^

``hasSeconds`` checks the seconds of the ``DateTime`` object.

.. code-block:: php

   <?php
   $dt = new DateTime('01:02:03');

   $this
       ->dateTime($dt)
           ->hasSeconds('03')    // passes
           ->hasSeconds('3')     // passes
           ->hasSeconds(3)       // passes
   ;

.. _date-time-has-time:

hasTime
^^^^^^^

``hasTime`` checks the time part of the ``DateTime`` object.

.. code-block:: php

   <?php
   $dt = new DateTime('01:02:03');

   $this
       ->dateTime($dt)
           ->hasTime('01', '02', '03')     // passes
           ->hasTime('1',  '2',  '3')      // passes
           ->hasTime(1,    2,    3)        // passes
   ;

.. _date-time-has-timezone:

hasTimezone
^^^^^^^^^^^

``hasTimezone`` checks the timezone of the ``DateTime`` object.

.. code-block:: php

   <?php
   $dt = new DateTime();

   $this
       ->dateTime($dt)
           ->hasTimezone('Europe/Paris')
   ;

.. _date-time-has-year:

hasYear
^^^^^^^

``hasYear`` checks the year of the ``DateTime`` object.

.. code-block:: php

   <?php
   $dt = new DateTime('1981-02-13');

   $this
       ->dateTime($dt)
           ->hasYear(1981)     // passes
   ;

.. _date-time-is-clone-of:

isCloneOf
^^^^^^^^^

.. hint::
   ``isCloneOf`` is an inherited method from the ``object`` asserter.
   For more information, you can read the :ref:`object::isCloneOf <object-is-clone-of>` documentation


.. _date-time-is-equal-to:

isEqualTo
^^^^^^^^^

.. hint::
   ``isEqualTo`` is an inherited method from the ``object`` asserter.
   For more information, you can read the :ref:`object::isEqualTo <object-is-equal-to>` documentation


.. _dat-time-is-identical-to:

isIdenticalTo
^^^^^^^^^^^^^

.. hint::
   ``isIdenticalTo`` is an inherited method from the ``object`` asserter.
   For more information, you can read the :ref:`object::isIdenticalTo <object-is-identical-to>` documentation


.. _date-time-is-instance-of:

isInstanceOf
^^^^^^^^^^^^

.. hint::
   ``isInstanceOf`` is an inherited method from the ``object`` asserter.
   For more information, you can read the :ref:`object::isInstanceOf <object-is-instance-of>` documentation


.. _date-time-is-not-equal-to:

isNotEqualTo
^^^^^^^^^^^^

.. hint::
   ``isNotEqualTo`` is an inherited method from the ``object`` asserter.
   For more information, you can read the :ref:`object::isNotEqualTo <object-is-not-equal-to>` documentation


.. _date-time-is-not-identical-to:

isNotIdenticalTo
^^^^^^^^^^^^^^^^

.. hint::
   ``isNotIdenticalTo`` is an inherited method from the ``object`` asserter.
   For more information, you can read the :ref:`object::isNotIdenticalTo <object-is-not-identical-to>` documentation




.. _mysql-date-time:

mysqlDateTime
~~~~~~~~~~~~~

This is the asserter for objects representing a MySQL date, based on the `DateTime <http://php.net/datetime>`_ object.

The date must use a format compatible with MySQL and other DBMS, in particular « Y-m-d H:i:s » (for more information read the `date() <http://php.net/date>`_ function document on the PHP manual).

The check will fail if you pass a value that is not a ``DateTime`` object (or an instance of a class that extends it).

.. _mysql-date-time-has-date:

hasDate
^^^^^^^

.. hint::
   ``hasDate`` is an inherited method from the ``dateTime`` asserter.
   For more information, you can read the :ref:`dateTime::hasDate <date-time-has-date>` documentation


.. _mysql-date-time-has-date-and-time:

hasDateAndTime
^^^^^^^^^^^^^^

.. hint::
   ``hasDateAndTime`` is an inherited method from the ``dateTime`` asserter.
   For more information, you can read the :ref:`dateTime::hasDateAndTime <date-time-has-date-and-time>` documentation


.. _mysql-date-time-has-day:

hasDay
^^^^^^

.. hint::
   ``hasDay`` is an inherited method from the ``dateTime`` asserter.
   For more information, you can read the :ref:`dateTime::hasDay <date-time-has-day>` documentation


.. _mysql-date-time-has-hours:

hasHours
^^^^^^^^

.. hint::
   ``hasHours`` is an inherited method from the ``dateTime`` asserter.
   For more information, you can read the :ref:`dateTime::hasHours <date-time-has-hours>` documentation


.. _mysql-date-time-has-minutes:

hasMinutes
^^^^^^^^^^

.. hint::
   ``hasMinutes`` is an inherited method from the ``dateTime`` asserter.
   For more information, you can read the :ref:`dateTime::hasMinutes <date-time-has-minutes>` documentation


.. _mysql-date-time-has-month:

hasMonth
^^^^^^^^

.. hint::
   ``hasMonth`` is an inherited method from the ``dateTime`` asserter.
   For more information, you can read the :ref:`dateTime::hasMonth <date-time-has-month>` documentation


.. _mysql-date-time-has-seconds:

hasSeconds
^^^^^^^^^^

.. hint::
   ``hasSeconds`` is an inherited method from the ``dateTime`` asserter.
   For more information, you can read the :ref:`dateTime::hasSeconds <date-time-has-seconds>` documentation


.. _mysql-date-time-has-time:

hasTime
^^^^^^^

.. hint::
   ``hasTime`` is an inherited method from the ``dateTime`` asserter.
   For more information, you can read the :ref:`dateTime::hasTime <date-time-has-time>` documentation


.. _mysql-date-time-has-timezone:

hasTimezone
^^^^^^^^^^^

.. hint::
   ``hasTimezone`` is an inherited method from the ``dateTime`` asserter.
   For more information, you can read the :ref:`dateTime::hasTimezone <date-time-has-timezone>` documentation


.. _mysql-date-time-has-year:

hasYear
^^^^^^^

.. hint::
   ``hasYear`` is an inherited method from the ``dateTime`` asserter.
   For more information, you can read the :ref:`dateTime::hasYear <date-time-has-timezone>` documentation


.. _mysql-date-time-is-clone-of:

isCloneOf
^^^^^^^^^

.. hint::
   ``isCloneOf`` is an inherited method from the ``object`` asserter.
   For more information, you can read the :ref:`object::isCloneOf <object-is-clone-of>` documentation


.. _mysql-date-time-is-equal-to:

isEqualTo
^^^^^^^^^

.. hint::
   ``isEqualTo`` is an inherited method from the ``object`` asserter.
   For more information, you can read the :ref:`object::isEqualTo <object-is-equal-to>` documentation


.. _mysql-date-time-is-identical-to:

isIdenticalTo
^^^^^^^^^^^^^

.. hint::
   ``isIdenticalTo`` is an inherited method from the ``object`` asserter.
   For more information, you can read the :ref:`object::isIdenticalTo <object-is-identical-to>` documentation


.. _mysql-date-time-is-instance-of:

isInstanceOf
^^^^^^^^^^^^

.. hint::
   ``isInstanceOf`` is an inherited method from the ``object`` asserter.
   For more information, you can read the :ref:`object::isInstanceOf <object-is-instance-of>` documentation


.. _mysql-date-time-is-not-equal-to:

isNotEqualTo
^^^^^^^^^^^^

.. hint::
   ``isNotEqualTo`` is an inherited method from the ``object`` asserter.
   For more information, you can read the :ref:`object::isNotEqualTo <object-is-not-equal-to>` documentation


.. _mysql-date-time-is-not-identical-to:

isNotIdenticalTo
^^^^^^^^^^^^^^^^

.. hint::
   ``isNotIdenticalTo`` is an inherited method from the ``object`` asserter.
   For more information, you can read the :ref:`object::isNotIdenticalTo <object-is-not-identical-to>` documentation




.. _exception-anchor:

exception
~~~~~~~~~

This is the asserter for exceptions.

.. code-block:: php

   <?php
   $this
       ->exception(
           function($test) use($myObject) {
               // this throws an exception: throw new \Exception;
               $myObject->doOneThing('wrongParameter');
           }
       )
   ;

.. note::
   The syntax use anonymous functions (also named closures) introduced in PHP 5.3. For more information read `the PHP manual <http://php.net/functions.anonymous>`_.


.. _has-code:

hasCode
^^^^^^^

``hasCode`` checks the exception code

.. code-block:: php

   <?php
   $this
       ->exception(
           function() use($myObject) {
               // this throws an exception: throw new \Exception('Message', 42);
               $myObject->doOneThing('wrongParameter');
           }
       )
           ->hasCode(42)
   ;

.. _has-default-code:

hasDefaultCode
^^^^^^^^^^^^^^

``hasDefaultCode`` checks that the exception code is the default value, 0.

.. code-block:: php

   <?php
   $this
       ->exception(
           function() use($myObject) {
               // this throws an exception: throw new \Exception;
               $myObject->doOneThing('wrongParameter');
           }
       )
           ->hasDefaultCode()
   ;

.. note::
   ``hasDefaultCode`` is equivalent to ``hasCode(0)``.


.. _has-message:

hasMessage
^^^^^^^^^^

``hasMessage`` checks the exception message

.. code-block:: php

   <?php
   $this
       ->exception(
           function() use($myObject) {
               // this throws an exception: throw new \Exception('Message');
               $myObject->doOneThing('wrongParameter');
           }
       )
           ->hasMessage('Message')     // passes
           ->hasMessage('message')     // fails
   ;

.. _has-nested-exception:

hasNestedException
^^^^^^^^^^^^^^^^^^

``hasNestedException`` checks that the exception contains a reference to the previous exception. If the exception class is given, it will also check the exception class.

.. code-block:: php

   <?php
   $this
       ->exception(
           function() use($myObject) {
               // this throws an exception: throw new \Exception('Message');
               $myObject->doOneThing('wrongParameter');
           }
       )
           ->hasNestedException()      // fails

       ->exception(
           function() use($myObject) {
               try {
                   // this throws an exception: throw new \FirstException('Message 1', 42);
                   $myObject->doOneThing('wrongParameter');
               }
               // ... the exception is catched
               catch(\FirstException $e) {
                   // ... then thrown again, wrapped in a second exception
                   throw new \SecondException('Message 2', 24, $e);
               }
           }
       )
           ->isInstanceOf('\FirstException')           // fails
           ->isInstanceOf('\SecondException')          // passes

           ->hasNestedException()                      // passes
           ->hasNestedException(new \FirstException)   // passes
           ->hasNestedException(new \SecondException)  // fails
   ;

.. _exception-is-clone-of:

isCloneOf
^^^^^^^^^

.. hint::
   ``isCloneOf`` is an inherited method from the ``object`` asserter.
   For more information, you can read the :ref:`object::isCloneOf <object-is-clone-of>` documentation


.. _exception-is-equal-to:

isEqualTo
^^^^^^^^^

.. hint::
   ``isEqualTo`` is an inherited method from the ``object`` asserter.
   For more information, you can read the :ref:`object::isEqualTo <object-is-equal-to>` documentation


.. _exception-is-identical-to:

isIdenticalTo
^^^^^^^^^^^^^

.. hint::
   ``isIdenticalTo`` is an inherited method from the ``object`` asserter.
   For more information, you can read the :ref:`object::isIdenticalTo <object-is-identical-to>` documentation


.. _exception-is-instance-of:

isInstanceOf
^^^^^^^^^^^^

.. hint::
   ``isInstanceOf`` is an inherited method from the ``object`` asserter.
   For more information, you can read the :ref:`object::isInstanceOf <object-is-instance-of>` documentation


.. _exception-is-not-equal-to:

isNotEqualTo
^^^^^^^^^^^^

.. hint::
   ``isNotEqualTo`` is an inherited method from the ``object`` asserter.
   For more information, you can read the :ref:`object::isNotEqualTo <object-is-not-equal-to>` documentation


.. _exception-is-not-identical-to:

isNotIdenticalTo
^^^^^^^^^^^^^^^^

.. hint::
   ``isNotIdenticalTo`` is an inherited method from the ``object`` asserter.
   For more information, you can read the :ref:`object::isNotIdenticalTo <object-is-not-identical-to>` documentation


.. _message-anchor:

message
^^^^^^^

``message`` gives you an asserter of type :ref:`string <string-anchor>` containing the thrown exception message

.. code-block:: php

   <?php
   $this
       ->exception(
           function() {
               throw new \Exception('My custom message to test');
           }
       )
           ->message
               ->contains('message')
   ;



.. _array-anchor:

array
~~~~~

This is the asserter for arrays.

.. note::
   ``array`` being a PHP reserved keyword, it was not possible to create an ``array`` asserter class. That's why its name is actually ``phpArray``. You may encounter some ``->phpArray()`` or des ``->array()``.


It is advised to only use ``->array()`` to simplify test reading.

.. _array-contains:

contains
^^^^^^^^

``contains`` checks that an array contains the given value.

.. code-block:: php

   <?php
   $fibonacci = array('1', 2, '3', 5, '8', 13, '21');

   $this
       ->array($fibonacci)
           ->contains('1')     // passes
           ->contains(1)       // passes, because it does not ...
           ->contains('2')     // ... check the type
           ->contains(10)      // fails
   ;

.. note::
   ``contains`` does not search recursively.


.. warning::
   ``contains`` does not check the type. If you want to check the type, use :ref:`strictlyContains <strictly-contains>`.


.. _contains-values:

containsValues
^^^^^^^^^^^^^^

``containsValues`` checks that an array contains all the values of the given array.

.. code-block:: php

   <?php
   $fibonacci = array('1', 2, '3', 5, '8', 13, '21');

   $this
       ->array($array)
           ->containsValues(array(1, 2, 3))        // passes
           ->containsValues(array('5', '8', '13')) // passes
           ->containsValues(array(0, 1, 2))        // fails
   ;

.. note::
   ``containsValues`` does not search recursively.


.. warning::
   ``containsValues`` does not check the type. If you want to check the type, use :ref:`strictlyContainsValues <strictly-contains-values>`.


.. _has-key:

hasKey
^^^^^^

``hasKey`` checks that the array contains the given key.

.. code-block:: php

   <?php
   $fibonacci = array('1', 2, '3', 5, '8', 13, '21');
   $atoum     = array(
       'name'        => 'atoum',
       'owner'       => 'mageekguy',
   );

   $this
       ->array($fibonacci)
           ->hasKey(0)         // passes
           ->hasKey(1)         // passes
           ->hasKey('1')       // passes
           ->hasKey(10)        // fails

       ->array($atoum)
           ->hasKey('name')    // passes
           ->hasKey('price')   // fails
   ;

.. note::
   ``hasKey`` does not search recursively.


.. warning::
   ``hasKey`` does not check the type..


.. _has-keys:

hasKeys
^^^^^^^

``hasKeys`` checks that the keys of the array contains all the values of the given array.

.. code-block:: php

   <?php
   $fibonacci = array('1', 2, '3', 5, '8', 13, '21');
   $atoum     = array(
       'name'        => 'atoum',
       'owner'       => 'mageekguy',
   );

   $this
       ->array($fibonacci)
           ->hasKeys(array(0, 2, 4))           // passes
           ->hasKeys(array('0', 2))            // passes
           ->hasKeys(array('4', 0, 3))         // passes
           ->hasKeys(array(0, 3, 10))          // fails

       ->array($atoum)
           ->hasKeys(array('name', 'owner'))   // passes
           ->hasKeys(array('name', 'price'))   // fails
   ;

.. note::
   ``hasKeys`` does not search recursively.


.. warning::
   ``hasKeys`` does not check the type.


.. _array-has-size:

hasSize
^^^^^^^

``hasSize`` checks the array size.

.. code-block:: php

   <?php
   $fibonacci = array('1', 2, '3', 5, '8', 13, '21');

   $this
       ->array($fibonacci)
           ->hasSize(7)        // passes
           ->hasSize(10)       // fails
   ;

.. note::
   ``hasSize`` is not recursive.


.. _array-is-empty:

isEmpty
^^^^^^^

``isEmpty`` checks that the array is empty.

.. code-block:: php

   <?php
   $emptyArray    = array();
   $nonEmptyArray = array(null, null);

   $this
       ->array($emptyArray)
           ->isEmpty()         // passes

       ->array($nonEmptyArray)
           ->isEmpty()         // fails
   ;

.. _array-is-equal-to:

isEqualTo
^^^^^^^^^

.. hint::
   ``isEqualTo`` is an inherited method from the ``variable`` asserter.
   For more information, you can read the :ref:`variable::isEqualTo <variable-is-equal-to>` documentation


.. _array-is-identical-to:

isIdenticalTo
^^^^^^^^^^^^^

.. hint::
   ``isIdenticalTo`` is an inherited method from the ``variable`` asserter.
   For more information, you can read the :ref:`variable::isIdenticalTo <variable-is-identical-to>` documentation


.. _array-is-not-empty:

isNotEmpty
^^^^^^^^^^

``isNotEmpty`` checks that an array is not empty.

.. code-block:: php

   <?php
   $emptyArray    = array();
   $nonEmptyArray = array(null, null);

   $this
       ->array($emptyArray)
           ->isNotEmpty()      // fails

       ->array($nonEmptyArray)
           ->isNotEmpty()      // passes
   ;

.. _array-is-not-equal-to:

isNotEqualTo
^^^^^^^^^^^^

.. hint::
   ``isNotEqualTo`` is an inherited method from the ``variable`` asserter.
   For more information, you can read the :ref:`variable::isNotEqualTo <variable-is-not-equal-to>` documentation


.. _array-is-not-identical-to:

isNotIdenticalTo
^^^^^^^^^^^^^^^^

.. hint::
   ``isNotIdenticalTo`` is an inherited method from the ``variable`` asserter.
   For more information, you can read the :ref:`variable::isNotIdenticalTo <variable-is-not-identical-to>` documentation


.. _keys-anchor:

keys
^^^^

``keys`` gives you an :ref:`array <array-anchor>` asserter containing the keys of the array.

.. code-block:: php

   <?php
   $atoum = array(
       'name'  => 'atoum',
       'owner' => 'mageekguy',
   );

   $this
       ->array($atoum)
           ->keys
               ->isEqualTo(
                   array(
                       'name',
                       'owner',
                   )
               )
   ;

.. _array-not-contains:

notContains
^^^^^^^^^^^

``notContains`` checks that an array does not contains the given value.

.. code-block:: php

   <?php
   $fibonacci = array('1', 2, '3', 5, '8', 13, '21');

   $this
       ->array($fibonacci)
           ->notContains(null)         // passes
           ->notContains(1)            // fails
           ->notContains(10)           // passes
   ;

.. note::
   ``notContains`` does not search recursively.


.. warning::
   ``notContains`` does not check the type. If you want to also check the type, use :ref:`strictlyNotContains <strictly-not-contains>`.


.. _not-contains-values:

notContainsValues
^^^^^^^^^^^^^^^^^

``notContainsValues`` checks that the array does not contain any value of the given array.

.. code-block:: php

   <?php
   $fibonacci = array('1', 2, '3', 5, '8', 13, '21');

   $this
       ->array($array)
           ->notContainsValues(array(1, 4, 10))    // fails
           ->notContainsValues(array(4, 10, 34))   // passes
           ->notContainsValues(array(1, '2', 3))   // fails
   ;

.. note::
   ``notContainsValues`` does not search recursively.


.. warning::
   ``notContainsValues`` does not check the type. If you want to also check the type, use :ref:`strictlyNotContainsValues <strictly-not-contains-values>`.


.. _not-has-key:

notHasKey
^^^^^^^^^

``notHasKey`` checks that an array does not contain the given key.

.. code-block:: php

   <?php
   $fibonacci = array('1', 2, '3', 5, '8', 13, '21');
   $atoum     = array(
       'name'  => 'atoum',
       'owner' => 'mageekguy',
   );

   $this
       ->array($fibonacci)
           ->notHasKey(0)          // fails
           ->notHasKey(1)          // fails
           ->notHasKey('1')        // fails
           ->notHasKey(10)         // passes

       ->array($atoum)
           ->notHasKey('name')     // fails
           ->notHasKey('price')    // passes
   ;

.. note::
   ``notHasKey`` does not search recursively.


.. warning::
   ``notHasKey`` does not check the type.


.. _not-has-keys:

notHasKeys
^^^^^^^^^^

``notHasKeys`` checks that the array keys does not contain any of the given values.

.. code-block:: php

   <?php
   $fibonacci = array('1', 2, '3', 5, '8', 13, '21');
   $atoum     = array(
       'name'        => 'atoum',
       'owner'       => 'mageekguy',
   );

   $this
       ->array($fibonacci)
           ->notHasKeys(array(0, 2, 4))            // fails
           ->notHasKeys(array('0', 2))             // fails
           ->notHasKeys(array('4', 0, 3))          // fails
           ->notHasKeys(array(10, 11, 12))         // passes

       ->array($atoum)
           ->notHasKeys(array('name', 'owner'))    // fails
           ->notHasKeys(array('foo', 'price'))     // passes
   ;

.. note::
   ``notHasKeys`` does not search recursively.


.. warning::
   ``notHasKeys`` does not check the type.


.. _size-anchor:

size
^^^^

``size`` gives you an :ref:`integer <integer-anchor>` asserter containing the array size.

.. code-block:: php

   <?php
   $fibonacci = array('1', 2, '3', 5, '8', 13, '21');

   $this
       ->array($fibonacci)
           ->size
               ->isGreaterThan(5)
   ;

.. _strictly-contains:

strictlyContains
^^^^^^^^^^^^^^^^

``strictlyContains`` checks that an array strictly contains the given value (same value and type).

.. code-block:: php

   <?php
   $fibonacci = array('1', 2, '3', 5, '8', 13, '21');

   $this
       ->array($fibonacci)
           ->strictlyContains('1')     // passes
           ->strictlyContains(1)       // fails
           ->strictlyContains('2')     // fails
           ->strictlyContains(2)       // passes
           ->strictlyContains(10)      // fails
   ;

.. note::
   ``strictlyContains`` does not search recursively.


.. warning::
   ``strictlyContains`` checks the type. If you do not want to check the type, use :ref:`contains <array-contains>`.


.. _strictly-contains-values:

strictlyContainsValues
^^^^^^^^^^^^^^^^^^^^^^

``strictlyContainsValues`` checks that an array strictly contains of all the given values (same value and type).

.. code-block:: php

   <?php
   $fibonacci = array('1', 2, '3', 5, '8', 13, '21');

   $this
       ->array($array)
           ->strictlyContainsValues(array('1', 2, '3'))    // passes
           ->strictlyContainsValues(array(1, 2, 3))        // fails
           ->strictlyContainsValues(array(5, '8', 13))     // passes
           ->strictlyContainsValues(array('5', '8', '13')) // fails
           ->strictlyContainsValues(array(0, '1', 2))      // fails
   ;

.. note::
   ``strictlyContainsValues`` does not search recursively.


.. warning::
   ``strictlyContainsValues`` checks the type. If you do not want to check the type, use :ref:`containsValues <contains-values>`.


.. _strictly-not-contains:

strictlyNotContains
^^^^^^^^^^^^^^^^^^^

``strictlyNotContains`` checks that the array strictly does not contain the given value (same value and type).

.. code-block:: php

   <?php
   $fibonacci = array('1', 2, '3', 5, '8', 13, '21');

   $this
       ->array($fibonacci)
           ->strictlyNotContains(null)         // passes
           ->strictlyNotContains('1')          // fails
           ->strictlyNotContains(1)            // passes
           ->strictlyNotContains(10)           // passes
   ;

.. note::
   ``strictlyNotContains`` does not search recursively.


.. warning::
   ``strictlyNotContains`` checks the type. If you do not want to check the type, use :ref:`notContains <array-not-contains>`.


.. _strictly-not-contains-values:

strictlyNotContainsValues
^^^^^^^^^^^^^^^^^^^^^^^^^

``strictlyNotContainsValues`` checks that an array strictly does not contain any of the given values (same value and type).

.. code-block:: php

   <?php
   $fibonacci = array('1', 2, '3', 5, '8', 13, '21');

   $this
       ->array($array)
           ->strictlyNotContainsValues(array('1', 4, 10))  // fails
           ->strictlyNotContainsValues(array(1, 4, 10))    // passes
           ->strictlyNotContainsValues(array(4, 10, 34))   // passes
           ->strictlyNotContainsValues(array('1', 2, '3')) // fails
           ->strictlyNotContainsValues(array(1, '2', 3))   // passes
   ;

.. note::
   ``strictlyNotContainsValues`` does not search recursively.


.. warning::
   ``strictlyNotContainsValues`` checks the type. If you do not want to check the type, use :ref:`notContainsValues <not-contains-values>`.




.. _string-anchor:

string
~~~~~~

This is the asserter for strings.

.. _string-contains:

contains
^^^^^^^^

``contains`` checks that the string contains the given string.

.. code-block:: php

   <?php
   $string = 'Hello world';

   $this
       ->string($string)
           ->contains('ll')    // passes
           ->contains(' ')     // passes
           ->contains('php')   // fails
   ;

.. _string-has-length:

hasLength
^^^^^^^^^

``hasLength`` checks the string length.

.. code-block:: php

   <?php
   $string = 'Hello world';

   $this
       ->string($string)
           ->hasLength(11)     // passes
           ->hasLength(20)     // fails
   ;

.. _string-has-length-greater-than:

hasLengthGreaterThan
^^^^^^^^^^^^^^^^^^^^

``hasLengthGreaterThan`` checks that the string length is greater than the given value.

.. code-block:: php

   <?php
   $string = 'Hello world';

   $this
       ->string($string)
           ->hasLengthGreaterThan(10)     // passes
           ->hasLengthGreaterThan(20)     // fails
   ;

.. _string-has-length-less-than:

hasLengthLessThan
^^^^^^^^^^^^^^^^^

``hasLengthLessThan`` checks that the string length is less than the given value.

.. code-block:: php

   <?php
   $string = 'Hello world';

   $this
       ->string($string)
           ->hasLengthLessThan(20)     // passes
           ->hasLengthLessThan(10)     // fails
   ;

.. _string-is-empty:

isEmpty
^^^^^^^

``isEmpty`` checks that the string is empty.

.. code-block:: php

   <?php
   $emptyString    = '';
   $nonEmptyString = 'atoum';

   $this
       ->string($emptyString)
           ->isEmpty()             // passes

       ->string($nonEmptyString)
           ->isEmpty()             // fails
   ;

.. _string-is-equal-to:

isEqualTo
^^^^^^^^^

.. hint::
   ``isEqualTo`` is an inherited method from the ``variable`` asserter.
   For more information, you can read the :ref:`variable::isEqualTo <variable-is-equal-to>` documentation


.. _string-is-equal-to-contents-of-file:

isEqualToContentsOfFile
^^^^^^^^^^^^^^^^^^^^^^^

``isEqualToContentsOfFile`` checks that the string is equal to the content of the given file path.

.. code-block:: php

   <?php
   $this
       ->string($string)
           ->isEqualToContentsOfFile('/path/to/file')
   ;

.. note::
   The test fails if the file does not exist.


.. _string-is-identical-to:

isIdenticalTo
^^^^^^^^^^^^^

.. hint::
   ``isIdenticalTo`` is an inherited method from the ``variable`` asserter.
   For more information, you can read the :ref:`variable::isIdenticalTo <variable-is-identical-to>` documentation


.. _string-is-not-empty:

isNotEmpty
^^^^^^^^^^

``isNotEmpty`` checks that the string is not empty.

.. code-block:: php

   <?php
   $emptyString    = '';
   $nonEmptyString = 'atoum';

   $this
       ->string($emptyString)
           ->isNotEmpty()          // fails

       ->string($nonEmptyString)
           ->isNotEmpty()          // passes
   ;

.. _string-is-not-equal-to:

isNotEqualTo
^^^^^^^^^^^^

.. hint::
   ``isNotEqualTo`` is an inherited method from the ``variable`` asserter.
   For more information, you can read the :ref:`variable::isNotEqualTo <variable-is-not-equal-to>` documentation


.. _string-is-not-identical-to:

isNotIdenticalTo
^^^^^^^^^^^^^^^^

.. hint::
   ``isNotIdenticalTo`` is an inherited method from the ``variable`` asserter.
   For more information, you can read the :ref:`variable::isNotIdenticalTo <variable-is-not-identical-to>` documentation


.. _length-anchor:

length
^^^^^^

``length`` gives you an :ref:`integer <integer-anchor>` asserter containing the string length.

.. code-block:: php

   <?php
   $string = 'atoum'

   $this
       ->string($string)
           ->length
               ->isGreaterThanOrEqualTo(5)
   ;

.. _string-match:

match
^^^^^

``match`` checks that the string matches a regular expression.

.. code-block:: php

   <?php
   $phone = '0102030405';
   $vdm   = "Aujourd'hui, à 57 ans, mon père s'est fait tatouer une licorne sur l'épaule. VDM";

   $this
       ->string($phone)
           ->match('#^0[1-9]\d{8}$#')

       ->string($vdm)
           ->match("#^Aujourd'hui.*VDM$#")
   ;

.. _string-not-contains:

notContains
^^^^^^^^^^^

``notContains`` checks that the string does not contain the given string.

.. code-block:: php

   <?php
   $string = 'Hello world';

   $this
       ->string($string)
           ->notContains('php')   // passes
           ->notContains(';')     // passes
           ->notContains('ll')    // fails
           ->notContains(' ')     // fails
   ;



.. _cast-to-string:

castToString
~~~~~~~~~~~~

This is the asserter for casting objects to sting.

.. code-block:: php

   <?php
   class AtoumVersion {
       private $version = '1.0';

       public function __toString() {
           return 'atoum v' . $this->version;
       }
   }

   $this
       ->castToString(new AtoumVersion())
           ->isEqualTo('atoum v1.0')
   ;

.. _cast-to-string-contains:

contains
^^^^^^^^

.. hint::
   ``contains`` is an inherited method from the ``string`` asserter.
   For more information, you can read the :ref:`string::contains <string-contains>` documentation


.. _cast-to-string-not-contains:

notContains
^^^^^^^^^^^

.. hint::
   ``notContains`` is an inherited method from the ``string`` asserter.
   For more information, you can read the :ref:`string::notContains <string-not-contains>` documentation


.. _cast-to-string-has-length:

hasLength
^^^^^^^^^

.. hint::
   ``hasLength`` is an inherited method from the ``string`` asserter.
   For more information, you can read the :ref:`string::hasLength <string-has-length>` documentation


.. _cast-to-string-has-length-greater-than:

hasLengthGreaterThan
^^^^^^^^^^^^^^^^^^^^

.. hint::
   ``hasLengthGreaterThan`` is an inherited method from the ``string`` asserter.
   For more information, you can read the :ref:`string::hasLengthGreaterThan <string-has-length-greater-than>` documentation


.. _cast-to-string-has-length-less-than:

hasLengthLessThan
^^^^^^^^^^^^^^^^^

.. hint::
   ``hasLengthLessThan`` is an inherited method from the ``string`` asserter.
   For more information, you can read the :ref:`string::hasLengthLessThan <string-has-length-less-than>` documentation


.. _cast-to-string-is-empty:

isEmpty
^^^^^^^

.. hint::
   ``isEmpty`` is an inherited method from the ``string`` asserter.
   For more information, you can read the :ref:`string::isEmpty <string-is-empty>` documentation


.. _cast-to-string-is-equal-to:

isEqualTo
^^^^^^^^^

.. hint::
   ``isEqualTo`` is an inherited method from the ``variable`` asserter.
   For more information, you can read the :ref:`variable::isEqualTo <variable-is-equal-to>` documentation


.. _cast-to-string-is-equal-to-contents-of-file:

isEqualToContentsOfFile
^^^^^^^^^^^^^^^^^^^^^^^

.. hint::
   ``isEqualToContentsOfFile`` is an inherited method from the ``string`` asserter.
   For more information, you can read the :ref:`string::isEqualToContentsOfFile <string-is-equal-to-contents-of-file>` documentation


.. _cast-to-string-is-identical-to:

isIdenticalTo
^^^^^^^^^^^^^

.. hint::
   ``isIdenticalTo`` is an inherited method from the ``variable`` asserter.
   For more information, you can read the :ref:`variable::isIdenticalTo <variable-is-identical-to>` documentation


.. _cast-to-string-is-not-empty:

isNotEmpty
^^^^^^^^^^

.. hint::
   ``isNotEmpty`` is an inherited method from the ``string`` asserter.
   For more information, you can read the :ref:`string::isNotEmpty <string-is-not-empty>` documentation


.. _cast-to-string-is-not-equal-to:

isNotEqualTo
^^^^^^^^^^^^

.. hint::
   ``isNotEqualTo`` is an inherited method from the ``variable`` asserter.
   For more information, you can read the :ref:`variable::isNotEqualTo <variable-is-not-equal-to>` documentation


.. _cast-to-string-is-not-identical-to:

isNotIdenticalTo
^^^^^^^^^^^^^^^^

.. hint::
   ``isNotIdenticalTo`` is an inherited method from the ``variable`` asserter.
   For more information, you can read the :ref:`variable::isNotIdenticalTo <variable-is-not-identical-to>` documentation


.. _cast-to-string-match:

match
^^^^^

.. hint::
   ``match`` is an inherited method from the ``string`` asserter.
   For more information, you can read the :ref:`string::match <string-match>` documentation




.. _hash-anchor:

hash
~~~~

This is the asserter for hashes.

.. _hash-contains:

contains
^^^^^^^^

.. hint::
   ``contains`` is an inherited method from the ``string`` asserter.
   For more information, you can read the :ref:`string::contains <string-contains>` documentation


.. _hash-is-equal-to:

isEqualTo
^^^^^^^^^

.. hint::
   ``isEqualTo`` is an inherited method from the ``variable`` asserter.
   For more information, you can read the :ref:`variable::isEqualTo <variable-is-equal-to>` documentation


.. _hash-is-equal-to-contents-of-file:

isEqualToContentsOfFile
^^^^^^^^^^^^^^^^^^^^^^^

.. hint::
   ``isEqualToContentsOfFile`` is an inherited method from the ``string`` asserter.
   For more information, you can read the :ref:`string::isEqualToContentsOfFile <string-is-equal-to-contents-of-file>` documentation


.. _hash-is-identical-to:

isIdenticalTo
^^^^^^^^^^^^^

.. hint::
   ``isIdenticalTo`` is an inherited method from the ``variable`` asserter.
   For more information, you can read the :ref:`variable::isIdenticalTo <variable-is-identical-to>` documentation


.. _is-md5:

isMd5
^^^^^

``isMd5`` checks that the string is a valid ``md5``, an hexadecimal string of 32 characters.

.. code-block:: php

   <?php
   $hash    = hash('md5', 'atoum');
   $notHash = 'atoum';

   $this
       ->hash($hash)
           ->isMd5()       // passes
       ->hash($notHash)
           ->isMd5()       // fails
   ;

.. _hash-is-not-equal-to:

isNotEqualTo
^^^^^^^^^^^^

.. hint::
   ``isNotEqualTo`` is an inherited method from the ``variable`` asserter.
   For more information, you can read the :ref:`variable::isNotEqualTo <variable-is-not-equal-to>` documentation


.. _hash-is-not-identical-to:

isNotIdenticalTo
^^^^^^^^^^^^^^^^

.. hint::
   ``isNotIdenticalTo`` is an inherited method from the ``variable`` asserter.
   For more information, you can read the :ref:`variable::isNotIdenticalTo <variable-is-not-identical-to>` documentation


.. _is-sha1:

isSha1
^^^^^^

``isSha1`` checks that the string is a ``sha1``, an hexadecimal string of 40 characters.

.. code-block:: php

   <?php
   $hash    = hash('sha1', 'atoum');
   $notHash = 'atoum';

   $this
       ->hash($hash)
           ->isSha1()      // passes
       ->hash($notHash)
           ->isSha1()      // fails
   ;

.. _is-sha256:

isSha256
^^^^^^^^

``isSha256`` checks that the string is a ``sha256``, an hexadecimal string of 64 characters.

.. code-block:: php

   <?php
   $hash    = hash('sha256', 'atoum');
   $notHash = 'atoum';

   $this
       ->hash($hash)
           ->isSha256()    // passes
       ->hash($notHash)
           ->isSha256()    // fails
   ;

.. _is-sha512:

isSha512
^^^^^^^^

``isSha512`` checks that the string is a ``sha512``, an hexadecimal string of 128 characeters.

.. code-block:: php

   <?php
   $hash    = hash('sha512', 'atoum');
   $notHash = 'atoum';

   $this
       ->hash($hash)
           ->isSha512()    // passes
       ->hash($notHash)
           ->isSha512()    // fails
   ;

.. _hash-not-contains:

notContains
^^^^^^^^^^^

.. hint::
   ``notContains`` is an inherited method from the ``string`` asserter.
   For more information, you can read the :ref:`string::notContains <string-not-contains>` documentation




.. _output-anchor:

output
~~~~~~

This is the asserter for output streams, that is supposed to be displayed on the screen.

.. code-block:: php

   <?php
   $this
       ->output(
           function() {
               echo 'Hello world';
           }
       )
   ;

.. note::
   The syntax use anonymous functions (also named closures) introduced in PHP 5.3. For more information read `the PHP manual <http://php.net/functions.anonymous>`_.


.. _output-contains:

contains
^^^^^^^^

.. hint::
   ``contains`` is an inherited method from the ``string`` asserter.
   For more information, you can read the :ref:`string::contains <string-contains>` documentation


.. _output-has-length:

hasLength
^^^^^^^^^

.. hint::
   ``hasLength`` is an inherited method from the ``string`` asserter.
   For more information, you can read the :ref:`string::hasLength <string-has-length>` documentation


.. _output-has-length-greater-than:

hasLengthGreaterThan
^^^^^^^^^^^^^^^^^^^^

.. hint::
   ``hasLengthGreaterThan`` is an inherited method from the ``string`` asserter.
   For more information, you can read the :ref:`string::hasLengthGreaterThan <string-has-length-greater-than>` documentation


.. _output-has-length-less-than:

hasLengthLessThan
^^^^^^^^^^^^^^^^^

.. hint::
   ``hasLengthLessThan`` is an inherited method from the ``string`` asserter.
   For more information, you can read the :ref:`string::hasLengthLessThan <string-has-length-less-than>` documentation


.. _output-is-empty:

isEmpty
^^^^^^^

.. hint::
   ``isEmpty`` is an inherited method from the ``string`` asserter.
   For more information, you can read the :ref:`string::isEmpty <string-is-empty>` documentation


.. _output-is-equal-to:

isEqualTo
^^^^^^^^^

.. hint::
   ``isEqualTo`` is an inherited method from the ``variable`` asserter.
   For more information, you can read the :ref:`variable::isEqualTo <variable-is-equal-to>` documentation


.. _output-is-equal-to-contents-of-file:

isEqualToContentsOfFile
^^^^^^^^^^^^^^^^^^^^^^^

.. hint::
   ``isEqualToContentsOfFile`` is an inherited method from the ``string`` asserter.
   For more information, you can read the :ref:`string::isEqualToContentsOfFile <string-is-equal-to-contents-of-file>` documentation


.. _output-is-identical-to:

isIdenticalTo
^^^^^^^^^^^^^

.. hint::
   ``isIdenticalTo`` is an inherited method from the ``variable`` asserter.
   For more information, you can read the :ref:`variable::isIdenticalTo <variable-is-identical-to>` documentation


.. _output-is-not-empty:

isNotEmpty
^^^^^^^^^^

.. hint::
   ``isNotEmpty`` is an inherited method from the ``string`` asserter.
   For more information, you can read the :ref:`string::isNotEmpty <string-is-not-empty>` documentation


.. _output-is-not-equal-to:

isNotEqualTo
^^^^^^^^^^^^

.. hint::
   ``isNotEqualTo`` is an inherited method from the ``variable`` asserter.
   For more information, you can read the :ref:`variable::isNotEqualTo <variable-is-not-equal-to>` documentation


.. _output-is-not-identical-to:

isNotIdenticalTo
^^^^^^^^^^^^^^^^

.. hint::
   ``isNotIdenticalTo`` is an inherited method from the ``variable`` asserter.
   For more information, you can read the :ref:`variable::isNotIdenticalTo <variable-is-not-identical-to>` documentation


.. _output-match:

match
^^^^^

.. hint::
   ``match`` is an inherited method from the ``string`` asserter.
   For more information, you can read the :ref:`string::match <string-match>` documentation


.. _output-not-contains:

notContains
^^^^^^^^^^^

.. hint::
   ``notContains`` is an inherited method from the ``string`` asserter.
   For more information, you can read the :ref:`string::notContains <string-not-contains>` documentation




.. _utf8-string:

utf8String
~~~~~~~~~~

This is the asserter for UTF-8 strings.

.. note::
   ``utf8Strings`` uses the ``mb_*`` functions to handle multi-bytes strings. Read the PHP manual for more information about the extension `mbstring <http://php.net/mbstring>`_.


.. _utf8-string-contains:

contains
^^^^^^^^

.. hint::
   ``contains`` is an inherited method from the ``string`` asserter.
   For more information, you can read the :ref:`string::contains <string-contains>` documentation


.. _utf8-string-has-length:

hasLength
^^^^^^^^^

.. hint::
   ``hasLength`` is an inherited method from the ``string`` asserter.
   For more information, you can read the :ref:`string::hasLength <string-has-length>` documentation


.. _utf8-string-has-length-greater-than:

hasLengthGreaterThan
^^^^^^^^^^^^^^^^^^^^

.. hint::
   ``hasLengthGreaterThan`` is an inherited method from the ``string`` asserter.
   For more information, you can read the :ref:`string::hasLengthGreaterThan <string-has-length-greater-than>` documentation


.. _utf8-string-has-length-less-than:

hasLengthLessThan
^^^^^^^^^^^^^^^^^

.. hint::
   ``hasLengthLessThan`` is an inherited method from the ``string`` asserter.
   For more information, you can read the :ref:`string::hasLengthLessThan <string-has-length-less-than>` documentation


.. _utf8-string-is-empty:

isEmpty
^^^^^^^

.. hint::
   ``isEmpty`` is an inherited method from the ``string`` asserter.
   For more information, you can read the :ref:`string::isEmpty <string-is-empty>` documentation


.. _utf8-string-is-equal-to:

isEqualTo
^^^^^^^^^

.. hint::
   ``isEqualTo`` is an inherited method from the ``variable`` asserter.
   For more information, you can read the :ref:`variable::isEqualTo <variable-is-equal-to>` documentation


.. _utf8-string-is-equal-to-contents-of-file:

isEqualToContentsOfFile
^^^^^^^^^^^^^^^^^^^^^^^

.. hint::
   ``isEqualToContentsOfFile`` is an inherited method from the ``string`` asserter.
   For more information, you can read the :ref:`string::isEqualToContentsOfFile <string-is-equal-to-contents-of-file>` documentation


.. _utf8-string-is-identical-to:

isIdenticalTo
^^^^^^^^^^^^^

.. hint::
   ``isIdenticalTo`` is an inherited method from the ``variable`` asserter.
   For more information, you can read the :ref:`variable::isIdenticalTo <variable-is-identical-to>` documentation


.. _utf8-string-is-not-empty:

isNotEmpty
^^^^^^^^^^

.. hint::
   ``isNotEmpty`` is an inherited method from the ``string`` asserter.
   For more information, you can read the :ref:`string::isNotEmpty <string-is-not-empty>` documentation


.. _utf8-string-is-not-equal-to:

isNotEqualTo
^^^^^^^^^^^^

.. hint::
   ``isNotEqualTo`` is an inherited method from the ``variable`` asserter.
   For more information, you can read the :ref:`variable::isNotEqualTo <variable-is-not-equal-to>` documentation


.. _utf8-string-is-not-identical-to:

isNotIdenticalTo
^^^^^^^^^^^^^^^^

.. hint::
   ``isNotIdenticalTo`` is an inherited method from the ``variable`` asserter.
   For more information, you can read the :ref:`variable::isNotIdenticalTo <variable-is-not-identical-to>` documentation


.. _utf8-string-match:

match
^^^^^

.. hint::
   ``match`` is an inherited method from the ``string`` asserter.
   For more information, you can read the :ref:`string::match <string-match>` documentation


.. note::
   Don't forget to add the ``u`` to your regular expression. For more information read the `PHP manual <http://php.net/reference.pcre.pattern.modifiers>`_.


.. code-block:: php

   <?php
   $vdm = "Aujourd'hui, à 57 ans, mon père s'est fait tatouer une licorne sur l'épaule. VDM";

   $this
       ->utf8String($vdm)
           ->match("#^Aujourd'hui.*VDM$#u")
   ;

.. _utf8-string-not-contains:

notContains
^^^^^^^^^^^

.. hint::
   ``notContains`` is an inherited method from the ``string`` asserter.
   For more information, you can read the :ref:`string::notContains <string-not-contains>` documentation




.. _after-destruction-of:

afterDestructionOf
~~~~~~~~~~~~~~~~~~

This is the asserter for object destruction.

The asserter only receives an object, make sure the ``__destruct()`` method is defined and call it.

If ``__destruct()`` exists and calling does not raise any error or exception, the test will pass.

.. code-block:: php

   <?php
   $this
       ->afterDestructionOf($objectWithDestructor)     // passes
       ->afterDestructionOf($objectWithoutDestructor)  // fails
   ;



.. _error-anchor:

error
~~~~~

This is the asserter for errors.

.. code-block:: php

   <?php
   $this
       ->when(
           function() {
               trigger_error('message');
           }
       )
           ->error()
               ->exists() // or notExists
   ;

.. note::
   The syntax use anonymous functions (also named closures) introduced in PHP 5.3. For more information read `the PHP manual <http://php.net/functions.anonymous>`_.


.. warning::
   The errors types E_ERROR, E_PARSE, E_CORE_ERROR, E_CORE_WARNING, E_COMPILE_ERROR, E_COMPILE_WARNING along with most of the E_STRICT can't be handled by this function.


.. _exists-anchor:

exists
^^^^^^

``exists`` checks that an error has been raised when calling the anonymous function.

.. code-block:: php

   <?php
   $this
       ->when(
           function() {
               trigger_error('message');
           }
       )
           ->error()
               ->exists()      // passes

       ->when(
           function() {
               // code sans erreur
           }
       )
           ->error()
               ->exists()      // fails
   ;

.. _not-exists:

notExists
^^^^^^^^^

``notExists`` checks that no error has been raised when calling the anonymous function.

.. code-block:: php

   <?php
   $this
       ->when(
           function() {
               trigger_error('message');
           }
       )
           ->error()
               ->notExists()   // fails

       ->when(
           function() {
               // no error there
           }
       )
           ->error()
               ->notExists()   // passes
   ;

.. _with-type:

withType
^^^^^^^^

``withType`` checks the raised error type.

.. code-block:: php

   <?php
   $this
       ->when(
           function() {
               trigger_error('message');
           }
       )
           ->error()
               ->withType(E_USER_NOTICE)   // passes
               ->withType(E_USER_WARNING)  // fails
   ;



.. _class-anchor:

class
~~~~~

This is the asserter for classes.

.. code-block:: php

   <?php
   $object = new \StdClass;

   $this
       ->class(get_class($object))

       ->class('\StdClass')
   ;

.. note::
   ``class`` being a reserved PHP keyword, it wasn't possible to create a ``class`` asserter. It is actually named ``phpClass`` and a ``class`` alias has been added. You may encounter ``->phpClass()`` or ``->class()``.


It is advised to only use ``->class()``.

.. _has-interface:

hasInterface
^^^^^^^^^^^^

``hasInterface`` checks that the class implements the given interface.

.. code-block:: php

   <?php
   $this
       ->class('\ArrayIterator')
           ->hasInterface('Countable')     // passes

       ->class('\StdClass')
           ->hasInterface('Countable')     // fails
   ;

.. _has-method:

hasMethod
^^^^^^^^^

``hasMethod`` checks that the class contains the given method.

.. code-block:: php

   <?php
   $this
       ->class('\ArrayIterator')
           ->hasMethod('count')    // passes

       ->class('\StdClass')
           ->hasMethod('count')    // fails
   ;

.. _has-no-parent:

hasNoParent
^^^^^^^^^^^

``hasNoParent`` checks that the class does not inherit from any class.

.. code-block:: php

   <?php
   $this
       ->class('\StdClass')
           ->hasNoParent()     // passes

       ->class('\FilesystemIterator')
           ->hasNoParent()     // fails
   ;

.. warning::
   A class can implements one or more interface while not inheriting from any class. ``hasNoParent`` does not check implementd interfaces, only inherited classes.


.. _has-parent:

hasParent
^^^^^^^^^

``hasParent`` checks that the class inherits from a class.

.. code-block:: php

   <?php
   $this
       ->class('\StdClass')
           ->hasParent()       // fails

       ->class('\FilesystemIterator')
           ->hasParent()       // passes
   ;

.. warning::
   A class can implements one or more interface while not inheriting from any class. ``hasParent`` does not check implementd interfaces, only inherited classes.


.. _is-abstract:

isAbstract
^^^^^^^^^^

``isAbstract`` checks that the class is abstract.

.. code-block:: php

   <?php
   $this
       ->class('\StdClass')
           ->isAbstract()       // fails
   ;

.. _is-subclass-of:

isSubclassOf
^^^^^^^^^^^^

``isSubclassOf`` checks that the class inherits from the given class.

.. code-block:: php

   <?php
   $this
       ->class('\FilesystemIterator')
           ->isSubclassOf('\DirectoryIterator')    // passes
           ->isSubclassOf('\SplFileInfo')          // passes
           ->isSubclassOf('\StdClass')             // fails
   ;


.. _mock-anchor:

mock
~~~~

This is the asserter for mocks.

.. code-block:: php

   <?php
   $mock = new \mock\MyClass;

   $this
       ->mock($mock)
   ;

.. note::
   For more information on how to create mocks see :ref:`Mocks <mocks-anchor>`;


.. _call-anchor:

call
^^^^

``call`` let you specify which method of the mock to check

.. code-block:: php

   <?php
   $mock = new \mock\MyFirstClass;

   $this
       ->object(new MySecondClass($mock))

       ->mock($mock)
           ->call('myMethod')
               ->once()
   ;

.. _at-least-once:

atLeastOnce
```````````

``atLeastOnce`` check that the tested method (see :ref:`call <call-anchor>`) has been called at least once.

.. code-block:: php

   <?php
   $mock = new \mock\MyFirstClass;

   $this
       ->object(new MySecondClass($mock))

       ->mock($mock)
           ->call('myMethod')
               ->atLeastOnce()
   ;

.. _exactly-anchor:

exactly
```````

``exactly`` check that the tested method (see :ref:`call <call-anchor>`) has been called a specific number of times.

.. code-block:: php

   <?php
   $mock = new \mock\MyFirstClass;

   $this
       ->object(new MySecondClass($mock))

       ->mock($mock)
           ->call('myMethod')
               ->exactly(2)
   ;

.. _never-anchor:

never
`````

``never`` check that the tested method (see :ref:`call <call-anchor>`) has never been called.

.. code-block:: php

   <?php
   $mock = new \mock\MyFirstClass;

   $this
       ->object(new MySecondClass($mock))

       ->mock($mock)
           ->call('myMethod')
               ->never()
   ;

.. note::
   ``never`` is equivalent to :ref:`exactly <exactly-anchor>` (0).


.. _once-twice-thrice:

once/twice/thrice
`````````````````
This asserters check that the tested method (see :ref:`call <call-anchor>`) has been called exactly:

* once
* twice
* thrice

.. code-block:: php

   <?php
   $mock = new \mock\MyFirstClass;

   $this
       ->object(new MySecondClass($mock))

       ->mock($mock)
           ->call('myMethod')
               ->once()
           ->call('mySecondMethod')
               ->twice()
           ->call('myThirdMethod')
               ->thrice()
   ;

.. note::
   ``once``, ``twice`` et ``thrice`` are respectively equivalent to :ref:`exactly <exactly-anchor>` (1), :ref:`exactly <exactly-anchor>` (2) et :ref:`exactly <exactly-anchor>` (3).


.. _with-any-arguments:

withAnyArguments
````````````````

``withAnyArguments`` let you not specify the expected argument when the tested method is called (see :ref:`call <call-anchor>`).

This is especially useful to reset arguments, like this example:

.. code-block:: php

   <?php
   $mock = new \mock\MyFirstClass;

   $this
       ->object(new MySecondClass($mock))

       ->mock($mock)
           ->call('myMethod')
               ->withArguments('first')     ->once()
               ->withArguments('second')    ->once()
               ->withAnyArguments()->exactly(2)
   ;

.. _with-arguments:

withArguments
`````````````

``withArguments`` let you specify the expected arguments that tested method should receive when called (see :ref:`call <call-anchor>`).

.. code-block:: php

   <?php
   $mock = new \mock\MyFirstClass;

   $this
       ->object(new MySecondClass($mock))

       ->mock($mock)
           ->call('myMethod')
               ->withArguments('first', 'second')->once()
   ;

.. warning::
   ``withArguments`` does not check the arguments type. If you also want to check the type, use :ref:`withIdenticalArguments <with-identical-arguments>`.


.. _with-identical-arguments:

withIdenticalArguments
``````````````````````

``withIdenticalArguments`` let you specify the expected arguments that tested method should receive when called (see :ref:`call <call-anchor>`).

.. code-block:: php

   <?php
   $mock = new \mock\MyFirstClass;

   $this
       ->object(new MySecondClass($mock))

       ->mock($mock)
           ->call('myMethod')
               ->withIdenticalArguments('first', 'second')->once()
   ;

.. warning::
   ``withIdenticalArguments`` checks the arguments type. If you do not want to check the type, use :ref:`withArguments <with-arguments>`.


.. _was-called:

wasCalled
^^^^^^^^^

``wasCalled`` checks that at least one method of the mock has been called at least once.

.. code-block:: php

   <?php
   $mock = new \mock\MyFirstClass;

   $this
       ->object(new MySecondClass($mock))

       ->mock($mock)
           ->wasCalled()
   ;

.. _was-not-called:

wasNotCalled
^^^^^^^^^^^^

``wasNotCalled`` checks that no method of the mock has been called.

.. code-block:: php

   <?php
   $mock = new \mock\MyFirstClass;

   $this
       ->object(new MySecondClass($mock))

       ->mock($mock)
           ->wasNotCalled()
   ;


.. _stream-anchor:

stream
~~~~~~

This is the asserter for streams.

.. important::
   Unfortunately, I do not know how it works, feel free to contribute!


.. _is-read:

isRead
^^^^^^

.. important::
   We need help to write this section !


.. _is-write:

isWrite
^^^^^^^

.. important::
   We need help to write this section !



.. _writing-help:

Writing help
------------

There are several ways to write unit tests with atoum, and one of them is to use keywords like ``given``, ``if``, ``and`` or ``then``, ``when`` or ``assert``.

.. _if--and--then:

if, and, then
~~~~~~~~~~~~~

Usage of this keywords is really intuitive:

.. code-block:: php

   <?php
   $this
       ->given($computer = new computer()))
       ->if($computer->prepare())
       ->and
           $computer->setFirstOperand(2),
           $computer->setSecondOperand(2)
       )
       ->then
           ->object($computer->add())
               ->isIdenticalTo($computer)
           ->integer($computer->getResult())
               ->isEqualTo(4)
   ;

It is important to note that theses keywords do not change anything technically or functionally. Their only goal is to ease the test comprehension, which next developers will be thankful for :).

Thereby, ``given``, ``if`` et ``and``  let you define the prior conditions so that the assertions that follow the ``then`` keyword pass.

However, there is no grammar defining the order these keywords are used, and no syntax check is done by atoum.

It is the developer responsibility to use them wisely, though it is possible to write tests like this:

.. code-block:: php

   <?php
   $this
       ->and($computer = new computer()))
       ->and($computer->setFirstOperand(2))
       ->then
       ->if($computer->setSecondOperand(2))
           ->object($computer->add())
               ->isIdenticalTo($computer)
           ->integer($computer->getResult())
               ->isEqualTo(4)
   ;

For the same reasons, the use of ``then`` is optional.

It is also important to note that it is possible to write the same test without any keyword:

.. code-block:: php

   <?php
   $computer = new computer();
   $computer->setFirstOperand(2);
   $computer->setSecondOperand(2);

   $this
       ->object($computer->add())
           ->isIdenticalTo($computer)
       ->integer($computer->getResult())
           ->isEqualTo(4)
   ;

There is not speed difference, the only important thing is to chose one way of doing it and stick with it.

.. _when-anchor:

when
~~~~

In addition to ``given``, ``if``, ``and`` and ``then``, there are other keywords.

One of them is ``when``. It adds a feature to get around the fact that it is forbidden to write the following code in PHP:

.. code-block:: php

   <?php
   $this
       ->if($object = new object($valueAtKey0 = uniqid()))
       ->and(unset($object[0]))
       ->then
           ->sizeOf($object)
               ->isZero()
   ;

PHP will raise the following fatal error: ``Parse error: syntax error, unexpected 'unset' (T_UNSET), expecting »)'``

It is impossible to use ``unset()`` as a function argument.

To fix this problem the ``when`` keyword is capable of evaluating the anonymous function that you may pass as an argument. The previous may then be written like this:

.. code-block:: php

   <?php
   $this
       ->if($object = new object($valueAtKey0 = uniqid()))
       ->when(
           function() use ($object) {
               unset($object[0]);
           }
       )
       ->then
         ->sizeOf($object)
           ->isZero()
   ;

Of course, if ``when`` does not receive any anonymous function, it will behave exactly like ``given``, ``if``, ``and`` and ``then``.

.. _assert-anchor:

assert
~~~~~~

Finally, there is also the ``assert`` keyword.

The following test will be used to illustrate its usage:

.. code-block:: php

   <?php
   $this
       ->if($foo = new \mock\foo())
       ->and($bar = new bar($foo))
       ->and($bar->doSomething())
       ->then
           ->mock($foo)
               ->call('doOtherThing')
                   ->once()

       ->if($bar->setValue(uniqid())
       ->then
           ->mock($foo)
               ->call('doOtherThing')
                   ->exactly(2)
   ;

This test has a drawback maintenance wise, because if the developer wants to add one or more new calls to bar::doOtherThing() between the two existing calls, he will have to update the value given to exactly().
To fix this issue, you can reset a mock using 2 ways:

* using ``$mock->getMockController()->resetCalls()`` ;
* using utilisant $this->resetMock($mock).

.. code-block:: php

   <?php
   $this
       ->if($foo = new \mock\foo())
       ->and($bar = new bar($foo))
       ->and($bar->doSomething())
       ->then
           ->mock($foo)
               ->call('doOtherThing')
                   ->once()

       // 1st way
       ->if($foo->getMockController()->resetCalls())
       ->and($bar->setValue(uniqid())
       ->then
           ->mock($foo)
               ->call('doOtherThing')
                   ->once()

       // 2nd way
       ->if($this->resetMock($foo))
       ->and($bar->setValue(uniqid())
       ->then
           ->mock($foo)
               ->call('doOtherThing')
                   ->once()
   ;

These methods reset the controller memory, it is then possible to write the next assertion as if the mock was never called.

The ``assert`` keyword let you avoid having to explicitly call ``resetCalls()`` and it also triggers the memory reset of all the adapters and mock controllers defined when it is used.

Thanks to this feature, it is possible to write the previous test in a more readable simpler way by passing a string describing the next assertions.

.. code-block:: php

   <?php
   $this
       ->assert('Foo est vide')
           ->if($foo = new \mock\foo())
           ->and($bar = new bar($foo))
           ->and($bar->doSomething())
           ->then
               ->mock($foo)
                   ->call('doOtherThing')
                       ->once()

       ->assert('Foo a une valeur')
           ->if($bar->setValue(uniqid())
           ->then
               ->mock($foo)
                   ->call('doOtherThing')
                       ->once()
   ;

The string will be used by atoum in atoum generated messages if one of the assertions fails.

.. _the-loop-mode:

The loop mode
------------

When a developer works using TDD, he uses the following workflow:

# he start by writing a test for a piece of code to be written
# he runs the test which fails
# he writes some code to make the test pass
# he edits the test and continues with step 2.

This means he has to:

* write the test in his editor
* quit his editor to start the command to run the test
* get back to his editor to write some code
* get back to the terminal to restart the command
* get back to his editor to write some code
* ...

Here we can spot some steps repeated until the feature is fully developed.

However, this cycle is complex and requires many switches between softwares and recurrent typing of the same command to start the tests.

atoum provides a loop mode available using the ``-l`` or ``--loop`` arguments on the command line. This will allow the developer to work without worrying about restarting the command.

Once test have run, if they are all passing, atoum will stay in pause:

. code-block:: shell

   $ php tests/units/classes/adapter.php -l
   > atoum version DEVELOPMENT by Frédéric Hardy (/Users/fch/Atoum)
   > PHP path: /usr/local/bin/php
   > PHP version:
   > PHP 5.3.8 (cli) (built: Sep 21 2011 23:14:37)
   > Copyright (c) 1997-2011 The PHP Group
   > Zend Engine v2.3.0, Copyright (c) 1998-2011 Zend Technologies
   >     with Xdebug v2.1.1, Copyright (c) 2002-2011, by Derick Rethans
   > mageekguy\atoum\tests\units\adapter...
   [S___________________________________________________________][1/1]
   > Test duration: 0.02 second.
   > Memory usage: 0.25 Mb.
   > Total test duration: 0.02 second.
   > Total test memory usage: 0.25 Mb.
   > Code coverage value: 100.00%
   > Running duration: 0.16 second.
   Success (1 test, 0 method, 2 assertions, 0 error, 0 exception) !
   Press <Enter> to reexecute, press any other key to stop...

If the developer presses anything but ``Enter``, atoum will terminate.

Otherwise atoum will restart the same tests without any other action from the developer.

If some tests do not pass, some assertions failed, errors were raised or exceptions were thrown, atoum will also stay in pause:

.. code-block:: shell

   $ php tests/units/classes/adapter.php -l
   > atoum version DEVELOPMENT by Frédéric Hardy (/Users/fch/Atoum)
   > PHP path: /usr/local/bin/php
   > PHP version:
   > PHP 5.3.8 (cli) (built: Sep 21 2011 23:14:37)
   > Copyright (c) 1997-2011 The PHP Group
   > Zend Engine v2.3.0, Copyright (c) 1998-2011 Zend Technologies
   >     with Xdebug v2.1.1, Copyright (c) 2002-2011, by Derick Rethans
   > mageekguy\atoum\tests\units\adapter...
   [F___________________________________________________________][1/1]
   > Test duration: 0.00 second.
   > Memory usage: 0.00 Mb.
   > Total test duration: 0.00 second.
   > Total test memory usage: 0.00 Mb.
   > Running duration: 0.17 second.
   Failure (1 test, 0 method, 1 failure, 0 error, 0 exception) !
   > There is 1 failure:
   > mageekguy\atoum\tests\units\adapter::test__call():
   In file /Users/fch/Atoum/tests/units/classes/adapter.php on line 17, mageekguy\atoum\asserters\string::isEqualTo() failed: strings are not equals
   -Reference
   +Data
   @@ -1 +1 @@
   -string(13) "4ea0354cd717c"
   +string(32) "19798c230d5462b3bdae194f364feffa"
   Press <Enter> to reexecute, press any other key to stop...

Here again, if the developer presses anything but ``Enter`` atoum will terminate.

However, pressing ``Enter`` will make atoum restart only failing tests until they pass again.

When everything get back to green, atoum will restart the whole test suite ensuring that fixing fails did not broke anything else.

Of course, the loop mode will only run `the selected test files <chapter3.html#files-to-execute>`_ par atoum.

.. _the-debug-mode:

The debug mode
-------------

Sometimes tests fail and it's hard to find why.

In this case, you can probably use a debugger or functions like ``var_dump()`` or ``print_r``, or you can debug directly in unit tests.

atoum provides some tools to help you in this process, debugging directly in unit tests.

Those tools are only available when you run atoum and enable the debug mode using the ``--debug`` command line argument, this is to avoid unexpected debug output when running in standard mode.

When the developer enables the debug mode, he gains access to three methods:

* ``dump()`` to dump the content of a variable
* ``stop()`` to stop a running test
* ``executeOnFailure()`` to set a closure to be executed when an assertion fails

Those three method are accessible through the atoum fluent interface.

dump
~~~~

The ``dump()`` method can be used as follow:

.. code-block:: php

   <?php
   $this
       ->if($foo = new foo())
       ->then
           ->object($foo->setBar($bar = new bar()))
               ->isIdenticalTo($foo)
           ->dump($foo->getBar())
   ;

When running this test, the ``foo::getBar()`` return value will be printed on the standard output.

You can also pass multiple arguments to the ``dump()`` method:

.. code-block:: php

   <?php
   $this
       ->if($foo = new foo())
       ->then
           ->object($foo->setBar($bar = new bar()))
               ->isIdenticalTo($foo)
           ->dump($foo->getBar(), $bar)
   ;

.. _stop-anchor:

stop
~~~~

The ``stop()``method is also easy to use:

.. code-block:: php

   <?php
   $this
       ->if($foo = new foo())
       ->then
           ->object($foo->setBar($bar = new bar()))
               ->isIdenticalTo($foo)
           ->stop() // The test will stop here if --debug is used
           ->object($foo->getBar())
               ->isIdenticalTo($bar)
   ;

.. _execute-on-failure:

executeOnFailure
~~~~~~~~~~~~~~~~

The ``executeOnFailure()`` is really mighty and also easy to use.

In fact she takes a closure as its single argument and run it only if any assertion fails:

.. code-block:: php

   <?php
   $this
       ->if($foo = new foo())
       ->executeOnFailure(
           function() use ($foo) {
               var_dump($foo);
           }
       )
       ->then
           ->object($foo->setBar($bar = new bar()))
               ->isIdenticalTo($foo)
           ->object($foo->getBar())
               ->isIdenticalTo($bar)
   ;

In the previous example, unlike with ``dump()`` which always prints the value, the variable ``foo`` will only be printed if any of the following assertion fails.

Of course you can call ``executeOnFailure()`` several times in a test to execute multiple closures when an assertion fails.
