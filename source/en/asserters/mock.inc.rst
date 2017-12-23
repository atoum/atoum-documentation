.. _mock-asserter:

mock
****

It's the assertion dedicated to mocks.

.. code-block:: php

   <?php
   $mock = new \mock\MyClass;

   $this
       ->mock($mock)
   ;

.. note::
   Refer to the documentation of :ref:`mock <mocking_systems>` for more information on how to create and manage mocks.

.. _call-anchor:

call
====

``call`` let you specify which method of mock to check, it call must be followed by a call to one of the following verification method like :ref:`atLeastOnce <at-least-once>`, :ref:`once/twice/thrice <once-twice-thrice>`, :ref:`exactly <exactly-anchor>`, etc...

.. code-block:: php

   <?php

   $this
       ->given($mock = new \mock\MyFirstClass)
       ->and($object = new MySecondClass($mock))

       ->if($object->methodThatCallMyMethod())  // This will call myMethod from $mock
       ->then

       ->mock($mock)
           ->call('myMethod')
               ->once()
   ;

.. _mock-after:

after
`````

``after`` checks if the method has been called after the one passed as parameter.

.. code-block:: php

   <?php
   $this
       ->when($mock = new \mock\example)
       ->if(
           $mock->test2(),
           $mock->test()
       )
       ->mock($mock)
       ->call('test')
           ->after($this->mock($mock)->call('test2')->once())
           ->once() // passes
   ;

   $this
       ->when($mock = new \mock\example)
       ->if(
           $mock->test(),
           $mock->test2()
       )
       ->mock($mock)
       ->call('test')
           ->after($this->mock($mock)->call('test2')->once())
           ->once() // fails
   ;

.. _at-least-once:

atLeastOnce
```````````

``atLeastOnce`` check that the tested method (see :ref:`call <call-anchor>`) from the mock has been called at least once.

.. code-block:: php

   <?php
   $mock = new \mock\MyFirstClass;

   $this
       ->object(new MySecondClass($mock))

       ->mock($mock)
           ->call('myMethod')
               ->atLeastOnce()
   ;

.. _mock-before:

before
``````

``before`` checks if the method has been called before the one passed as parameter.

.. code-block:: php

   <?php
   $this
       ->when($mock = new \mock\example)
       ->if(
           $mock->test(),
           $mock->test2()
       )
       ->mock($mock)
       ->call('test')
           ->before($this->mock($mock)->call('test2')->once())
           ->once() // passes
   ;

   $this
       ->when($mock = new \mock\example)
       ->if(
           $mock->test2(),
           $mock->test()
       )
       ->mock($mock)
       ->call('test')
           ->before($this->mock($mock)->call('test2')->once())
           ->once() // fails
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

.. note::
   You can have a simplified version with ``->{2}``.

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
   ``never`` is equivalent to :ref:`exactly(0)<exactly-anchor>`.


.. _once-twice-thrice:

once/twice/thrice
`````````````````
This asserters check that the tested method (see :ref:`call<call-anchor>`) from the tested mock has been called exactly:

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
   ``once``, ``twice`` and ``thrice`` are respectively equivalent to :ref:`exactly(1)<exactly-anchor>`, :ref:`exactly(2)<exactly-anchor>` and :ref:`exactly(3)<exactly-anchor>`.


.. _with-any-arguments:

withAnyArguments
````````````````

``withAnyArguments`` allow to check any argument, non-specified, when we call the tested method (see :ref:`call<call-anchor>`) of tested mock.

This method is useful to reset the arguments of tested method, like in the following example:

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

``withArguments`` let you specify the expected arguments that the tested method should receive when called (see :ref:`call <call-anchor>`).

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
   | ``withArguments`` does not check the arguments type.
   | If you also want to check the type, use :ref:`withIdenticalArguments <with-identical-arguments>`.


.. _with-identical-arguments:

withIdenticalArguments
``````````````````````

``withIdenticalArguments`` let you specify the expected typed arguments that tested method should receive when called (see :ref:`call <call-anchor>`).

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
   | ``withIdenticalArguments`` checks the arguments type.
   |  If you do not want to check the type, use :ref:`withArguments <with-arguments>`.



.. _with-at-least-arguments:

withAtLeastArguments
````````````````````

``withAtLeastArguments`` let you specify the minimum expected arguments that tested method should receive when called (see :ref:`call <call-anchor>`).

.. code-block:: php

   <?php
   $this
      ->if($mock = new \mock\example)
      ->and($mock->test('a', 'b'))
      ->mock($mock)
      ->call('test')
            ->withAtLeastArguments(array('a'))->once() //passes
            ->withAtLeastArguments(array('a', 'b'))->once() //passes
            ->withAtLeastArguments(array('c'))->once() //fails
   ;

.. warning::
   | ``withAtLeastArguments`` does not check the arguments type.
   | If you also want to check the type, use :ref:`withAtLeastIdenticalArguments <with-at-least-identical-arguments>`.



.. _with-at-least-identical-arguments:

withAtLeastIdenticalArguments
`````````````````````````````

``withAtLeastIdenticalArguments`` let you specify the minimum expected typed arguments that tested method should receive when called (see :ref:`call <call-anchor>`).

.. code-block:: php

   <?php
   $this
       ->if($mock = new \mock\example)
       ->and($mock->test(1, 2))
       ->mock($mock)
           ->call('test')
           ->withAtLeastIdenticalArguments(array(1))->once() //passes
           ->withAtLeastIdenticalArguments(array(1, 2))->once() //passes
           ->withAtLeastIdenticalArguments(array('1'))->once() //fails
   ;

.. warning::
   | ``withAtLeastIdenticalArguments`` checks the arguments type.
   | If you do not want to check the type, use :ref:`withIdenticalArguments <with-at-least-arguments>`.


.. _without-any-argument:

withoutAnyArgument
``````````````````

``withoutAnyArgument`` lets you indicate that the method should not receive any argument when called (see :ref:`call <call-anchor>`).

.. code-block:: php

   <?php
   $this
       ->when($mock = new \mock\example)
       ->if($mock->test())
       ->mock($mock)
           ->call('test')
               ->withoutAnyArgument()->once() // passes
       ->if($mock->test2('argument'))
       ->mock($mock)
           ->call('test2')
               ->withoutAnyArgument()->once() // fails
   ;

.. note::
      ``withoutAnyArgument`` is equivalent to call :ref:`withAtLeastArguments<with-at-least-arguments>` with an empty array: ``->withAtLeastArguments(array())``.

.. _mock-receive:

receive
=======

It's an alias of :ref:`call-anchor`.

.. code-block:: php

   <?php
   $this
       ->given(
           $connection = new mock\connection
       )
       ->if(
           $this->newTestedInstance($connection)
       )
       ->then
           ->object($this->testedInstance->noMoreValue())->isTestedInstance
           ->mock($connection)->receive('newPacket')->withArguments(new packet)->once;

      // same as
      $this->mock($connection)->call('newPacket')->withArguments(new packet)->once;

.. _was-called:

wasCalled
=========

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
============

``wasNotCalled`` checks that no method of the mock has been called.

.. code-block:: php

   <?php
   $mock = new \mock\MyFirstClass;

   $this
       ->object(new MySecondClass($mock))

       ->mock($mock)
           ->wasNotCalled()
   ;
