# Lancement des tests

## Exécutable

atoum dispose d'un exécutable qui vous permet de lancer vos tests en ligne de commande.

### Avec l'archive phar

Si vous utilisez l'archive phar, elle est elle-même l'exécutable.

#### linux / mac

    [bash]
    php path/to/mageekguy.atoum.phar

#### windows

    [bash]
    X:\Path\To\php.exe X:\Path\To\mageekguy.atoum.phar


### Avec les sources

Si vous utilisez les sources, l'exécutable se trouve dans path/to/atoum/bin.

#### linux / mac

    [bash]
    php path/to/bin/atoum

    # OU #

    ./path/to/bin/atoum

#### windows

    [bash]
    X:\Path\To\php.exe X:\Path\To\bin\atoum\bin


### Exemples dans le reste de la documentation

Dans les exemples suivants, les commandes pour lancer les tests avec atoum seront écrit avec la
forme suivante:

    [bash]
    ./bin/atoum

C'est exactement la commande que vous pourriez utiliser si vous avez
[installé atoum avec composer](#composer) sous Linux.



## Fichiers à exécuter

### Par fichiers

Pour lancer les tests d'un fichier, il vous suffit d'utiliser l'option -f ou --files.

    [bash]
    ./bin/atoum -f tests/units/MyTest.php


### Par répertoires

Pour lancer les tests d'un répertoire, il vous suffit d'utiliser l'option -d ou --directories.

    [bash]
    ./bin/atoum -d tests/units


## Filtres

Une fois que vous avez préciser à atoum [quels fichiers exécuter](#fichiers-a-executer), vous pouvez
filtrer ce qui sera réellement exécuter.

### Par espace de noms

Pour filtrer sur l'espace de noms, c'est à dire n'exécuter que les tests d'un espace de noms donné,
il vous suffit d'utiliser l'option -ns ou --namespaces.

    [bash]
    ./bin/atoum -d tests/units -ns mageekguy\\atoum\\tests\\units\\asserters

**Note**: il est important de doubler chaque backslash pour éviter qu'ils soient interprétés par le
shell.

### Une classe ou une méthode

Pour filtrer sur la classe ou une méthode, c'est à dire n'exécuter que les tests d'une classe ou
d'une méthode donnée, il vous suffit d'utiliser l'option -m ou --methods.

    [bash]
    ./bin/atoum -d tests/units -m mageekguy\\atoum\\tests\\units\\asserters\\string::testContains

**Note**: il est important de doubler chaque backslash pour éviter qu'ils soient interprétés par le
shell.

Vous pouvez remplacer le nom de la classe ou de la méthode par "*" pour signifier "tous".

Si vous remplacez le nom de la méthode par "*", cela revient à dire que vous filtrez par classe.

    [bash]
    ./bin/atoum -d tests/units -m mageekguy\\atoum\\tests\\units\\asserters\\string::*

Si vous remplacez le nom de la class par "*", cela revient à dire que vous filtrez par méthode.

    [bash]
    ./bin/atoum -d tests/units -m *::testContains

### Tags

Tout comme de nombreux outils dont [Behat](http://behat.org), atoum vous permet de tagger vos tests
unitaires et de n'exécuter que ceux ayant un ou plusieurs tags spécifiques.

Pour cela, il faut commencer par définir un ou plusieurs tags pour une ou plusieurs classes de tests
unitaires.

Cela se fait très simplement grâce aux annotations et à la balise @tags:

    [php]
    <?php

    namespace vendor\project\tests\units;

    require_once __DIR__ . '/mageekguy.atoum.phar';

    use mageekguy\atoum;

    /**
     * @tags thisIsOneTag thisIsTwoTag thisIsThreeTag
     */
    class foo extends atoum\test
    {
        public function testBar()
        {
            ...
        }
    }

De la même manière, il est également possible de tagger les méthodes de test.

*Note**: les tags définis au niveau d'une méthode prennent le pas sur ceux définis au niveau de la
classe.

    [php]
    <?php

    namespace vendor\project\tests\units;

    require_once __DIR__ . '/mageekguy.atoum.phar';

    use mageekguy\atoum;

    class foo extends atoum\test
    {
        /**
         * @tags thisIsOneMethodTag thisIsTwoMethodTag thisIsThreeMethodTag
         */
        public function testBar()
        {
            ...
        }
    }

Une fois les tags nécessaires définis, il n'y a plus qu'à exécuter les tests avec le ou les tags
adéquates à l'aide de l'option --tags, ou -t dans sa version courte:

    [shell]
    ./bin/atoum -d tests/units -t thisIsOneTag

Attention, cette instruction n'a de sens que s'il y a une ou plusieurs classes de tests unitaires et
qu'au moins l'une d'entres elles porte le tag spécifié. Dans le cas contraire, aucun test ne sera
exécuté.

Il est possible de définir plusieurs tags:

    [shell]
    ./bin/atoum -d tests/units -t thisIsOneTag thisIsThreeTag

Dans ce dernier cas, les classes de tests ayant été taggés soit avec thisIsOneTag, soit avec
thisIsThreeTag, seront les seules à être exécutées.

## Fichier de configuration

TODO

### Couverture du code

Par défaut, si PHP dispose de l'extension [Xdebug](http://xdebug.org), atoum indique en ligne de
commande le taux de couverture du code par les tests venant d'être exécutés.

Si le taux de couverture est de 100%, atoum se contente de l'indiquer. Mais dans le cas contraire,
il affiche le taux de couverture globale ainsi que celui de chaque méthode de la classe testée sous
la forme la forme d'un pourcentage.

    [shell]
    # php tests/units/classes/template.php 
    > atoum version DEVELOPMENT by Frederic Hardy (/Users/fch/Atoum)
    > PHP path: /usr/local/bin/php
    > PHP version:
    => PHP 5.3.8 (cli) (built: Sep 21 2011 23:14:37)
    => Copyright (c) 1997-2011 The PHP Group
    => Zend Engine v2.3.0, Copyright (c) 1998-2011 Zend Technologies
    =>     with Xdebug v2.1.1, Copyright (c) 2002-2011, by Derick Rethans
    > mageekguy\atoum\tests\units\template...
    [SSSSSSSSSSSSSSSSSSSSSSSSSSS_________________________________][27/27]
    => Test duration: 15.63 seconds.
    => Memory usage: 8.25 Mb.
    > Total test duration: 15.63 seconds.
    > Total test memory usage: 8.25 Mb.
    > Code coverage value: 92.52%
    => Class mageekguy\atoum\template: 91.14%
    ==> mageekguy\atoum\template::setWith(): 80.00%
    ==> mageekguy\atoum\template::resetChildrenData(): 25.00%
    ==> mageekguy\atoum\template::addToParent(): 0.00%
    ==> mageekguy\atoum\template::unsetAttribute(): 0.00%
    => Class mageekguy\atoum\template\data: 96.43%
    ==> mageekguy\atoum\template\data::__toString(): 0.00%
    > Running duration: 2.36 seconds.
    Success (1 test, 27 methods, 485 assertions, 0 error, 0 exception) !

Il est cependant possible d'obtenir une représentation plus précise du taux de couverture du code
par les tests, sous la forme d'un rapport au format HTML.

Pour l'obtenir, il suffit de se baser sur les modèles de fichiers de configuration inclus dans
atoum.

Si vous utliser l'archive PHAR, il faut les extraire en utilisant la commande suivante:

    [php]
    php mageekguy.atoum.phar -er /path/to/destination/directory

Une fois l'extraction effectuée, vous devriez avoir dans le répertoire
/path/to/destination/directory un répertoire nommé resources/configurations/runner.

Dans le cas ou vous utilisez atoum avec un ([clone du dépôt github](#github) ou avec
[composer](#composer), les modèles se trouvent dans /path/to/atoum/resources/configurations/runner

Dans ce répertoire, il y a, entre autre chose intéressante, un modèle de fichier de configuration
pour atoum nommé coverage.php.dist qu'il vous faudra copier à l'emplacement de votre choix, par
exemple sous le nom coverage.php.

Une fois la copie réalisée, il n'y a plus qu'à la modifier à l'aide de l'éditeur de votre choix afin
de définir le répertoire dans lequel les fichiers HTML devront être générés ainsi que l'URL à partir
de laquelle le rapport devra être accessible.

Par exemple:

    [php]
    $coverageField = new atoum\report\fields\runner\coverage\html(
        'Code coverage de mon projet',
        '/path/to/destination/directory'
    );

    $coverageField->setRootUrl('http://url/of/web/site');

**Note**: il est également possible de modifier le titre du rapport à l'aide du premier argument du
constructeur de la classe mageekguy\atoum\report\fields\runner\coverage\html.

Une fois tout cela effectué, il n'y a plus qu'à utiliser le fichier de configuration lors de
l'exécution des tests, de la manière suivante:

    [shell]
    ./bin/atoum -c path/to/coverage.php -d tests/units

Une fois les tests exécutés, atoum générera alors le rapport de couverture du code au format HTML
dans le répertoire que vous aurez défini précédemment, et il sera lisible à l'aide du navigateur de
votre choix.

**Note**: le calcul du taux de couverture du code par les tests ainsi que la génération du rapport
correspondant peuvent ralentir de manière notable l'exécution des tests.
Il peut être alors intéressant de ne pas utiliser systématiquement le fichier de configuration
correspondant, ou bien de les désactiver temporairement à l'aide de l'argument -ncc.

## Fichier de bootstrap

atoum autorise la définition d'un fichier de "bootstrap" qui sera exécuté avant chaque méthode de
test et qui permet donc d'initialiser l'environnement d'exécution des tests.

Il devient ainsi possible de définir, par exemple, une fonction d'auto-chargement de classes, de
lire un fichier de configuration ou de réaliser tout autre opération nécessaires à la bonne
exécution des tests.

La définition de ce fichier de "bootstrap" peut se faire de deux façons différentes, soit en ligne
de commande, soit via un fichier de configuration.

En ligne de commande, il faut utiliser au choix l'argument -bf ou l'argument --bootstrap-file suivi
du chemin relatif ou absolu vers le fichier concerné:

    [shell]
    ./bin/atoum -bf path/to/bootstrap/file

**Note**: un fichier de bootstrap n'est pas un fichier de configuration et n'a donc pas les mêmes
possibilités.

Dans un fichier de configuration, atoum est configurable via la variable $runner, qui n'est pas
définie dans un fichier de "bootstrap".

De plus, ils ne sont pas inclus au même moment, puisque le fichier de configuration est inclus par
atoum avant le début de l'exécution des tests mais après le lancement des tests, alors que le
fichier de "bootstrap", s'il est défini, est le tout premier fichier inclus par atoum proprement
dit. Enfin, le fichier de "bootstrap" peut permettre de ne pas avoir à inclure systématiquement le
fichier scripts/runner.php ou l'archive PHAR de atoum dans les classes de test.

Cependant, dans ce cas, il ne sera plus possible d'exécuter directement un fichier de test
directement via l'exécutable PHP en ligne de commande.

Pour cela, il suffit d'inclure dans le fichier de "bootstrap" le fichier scripts/runner.php ou
l'archive PHAR de atoum et d'exécuter systématiquement les tests en ligne de commande via
scripts/runner.php ou l'archive PHAR.

Le fichier de "bootstrap" doit donc au minimum contenir ceci:

    [php]
    <?php

    // si l'archive PHAR est utilisée :
    require_once path/to/mageekguy.atoum.phar;

    // ou si les sources sont utilisés :
    // require_once path/atoum/scripts/runner.php

## Option de la ligne de commande

La plupart des options existent sous 2 forme, une courte de 1 à 6 caractères et une longue, plus
explicative. Ces 2 formes font strictement la même chose. Vous pouvez utiliser indifférement l'une
ou l'autre forme.

Certaines options acceptent plusieurs valeurs :

    [bash]
    ./bin/atoum -f tests/units/MyFirstTest.php tests/units/MySecondTest.php


**Note**: vous ne devez mettre qu'une seule fois chaque option.
Dans le cas contraire, seul le dernier est pris en compte.

    [bash]
    # Ne test que MySecondTest.php
    ./bin/atoum -f MyFirstTest.php -f MySecondTest.php

    # Ne test que MyThirdTest.php et MyFourthTest.php
    ./bin/atoum -f MyFirstTest.php MySecondTest.php -f MyThirdTest.php MyFourthTest.php

### -bf <file> / --bootstrap-file <file>

Cette option vous permet de spécifier le chemin vers le fichier de bootstrap.

    [shell]
    ./bin/atoum -bf /path/to/bootstrap.php
    ./bin/atoum --bootstrap-file /path/to/bootstrap.php

### -c <file> / --configuration <file>

Cette option vous permet de spécifier le chemin vers le fichier de configuration à utiliser pour
lancer les tests.

    [shell]
    ./bin/atoum -c config/atoum.php
    ./bin/atoum --configuration tests/units/conf/coverage.php

### -d <directories> / --directories <directories>

Cette option vous permet de spécifier le ou les répertoires de tests à lancer.

    [shell]
    ./bin/atoum -d tests/units/db/
    ./bin/atoum --directories tests/units/db/ tests/units/entities/

### --debug

Cette option vous permet d'activer le mode debug

    [shell]
    ./bin/atoum --debug

**Note**: reportez-vous à la section sur le [mode debug](#le-mode-debug) pour avoir plus
d'informations.

### -drt <string> / --default-report-title <string>

Cette option vous permet de spécifier le titre par défaut des rapports générés par atoum.

    [shell]
    ./bin/atoum -drt Title
    ./bin/atoum --default-report-title "My Title"

**Note**: si le titre comporte des espaces, il faut obligatoirement l'entourer de guillemets.

### -f <files> / --files <files>

Cette option vous permet de spécifier le ou les fichiers de tests à lancer.

    [shell]
    ./bin/atoum -f tests/units/db/mysql.php
    ./bin/atoum --files tests/units/db/mysql.php tests/units/db/pgsql.php

### -ft / --force-terminal

Cette option vous permet de forcer la sortie vers le terminal.

    [shell]
    ./bin/atoum -ft
    ./bin/atoum --force-terminal

### -g <pattern> / --glob <pattern>

Cette option vous permet de spécifier les fichiers de tests à lancer en fonction d'un schéma.

    [shell]
    ./bin/atoum -g ???
    ./bin/atoum --glob ???

### -h / --help

Cette option vous permet d'afficher la liste des options disponibles.

    [shell]
    ./bin/atoum -h
    ./bin/atoum --help

### -l / --loop

Cette option vous permet d'activer le mode loop d'atoum.

    [shell]
    ./bin/atoum -l
    ./bin/atoum --loop

**Note**: reportez-vous à la section sur le [mode loop](#le-mode-loop) pour avoir plus
d'informations.

### -m <class::method> / --methods <class::methods>

Cette option vous permet de filtrer les classes et les méthodes à lancer.

    [shell]
    # lance uniquement la méthode testMyMethod de la classe vendor\\project\\test\\units\\myClass
    ./bin/atoum -m vendor\\project\\test\\units\\myClass::testMyMethod
    ./bin/atoum --methods vendor\\project\\test\\units\\myClass::testMyMethod

    # lance toutes les méthodes de test de la classe vendor\\project\\test\\units\\myClass
    ./bin/atoum -m vendor\\project\\test\\units\\myClass::*
    ./bin/atoum --methods vendor\\project\\test\\units\\myClass::*

    # lance uniquement les méthodes testMyMethod de toutes les classes de test
    ./bin/atoum -m *::testMyMethod
    ./bin/atoum --methods *::testMyMethod

**Note**: reportez-vous à la section sur les
[filtres par classe ou méthode](#une-classe-ou-une-methode) pour avoir plus d'informations.

### -mcn <integer> / --max-children-number <integer>

Cette option vous permet de définir le nombre maximum de processus lancé pour exécuter les tests.

    [shell]
    ./bin/atoum -mcn 5
    ./bin/atoum --max-children-number 3

### -ncc / --no-code-coverage

Cette option vous permet de désactiver la génération du rapport de la coverture de code.

    [shell]
    ./bin/atoum -ncc
    ./bin/atoum --no-code-coverage
    
### -nccfc <classes> / --no-code-coverage-for-classes <classes>

Cette option vous permet de désactiver la génération du rapport de la coverture de code pour une ou
plusieurs classe.

    [shell]
    ./bin/atoum -nccfc vendor\\project\\db\\mysql
    ./bin/atoum --no-code-coverage-for-classes vendor\\project\\db\\mysql vendor\\project\\db\\pgsql

**Note**: il est important de doubler chaque backslash pour éviter qu'ils soient interprétés par le
shell.
    
### -nccfns <namespaces> / --no-code-coverage-for-namespaces <namespaces>

Cette option vous permet de désactiver la génération du rapport de la coverture de code pour un ou
plusieurs espaces de noms.

    [shell]
    ./bin/atoum -nccfns vendor\\outside\\lib
    ./bin/atoum --no-code-coverage-for-namespaces vendor\\outside\\lib1 vendor\\outside\\lib2

**Note**: il est important de doubler chaque backslash pour éviter qu'ils soient interprétés par le
shell.
    
### -nccid <directories> / --no-code-coverage-in-directories <directories>

Cette option vous permet de désactiver la génération du rapport de la coverture de code pour un ou
plusieurs répertoires.

    [shell]
    ./bin/atoum -nccid /path/to/exclude
    ./bin/atoum --no-code-coverage-in-directories /path/to/exclude/1 /path/to/exclude/2

### -ns <namespaces> / --namespaces <namespaces>

Cette option vous permet de filtrer les classes et les méthodes en fonction des espaces de noms.

    [shell]
    ./bin/atoum -ns mageekguy\\atoum\\tests\\units\\asserters
    ./bin/atoum --namespaces mageekguy\\atoum\\tests\\units\\asserters

**Note**: reportez-vous à la section sur les [filtres par espace de noms](#par-espace-de-noms) pour avoir plus
d'informations.

### -p <file> / --php <file>

Cette option vous permet de spécifier le chemin de l'exécutable php à utiliser pour lancer vos
tests.

    [shell]
    ./bin/atoum -p /usr/bin/php5
    ./bin/atoum --php /usr/bin/php5

Par défaut, la valeur est recherchée parmis les valeurs suivantes (dans l'ordre):

* constante PHP_BINARY
* variable d'environnement PHP_PEAR_PHP_BIN
* variable d'environnement PHPBIN
* constante PHP_BINDIR + '/php'

### -sf <file> / --score-file <file>

Cette option vous permet de spécifier le chemin vers le fichier des résultats créé par atoum.

    [shell]
    ./bin/atoum -sf /path/to/atoum.score
    ./bin/atoum --score-file /path/to/atoum.score

### -t <tags> / --tags <tags>

Cette option vous permet de filtrer les classes et les méthodes à lancer en fonction des tags.

    [shell]
    ./bin/atoum -t OneTag
    ./bin/atoum --tags OneTag TwoTag

**Note**: reportez-vous à la section sur les [filtres par tags](#tags) pour avoir plus
d'informations.

### --test-all

Cette option vous permet de lancer les tests se trouvant dans les répertoires définis dans le
fichier de configuration via $script->addTestAllDirectory('path/to/directory').

    [shell]
    ./bin/atoum --test-all

### --test-it

Cette option vous permet de lancer les tests unitaires d'atoum pour vérifier qu'il tourne sans
problème sur votre serveur.

    [shell]
    ./bin/atoum --test-it

### -tfe <extensions> / --test-file-extensions <extensions>

Cette option vous permet de spécifier le ou les extensions des fichiers de tests à lancer.

    [shell]
    ./bin/atoum -tfe phpt
    ./bin/atoum --test-file-extensions phpt php5t

### -ulr / --use-light-report

Cette option vous permet d'alléger la sortie généré par atoum.

    [shell]
    ./bin/atoum -ulr
    ./bin/atoum --use-light-report
    
    [SSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSS>][  59/1141]
    [SSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSS>][ 118/1141]
    [SSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSS>][ 177/1141]
    [SSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSS>][ 236/1141]
    [SSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSS>][ 295/1141]
    [SSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSS>][ 354/1141]
    [SSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSS>][ 413/1141]
    [SSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSS>][ 472/1141]
    [SSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSS>][ 531/1141]
    [SSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSS>][ 590/1141]
    [SSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSS>][ 649/1141]
    [SSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSS>][ 708/1141]
    [SSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSS>][ 767/1141]
    [SSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSS>][ 826/1141]
    [SSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSS>][ 885/1141]
    [SSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSS>][ 944/1141]
    [SSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSS>][1003/1141]
    [SSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSS>][1062/1141]
    [SSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSS>][1121/1141]
    [SSSSSSSSSSSSSSSSSSSS________________________________________][1141/1141]
    Success (154 tests, 1141/1141 methods, 0 void method, 0 skipped method, 16875 assertions) !

### -v / --version

Cette option vous permet d'afficher la version courante d'atoum.

    [shell]
    ./bin/atoum -v
    ./bin/atoum --version

    atoum version DEVELOPMENT by Frédéric Hardy (/path/to/atoum)

## Sortie

TODO