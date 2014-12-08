.. _introduction-anchor:

Introduction
============

.. _what-is-atoum:

What is atoum ?
---------------


atoum is a unit test framework, like PHPUnit or SimpleTest. Though, we believe it has an edge on its counterparts thanks to :

* Its more modern approach and its use of PHP's latest features.
* Its simplicity and fast learning curve.
* Its fluent interface, making the tests as readable as the english natural langage


.. _download---install:

Download & Install
------------------

As of today, atoum doesn't have any version number. However, it is both usable and stable. If you're willing to use it, 
download the latest stable build. Even though atoum continuously benefits from new features, backward compatibility is one 
of the main priority of the developers. 

There are several ways to install atoum :

* :ref:`As a PHAR archive <p-h-a-r>`
* :ref:`Using composer <composer-anchor>`
* :ref:`Using an install script <installer-anchor>`
* :ref:`Cloning the github repository <github-anchor>`
* :ref:`Using a Symfony2 Bundle <symfony2-bundle>`.
* :ref:`Using a Zend Framework 2 component <zend-framework-2-component>`


.. _p-h-a-r:

PHAR
~~~~

atoum is distributed as a PHAR archive, an archive format dedicated to PHP, available since PHP 5.3.

.. _installation-anchor:

Installation
^^^^^^^^^^^^

You can download the latest stable version of atoum directly from the official website : `http://downloads.atoum.org/nightly/mageekguy.atoum.phar <http://downloads.atoum.org/nightly/mageekguy.atoum.phar>`_

.. _updating-anchor:

Updating
^^^^^^^^

Updating atoum's PHAR is easy thanks to its command line tools :

.. code-block:: shell

   php -d phar.readonly=0 mageekguy.atoum.phar --update

.. note::
   To update atoum, PHP needs to be able to update PHAR archives, which is disabled by default, this is why you have to specify the option ``-d phar.readonly=0``.


If a newer version of atoum exists, it will be downloaded and installed in the archive itself :

.. code-block:: shell

   php -d phar.readonly=0 mageekguy.atoum.phar --update
   Checking if a new version is available... Done !
   Update to version 'nightly-1568-201210311708'... Done !
   Enable version 'nightly-1568-201210311708'... Done !
   Atoum has been updated from version 'old-version' to 'nightly-1568-201210311708' successfully !

If no newer version is available, atoum will just stop without doing anything.

.. code-block:: shell

   php -d phar.readonly=0 mageekguy.atoum.phar --update
   Checking if a new version is available... Done !
   There is no new version available !

atoum won't ask you for confirmation before proceeding with the update as it is very easy to go back to a previous version.

.. _listing-available-versions-present-in-atoum-s-archive:

Listing available versions present in atoum's archive
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

To show the list of versions contained in its archive, you'll use the ``--list-available-versions`` (or the shorter ``-lav``) argument.

.. code-block:: shell

   php mageekguy.atoum.phar -lavnightly-941-201201011548
   
   * nightly-1568-201210311708

Available versions will be shown. The one prefixed with a ``*`` is active.

.. _updating-the-current-version:

Updating the current version
^^^^^^^^^^^^^^^^^^^^^^^^^^^^

To activate a different version of atoum, use the ``--enable-version`` (or the shorter ``-ev``) argument with the name of the version you want to activate.

.. code-block:: shell

   php -d phar.readonly=0 mageekguy.atoum.phar -ev DEVELOPMENT

.. note::
   Updating the current version of atoum needs PHP to be able to update PHAR archives, which is disabled by default, this is why you have to specify the option ``-d phar.readonly=0``.


.. _removing-older-versions:

Removing older versions
^^^^^^^^^^^^^^^^^^^^^^^

If you want to remove a version of atoum from the archive, use the --delete-version (or shorter -dv) argument followed by the name of the version you want to remove.

.. code-block:: shell

   php -d phar.readonly=0 mageekguy.atoum.phar -dv nightly-941-201201011548

The version will be removed.

.. note::
   You cannot remove the active version.


