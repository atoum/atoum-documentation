.. _introduction-anchor:

Introduction
============

.. _qu-est-ce-que-atoum:

Qu’est-ce que atoum ?
---------------------
atoum est un framework de test unitaire, tout comme PHPUnit ou SimpleTest, mais il présente quelques avantages par rapport à ces derniers :

* il est moderne et utilise les innovations des dernières versions de PHP (>= 5.3) ;
* il est simple et facile à maitriser ;
* il est intuitif, sa syntaxe se veut la plus proche du langage naturel anglais.


.. _telechargement-et-installation:

Téléchargement et installation
------------------------------

Pour le moment, atoum n’a pas de numéro de version. Il est néanmoins d’ores et déjà stable et utilisable. Si vous souhaitez l’utiliser, il vous suffit de télécharger la dernière version. Malgré les évolutions constantes d’atoum, la rétrocompatibilité est une des priorités de ses développeurs.
Vous pouvez installer atoum de plusieurs manières :

* :ref:`en téléchargeant l’archive PHAR <archive-p-h-a-r>` ;
* :ref:`à l’aide de composer <composer-anchor>` ;
* :ref:`en utilisant un script d’installation <script-d-installation>` ;
* :ref:`en clonant le dépôt github <github-anchor>` ;
* :ref:`en utilisant un plug-in symfony 1 <plugin-symfony-1>` ;
* :ref:`en utilisant un bundle Symfony 2 <bundle-symfony-2>`.
* :ref:`en utilisant un composant Zend Framework 2 <composant-zend-framework-2>`.


.. _archive-p-h-a-r:

Archive PHAR
~~~~~~~~~~~~

Une archive PHAR (PHp ARchive) est créée automatiquement à chaque modification d’atoum.

PHAR est un format d’archive d’application PHP, disponible depuis PHP 5.3.

.. _installation-anchor:

Installation
^^^^^^^^^^^^

Vous pouvez télécharger la dernière version stable d’atoum directement depuis le site officiel : `http://downloads.atoum.org/nightly/mageekguy.atoum.phar <http://downloads.atoum.org/nightly/mageekguy.atoum.phar>`_

.. _mise-a-jour:

Mise à jour
^^^^^^^^^^^

La mise à jour de l’archive est très simple. Il vous suffit de lancer la commande suivante :

.. code-block:: shell

   $ php -d phar.readonly=0 mageekguy.atoum.phar --update

.. note::
   La mise à jour d’atoum nécessite la modification de l’archive PHAR. Or par défaut, la configuration de PHP ne l’autorise pas. Voilà pourquoi il faut utiliser la directive ``-d phar.readonly=0``.


Si une version plus récente existe, elle sera alors téléchargée automatiquement et installée au sein de l’archive :

.. code-block:: shell

   $ php -d phar.readonly=0 mageekguy.atoum.phar --update
   Checking if a new version is available... Done !
   Update to version 'nightly-1568-201210311708'... Done !
   Enable version 'nightly-1568-201210311708'... Done !
   Atoum was updated to version 'nightly-1568-201210311708' successfully !

S’il n’existe pas de version plus récente, atoum s’arrêtera immédiatement :

.. code-block:: shell

   $ php -d phar.readonly=0 mageekguy.atoum.phar --update
   Checking if a new version is available... Done !
   There is no new version available !

Atoum ne demande aucune confirmation de la part de l’utilisateur pour réaliser la mise à jour, car il est très facile de revenir à une version précédente.

.. _lister-les-versions-contenues-dans-l-archive:

Lister les versions contenues dans l’archive
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Pour afficher les versions contenues dans l’archive au fur et à mesure des mises à jour, il faut faire appel à l’argument ``--list-available-versions``, ou ``-lav`` en version abrégée :

.. code-block:: shell

   $ php mageekguy.atoum.phar -lavnightly-941-201201011548

   * nightly-1568-201210311708

La liste des versions présentes dans l’archive est alors affichée, la version actuellement active étant précédée de ``*``.

.. _changer-la-version-courante:

Changer la version courante
^^^^^^^^^^^^^^^^^^^^^^^^^^^

Pour activer une autre version, il suffit d’utiliser l’argument ``--enable-version``, ou ``-ev`` en version abrégée, suivi du nom de la version à utiliser :

.. code-block:: shell

   $ php -d phar.readonly=0 mageekguy.atoum.phar -ev DEVELOPMENT

