# Cookbook

## Test d'un singleton

Pour tester si une méthode retourne bien systématiquement la même instance d'un objet,
vérifiez que deux appels successifs à la méthode testée sont bien identiques.

    [php]
    $this
        ->object(\Singleton::getInstance())
            ->isInstanceOf('Singleton')
            ->isIdenticalTo(\Singleton::getInstance())
    ;


## Utilisation dans behat

TODO: https://github.com/atoum/atoum/wiki/atoum-et-Behat

## Utilisation dans Jenkins (ou Hudson)

TODO: https://github.com/atoum/atoum/wiki/atoum-et-Jenkins-(ou-Hudson)

## Hook git

Une bonne pratique, lorsqu'on utilise un logiciel de gestion de versions, est de ne jamais
ajouter à un dépôt du code non fonctionnel, afin de pouvoir récupérer une version propre
et utilisable du code à tout moment et à n'importe quel endroit de l'historique du dépôt.

Cela implique donc, entre autre, que les tests unitaires doivent passer dans leur
intégralité avant que les fichiers créés ou modifiés soient ajoutés au dépôt, et en
conséquence, le développeur est censé exécuter les tests unitaires avant d'intégrer
son code dans le dépôt.  

Cependant, dans les faits, il est très facile pour le développeur d'omettre cette étape,
et votre dépôt peut donc contenir à plus ou moins brève échéance du code ne respectant
 pas les contraintes imposées par les tests unitaires.

Heureusement, les logiciels de gestion de versions en général et Git en particulier dispose
d'un mécanisme, connu sous le nom de*hook de pré-commit permettant d'exécuter
automatiquement des tâches lors de l'ajout de code dans un dépôt.

L'installation d'un *hook de pré-commit* est très simple et se déroule en deux étapes.

### Étape 1 : Création du script à exécuter

Lors de l'ajout de code à un dépôt, Git recherche le fichier `.git/hook/pre-commit` à la
racine du dépôt et l'exécute s'il existe et qu'il dispose des droits nécessaires.

Pour mettre en place le *hook*, il vous faut donc créer le fichier `.git/hook/pre-commit`
et y ajouter le code suivant :

    [php]
    #!/usr/bin/env php
    <?php
    
    $_SERVER['_'] = '/usr/bin/php';
    
    exec('git diff --cached --name-only --diff-filter=ACMR | grep ".php"', $phpFiles);
    
    if ($phpFilesNumber = sizeof($phpFiles) > 0)
    {
       echo $phpFilesNumber . ' PHP files staged, launch all unit test...' . PHP_EOL;
    
       foreach (new \recursiveIteratorIterator(new \recursiveDirectoryIterator(__DIR__ . '/../../')) as $path => $file)
       {
         if (substr($path, -4) === '.php' && strpos($path, '/Tests/Units/') !== false)
         {
           require_once $path;
         }
       }
    }
    
Le code ci-dessous suppose que vos tests unitaires sont dans des fichiers ayant
l'extension `.php` et dans des répertoires dont le chemin contient `/Tests/Units/`.
Si ce n'est pas votre cas, vous devrez modifier le script suivant votre contexte.

**Note**: dans l'exemple ci-dessus, les fichiers de test doivent inclure atoum pour
que le hook fonctionne.


Les tests étant executés très rapidement avec atoum, on peut donc lancer l'ensemble
des tests unitaires avant chaque commit avec un hook comme celui-ci :

    [php]
    #!/bin/sh
    ./bin/atoum -d tests/

### Étape 2 : Ajout des droits d'exécution

Pour être utilisable par Git, le fichier `.git/hook/pre-commit` doit être rendu
exécutable à l'aide de la commande suivante, exécutée en ligne de commande à
partir du répertoire de votre dépôt :

    [shell]
    chmod u+x `.git/hook/pre-commit`

À partir de cet instant, les tests unitaires contenus dans les répertoires dont le
chemin contient `/Tests/Units/` seront lancés automatiquement lorsque vous effectuerez
la commande `git commit`, si des fichiers ayant l'extension `.php` ont été modifiés.

Et si d'aventure un test ne passe pas, les fichiers ne seront pas ajoutés au dépôt.
Il vous faudra alors effectuer les corrections nécessaires, utiliser la commande `git add`
sur les fichiers modifiés et utiliser à nouveau `git commit`.