.. note::
   Removing a version of atoum needs PHP to be able to update PHAR archives, which is disabled by default, this is why you have to specify the option ``-d phar.readonly=0``.


.. _composer-anchor:

Composer
~~~~~~~~

`Composer <http://getcomposer.org/>`_ is a tool for dependency management in PHP.

Start by downloading and installing Composer

.. code-block:: shell

   curl -s https://getcomposer.org/installer | php

Then, create a composer.json file at the root of your project, containing

.. code-block:: json

   {
       "require": {
           "atoum/atoum": "dev-master"
       }
   }

Finally execute :

.. code-block:: shell

   php composer.phar install

.. _installer-anchor:

Installer
~~~~~~~~~

You will also be able to install atoum with its dedicated `script <https://github.com/atoum/atoum-installer>`_:

.. code-block:: shell

   curl https://raw.github.com/atoum/atoum-installer/master/installer | php -- --phar
   php mageekguy.atoum.phar -v
   atoum version nightly-xxxx-yyyymmddhhmm by Frédéric Hardy (phar:///path/to/mageekguy.atoum.phar)

This script lets you install atoum locally (in a project, see the previous example) or as a system-wide utility:

.. code-block:: shell

   curl https://raw.github.com/atoum/atoum-installer/master/installer | sudo php -- --phar --global
   which atoum
   /usr/local/bin/atoum

Options are available and let you tweak the installation process : see the `documentation <https://github.com/atoum/atoum-installer/blob/master/README.md>`_ for more details.

.. _github-anchor:

Github
~~~~~~

If you want to use atoum directly from its sources, you can clone or fork its git repository on github : git://github.com/atoum/atoum.git

.. _symfony-1-plugin:

Symfony 1 plugin
~~~~~~~~~~~~~~~~

If you want to use atoum in a symfony 1 project, you can do so thanks to the `sfAtoumPlugin plugin <https://github.com/atoum/sfAtoumPlugin>`_

Install instructions are available on the project's page.

.. _symfony2-bundle:

Symfony2 bundle
~~~~~~~~~~~~~~~

If you want to use atoum in a Symfony2 project, you can do so thanks to the `atoum Bundle <https://github.com/atoum/AtoumBundle>`_.

Install instructions are available on the project's page.

.. _zend-framework-2-component:

Zend Framework 2 component
~~~~~~~~~~~~~~~~~~~~~~~~~~

A library is available to use atoum with Zend Framework 2. Documentation and examples are available at the following address : `https://github.com/blanchonvincent/zend-framework-test-atoum <https://github.com/blanchonvincent/zend-framework-test-atoum>`_.

You'll find every install instructions there.

.. _a-quick-overview-of-atoum-s-philosophy:

A quick overview of atoum's philosophy
--------------------------------------

.. _very-basic-example:

Very basic example
~~~~~~~~~~~~~~~~~~

atoum wants you to write a test class for each class you want to test. As an example, if you want to test the famous HelloTheWorld class, you'll have to write the test\units\HelloTheWorld test class.

NOTE : atoum is, of course, namespaces aware. As an example, to test the Hello\The\World class, you'll write the \Hello\The\tests\units\World class.

Here is the code of your HelloTheWorld class that we'll be using as a first example. This class will be located in PROJECT_PATH/classes/HelloTheWorld.php

.. code-block:: php

   <?php
   /**
    * The class to be tested
    */
   class HelloTheWorld
   {
       public function getHiAtoum ()
       {
           return "Hi atoum !";
       }
   }

Now, let's write our first test class. This class will be located in PROJECT_PATH/tests/HelloTheWorld.php

.. code-block:: php

   <?php
   //Your test classes are in a dedicated namespace
   namespace tests\units;
   
   //You have to include your tested class
   require_once __DIR__.'/../classes/HelloTheWorld.php';
   
   //You now include atoum, using its phar archive
   require_once __DIR__.'/atoum/mageekguy.atoum.phar';
   
   use \mageekguy\atoum;
   
   /**
    * Test class for \HelloTheWorld
    * Test classes extend from atoum\test
    */
   class HelloTheWorld extends atoum\test
   {
       public function testGetHiAtoum ()
       {
           //new instance of the tested class
           $helloToTest = new \HelloTheWorld();
   
           $this->assert
                       //we expect the getHiAtoum method to return a string
                       ->string($helloToTest->getHiAtoum())
                       //and the string should be Hi atoum !
                       ->isEqualTo('Hi atoum !');
       }
   }

Now, let's launch the tests

.. code-block:: shell

   php -f ./test/HelloTheWorld.php

You will see something like this

.. code-block:: shell

   > atoum version nightly-941-201201011548 by Frédéric Hardy (phar:///home/documentation/projects/tests/atoum/mageekguy.atoum.phar/1)
   > PHP path: /usr/bin/php5
   > PHP version:
   > PHP 5.3.6-13ubuntu3.3 with Suhosin-Patch (cli) (built: Dec 13 2011 18:37:10)
   > Copyright (c) 1997-2011 The PHP Group
   > Zend Engine v2.3.0, Copyright (c) 1998-2011 Zend Technologies
   >     with Xdebug v2.1.2, Copyright (c) 2002-2011, by Derick Rethans
   > tests\units\HelloTheWorld...
   [S___________________________________________________________][1/1]
   > Test duration: 0.01 second.
   > Memory usage: 0.00 Mb.
   > Total test duration: 0.01 second.
   > Total test memory usage: 0.00 Mb.
   > Code coverage value: 100.00%
   > Running duration: 0.16 second.
   Success (1 test, 1/1 method, 2 assertions, 0 error, 0 exception) !
We've just tested that the getHiAtoum method :

* returns a string;
* and that this string is the expected 'Hi atoum !' string.

All tests passed. You're done, your code is rock solid !

.. _rule-of-thumb:

Rule of Thumb
~~~~~~~~~~~~~
The basics when you’re testing things using atoum are the following :

* Tell atoum what you want to work on (a variable, an object, a string, an integer, …)
* Tell atoum the state the element is expected to be in (is equal to, is null, exists, …).

.. _using-atoum-with-your-favorite-i-d-e:

Using atoum with your favorite IDE
----------------------------------

.. _sublime-text-2:

Sublime Text 2
~~~~~~~~~~~~~~

A `SublimeText 2 plugin <https://github.com/toin0u/Sublime-atoum>`_ enables you to launch tests and see the results directly in the editor.

Required instructions to install the plugin are available here `the author's blog <http://sbin.dk/2012/05/19/atoum-sublime-text-2-plugin/>`_.

.. _v-i-m:

VIM
~~~

atoum is bundled with a plugin dedicated to VIM.

It enables you to launch tests without leaving VIM, and to get the matching report in the editor's screen.

You can navigate through potential errors, directly going to the line where assertions failed thanks to matching key strokes.

.. _installing-the-v-i-m-plugin:

Installing the VIM plugin
~~~~~~~~~~~~~~~~~~~~~~~~~

If you're not using the PHAR archive, you'll find the plugin in resources/vim/atoum.vba.

If you're using the PHAR archive, you can ask atoum to extract the file with the command line

.. code-block:: shell

   php mageekguy.atoum.phar --extractResourcesTo path/to/a/directory

Once you have the atoum.vba file, use VIM to edit its content

.. code-block:: shell

   vim path/to/atoum.vba

And ask VIM to install the plugin with

.. code-block:: vim

   :source %

.. _using-atoum-and-v-i-m:

Using atoum and VIM
~~~~~~~~~~~~~~~~~~~

Of course, to work properly, the plugin needs to be correctly installed, and you're supposed to be editing a test case based on atoum.

The following command line asks for tests execution:

.. code-block:: vim

   :Atoum

Tests are launched and a report, based on your atoum configuration in ftplugin/php/atoum.vim of your .vim directory, is generated in a new screen.

Feel free to link this command with a shortcut of your own. i.e. adding the following line to your .vimrc file :

.. code-block:: vim

   nnoremap *.php :Atoum

The F12 function key will now trigger the :Atoum command.

.. _managing-atoum-s-configuration-file:

Managing atoum's configuration file
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

You can specify another configuration file by adding the following line to your .vimrc file:

.. code-block:: vim

   call atoum#defineConfiguration('/path/to/project/directory', '/path/to/atoum/configuration/file', '.php')

The atoum#defineConfiguration function enables you to define the configuration file to use based on your unit test directory.
it takes 3 arguments :

* The path to the unit tests directory
* The path to the atoum's configuration file to be considered
* The extension of the unit test files that will be concerned

If you want to know more about the plugin, you can use the embedded help in VIM thanks to the following command :

.. code-block:: vim

   :help atoum

Automatically open failing tests
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
atoum is able to automatically open failing tests after suite has run. Here are the supported editors:

* :ref:`macvim <macvim-anchor>` (Mac OS X)
* :ref:`gvim <gvim-anchor>` (Unix)
* :ref:`PhpStorm <php-storm>` (Mac OS X/Unix)
* :ref:`gedit <gedit-anchor>` (Unix)

To use this feature you will have to edit your `configuration file <chapter3.html#configuration-files>`_:

.. _macvim-anchor:

macvim
^^^^^^

.. code-block:: php

   <?php
   use
       mageekguy\atoum,
       mageekguy\atoum\report\fields\runner\failures\execute\macos
   ;

   $stdOutWriter = new atoum\writers\std\out();
   $cliReport = new atoum\reports\realtime\cli();
   $cliReport->addWriter($stdOutWriter);

   $cliReport->addField(new macos\macvim());

   $runner->addReport($cliReport);


.. _gvim-anchor:

gvim
^^^^

.. code-block:: php

   <?php
   use
       mageekguy\atoum,
       mageekguy\atoum\report\fields\runner\failures\execute\unix
   ;

   $stdOutWriter = new atoum\writers\std\out();
   $cliReport = new atoum\reports\realtime\cli();
   $cliReport->addWriter($stdOutWriter);

   $cliReport->addField(new unix\gvim());

   $runner->addReport($cliReport);


.. _php-storm:

PhpStorm
^^^^^^^^

If you are on Mac OS X, use the following configuration:

.. code-block:: php

   <?php
   use
       mageekguy\atoum,
       mageekguy\atoum\report\fields\runner\failures\execute\macos
   ;

   $stdOutWriter = new atoum\writers\std\out();
   $cliReport = new atoum\reports\realtime\cli();
   $cliReport->addWriter($stdOutWriter);

   $cliReport
       // If PhpStorm is installed in in /Applications
       ->addField(new macos\phpstorm())

       // If PhpStorm is installed anywhere else
       // ->addField(
       //     new macos\phpstorm(
       //         '/path/to/PhpStorm.app/Contents/MacOS/webide'
       //     )
       // )
   ;

   $runner->addReport($cliReport);


On Unix, use the following configuration:

.. code-block:: php

   <?php
   use
       mageekguy\atoum,
       mageekguy\atoum\report\fields\runner\failures\execute\unix
   ;

   $stdOutWriter = new atoum\writers\std\out();
   $cliReport = new atoum\reports\realtime\cli();
   $cliReport->addWriter($stdOutWriter);

   $cliReport
       ->addField(
           new unix\phpstorm('/chemin/vers/PhpStorm/bin/phpstorm.sh')
       )
   ;

   $runner->addReport($cliReport);

.. _gedit-anchor:

gedit
^^^^^

.. code-block:: php

   <?php
   use
       mageekguy\atoum,
       mageekguy\atoum\report\fields\runner\failures\execute\unix
   ;

   $stdOutWriter = new atoum\writers\std\out();
   $cliReport = new atoum\reports\realtime\cli();
   $cliReport->addWriter($stdOutWriter);

   $cliReport->addField(new unix\gedit());

   $runner->addReport($cliReport);
