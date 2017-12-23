.. _fine_tuning:

Fine tuning atoum behaviour
###########################


.. _initialization_method:

The initialization methods
**************************

Here is the process, when atoum executes the test methods of a class with the default engine (``concurrent``) :

#. call of the ``setUp()`` method from the tested class ;
#. launch of a PHP sub-process for **each** test method ;
#. in the PHP sub-process, call of the ``beforeTestMethod()`` method of the test class ;
#. in the PHP sub-process, call of the test method ;
#. in the PHP sub-process, call of the ``afterTestMethod()`` method of the test class ;
#. once the PHP sub-process finished, call of the ``tearDown()`` method from the test class.

.. note::
   For more information on the execution engine of test in atoum, you can read the section about the annotation :ref:`@engine <@engine>`.

The methods ``setUp()`` and ``tearDown()`` allow respectively to initialize and clean up the test environment for all the test method of the running class.

The methods ``beforeTestMethod()`` and ``afterTestMethod()`` allows respectively to initialize and clean up the execution environment of the individual tests for all test method of the class. In contrast of ``setUp()`` and ``tearDown()``, they are executed in the same subprocess.

It's also the reason why the methods  ``beforeTestMethod()`` and ``afterTestMethod()`` accept as argument the name of the test method executed, in order to adjust the treatment accordingly.

.. code-block:: php

   <?php
   namespace vendor\project\tests\units;

   use
       mageekguy\atoum,
       vendor\project
   ;

   require __DIR__ . '/atoum.phar';

   class bankAccount extends atoum
   {
       public function setUp()
       {
           // Executed *before all* test methods.
           // global initialization.
       }

       public function beforeTestMethod($method)
       {
           // Executed *before each* test method.

           switch ($method)
           {
               case 'testGetOwner':
                   // Initialization for testGetOwner().
               break;

               case 'testGetOperations':
                   // Initialization for testGetOperations().
               break;
           }
       }

       public function testGetOwner()
       {
           // ...
       }

       public function testGetOperations()
       {
           // ...
       }

       public function afterTestMethod($method)
       {
           // Executed *after each* test method.

           switch ($method)
           {
               case 'testGetOwner':
                   // Cleaning for testGetOwner().
               break;

               case 'testGetOperations':
                   // Cleaning for testGetOperations().
               break;
           }
       }

       public function tearDown()
       {
           // Executed after all the test methods.
           // Overall cleaning.
       }
   }

By default, the ``setUp()``, ``beforeTestMethod()``, ``afterTestMethod()`` and ``tearDown()`` methods does absolutely nothing.

It is therefore the responsibility of the developer to overload when needed in the test classes concerned.