.. note::
   La modification de la version courante nécessite la modification de l’archive PHAR. Or par défaut, la configuration de php ne l’autorise pas. Voilà pourquoi il faut utiliser la directive ``-d phar.readonly=0``.


.. _suppression-d-anciennes-versions:

Suppression d’anciennes versions
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Au cours du temps, l’archive peut contenir plusieurs versions d’atoum qui ne sont plus utilisées.

Pour les supprimer, il suffit d’utiliser l’argument ``--delete-version``, ou ``-dv`` dans sa version abrégée, suivi du nom de la version à supprimer :

.. code-block:: shell

   $ php -d phar.readonly=0 mageekguy.atoum.phar -dv nightly-941-201201011548

La version est alors supprimée.

.. warning::
   Il n’est pas possible de supprimer la version active.


.. note::
   La suppression d’une version nécessite la modification de l’archive PHAR. Or par défaut, la configuration de PHP ne l’autorise pas. Voilà pourquoi il faut utiliser la directive ``-d phar.readonly=0``.



.. _composer-anchor:

Composer
~~~~~~~~

`Composer <http://getcomposer.org>`_ est un outil de gestion de dépendance en PHP.

Commencez par installer composer :

.. code-block:: shell

   $ curl -s https://getcomposer.org/installer | php

Créez ensuite un fichier ``composer.json`` contenant le JSON (JavaScript Object Notation) suivant :

.. code-block:: json

   {
       "require" : {
           "atoum/atoum" : "dev-master"
       }
   }

Enfin, exécutez la commande suivante :

.. code-block:: shell

   $ php composer.phar install

.. _script-d-installation:

Script d’installation
~~~~~~~~~~~~~~~~~~~~~

Vous pouvez installer atoum grâce à un `script <https://github.com/atoum/atoum-installer>`_ dédié à cette tâche :

