
.. _first-tests:


First Tests
##################

You need to write a test class for each tested class.

Imagine that you want to test the traditional class ``HelloWorld``, then you must create the test class ``test\units\HelloWorld``.

Warning

If you would like to follow the first steps with atoum it is recommended to install the extension atoum-stubs. Using an IDE will issue a warning.

.. note::
	atoum uses namespaces. For example, to test the ``Vendor\Project\HelloWorld`` class, you must create the class ``Vendor\Project\tests\units\HelloWorld`` or ``tests\units\Vendor\Project\HelloWorld``.

Here is the code of the ``HelloWorld`` class that we will test.

.. code-block:: php

   <?php
   # src/Vendor/Project/HelloWorld.php

   namespace Vendor\Project;

   class HelloWorld
   {
       public function getHiAtoum ()
       {
           return 'Hi atoum !';
       }
   }


Now, here is the code of the test class that we could write.

.. code-block:: php

   <?php
   # src/Vendor/Project/tests/units/HelloWorld.php

   // The test class has is own namespace :
   // The namespace of the tested class + "test\units"
   namespace Vendor\Project\tests\units;

   // You must include the tested class (if you don't have an autoloader)
   require_once __DIR__ . '/../../HelloWorld.php';

   use atoum;

   /*
    * Test class for Vendor\Project\HelloWorld
    *
    * Note that they had the same name that the tested class
    * and that it derives frim the atoum class
    */
   class HelloWorld extends atoum
   {
       /*
        * This method is dedicated to the getHiAtoum() method
        */
       public function testGetHiAtoum ()
       {
           $this
               // creation of a new instance of the tested class
               ->given($this->newTestedInstance)

               ->then

	               // we test that the getHiAtoum method returns
	               // a string...
	               ->string($this->testedInstance->getHiAtoum())
	                   // ... and that this string is the one we want,
	                   // namely 'Hi atoum !'
	                   ->isEqualTo('Hi atoum !')
           ;
       }
   }

Now, launch our tests.
You should see something like this:

.. code-block:: shell

   $ ./vendor/bin/atoum -f src/Vendor/Project/tests/units/HelloWorld.php
   > PHP path: /usr/bin/php
   > PHP version:
   => PHP 5.6.3 (cli) (built: Nov 13 2014 18:31:57)
   => Copyright (c) 1997-2014 The PHP Group
   => Zend Engine v2.6.0, Copyright (c) 1998-2014 Zend Technologies
   > Vendor\Project\tests\units\HelloWorld...
   [S___________________________________________________________][1/1]
   => Test duration: 0.00 second.
   => Memory usage: 0.25 Mb.
   > Total test duration: 0.00 second.
   > Total test memory usage: 0.25 Mb.
   > Running duration: 0.04 second.
   Success (1 test, 1/1 method, 0 void method, 0 skipped method, 2 assertions)!


We just test that the method ``getHiAtoum``:

* returns a :ref:`string<string-anchor>`;
* that :ref:`is equals to<string-is-equal-to>` ``"Hi atoum !"``.

The tests passed, everything is green. Your code is solid as a rock with atoum!


Dissecting the test
*******************
It's important to understand each part of the test. Let's look at each section.

First, we use the namespace ``Vendor\Project\tests\units`` where ``Vendor\Project`` is the namespace of the class and ``tests\units`` the part of the namespace use by atoum to understand that we are in the test namespace. This special namespace is configurable as explained in the :ref:`appropriate section<cookbook_change_default-namespace>`.
Then, inside the test method, we use a special syntax :ref:`given and then<given-if-and-then>`. They do nothing other than making the test more readable.
Finally we use a couple more simple tricks, :ref:`newTestedInstance and testedInstance<newTestedInstance>` to get a new instance of the tested class.