## Changer l'espace de nom par défaut

Au début de l'exécution d'une classe de test, atoum calcule le nom de la classe testée.
Pour cela, par défaut, il remplace dans le nom de la classe de test l'expression 
régulière `#(?:^|\\\)tests?\\\units?\\\#i` par le caractère `\`.

Ainsi, si la classe de test porte le nom `\vendor\project\tests\units\foo`, il en déduira 
que la classe testée porte le nom `\vendor\project\foo`.
Cependant, il peut être nécessaire que l'espace de nom des classes de test ne corresponde
pas à cette expression régulière, et dans ce cas, *atoum* s'arrête alors avec le message d'erreur suivant :

    [shell]
    ==> exception 'mageekguy\atoum\exceptions\runtime' with message 'Test class 'project\vendor\my\tests\foo' is not in a namespace which match pattern '#(?:^|\\)ests?\\unit?s\\#i'' in /path/to/unit/tests/foo.php


Il faut donc modifier l'expression régulière utilisée, et il est possible de le faire de
plusieurs manières.
La plus simple est de faire appel à l'annotions `@namespace` appliquée à la classe de test,
de la manière suivante :

    [php]
    <?php
    
    namespace vendor\project\my\tests;
    
    require_once __DIR__ . '/mageekguy.atoum.phar';
    
    use mageekguy\atoum;
    
    /**
     * @namespace #\\\my\\\tests\\\#
     */
    abstract class aClass extends atoum\test
    {
       public function testBar()
       {
          /* ... */
       }
    }


Cette méthode est simple et rapide à mettre en œuvre, mais elle présente l'inconvénient de devoir
être répétée dans chaque classe de test, ce qui peut compliquer leur maintenance en cas de
modification de leur espace de nom.
L'alternative consiste à faire appel à la méthode `\mageekguy\atoum\test::setTestNamespace()` dans
le constructeur de la classe de test, de la manière suivante :

    [php]
    <?php
    
    namespace vendor\project\my\tests;
    
    require_once __DIR__ . '/mageekguy.atoum.phar';
    
    use mageekguy\atoum;
    
    abstract class aClass extends atoum\test
    {
       public function __construct(score $score = null, locale $locale = null, adapter $adapter = null)
       {
          $this->setTestNamespace('#\\\my\\\tests\\\#');
    
          parent::__construct($score, $locale, $adapter);
       }
    
       public function testBar()
       {
          /* ... */
       }
    }
    

La méthode `\mageekguy\atoum\test::setTestNamespace()` accepte en effet un unique argument qui
doit être l'expression régulière correspondant à l'espace de nom de votre classe de test.
Et pour ne pas avoir à répéter l'appel à cette méthode dans chaque classe de test, il suffit
de le faire une bonne fois pour toute dans une classe abstraite de la manière suivante :

    [php]
    <?php

    namespace vendor\project\my\tests;
    
    require_once __DIR__ . '/mageekguy.atoum.phar';
    
    use mageekguy\atoum;
    
    abstract class Test extends atoum\test
    {
       public function __construct(score $score = null, locale $locale = null, adapter $adapter = null)
       {
           $this->setTestNamespace('#\\\my\\\tests\\\#');
    
          parent::__construct($score, $locale, $adapter);
       }
    }
    

Ainsi, vous n'aurez plus qu'à faire dériver vos classes de tests unitaires de cette classe abstraite :

    [php]
    <?php
    
    namespace vendor\project\my\tests\modules;
    
    require_once __DIR__ . '/mageekguy.atoum.phar';
    
    use mageekguy\atoum;
    use vendor\project\my\tests;
    
    class aModule extends tests\Test
    {
       public function testDoSomething()
       {
          /* ... */
       }
    }
    

En cas de modification de l'espace de nommage réservé aux tests unitaires, il ne sera donc
nécessaire de ne modifier que la classe abstraite.

De plus, il n'est pas obligatoire d'utiliser une expression régulière, que ce soit au niveau de
l'annotation `@namespace` ou de la méthode  `\mageekguy\atoum\test::setTestNamespace()`, et une
simple chaîne de caractères peut également fonctionner.

En effet, *atoum* fait appel par défaut à une expression régulière afin que son utilisateur
puisse utiliser par défaut un large panel d'espaces de nom sans avoir besoin de le configurer
à ce niveau.
Cela lui permet donc d'accepter par exemple sans configuration particulière les espaces de nom
suivants :

* `\test\unit\`
* `\Test\Unit\`
* `\tests\units\`
* `\Tests\Units\`
* `\TEST\UNIT\`

Cependant, en règle général, l'espace de nom utilisé pour les classes de test est fixe,
et il n'est donc pas nécessaire de recourir à une expression régulière si celle par défaut
ne convient pas.
Dans notre cas, elle pourrait être remplacé par la chaîne de caractères `\my\tests`, par
 exemple grâce à l'annotation `@namespace` :

    [php]
    <?php
    
    namespace vendor\project\my\tests;
    
    require_once __DIR__ . '/mageekguy.atoum.phar';
    
    use mageekguy\atoum;
    
    /**
     * @namespace \my\tests\
     */
    abstract class aClass extends atoum\test
    {
       public function testBar()
       {
          /* ... */
       }
    }
    
Attention, il était possible auparavant d'utiliser la méthode
`\mageekguy\atoum\test::setTestsSubNamespace()`, mais cette dernière est dépréciée
en faveur de `\mageekguy\atoum\test::setTestNamespace()`.


## Utilisation avec ezPublish 

TODO: https://github.com/atoum/atoum/wiki/Utiliser-atoum-avec-eZ-publish

## Utilisation avec Symfony 2

Si vous souhaitez utiliser atoum au sein de vos projets Symfony, vous pouvez installer le Bundle
[JediAtoumBundle](#bundle-symfony-2).

Si vous souhaitez installer et configurer atoum manuellement, voici comment faire.

### Étape 1: installation d'atoum

Si vous utilisez Symfony 2.0, [téléchargez l'archive PHAR](#archive-phar) et placez-la dans le
répertoire vendor qui est à la racine de votre projet.

Si vous utilisez Symfony 2.1, [ajoutez atoum dans votre fichier composer.json](#composer).

### Étape 2: création de la classe de test

Imaginons que nous voulions tester cet Entity:

    [php]
    <?php
    // src/Acme/DemoBundle/Entity/Car.php
    namespace Acme\DemoBundle\Entity;

    use Doctrine\ORM\Mapping as ORM;

    /**
     * Acme\DemoBundle\Entity\Car
     * @ORM\Table(name="car")
     * @ORM\Entity(repositoryClass="Acme\DemoBundle\Entity\CarRepository")
     */
    class Car
    {
        /**
         * @var integer $id
         * @ORM\Column(name="id", type="integer")
         * @ORM\Id
         * @ORM\GeneratedValue(strategy="AUTO")
         */
        private $id;

        /**
         * @var string $name
         * @ORM\Column(name="name", type="string", length=255)
         */
        private $name;

        /**
         * @var integer $max_speed
         * @ORM\Column(name="max_speed", type="integer")
         */

        private $max_speed;
    }

**Note**: pour plus d'informations sur la création d'Entity dans Symfony 2,
reportez-vous au
[manuel Symfony](http://symfony.com/fr/doc/current/book/doctrine.html#creer-une-classe-entite). 

Créez le répertoire Tests/Units dans votre Bundle (par exemple src/Acme/DemoBundle/Tests/Units).
C'est dans ce répertoire que seront stoqués tous les tests de ce Bundle.

Créez un fichier Test.php qui servira de base à tous les futurs tests de ce Bundle.

    [php]
    <?php
    // src/Acme/DemoBundle/Tests/Units/Test.php
    namespace Acme\DemoBundle\Tests\Units;

    // On inclus et active le class loader
    require_once __DIR__ . '/../../../../../vendor/symfony/symfony/src/Symfony/Component/ClassLoader/UniversalClassLoader.php';

    $loader = new \Symfony\Component\ClassLoader\UniversalClassLoader();

    $loader->registerNamespaces(
        array(
            'Symfony'         => __DIR__ . '/../../../../../vendor/symfony/src',
            'Acme\DemoBundle' => __DIR__ . '/../../../../../src'
        )
    );

    $loader->register();

    use mageekguy\atoum;

    // Pour Symfony 2.0 uniquement !
    require_once __DIR__ . '/../../../../../vendor/mageekguy.atoum.phar';

    abstract class Test extends atoum\test
    {
        public function __construct(
            adapter $adapter = null,
            annotations\extractor $annotationExtractor = null,
            asserter\generator $asserterGenerator = null,
            test\assertion\manager $assertionManager = null,
            \closure $reflectionClassFactory = null
        )
        {
            $this->setTestNamespace('Tests\Units');
            parent::__construct(
                $adapter,
                $annotationExtractor,
                $asserterGenerator,
                $assertionManager,
                $reflectionClassFactory
            );
        }
    }

**Note**: l'inclusion de l'archive PHAR d'atoum n'est nécessaire que pour Symfony 2.0.
Supprimez cette ligne dans le cas où vous utilisez Symfony 2.1

**Note**: par défaut, atoum utilise le namespace tests/units pour les tests.
Or Symfony 2 et son class loader exigent des majuscules au début des noms.
Pour cette raison, nous changeons le namespace des tests grâce à la méthode
setTestNamespace('Tests\Units').

### Étape 3: écriture d'un test

Dans le répertoire Tests/Units, il vous suffit de recréer l'arborescence des classes que vous
souhaitez tester (par exemple src/Acme/DemoBundle/Tests/Units/Entity/Car.php).

Créons notre fichier de test:

    [php]
    <?php
    // src/Acme/DemoBundle/Tests/Units/Entity/Car.php
    namespace Acme\DemoBundle\Tests\Units\Entity;

    require_once __DIR__ . '/../Test.php';

    use Acme\DemoBundle\Tests\Units\Test;

    class Car extends Test
    {
        public function testGetName()
        {
            $this
                ->if($car = new \Acme\DemoBundle\Entity\Car())
                ->and($car->setName('Batmobile'))
                    ->string($car->getName())
                        ->isEqualTo('Batmobile')
                        ->isNotEqualTo('De Lorean')
            ;
        }
    }

### Étape 4: lancement des tests

Si vous utilisez Symfony 2.0:

    [bash]
    # Lancement des tests d'un fichier
    php vendor/mageekguy.atoum.phar -f src/Acme/DemoBundle/Tests/Units/Entity/Car.php

    # Lancement de tous les tests du Bundle
    php vendor/mageekguy.atoum.phar -d src/Acme/DemoBundle/Tests/Units

Si vous utilisez Symfony 2.1:

    [bash]
    # Lancement des tests d'un fichier
    ./bin/atoum -f src/Acme/DemoBundle/Tests/Units/Entity/Car.php

    # Lancement de tous les tests du Bundle
    ./bin/atoum -d src/Acme/DemoBundle/Tests/Units

**Note**: vous pouvez obtenir plus d'informations sur le [lancement des tests](#lancement-des-tests)
au chapitre 3.


Dans tous les cas, voilà ce que vous devriez obtenir:

    [bash]
    > PHP path: /usr/bin/php
    > PHP version:
    => PHP 5.3.15 with Suhosin-Patch (cli) (built: Aug 24 2012 17:45:44)
    => Copyright (c) 1997-2012 The PHP Group
    => Zend Engine v2.3.0, Copyright (c) 1998-2012 Zend Technologies
    =>     with Xdebug v2.1.3, Copyright (c) 2002-2012, by Derick Rethans
    > Acme\DemoBundle\Tests\Units\Entity\Car...
    [S___________________________________________________________][1/1]
    => Test duration: 0.01 second.
    => Memory usage: 0.50 Mb.
    > Total test duration: 0.01 second.
    > Total test memory usage: 0.50 Mb.
    > Code coverage value: 42.86%
    => Class Acme\DemoBundle\Entity\Car: 42.86%
    ==> Acme\DemoBundle\Entity\Car::getId(): 0.00%
    ==> Acme\DemoBundle\Entity\Car::setMaxSpeed(): 0.00%
    ==> Acme\DemoBundle\Entity\Car::getMaxSpeed(): 0.00%
    > Running duration: 0.24 second.
    Success (1 test, 1/1 method, 0 skipped method, 4 assertions) !    