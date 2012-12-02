# What is atoum ? #

atoum is a unit testing framework, like PHPUnit or SimpleTest.

atoum distinguished itself as :

* It is modern and based on the last PHP versions
* It is quite simple and straightforward, with a very limited learning curve
* It is intuitive : its API wants to be as close as possible as a natural language

## Download & Install ##

For now, atoum is not tagged with a version number. If you want to use atoum, just download the last
stable version. atoum aims to provide backward compatibility anyway.

You can install atoum several ways :

* [As a PHAR archive](#phar)
* [Using composer](#composer)
* [Using an install script](#installer)
* [Cloning github repository](#github)
* [Using zend framework 2 component](#using-zend-framework-2-component)
* [Using a symfony 1.4 plugin](#using-the-symfony-14-plugin) ;


### PHAR

atoum is distributed as a PHAR archive, an archive format dedicated to PHP, available since PHP 5.3.

You can download the latest stable version of atoum directly from the official website here :
http://downloads.atoum.org/nightly/mageekguy.atoum.phar

### Composer

[Composer](http://getcomposer.org/) is a tool for dependency management in PHP.

To install atoum through composer, you must install composer

    [shell]
    curl -s https://getcomposer.org/installer | php

Then, create a file named composer.json who contains 

    [json]
    {
        "require": {
            "atoum/atoum": "dev-master"
        }
    }

And finally execute 

    [shell]
    php composer.phar install

### Installer

You will also be able to install atoum with it's dedicated [script](https://github.com/atoum/atoum-installer):

    [shell]
    curl https://raw.github.com/atoum/atoum-installer/master/installer | php -- --phar
    php mageekguy.atoum.phar -v
    atoum version nightly-xxxx-yyyymmddhhmm by Frédéric Hardy (phar:///path/to/mageekguy.atoum.phar)

This script lets you install atoum locally (in a project, see the previous example) or as a system-wide utility:

    [shell]
    curl https://raw.github.com/atoum/atoum-installer/master/installer | sudo php -- --phar --global
    which atoum
    /usr/local/bin/atoum

Options are available and let you tweak the installation process : see the
[documentation](https://github.com/atoum/atoum-installer/blob/master/README.md) for more details.

### Github

If you want to use atoum directly from it's sources, you can clone or fork its git repository on
github : git://github.com/atoum/atoum.git

### Using zend framework 2 component

A library is available to use Atoum with zend framework 2. Documentation and exemples are available
at the following address :
[https://github.com/blanchonvincent/zend-framework-test-atoum](https://github.com/blanchonvincent/zend-framework-test-atoum).

### Using the symfony 1.4 plugin

A plugin is available to use atoum with symfony 1.

Documentation is available in [the cookbook](#usage-with-symfony-14).


## A quick overview of atoum's philosophy ##

### Very basic example ###

atoum wants you to write a test class for each class you want to test. As an example, if you want to
test the famous HelloTheWorld class, you'll have to write the test\units\HelloTheWorld test class.

NOTE : atoum is, of course, namespace aware. As an example, to test the Hello\The\World class,
you'll write the \Hello\The\tests\units\World class.

Here is the code of your HelloTheWorld class that we'll be using as a first example. This class will
be located in PROJECT_PATH/classes/HelloTheWorld.php

    [php]
    <?php
    /**
     * The class to be tested
     */
    class HelloTheWorld
    {
        public function getHiBob ()
        {
            return "Hi Bob !";
        }
    }

Now, let's write our first test class. This class will be located in
PROJECT_PATH/tests/HelloTheWorld.php

    [php]
    <?php
    //Your test classes are in a dedicated namespace
    namespace tests\units;

    //You have to include your tested class
    require_once __DIR__.'/../classes/HelloTheWorld.php';

    //You now include atoum, using it’s phar archive
    require_once __DIR__.'/atoum/mageekguy.atoum.phar';

    use \mageekguy\atoum;

    /**
     * Test class for \HelloTheWorld
     * Test classes extends from atoum\test
     */
    class HelloTheWorld extends atoum\test
    {
        public function testGetHiBob ()
        {
            //new instance of the tested class
            $helloToTest = new \HelloTheWorld();

            $this->assert
                        //we expect the getHiBob method to return a string
                        ->string($helloToTest->getHiBob())
                        //and the string should be Hi Bob !
                        ->isEqualTo('Hi Bob !');
        }
    }

Now, let's launch the tests

    [bash]
    php -f ./test/HelloTheWorld.php

You will see something like this

    [bash]
    > atoum version nightly-941-201201011548 by Frédéric Hardy (phar:///home/documentation/projects/tests/atoum/mageekguy.atoum.phar/1)
    > PHP path: /usr/bin/php5
    > PHP version:
    => PHP 5.3.6-13ubuntu3.3 with Suhosin-Patch (cli) (built: Dec 13 2011 18:37:10)
    => Copyright (c) 1997-2011 The PHP Group
    => Zend Engine v2.3.0, Copyright (c) 1998-2011 Zend Technologies
    =>     with Xdebug v2.1.2, Copyright (c) 2002-2011, by Derick Rethans
    > tests\units\HelloTheWorld...
    [S___________________________________________________________][1/1]
    => Test duration: 0.01 second.
    => Memory usage: 0.00 Mb.
    > Total test duration: 0.01 second.
    > Total test memory usage: 0.00 Mb.
    > Code coverage value: 100.00%
    > Running duration: 0.16 second.
    Success (1 test, 1/1 method, 2 assertions, 0 error, 0 exception) !

You're done, your code is rock solid !

### Rule of Thumb ###

The basics when you’re testing things using atoum are the following :
*    Tell atoum what you want to work on (a variable, an object, a string, an integer, …)
*    Tell atoum the state the element is expected to be in (is equal to, is null, exists, …).

In the above example we tested that the method getHiBob
*    did return a string (step 1),
*    and that this string was equal to « Hi Bob ! » (step 2).

### More asserters ###

There are of course a lot more asserters in atoum. To see the complete list, see chapter 2.

Let's see with a quick class some more asserters in atoum !

First, the class to be tested, located in PROJECT_PATH/classes/BasicTypes.php.

    [php]
    <?php
    class BasicTypes
    {
        public function getOne (){return 1;}
        public function getTrue(){return true;}
        public function getFalse(){return false;}
        public function getHello(){return 'hello';}
        public function create(){return new BasicTypes();}
        public function getFloat(){return 1.1;}
        public function getNull(){return null;}
        public function getEmptyArray(){return array();}
        public function getArraySizeOf3(){return range(0,2,1);}
    }

Now the test class, located in PROJECT_PATH/tests/BasicTypes.php

    [php]
    <?php //...
    class BasicTypes extends atoum\test
    {
        public function testBoolean ()
        {
            $bt = new \BasicTypes();
            $this->assert
                    ->boolean($bt->getFalse())
                        ->isFalse()//getFalse retourne bien false
                    ->boolean($bt->getTrue())
                        ->isTrue();//getTrue retourne bien true
        }

        public function testInteger ()
        {
            $bt = new \BasicTypes();
            $this->assert
                    ->integer($bt->getOne())
                    ->isEqualTo(1)
                    ->isGreaterThan(0);
        }

        public function testString()
        {
            $bt = new \BasicTypes();
            $this->assert
                    ->string($bt->getHello())
                    ->isNotEmpty()
                    ->isEqualTo('hello');
        }

        public function testObject ()
        {
            $bt = new \BasicTypes();
            $this->assert
                    ->object($bt->create())
                    ->isInstanceOf('BasicTypes')
                    ->isNotIdenticalTo($bt);//Une nouvelle instance
        }

        public function testFloat()
        {
            $bt = new \BasicTypes();
            $this->assert
                    ->float($bt->getFloat())
                    ->isEqualTo(1.1);
        }

        public function testArray()
        {
            $bt = new \BasicTypes();
            $this->assert
                    ->array($bt->getArraySizeOf3())
                        ->hasSize(3)
                        ->isNotEmpty()
                    ->array($bt->getEmptyArray())
                        ->isEmpty();
        }

        public function testNull ()
        {
            $bt = new \BasicTypes();
            $this->assert
                    ->variable($bt->getNull())
                    ->isNull();
        }
    }

Here you are, you saw a complet and basic example of tests using atoum.

### Testing a Singleton ###

To test if your method always returns the same instance of the same object, you can ask atoum to
check that the instances are identicals.

    [php]
    <?php //...
    class Singleton extends atoum\test
    {
        public function testGetInstance()
        {
            $this->assert
                    ->object(\Singleton::getInstance())
                        ->isInstanceOf('Singleton')
                        ->isIdenticalTo(\Singleton::getInstance());
        }
    }

### Testing exceptions ###

To test exceptions atoum is using closures (introduced in PHP 5.3).

    [php]
    class ExceptionLauncher extends atoum\test
    {
        public function testLaunchException ()
        {
            $exception = new \ExceptionLauncher();
            $this->assert
                     ->exception(function()use($exception){
                                    $exception->launchException();
                                })
                     ->isInstanceOf('LaunchedException')
                     ->hasMessage('Message in the exception');

        }
    }

### Testing errors ###

Again, atoum is nicely using closure to test errors (NOTICE, WARNING, …) :

    [php]
    class RaiseError extends atoum\test
    {
        public function testRaiseError ()
        {
            $error = new \RaiseError();

            $this->assert->object($error);
            $this->assert
                     ->when(function()use($error){
                            $error->raise();
                     })
                     ->error('This is an error', E_USER_WARNING)
                        ->exists();
                     //Sachant qu'il est possible de ne spécifier
                     // ni message ni type attendu.
        }
    }

### Testing using Mocks ###

Mocks are of course supported by atoum !
Generating a Mock from an interface

atoum can generate a mock directly from an interface.

    [php]
    class UsingWriter extends atoum\test
    {
        public function testWithMockedInterface ()
        {
            $this->mockGenerator->generate('\IWriter');
            $mockIWriter = new \mock\IWriter;

            $usingWriter = new \UsingWriter();
            //La méthode setIWriter attends un objet
            //qui implemente l'interface IWriter
            //  (setIWriter (IWriter $writer))
            $usingWriter->setIWriter($mockIWriter);

            $this->assert
                    ->when(function () use($usingWriter) {
                                    $usingWriter->write('hello');
                    })
                    ->mock($mockIWriter)
                        ->call('write')
                        ->once();
        }
    }

### Generating a Mock from a class ###

atoum can generate a mock directly from a class definition.

    [php]
    public function testWithMockedObject ()
    {
        $this->mockGenerator->generate('\Writer');
        $mockWriter = new \mock\Writer;

        $usingWriter = new \UsingWriter();
        //La méthode setWriter attends un objet
        //de type Writer (setWriter (Writer $writer))
        $usingWriter->setWriter($mockWriter);

        $this->assert
                ->when(function () use($usingWriter) {
                                $usingWriter->write('hello');
                })
                ->mock($mockWriter)
                    ->call('write')
                    ->once();
    }

There is also a shorter syntax to generate mock from a class definition.

    [php]
    public function testWithMockedObject ()
    {
        $mockWriter = new \mock\Writer;

        //...
    }

atoum is able to automatically find the class definition to mock on demand so you don't have to call
the mock generator.

When requesting a mock instance for a class, do not forget to specify the full class path (including
namespaces).

    [php]
    namespace Package\Writers
    {
        class SampleWriter implements Writer
        {
            //...
        }

    }

    namespace
    {
        class UsingWriter 
        {
            public function write(\Package\Writers\Writer $writer, $string) 
            {
                $writer->write($string);
            }
        }
    }

In this example, the class we want to mock lives in the Package\Writers namespace, so to request a
mock in our test we should do :

    [php]
    namespace Package\test\units;

    class UsingWriter extends atoum\test
    {
        public function testWrite()
        {                     
            $this
                ->if($mockWriter = new \mock\Package\Writers\SampleWriter())
                ->then()
                    ->when(function() use($mockWriter) {
                        $usingWriter = new \UsingWriter();
                        $usingWriter->write($mockWriter, 'Hello World!');  
                    })	
                    ->mock($mockWriter)
                        ->call('write')
                        ->withArguments('Hello World!')
                        ->once()
            ;
        }
    }

### Generating a Mock from scratch ###

atoum can also let you create and completely specify a mock object.

    [php]
    $this->mockGenerator->generate('WriterFree');
    $mockWriter = new \mock\WriterFree;
    $mockWriter->getMockController()->write = function($text){};

    $usingWriter = new \UsingWriter();
    $usingWriter->setFreeWriter($mockWriter);

    $this->assert
            ->when(function () use($usingWriter) {
                            $usingWriter->write('hello');
            })
            ->mock($mockWriter)
                ->call('write')
                ->once();