.. code-block:: shell

   $ curl https://raw.github.com/atoum/atoum-installer/master/installer | php -- --phar
   $ php mageekguy.atoum.phar -v
   atoum version nightly-xxxx-yyyymmddhhmm by Frédéric Hardy (phar:///path/to/mageekguy.atoum.phar)

Vous pourrez facilement installer atoum localement (dans un projet, voir exemple précédent) ou globalement sur le système :

.. code-block:: shell

   $ curl https://raw.github.com/atoum/atoum-installer/master/installer | sudo php -- --phar --global
   $ which atoum
   /usr/local/bin/atoum

Des options sont disponibles pour personnaliser l’installation d’atoum : reportez-vous à `la documentation (en) <https://github.com/atoum/atoum-installer/blob/master/README.md>`_ de l’installeur pour obtenir plus de détails.

.. _github-anchor:

Github
~~~~~~

Si vous souhaitez utiliser atoum directement depuis ses sources, vous pouvez cloner ou « forker » le dépôt github : `git://github.com/atoum/atoum.git <git://github.com/atoum/atoum.git>`_


.. _plugin-symfony-1:

Plugin symfony 1
~~~~~~~~~~~~~~~~

Si vous souhaitez utiliser atoum au sein d’un projet symfony 1, un plug-in existe et est disponible à l’adresse suivante : `https://github.com/atoum/sfAtoumPlugin <https://github.com/atoum/sfAtoumPlugin>`_.

Toutes les instructions pour son installation et son utilisation se trouvent `dans le cookbook <chapitre4.html#Utilisation-avec-symfony -1.4>`_ ainsi que sur son site.


.. _bundle-symfony-2:

Bundle Symfony 2
~~~~~~~~~~~~~~~~

Si vous souhaitez utiliser atoum au sein d’un projet Symfony 2, un bundle existe et est disponible à l’adresse suivante : `https://github.com/atoum/AtoumBundle <https://github.com/atoum/AtoumBundle>`_.

Toutes les instructions pour son installation et son utilisation sont disponibles sur cette page.


.. _composant-zend-framework-2:

Composant Zend Framework 2
~~~~~~~~~~~~~~~~~~~~~~~~~~

Si vous souhaitez utiliser atoum au sein d’un projet Zend Framework 2, un composant existe et est disponible à l’adresse suivante : `https://github.com/blanchonvincent/zend-framework-test-atoum <https://github.com/blanchonvincent/zend-framework-test-atoum>`_.

Toutes les instructions pour son installation et son utilisation sont disponibles sur cette page.


.. _la-philosophie-d-atoum:

La philosophie d’atoum
----------------------

.. _exemple-simple:

Exemple simple
~~~~~~~~~~~~~~

Vous devez écrire une classe de test pour chaque classe à tester.

Imaginez que vous vouliez tester la traditionnelle classe ``HelloWorld``, alors vous devez créer la classe de test ``test\units\HelloWorld``.

.. note::
   atoum utilise les espaces de noms. Par exemple, pour tester la classe ``Vendor\Project\HelloWorld``, vous devez créer la classe ``Vendor\Project\tests\units\HelloWorld``.


Voici le code de la classe ``HelloWorld`` que nous allons tester.

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

Maintenant, voici le code de la classe de test que nous pourrions écrire.

.. code-block:: php

   <?php
   # src/Vendor/Project/tests/units/HelloWorld.php

   // La classe de test à son propre namespace :
   // Le namespace de la classe à tester + "tests\units"
   namespace Vendor\Project\tests\units;

   // Vous devez inclure la classe à tester
   require_once __DIR__ . '/../../HelloWorld.php';

   use \atoum;

   /*
    * Classe de test pour \HelloWorld

    * Remarquez qu’elle porte le même nom que la classe à tester
    * et qu’elle dérive de la classe atoum
    */
   class HelloWorld extends atoum
   {
       /*
        * Cette méthode est dédiée à la méthode getHiAtoum()
        */
       public function testGetHiAtoum ()
       {
           // création d’une nouvelle instance de la classe à tester
           $helloToTest = new \Vendor\Project\HelloWorld();

           $this
               // nous testons que la méthode getHiAtoum retourne bien
               // une chaîne de caractère...
               ->string($helloToTest->getHiAtoum())
                   // ... et que la chaîne est bien celle attendue,
                   // c’est-à-dire 'Hi atoum !'
                   ->isEqualTo('Hi atoum !')
           ;
       }
   }

Maintenant, lançons nos tests.
Vous devriez voir quelque chose comme ça :

.. code-block:: shell

   $ php vendor/mageekguy.atoum.phar -f src/Vendor/Project/tests/units/HelloWorld.php
   > PHP path: /usr/bin/php
   > PHP version:
   > PHP 5.4.7 (cli) (built : Sep 13 2012 04:24:47)
   > Copyright (c) 1997-2012 The PHP Group
   > Zend Engine v2.4.0, Copyright (c) 1998-2012 Zend Technologies
   >     with Xdebug v2.2.1, Copyright (c) 2002-2012, by Derick Rethans
   > Vendor\Project\tests\units\HelloWorld...
   [S___________________________________________________________][1/1]
   > Test duration : 0.02 second.
   > Memory usage : 0.00 Mb.
   > Total test duration: 0.02 second.
   > Total test memory usage: 0.00 Mb.
   > Code coverage value: 100.00%
   > Running duration: 0.34 second.
   Success (1 test, 1/1 method, 2 assertions, 0 error, 0 exception) !

Nous venons de tester que la méthode ``getHiAtoum`` :

* retourne bien une chaîne de caractère ;
* que cette dernière est bien égale à ``"Hi atoum !"``.

Les tests sont passés, tout est au vert. Voilà, votre code est solide comme un roc grâce à atoum !


.. _principes-de-base:

Principes de base
~~~~~~~~~~~~~~~~~
Lorsque vous voulez tester une valeur, vous devez :

* indiquer le type de cette valeur (entier, décimal, tableau, chaîne de caractères, etc.) ;
* indiquer les contraintes devant s’appliquer à cette valeur (égal à, nulle, contenant quelque chose, etc.).


.. _integration-d-atoum-dans-votre-i-d-e:

Intégration d’atoum dans votre IDE
----------------------------------

.. _sublime-text-2:

Sublime Text 2
~~~~~~~~~~~~~~

Un `plug-in pour SublimeText 2 <https://github.com/toin0u/Sublime-atoum>`_ permet l’exécution des tests unitaires par atoum et la visualisation du résultat sans quitter l’éditeur.

Les informations nécessaires à son installation et à sa configuration sont disponibles `sur le blog de son auteur <http://sbin.dk/2012/05/19/atoum-sublime-text-2-plugin/>`_.

.. _v-i-m:

VIM
~~~

atoum est livré avec un plug-in facilitant son utilisation dans l’éditeur VIM.

Il permet d’exécuter les tests sans quitter VIM et d’obtenir le rapport correspondant dans une fenêtre de l’éditeur.

Il est alors possible de naviguer parmi les éventuelles erreurs, voire de se rendre à la ligne correspondant à une assertion ne passant pas à l’aide d’une simple combinaison de touches.

.. _installation-du-plug-in-atoum-pour-v-i-m:

Installation du plug-in atoum pour VIM
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Si vous n’utilisez pas atoum sous la forme d’une archive PHAR, vous trouverez le fichier correspondant au plug-in, nommé ``atoum.vba``, dans le répertoire ``ressources/vim``.

Dans le cas contraire, il faut demander à l’archive PHAR d’atoum d’extraire le fichier à l’aide de la commande suivante :

.. code-block:: shell

   $ php mageekguy.atoum.phar --extractResourcesTo path/to/a/directory

Une fois l’extraction réalisée, le fichier ``atoum.vba`` correspondant au plug-in pour VIM sera dans le répertoire ``path/to/a/directory/ressources/vim``.

Une fois en possession du fichier ``atoum.vba``, il faut l’éditer à l’aide de VIM :

.. code-block:: shell

   $ vim path/to/atoum.vba

Il n’y a plus ensuite qu’à demander à VIM l’installation du plug-in à l’aide de la commande :

.. code-block:: vim

   :source %

.. _utilisation-du-plug-in-atoum-pour-v-i-m:

Utilisation du plug-in atoum pour VIM
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Pour utiliser le plug-in, atoum doit évidemment être installé et vous devez être en train d’éditer un fichier contenant une classe de tests unitaires basée sur atoum.

Une fois dans cette configuration, la commande suivante lancera l’exécution des tests :

.. code-block:: vim

   :Atoum

Les tests sont alors exécutés, et une fois qu’ils sont terminés, un rapport basé sur le fichier de configuration de atoum qui se trouve dans le répertoire ``ftplugin/php/atoum.vim`` de votre répertoire ``.vim`` est généré dans une nouvelle fenêtre.

Évidemment, vous êtes libre de lier cette commande à la combinaison de touches de votre choix, en ajoutant par exemple la ligne suivante dans votre fichier ``.vimrc`` :

.. code-block:: vim

   nnoremap *.php <F12> :Atoum<CR>

L’utilisation de la touche ``F12`` de votre clavier en mode normal appellera alors la commande ``:Atoum``.

.. _gestion-des-fichiers-de-configuration-de-atoum:

Gestion des fichiers de configuration de atoum
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Vous pouvez indiquer un autre fichier de configuration pour atoum en ajoutant la ligne suivante à votre fichier ``.vimrc`` :

.. code-block:: vim

   call atoum#defineConfiguration('/path/to/project/directory', '/path/to/atoum/configuration/file', '.php')

La fonction ``atoum#defineConfiguration`` permet en effet de définir le fichier de configuration à utiliser en fonction du répertoire ou se trouve le fichier de tests unitaires.
Elle accepte pour cela trois arguments :

* un chemin d’accès vers le répertoire contenant les tests unitaires ;
* un chemin d’accès vers le fichier de configuration de atoum devant être utilisé ;
* l’extension des fichiers de tests unitaires concernés.

Pour plus de détails sur l’utilisation du plug-in, une aide est disponible dans VIM à l’aide de la commande suivante :

.. code-block:: vim

   :help atoum

.. _ouvrir-automatiquement-les-tests-en-echec:

Ouvrir automatiquement les tests en échec
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
atoum est capable d’ouvrir automatiquement les fichiers des tests en échec à la fin de l’exécution. Plusieurs éditeurs sont actuellement supportés :

* :ref:`macvim <macvim-anchor>` (Mac OS X)
* :ref:`gvim <gvim-anchor>` (Unix)
* :ref:`PhpStorm <php-storm>` (Mac OS X/Unix)
* :ref:`gedit <gedit-anchor>` (Unix)

Pour utiliser cette fonctionnalité, vous devrez modifier le `fichier de configuration <chapitre3.html#fichier-de-configuration>`_ de atoum :

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

Si vous travaillez sous Mac OS X, utilisez la configuration suivante :

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
       // Si PhpStorm est installé dans /Applications
       ->addField(new macos\phpstorm())

       // Si vous avez installé PhpStorm
       // dans un dossier différent de /Applications
       // ->addField(
       //     new macos\phpstorm(
       //         '/path/to/PhpStorm.app/Contents/MacOS/webide'
       //     )
       // )
   ;

   $runner->addReport($cliReport);


Dans un environnement Unix, utilisez la configuration suivante :

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
