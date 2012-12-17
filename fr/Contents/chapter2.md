# Écrire ses tests

## Assertions

### variable

C'est l'assertion de base de toutes les variables. Elle contient les tests nécessaires à n'importe
quel type de variable.

#### isCallable

isCallable vérifie que la variable peut être appelée comme fonction.

    [php]
    $f = function() {
        // code
    };

    $this
        ->variable($f)
            ->isCallable()  // passe

        ->variable('\Vendor\Project\foobar')
            ->isCallable()

        ->variable(array('\Vendor\Project\Foo', 'bar'))
            ->isCallable()

        ->variable('\Vendor\Project\Foo::bar')
            ->isCallable()
    ;

#### isEqualTo

isEqualTo vérifie que la variable est égale à une certaine donnée.

    [php]
    $a = 'a';

    $this
        ->variable($a)
            ->isEqualTo('a')    // passe
    ;

**Note**: isEqualTo ne teste pas le type de la variable.
Si vous souhaitez vérifier également son type, utilisez [isIdenticalTo](#isidenticalto).

#### isIdenticalTo

isIdenticalTo vérifie que la variable a la même valeur et le même type qu'une certaine donnée.
Dans le cas d'objets, isIdenticalTo vérifie que les données pointent sur la même instance.

    [php]
    $a = '1';

    $this
        ->variable($a)
            ->isIdenticalTo(1)          // échoue
    ;

    $stdClass1 = new \StdClass();
    $stdClass2 = new \StdClass();
    $stdClass3 = $stdClass1;

    $this
        ->variable($stdClass1)
            ->isIdenticalTo(stdClass3)  // passe
            ->isIdenticalTo(stdClass2)  // échoue
    ;

**Note**: isIdenticalTo teste le type de la variable.
Si vous ne souhaitez pas vérifier son type, utilisez [isEqualTo](#isequalto).

#### isNotCallable

isNotCallable vérifie que la variable ne peut pas être appelée comme fonction.

    [php]
    $f = function() {
        // code
    };
    $int    = 1;
    $string = 'nonExistingMethod';

    $this
        ->variable($f)
            ->isNotCallable()   // échoue

        ->variable($int)
            ->isNotCallable()   // passe

        ->variable($string)
            ->isNotCallable()   // passe
    ;

#### isNotEqualTo

isNotEqualTo vérifie que la variable n'a pas la même valeur qu'une certaine donnée.

    [php]
    $a       = 'a';
    $aString = '1';

    $this
        ->variable($a)
            ->isNotEqualTo('b')     // passe
            ->isNotEqualTo('a')     // échoue

        ->variable($aString)
            ->isNotEqualTo($1)      // échoue
    ;

**Note**: isNotEqualTo ne teste pas le type de la variable.
Si vous souhaitez vérifier également son type, utilisez [isNotIdenticalTo](#isnotidenticalto).

#### isNotIdenticalTo

isNotIdenticalTo vérifie que la variable n'a ni le même type, ni la même valeur qu'une certaine
donnée.

Dans le cas d'objets, isNotIdenticalTo vérifie que les données ne pointent pas sur la même instance.

    [php]
    $a = '1';

    $this
        ->variable($a)
            ->isNotIdenticalTo(1)           // passe
    ;

    $stdClass1 = new \StdClass();
    $stdClass2 = new \StdClass();
    $stdClass3 = $stdClass1;

    $this
        ->variable($stdClass1)
            ->isNotIdenticalTo(stdClass2)   // passe
            ->isNotIdenticalTo(stdClass3)   // échoue
    ;

**Note**: isNotIdenticalTo teste le type de la variable.
Si vous ne souhaitez pas vérifier son type, utilisez [isNotEqualTo](#isnotequalto).

#### isNull

isNull vérifie que la variable est nulle.

    [php]
    $emptyString = '';
    $null        = null;

    $this
        ->variable($emptyString)
            ->isNull()              // échoue (c'est vide mais pas null)

        ->variable($null)
            ->isNull()              // passe
    ;

#### isNotNull

isNotNull vérifie que la variable n'est pas nulle.

    [php]
    $emptyString = '';
    $null        = null;

    $this
        ->variable($emptyString)
            ->isNotNull()           // passe (c'est vide mais pas null)

        ->variable($null)
            ->isNotNull()           // échoue
    ;



### boolean

C'est l'assertion dédiée aux booléens.

Elle étend [variable](#variable), toutes ses méthodes sont donc disponibles dans cette assertion.

Si vous essayez de tester une variable qui n'est pas un booléen avec cette assertion, cela échouera.

**Note**: null n'est pas considéré comme un booléen.
Reportez vous au manuel PHP pour voir ce que [is_bool](http://php.net/is_bool) considère ou non
comme un booléen.

#### isFalse

isFalse vérifie que le booléen est strictement égal à false.

    [php]
    $true  = true;
    $false = false;

    $this
        ->boolean($true)
            ->isFalse()     // échoue

        ->boolean($false)
            ->isFalse()     // passe
    ;

#### isTrue

isTrue vérifie que le booléen est strictement égal à true.

    [php]
    $true  = true;
    $false = false;

    $this
        ->boolean($true)
            ->isTrue()      // passe

        ->boolean($false)
            ->isTrue()      // échoue
    ;



### integer

C'est l'assertion dédiée aux entiers.

Elle étend [variable](#variable), toutes ses méthodes sont donc disponibles dans cette assertion.

Si vous essayez de tester une variable qui n'est pas un entier avec cette assertion, cela échouera.

**Note**: null n'est pas considéré comme un entier.
Reportez vous au manuel PHP pour voir ce que [is_int](http://php.net/is_int) considère ou non comme
un entier.

#### isGreaterThan

isGreaterThan vérifie que l'entier est strictement supérieur à une certaine donnée.

    [php]
    $zero = 0;

    $this
        ->integer($zero)
            ->isGreaterThan(-1)     // passe
            ->isGreaterThan('-1')   // échoue car "-1" n'est pas un entier
            ->isGreaterThan(0)      // échoue
    ;

#### isGreaterThanOrEqualTo

isGreaterThanOrEqualTo vérifie que l'entier est supérieur ou égal à une certaine donnée.

    [php]
    $zero = 0;

    $this
        ->integer($zero)
            ->isGreaterThanOrEqualTo(-1)    // passe
            ->isGreaterThanOrEqualTo(0)     // passe
            ->isGreaterThanOrEqualTo('-1')  // échoue car "-1" n'est pas un entier
    ;

#### isLessThan

isLessThan vérifie que l'entier est strictement inférieur à une certaine donnée.

    [php]
    $zero = 0;

    $this
        ->integer($zero)
            ->isLessThan(10)    // passe
            ->isLessThan('10')  // échoue car "10" n'est pas un entier
            ->isLessThan(0)     // échoue
    ;

#### isLessThanOrEqualTo

isLessThanOrEqualTo vérifie que l'entier est inférieur ou égal à une certaine donnée.

    [php]
    $zero = 0;

    $this
        ->integer($zero)
            ->isLessThanOrEqualTo(10)       // passe
            ->isLessThanOrEqualTo(0)        // passe
            ->isLessThanOrEqualTo('10')     // échoue car "10" n'est pas un entier
    ;

#### isZero

isZero vérifie que l'entier est égal à 0.

    [php]
    $zero    = 0;
    $notZero = -1;

    $this
        ->integer($zero)
            ->isZero()          // passe

        ->integer($notZero)
            ->isZero()          // échoue
    ;

**Note**: isZero est équivalent à isEqualTo(0).



### float

C'est l'assertion dédiée aux nombres décimaux.

Elle étend [integer](#integer), toutes ses méthodes sont donc disponibles dans cette assertion.
Évidemment, les méthodes héritées d'integer (isEqualTo, isGreaterThan, isLessThan, etc...) utilisées
à travers float attendent un nombre décimal et non plus un entier.

Si vous essayez de tester une variable qui n'est pas un nombre décimal avec cette assertion, cela
échouera.

**Note**: null n'est pas considéré comme un nombre décimal.
Reportez vous au manuel PHP pour voir ce que [is_float](http://php.net/is_float) considère ou non
comme un nombre décimal.

#### isNearlyEqualTo

isNearlyEqualTo vérifie que le décimal est suffisament égal à une certaine donnée.

En effet, les nombres décimaux ont une valeur interne qui n'est pas assez précise. Essayez par
exemple d'exécuter la commande suivante:

    [bash]
    $ php -r 'var_dump(1 - 0.97 === 0.03);'
    bool(false)

Le résultat devrait pourtant être true. 

**Note**: pour avoir plus d'informations sur ce phénomène, reportez vous au
[manuel PHP](http://php.net/types.float).

Cette méthode cherche donc à corriger ce problème.

    [php]
    $float = 1 - 0.97;

    $this
        ->float($float)
            ->isNearlyEqualTo(0.03) // passe
            ->isEqualTo(0.03)       // échoue
    ;

**Note**: pour avoir plus d'informations sur l'algorithme utilisé,
consultez le [floating point guide](http://www.floating-point-gui.de/errors/comparison/).



### sizeOf

C'est l'assertion dédiée aux tests sur la taille des tableaux et des objets implémentants
l'interface Countable.

Elle étend [integer](#integer), toutes ses méthodes sont donc disponibles dans cette assertion.

    [php]
    $array           = array(1, 2, 3);
    $countableObject = new GlobIterator('*');

    $this
        ->sizeOf($array)
            ->isEqualTo(3)

        ->sizeOf($countableObject)
            ->isGreatherThan(0)
    ;



### object

C'est l'assertion dédiée aux objets.

Elle étend [variable](#variable), toutes ses méthodes sont donc disponibles dans cette assertion.

Si vous essayez de tester une variable qui n'est pas un objet avec cette assertion, cela échouera.

**Note**: null n'est pas considéré comme un objet.
Reportez vous au manuel PHP pour voir ce que [is_object](http://php.net/is_object) considère ou non
comme un objet.

#### hasSize

hasSize vérifie la taille d'un objet qui implémente l'interface Countable.

    [php]
    $countableObject = new GlobIterator('*');

    $this
        ->object($countableObject)
            ->hasSize(3)
    ;

#### isCloneOf

isCloneOf vérifie qu'un objet est le clone d'un objet donné,
c'est à dire que les objets sont égaux mais ne pointent pas vers la même instance.

    [php]
    $object1 = new \StdClass;
    $object2 = new \StdClass;
    $object3 = clone($object1);
    $object4 = new \StdClass;
    $object4->foo = 'bar';

    $this
        ->object($object1)
            ->isCloneOf($object2)   // passe
            ->isCloneOf($object3)   // passe
            ->isCloneOf($object4)   // échoue
    ;

**Note**: pour avoir plus de précision sur la comparaison d'objet,
reportez vous au [manuel PHP](http://php.net/language.oop5.object-comparison).

#### isEmpty

isEmpty vérifie que la taille d'un objet implémentant l'interface Countable est égale à 0.

    [php]
    $countableObject = new GlobIterator('atoum.php');

    $this
        ->object($countableObject)
            ->isEmpty()
    ;

**Note**: isEmpty est équivalent à hasSize(0).

#### isInstanceOf

isInstanceOf vérifie qu'un objet est:
* une instance de la classe donnée,
* une sous-classe de la classe donnée (abstraite ou non),
* une instance d'une classe qui implémente l'interface donnée.

    [php]
    $object = new \StdClass();

    $this
        ->object($object)
            ->isInstanceOf('\StdClass')     // passe
            ->isInstanceOf('\Iterator')     // échoue
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
            ->isInstanceOf('\FooClass')     // passe
            ->isInstanceOf('\FooInterface') // passe
            ->isInstanceOf('\BarClass')     // échoue
            ->isInstanceOf('\StdClass')     // échoue

        ->object($bar)
            ->isInstanceOf('\FooClass')     // passe
            ->isInstanceOf('\FooInterface') // passe
            ->isInstanceOf('\BarClass')     // passe
            ->isInstanceOf('\StdClass')     // échoue
    ;

**Note**: les noms des classes et des interfaces doit être absolu et commencé par un antislash.



### dateInterval

C'est l'assertion dédiée à l'objet [DateInterval](http://php.net/dateinterval).

Elle étend [object](#object), toutes ses méthodes sont donc disponibles dans cette assertion.

Si vous essayez de tester une variable qui n'est pas un objet DateInterval (ou une classe qui
l'étend) avec cette assertion, cela échouera.

#### isEqualTo

isEqualTo vérifie que la durée de l'objet DateInterval est égale à la durée d'un autre objet
DateInterval.

    [php]
    $di = new DateInterval('P1D');

    $this
        ->dateInterval($di)
            ->isEqualTo(new DateInterval('P1D')     // passe
            ->isEqualTo(new DateInterval('P2D')     // échoue
    ;

#### isGreaterThan

isGreaterThan vérifie que la durée de l'objet DateInterval est supérieure à la durée d'un autre
objet DateInterval.

    [php]
    $di = new DateInterval('P2D');

    $this
        ->dateInterval($di)
            ->isGreaterThan(new DateInterval('P1D')     // passe
            ->isGreaterThan(new DateInterval('P2D')     // échoue
    ;

#### isGreaterThanOrEqualTo

isGreaterThanOrEqualTo vérifie que la durée de l'objet DateInterval est supérieure ou égale à la
durée d'un autre objet DateInterval.

    [php]
    $di = new DateInterval('P2D');

    $this
        ->dateInterval($di)
            ->isGreaterThanOrEqualTo(new DateInterval('P1D')     // passe
            ->isGreaterThanOrEqualTo(new DateInterval('P2D')     // passe
            ->isGreaterThanOrEqualTo(new DateInterval('P3D')     // échoue
    ;

#### isLessThan

isLessThan vérifie que la durée de l'objet DateInterval est inférieure à la durée d'un autre objet
DateInterval.

    [php]
    $di = new DateInterval('P1D');

    $this
        ->dateInterval($di)
            ->isLessThan(new DateInterval('P2D')     // passe
            ->isLessThan(new DateInterval('P1D')     // échoue
    ;

#### isLessThanOrEqualTo

isLessThanOrEqualTo vérifie que la durée de l'objet DateInterval est inférieure ou égale à la durée
d'un autre objet DateInterval.

    [php]
    $di = new DateInterval('P2D');

    $this
        ->dateInterval($di)
            ->isLessThanOrEqualTo(new DateInterval('P3D')     // passe
            ->isLessThanOrEqualTo(new DateInterval('P2D')     // passe
            ->isLessThanOrEqualTo(new DateInterval('P1D')     // échoue
    ;

#### isZero

isZero vérifie que la durée de l'objet DateInterval est égale à 0.

    [php]
    $di1 = new DateInterval('P0D');
    $di2 = new DateInterval('P1D');

    $this
        ->dateInterval($di1)
            ->isZero()      // passe
        ->dateInterval($di2)
            ->isZero()      // échoue
    ;
    



### dateTime

C'est l'assertion dédiée à l'objet [DateTime](http://php.net/datetime).

Elle étend [object](#object), toutes ses méthodes sont donc disponibles dans cette assertion.

Si vous essayez de tester une variable qui n'est pas un objet DateTime (ou une classe qui l'étend)
avec cette assertion, cela échouera.

#### hasDate

hasDate vérifie la partie date de l'objet DateTime.

    [php]
    $dt = new DateTime('1981-02-13');

    $this
        ->dateTime($dt)
            ->hasDate('1981', '02', '13')   // passe
            ->hasDate('1981', '2',  '13')   // passe
            ->hasDate(1981,   2,    13)     // passe
    ;

#### hasDateAndTime

hasDateAndTime vérifie la date et l'horaire de l'objet DateTime

    [php]
    $dt = new DateTime('1981-02-13 01:02:03');

    $this
        ->dateTime($dt)
            ->hasDateAndTime('1981', '02', '13', '01', '02', '03')  // passe
            ->hasDateAndTime('1981', '2',  '13', '1',  '2',  '3')   // passe
            ->hasDateAndTime(1981,   2,    13,   1,    2,    3)     // passe
    ;

#### hasDay

hasDay vérifie le jour de l'objet DateTime.

    [php]
    $dt = new DateTime('1981-02-13');

    $this
        ->dateTime($dt)
            ->hasDay(13)        // passe
    ;

#### hasHours

hasHours vérifie les heures de l'objet DateTime.

    [php]
    $dt = new DateTime('01:02:03');

    $this
        ->dateTime($dt)
            ->hasHours('01')    // passe
            ->hasHours('1')     // passe
            ->hasHours(1)       // passe
    ;

#### hasMinutes

hasMinutes vérifie les minutes de l'objet DateTime.

    [php]
    $dt = new DateTime('01:02:03');

    $this
        ->dateTime($dt)
            ->hasMinutes('02')  // passe
            ->hasMinutes('2')   // passe
            ->hasMinutes(2)     // passe
    ;

#### hasMonth

hasMonth vérifie le mois de l'objet DateTime.

    [php]
    $dt = new DateTime('1981-02-13');

    $this
        ->dateTime($dt)
            ->hasMonth(2)       // passe
    ;

#### hasSeconds

hasSeconds vérifie les secondes de l'objet DateTime.

    [php]
    $dt = new DateTime('01:02:03');

    $this
        ->dateTime($dt)
            ->hasSeconds('03')    // passe
            ->hasSeconds('3')     // passe
            ->hasSeconds(3)       // passe
    ;

#### hasTime

hasTime vérifie la partie horaire de l'objet DateTime

    [php]
    $dt = new DateTime('01:02:03');

    $this
        ->dateTime($dt)
            ->hasTime('01', '02', '03')     // passe
            ->hasTime('1',  '2',  '3')      // passe
            ->hasTime(1,    2,    3)        // passe
    ;

#### hasTimezone

hasTimezone vérifie le fuseau horaire de l'objet DateTime.

    [php]
    $dt = new DateTime();

    $this
        ->dateTime($dt)
            ->hasTimezone('Europe/Paris')
    ;

#### hasYear

hasYear vérifie l'année de l'objet DateTime.

    [php]
    $dt = new DateTime('1981-02-13');

    $this
        ->dateTime($dt)
            ->hasYear(1981)     // passe
    ;



### mysqlDateTime

C'est l'assertion dédiée aux objets décrivant une date MySQL et basé sur l'objet
[DateTime](http://php.net/datetime).

Les dates doivent utiliser le format MySQL (et de nombreux SGBD), c'est à dire 'Y-m-d H:i:s'
(Reportez vous à la documentation de la fonction [date()](http://php.net/date) du manuel PHP
pour connaitre la signification de ce format).

Elle étend [dateTime](#datetime), toutes ses méthodes sont donc disponibles dans cette assertion.

Si vous essayez de tester une variable qui n'est pas un objet DateTime (ou une classe qui l'étend)
avec cette assertion, cela échouera.



### exception

C'est l'assertion dédiée aux exceptions.

Elle étend [object](#object), toutes ses méthodes sont donc disponibles dans cette assertion.

    [php]
    $this
        ->exception(
            function() {
                // ce code lève une exception
                throw new \Exception;
            }
        )
    ;

**Note**: la syntaxe utilise les fonctions anonymes (aussi appelées fermetures ou closures)
introduites en PHP 5.3.
Reportez vous au [manuel PHP](http://php.net/functions.anonymous) pour avoir plus d'informations sur
le sujet.

#### hasCode

hasCode vérifie le code de l'exception.

    [php]
    $this
        ->exception(
            function() {
                throw new \Exception('Message', 42);
            }
        )
            ->hasCode(42)
    ;

#### hasDefaultCode

hasDefaultCode vérifie que le code de l'exception est la valeur par défaut, c'est à dire 0.

    [php]
    $this
        ->exception(
            function() {
                throw new \Exception;
            }
        )
            ->hasDefaultCode()
    ;

**Note**: hasDefaultCode est équivalent à hasCode(0).

#### hasMessage

hasMessage vérifie le message de l'exception.

    [php]
    $this
        ->exception(
            function() {
                throw new \Exception('Message');
            }
        )
            ->hasMessage('Message')     // passe
            ->hasMessage('message')     // échoue
    ;

#### hasNestedException

hasNestedException vérifie que l'exception contient une référence vers l'exception précédente.
Si l'exception est précisée, cela va également vérifier la classe de l'exception.

    [php]
    $this
        ->exception(
            function() {
                throw new \Exception('Message');
            }
        )
            ->hasNestedException()      // échoue

        ->exception(
            function() {
                try {
                    // Cette exception est levée...
                    throw new \FirstException('Message 1', 42);
                }
                // ... puis attrapée...
                catch(\FirstException $e) {
                    // ... et enfin relancée, encapsulée dans une seconde exception
                    throw new \SecondException('Message 2', 24, $e);
                }
            }
        )
            ->isInstanceOf('\FirstException')           // échoue
            ->isInstanceOf('\SecondException')          // passe

            ->hasNestedException()                      // passe
            ->hasNestedException(new \FirstException)   // passe
            ->hasNestedException(new \SecondException)  // échoue
    ;



### array

C'est l'assertion dédiée aux tableaux.

Elle étend [variable](#variable), toutes ses méthodes sont donc disponibles dans cette assertion.

**Note**: le mot-clef "array" étant réservé en PHP, il n'a pas été possible de créer une assertion
"array". Elle s'appelle donc "phpArray" et un alias "array" a été créé. Vous pourrez donc rencontrer
des ->phpArray() ou des ->array().

Il est conseillé d'utiliser exclusivement ->array().

#### contains

contains vérifie qu'un tableau contient une certaine donnée.

    [php]
    $fibonacci = array('1', 2, '3', 5, '8', 13, '21');

    $this
        ->array($fibonacci)
            ->contains('1')     // passe
            ->contains(1)       // passe, ne vérifie pas le type de la donnée
            ->contains('2')     // passe, ne vérifie pas le type de la donnée
            ->contains(10)      // échoue
    ;

**Note**: contains ne fait pas de recherche récursive.

**Note**: contains ne teste pas le type de la donnée.
Si vous souhaitez vérifier également son type, utilisez [strictlyContains](#strictlycontains).

#### containsValues

containsValues vérifie qu'un tableau contient toutes les données fournies dans un tableau.

    [php]
    $fibonacci = array('1', 2, '3', 5, '8', 13, '21');

    $this
        ->array($array)
            ->containsValues(array(1, 2, 3))        // passe
            ->containsValues(array('5', '8', '13')) // passe
            ->containsValues(array(0, 1, 2))        // échoue
    ;

**Note**: containsValues ne fait pas de recherche récursive.

**Note**: containsValues ne teste pas le type des données.
Si vous souhaitez vérifier également leurs types, utilisez
[strictlyContainsValues](#strictlycontainsvalues).

#### hasKey

hasKey vérifie qu'un tableau contient une certaine clef.

    [php]
    $fibonacci = array('1', 2, '3', 5, '8', 13, '21');
    $atoum     = array(
        'name'        => 'atoum',
        'owner'       => 'mageekguy',
        'description' => 'The modern, simple and intuitive PHP 5.3+ unit testing framework.',
    );

    $this
        ->array($fibonacci)
            ->hasKey(0)         // passe
            ->hasKey(1)         // passe
            ->hasKey('1')       // passe
            ->hasKey(10)        // échoue

        ->array($atoum)
            ->hasKey('name')    // passe
            ->hasKey('price')   // échoue
    ;

**Note**: hasKey ne fait pas de recherche récursive.

**Note**: hasKey ne teste pas le type des clefs.

#### hasKeys

hasKeys vérifie qu'un tableau contient toutes les clefs fournies dans un tableau.

    [php]
    $fibonacci = array('1', 2, '3', 5, '8', 13, '21');
    $atoum     = array(
        'name'        => 'atoum',
        'owner'       => 'mageekguy',
        'description' => 'The modern, simple and intuitive PHP 5.3+ unit testing framework.',
    );

    $this
        ->array($fibonacci)
            ->hasKeys(array(0, 2, 4))           // passe
            ->hasKeys(array('0', 2))            // passe
            ->hasKeys(array('4', 0, 3))         // passe
            ->hasKeys(array(0, 3, 10))          // échoue

        ->array($atoum)
            ->hasKeys(array('name', 'owner'))   // passe
            ->hasKeys(array('name', 'price'))   // échoue
    ;

**Note**: hasKeys ne fait pas de recherche récursive.

**Note**: hasKeys ne teste pas le type des clefs.

#### hasSize

hasSize vérifie la taille d'un tableau.

    [php]
    $fibonacci = array('1', 2, '3', 5, '8', 13, '21');

    $this
        ->array($fibonacci)
            ->hasSize(7)        // passe
            ->hasSize(10)       // échoue
    ;

**Note**: hasSize n'est pas récursif.

#### isEmpty

isEmpty vérifie qu'un tableau est vide.

    [php]
    $emptyArray    = array();
    $nonEmptyArray = array(null, null);

    $this
        ->array($emptyArray)
            ->isEmpty()         // passe

        ->array($nonEmptyArray)
            ->isEmpty()         // échoue
    ;

#### isNotEmpty

isNotEmpty vérifie qu'un tableau n'est pas vide.

    [php]
    $emptyArray    = array();
    $nonEmptyArray = array(null, null);

    $this
        ->array($emptyArray)
            ->isNotEmpty()      // échoue

        ->array($nonEmptyArray)
            ->isNotEmpty()      // passe
    ;

#### notContains

notContains vérifie qu'un tableau ne contient pas une donnée.

    [php]
    $fibonacci = array('1', 2, '3', 5, '8', 13, '21');

    $this
        ->array($fibonacci)
            ->notContains(null)         // passe
            ->notContains(1)            // échoue
            ->notContains(10)           // passe
    ;

**Note**: notContains ne fait pas de recherche récursive.

**Note**: notContains ne teste pas le type de la donnée.
Si vous souhaitez vérifier également son type, utilisez [strictlyNotContains](#strictlynotcontains).

#### notContainsValues

notContainsValues vérifie qu'un tableau ne contient aucune des données fournies dans un tableau.

    [php]
    $fibonacci = array('1', 2, '3', 5, '8', 13, '21');

    $this
        ->array($array)
            ->notContainsValues(array(1, 4, 10))    // échoue
            ->notContainsValues(array(4, 10, 34))   // passe
            ->notContainsValues(array(1, '2', 3))   // échoue
    ;

**Note**: notContainsValues ne fait pas de recherche récursive.

**Note**: notContainsValues ne teste pas le type des données.
Si vous souhaitez vérifier également leurs types, utilisez
[strictlyNotContainsValues](#strictlynotcontainsvalues).

#### notHasKey

notHasKey vérifie qu'un tableau ne contient pas une certaine clef.

    [php]
    $fibonacci = array('1', 2, '3', 5, '8', 13, '21');
    $atoum     = array(
        'name'        => 'atoum',
        'owner'       => 'mageekguy',
        'description' => 'The modern, simple and intuitive PHP 5.3+ unit testing framework.',
    );

    $this
        ->array($fibonacci)
            ->notHasKey(0)          // échoue
            ->notHasKey(1)          // échoue
            ->notHasKey('1')        // échoue
            ->notHasKey(10)         // passe

        ->array($atoum)
            ->notHasKey('name')     // échoue
            ->notHasKey('price')    // passe
    ;

**Note**: notHasKey ne fait pas de recherche récursive.

**Note**: notHasKey ne teste pas le type des clefs.

#### notHasKeys

notHasKeys vérifie qu'un tableau ne contient aucune des clefs fournies dans un tableau.

    [php]
    $fibonacci = array('1', 2, '3', 5, '8', 13, '21');
    $atoum     = array(
        'name'        => 'atoum',
        'owner'       => 'mageekguy',
        'description' => 'The modern, simple and intuitive PHP 5.3+ unit testing framework.',
    );

    $this
        ->array($fibonacci)
            ->notHasKeys(array(0, 2, 4))            // échoue
            ->notHasKeys(array('0', 2))             // échoue
            ->notHasKeys(array('4', 0, 3))          // échoue
            ->notHasKeys(array(10, 11, 12))         // passe

        ->array($atoum)
            ->notHasKeys(array('name', 'owner'))    // échoue
            ->notHasKeys(array('foo', 'price'))     // passe
    ;

**Note**: notHasKeys ne fait pas de recherche récursive.

**Note**: notHasKeys ne teste pas le type des clefs.

#### strictlyContains

strictlyContains vérifie qu'un tableau contient une certaine donnée (même valeur et même type).

    [php]
    $fibonacci = array('1', 2, '3', 5, '8', 13, '21');

    $this
        ->array($fibonacci)
            ->strictlyContains('1')     // passe
            ->strictlyContains(1)       // échoue
            ->strictlyContains('2')     // échoue
            ->strictlyContains(2)       // passe
            ->strictlyContains(10)      // échoue
    ;

**Note**: strictlyContains ne fait pas de recherche récursive.

**Note**: strictlyContains teste le type de la donnée. Si vous ne souhaitez pas vérifier son type,
utilisez [contains](#contains).

#### strictlyContainsValues

strictlyContainsValues vérifie qu'un tableau contient toutes les données fournies dans un tableau
(même valeur et même type).

    [php]
    $fibonacci = array('1', 2, '3', 5, '8', 13, '21');

    $this
        ->array($array)
            ->strictlyContainsValues(array('1', 2, '3'))    // passe
            ->strictlyContainsValues(array(1, 2, 3))        // échoue
            ->strictlyContainsValues(array(5, '8', 13))     // passe
            ->strictlyContainsValues(array('5', '8', '13')) // échoue
            ->strictlyContainsValues(array(0, '1', 2))      // échoue
    ;

**Note**: strictlyContainsValues ne fait pas de recherche récursive.

**Note**: strictlyContainsValues teste le type des données.
Si vous ne souhaitez pas vérifier leurs types, utilisez [containsValues](#containsvalues).

#### strictlyNotContains

strictlyNotContains vérifie qu'un tableau ne contient pas une donnée (même valeur et même type).

    [php]
    $fibonacci = array('1', 2, '3', 5, '8', 13, '21');

    $this
        ->array($fibonacci)
            ->strictlyNotContains(null)         // passe
            ->strictlyNotContains('1')          // échoue
            ->strictlyNotContains(1)            // passe
            ->strictlyNotContains(10)           // passe
    ;

**Note**: strictlyNotContains ne fait pas de recherche récursive.

**Note**: strictlyNotContains teste le type de la donnée.
Si vous ne souhaitez pas vérifier son type, utilisez [notContains](#notcontains).

#### strictlyNotContainsValues

strictlyNotContainsValues vérifie qu'un tableau ne contient aucune des données fournies dans un
tableau (même valeur et même type).

    [php]
    $fibonacci = array('1', 2, '3', 5, '8', 13, '21');

    $this
        ->array($array)
            ->strictlyNotContainsValues(array('1', 4, 10))  // échoue
            ->strictlyNotContainsValues(array(1, 4, 10))    // passe
            ->strictlyNotContainsValues(array(4, 10, 34))   // passe
            ->strictlyNotContainsValues(array('1', 2, '3')) // échoue
            ->strictlyNotContainsValues(array(1, '2', 3))   // passe
    ;

**Note**: strictlyNotContainsValues ne fait pas de recherche récursive.

**Note**: strictlyNotContainsValues teste le type des données.
Si vous ne souhaitez pas vérifier leurs types, utilisez [notContainsValues](#notcontainsvalues).



### string

C'est l'assertion dédiée aux chaines de caractères.

Elle étend [variable](#variable), toutes ses méthodes sont donc disponibles dans cette assertion.

#### contains

contains vérifie qu'une chaine de caractère contient une autre chaine de caractère donnée.

    [php]
    $string = 'Hello world';

    $this
        ->string($string)
            ->contains('ll')    // passe
            ->contains(' ')     // passe
            ->contains('php')   // échoue
    ;

#### hasLength

hasLength vérifie la taille d'une chaine de caractères.

    [php]
    $string = 'Hello world';

    $this
        ->string($string)
            ->hasLength(11)     // passe
            ->hasLength(20)     // échoue
    ;

#### hasLengthGreaterThan

hasLengthGreaterThan vérifie que la taille d'une chaine de caractères est plus grande qu'une valeur donnée.

    [php]
    $string = 'Hello world';

    $this
        ->string($string)
            ->hasLengthGreaterThan(10)     // passe
            ->hasLengthGreaterThan(20)     // échoue
    ;

#### hasLengthLessThan

hasLengthLessThan vérifie que la taille d'une chaine de caractères est plus petite qu'une valeur donnée.

    [php]
    $string = 'Hello world';

    $this
        ->string($string)
            ->hasLengthLessThan(20)     // passe
            ->hasLengthLessThan(10)     // échoue
    ;

#### isEmpty

isEmpty vérifie qu'une chaine de caractères est vide.

    [php]
    $emptyString    = '';
    $nonEmptyString = 'atoum';

    $this
        ->string($emptyString)
            ->isEmpty()             // passe
    
        ->string($nonEmptyString)
            ->isEmpty()             // échoue
    ;

#### isEqualToContentsOfFile

isEqualToContentsOfFile vérifie qu'une chaine de caractère est égale au contenu d'un fichier donné
par son chemin.

    [php]
    $this
        ->string($string)
            ->isEqualToContentsOfFile('/path/to/file')
    ;

**Note**: si le fichier n'existe pas, le test échoue.

#### isNotEmpty

isNotEmpty vérifie qu'une chaine de caractères n'est pas vide.

    [php]
    $emptyString    = '';
    $nonEmptyString = 'atoum';

    $this
        ->string($emptyString)
            ->isNotEmpty()          // échoue
    
        ->string($nonEmptyString)
            ->isNotEmpty()          // passe
    ;

#### match

match vérifie qu'une expression régulière correspond à la chaine de caractères.

    [php]
    $phone = '0102030405';
    $vdm   = "Aujourd'hui, à 57 ans, mon père s'est fait tatouer une licorne sur l'épaule. VDM";

    $this
        ->string($phone)
            ->match('#^0[1-9]\d{8}$#')

        ->string($vdm)
            ->match("#^Aujourd'hui.*VDM$#")
    ;



### castToString

C'est l'assertion dédiée aux tests sur le transtypage d'objets en chaine de caractères.

Elle étend [string](#string), toutes ses méthodes sont donc disponibles dans cette assertion.

    [php]
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



### hash

C'est l'assertion dédiée aux tests sur les hashs (empreintes numériques).

Elle étend [string](#string), toutes ses méthodes sont donc disponibles dans cette assertion.

#### isMd5

isMd5 vérifie que la chaine de caractère est au format md5, c'est à dire une chaine hexadécimale de
32 caractères.

    [php]
    $hash    = hash('md5', 'atoum');
    $notHash = 'atoum';

    $this
        ->hash($hash)
            ->isMd5()       // passe
        ->hash($notHash)
            ->isMd5()       // échoue
    ;

#### isSha1

isSha1 vérifie que la chaine de caractère est au format sha1, c'est à dire une chaine hexadécimale
de 40 caractères.

    [php]
    $hash    = hash('sha1', 'atoum');
    $notHash = 'atoum';

    $this
        ->hash($hash)
            ->isSha1()      // passe
        ->hash($notHash)
            ->isSha1()      // échoue
    ;

#### isSha256

isSha256 vérifie que la chaine de caractère est au format sha256, c'est à dire une chaine
hexadécimale de 64 caractères.

    [php]
    $hash    = hash('sha256', 'atoum');
    $notHash = 'atoum';

    $this
        ->hash($hash)
            ->isSha256()    // passe
        ->hash($notHash)
            ->isSha256()    // échoue
    ;

#### isSha512

isSha512 vérifie que la chaine de caractère est au format sha512, c'est à dire une chaine
hexadécimale de 128 caractères.

    [php]
    $hash    = hash('sha512', 'atoum');
    $notHash = 'atoum';

    $this
        ->hash($hash)
            ->isSha512()    // passe
        ->hash($notHash)
            ->isSha512()    // échoue
    ;



### output

C'est l'assertion dédiée aux tests sur les sorties, c'est à dire tout ce qui est censé être affiché
à l'écran.

Elle étend [string](#string), toutes ses méthodes sont donc disponibles dans cette assertion.

    [php]
    $this
        ->output(
            function() {
                echo 'Hello world';
            }
        )
    ;

**Note**: la syntaxe utilise les fonctions anonymes (aussi appelées fermetures ou closures)
introduites en PHP 5.3.
Reportez vous au [manuel PHP](http://php.net/functions.anonymous) pour avoir plus d'informations sur
le sujet.



### utf8String

C'est l'assertion dédiée aux chaines de caractères UTF-8.

Elle étend [string](#string), toutes ses méthodes sont donc disponibles dans cette assertion.

**Note**: utf8Strings utilise les fonctions mb_* pour gérer les chaines multi-octets.
Reportez vous au manuel PHP pour voir avoir plus d'information sur l'extension
[mbstring](http://php.net/mbstring).

#### match

match vérifie qu'une expression régulière correspond à la chaine de caractères.

    [php]
    $vdm   = "Aujourd'hui, à 57 ans, mon père s'est fait tatouer une licorne sur l'épaule. VDM";

    $this
        ->utf8String($vdm)
            ->match("#^Aujourd'hui.*VDM$#u")
    ;

**Note**: pensez à bien ajouter "u" comme option de recherche dans votre expression régulière.
Reportez vous au [manuel PHP](http://php.net/reference.pcre.pattern.modifiers) pour avoir plus
d'informations sur le sujet.



### afterDestructionOf

C'est l'assertion dédiée à la destruction des objets.

Cette assertion ne fait que prendre un objet, vérifier que la méthode __destruct() est bien définie 
puis l'appelle.

Si __destruct() existe bien et si son appel se passe sans erreur ni exception, alors le test passe.

    [php]
    $this
        ->afterDestructionOf($objectWithDestructor)     // passe

        ->afterDestructionOf($objectWithoutDestructor)  // échoue
    ;



### error

C'est l'assertion dédiée aux erreurs.

    [php]
    $this
        ->when(
            function() {
                trigger_error('message');
            }
        )
            ->error()
    ;

**Note**: la syntaxe utilise les fonctions anonymes (aussi appelées fermetures ou closures)
introduites en PHP 5.3.
Reportez vous au [manuel PHP](http://php.net/functions.anonymous) pour avoir plus d'informations sur
le sujet.

**Note**: les types d'erreur E_ERROR, E_PARSE, E_CORE_ERROR, E_CORE_WARNING, E_COMPILE_ERROR,
E_COMPILE_WARNING ainsi que la plupart des E_STRICT ne peuvent pas être gérés avec cette fonction.

#### exists

exists vérifie qu'une erreur a été levée lors de l'exécution du code précédent.

    [php]
    $this
        ->when(
            function() {
                trigger_error('message');
            }
        )
            ->error()
                ->exists()      // passe

        ->when(
            function() {
                // code sans erreur
            }
        )
            ->error()
                ->exists()      // échoue
    ;

#### notExists

notExists vérifie qu'aucune erreur n'a été levée lors de l'exécution du code précédent.

    [php]
    $this
        ->when(
            function() {
                trigger_error('message');
            }
        )
            ->error()
                ->notExists()   // échoue

        ->when(
            function() {
                // code sans erreur
            }
        )
            ->error()
                ->notExists()   // passe
    ;

#### withType

withType vérifie qu'aucune erreur n'a été levée lors de l'exécution du code précédent.

    [php]
    $this
        ->when(
            function() {
                trigger_error('message');
            }
        )
            ->error()
                ->notExists()   // échoue
    ;



### class

C'est l'assertion dédiée aux classes.

    [php]
    $object = new \StdClass;

    $this
        ->class(get_class($object))

        ->class('\StdClass')
    ;

**Note**: le mot-clef "class" étant réservé en PHP, il n'a pas été possible de créer une assertion
"class". Elle s'appelle donc "phpClass" et un alias "class" a été créé. Vous pourrez donc rencontrer
des ->phpClass() ou des ->class().

Il est conseillé d'utiliser exclusivement ->class().

#### hasInterface

hasInterface vérifie que la classe implémente une interface donnée.

    [php]
    $this
        ->class('\ArrayIterator')
            ->hasInterface('Countable')     // passe
            
        ->class('\StdClass')
            ->hasInterface('Countable')     // échoue
    ;

#### hasMethod

hasMethod vérifie que la classe contient une méthode donnée.

    [php]
    $this
        ->class('\ArrayIterator')
            ->hasMethod('count')    // passe
            
        ->class('\StdClass')
            ->hasMethod('count')    // échoue
    ;

#### hasNoParent

hasNoParent vérifie que la classe n'hérite d'aucune classe.

    [php]
    $this
        ->class('\StdClass')
            ->hasNoParent()     // passe
            
        ->class('\FilesystemIterator')
            ->hasNoParent()     // échoue
    ;

**Note**: une classe peut implémenter une ou plusieurs interfaces et n'hériter d'aucune classe.
hasNoParent ne vérifie pas les interfaces, uniquement les classes héritées.

#### hasParent

hasParent vérifie que la classe hérite bien d'une classe.

    [php]
    $this
        ->class('\StdClass')
            ->hasParent()       // échoue
            
        ->class('\FilesystemIterator')
            ->hasParent()       // passe
    ;

**Note**: une classe peut implémenter une ou plusieurs interfaces et n'hériter d'aucune classe.
hasParent ne vérifie pas les interfaces, uniquement les classes héritées.

#### isAbstract

isAbstract vérifie que la classe est abstraite.

    [php]
    $this
        ->class('\StdClass')
            ->isAbstract()       // échoue
    ;

#### isSubclassOf

isSubclassOf vérifie que la classe hérite de la classe donnée.

    [php]
    $this
        ->class('\FilesystemIterator')
            ->isSubclassOf('\DirectoryIterator')    // passe
            ->isSubclassOf('\SplFileInfo')          // passe
            ->isSubclassOf('\StdClass')             // échoue
    ;




### mock

C'est l'assertion dédiée aux bouchons.

    [php]
    $mock = new \mock\MyClass;

    $this
        ->mock($mock)
    ;

**Note**: reportez-vous à la documentation sur les [bouchons](#les-bouchons)
pour obtenir plus d'informations sur la façon de créer et gérer les bouchons.

#### wasCalled

wasCalled vérifie qu'au moins une méthode du mock a été appelée au moins une fois.

    [php]
    $mock = new \mock\MyFirstClass;

    $this
        ->object(new MySecondClass($mock))

        ->mock($mock)
            ->wasCalled()
    ;

#### wasNotCalled

wasNotCalled vérifie qu'aucune méthode du mock n'a été appelée.

    [php]
    $mock = new \mock\MyFirstClass;

    $this
        ->object(new MySecondClass($mock))

        ->mock($mock)
            ->wasNotCalled()
    ;

#### call

call permet de spécifier une méthode du mock à tester

    [php]
    $mock = new \mock\MyFirstClass;

    $this
        ->object(new MySecondClass($mock))

        ->mock($mock)
            ->call('myMethod')
                ->once()
    ;

##### atLeastOnce

atLeastOnce vérifie que la méthode testée (voir [call](#call)) du mock testé a été appelée au moins
une fois.

    [php]
    $mock = new \mock\MyFirstClass;

    $this
        ->object(new MySecondClass($mock))

        ->mock($mock)
            ->call('myMethod')
                ->atLeastOnce()
    ;

##### exactly

exactly vérifie que la méthode testée (voir [call](#call)) du mock testé exactement un certain
nombre de fois.

    [php]
    $mock = new \mock\MyFirstClass;

    $this
        ->object(new MySecondClass($mock))

        ->mock($mock)
            ->call('myMethod')
                ->exactly(2)
    ;

##### never

never vérifie que la méthode testée (voir [call](#call)) du mock testé n'a jamais été appelée.

    [php]
    $mock = new \mock\MyFirstClass;

    $this
        ->object(new MySecondClass($mock))

        ->mock($mock)
            ->call('myMethod')
                ->never()
    ;

**Note**: once est équivalent à [exactly](#exactly)(0).

##### once/twice/thrice

Ces assertions vérifient que la méthode testée (voir [call](#call)) du mock testé a été appelée
exactement :

* une fois (once)
* deux fois (twice)
* trois fois (thrice)

    [php]
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

**Note**: once, twice et thrice sont respectivement équivalents à un appel à [exactly](#exactly)(1),
[exactly](#exactly)(2) et [exactly](#exactly)(3).

##### withArguments

withArguments permet de spécifier les paramètres attendus lors de l'appel à la méthode testée
(voir [call](#call)) du mock testé.

    [php]
    $mock = new \mock\MyFirstClass;

    $this
        ->object(new MySecondClass($mock))

        ->mock($mock)
            ->call('myMethod')
                ->withArguments('first', 'second')->once()
    ;

**Note**: withArguments ne teste pas le type des arguments.
Si vous souhaitez vérifier également leurs types, utilisez
[withIdenticalArguments](#withidenticalarguments).

##### withIdenticalArguments

withIdenticalArguments permet de spécifier les paramètres attendus lors de l'appel à la méthode
testée (voir [call](#call)) du mock testé.

    [php]
    $mock = new \mock\MyFirstClass;

    $this
        ->object(new MySecondClass($mock))

        ->mock($mock)
            ->call('myMethod')
                ->withIdenticalArguments('first', 'second')->once()
    ;

**Note**: withIdenticalArguments teste le type des arguments.
Si vous ne souhaitez pas vérifier leurs types, utilisez [withArguments](#witharguments).

##### withAnyArguments

withAnyArguments permet de ne pas spécifier de paramètres attendus lors de l'appel à la méthode
testée (voir [call](#call)) du mock testé.

Cette méthode est surtout utile pour remettre à zéro les arguments, comme dans l'exemple suivant:

    [php]
    $mock = new \mock\MyFirstClass;

    $this
        ->object(new MySecondClass($mock))

        ->mock($mock)
            ->call('myMethod')
                ->withArguments('first')     ->once()
                ->withArguments('second')    ->once()
                ->withAnyArgumentsArguments()->exactly(2)
    ;




### stream

C'est l'assertion dédiée aux stream.

Malheureusement, je n'ai aucune espèce d'idée de son fonctionnement, alors n'hésitez pas à compléter
cette partie !

#### isRead

#### isWrite





## Aide à l'écriture

Il est possible d'écrire des tests unitaires avec atoum de plusieurs manières,
et l'une d'elle est d'utiliser des mots-clefs tels que if, and ou bien encore then, when ou assert.

### if, and, then

L'utilisation de ces mots clefs est très intuitive:

    [php]
    $this
        ->if($computer = new computer()))
        ->and($computer->setFirstOperand(2))
        ->and($computer->setSecondOperand(2))
        ->then
            ->object($computer->add())
                ->isIdenticalTo($computer)
            ->integer($computer->getResult())
                ->isEqualTo(4)
    ;

Il est important de noter ces mots-clefs n'apporte rien techniquement ou fonctionnellement parlant,
car ils n'ont pas d'autres but que de faciliter la compréhension du test et donc sa maintenance
en y ajoutant de la sémantique compréhensible facilement par l'Humain et plus particulièrement un
développeur.

Ainsi, if et and permettent de définir les pré-conditions nécessaires pour que les assertions qui
suivent le mot-clef then passent avec succès.

Cependant, il n'y a pas de grammaire régissant l'ordre d'utilisation de ces mots-clefs et aucune
vérification syntaxique n'est effectuée par atoum.

En conséquence, il est de la responsabilité du développeur de les utiliser de façon à ce que le test
soit signifiant, même s'il est par exemple tout à fait possible d'écrire le test de la manière
suivante:

    [php]
    $this
        ->and($computer = new computer()))
        ->and($computer->setFirstOperand(2))
        ->if($computer->setSecondOperand(2))
        ->then
            ->object($computer->add())
                ->isIdenticalTo($computer)
            ->integer($computer->getResult())
                ->isEqualTo(4)
    ;

Pour les mêmes raisons, l'utilisation de then est facultative.

Il est également important de noter qu'il est tout à fait possible d'écrire le même test en
n'utilisant aucun mot-clef:

    [php]
    $computer = new computer();
    $computer->setFirstOperand(2);
    $computer->setSecondOperand(2);

    $this
        ->object($computer->add())
            ->isIdenticalTo($computer)
        ->integer($computer->getResult())
            ->isEqualTo(4)
    ;

Le test ne sera pas plus lent ou plus rapide à exécuter et il n'y a aucun avantage à utiliser une
notation plutôt qu'une autre, l'important étant d'en choisir une et de s'y tenir pour faciliter la
maintenance des tests (la problématique est exactement la même que celle des conventions de codage).

### when

En plus de if, and et then, il existe également d'autres mots-clefs.

L'un d'entre eux est when et qui dispose d'une fonctionnalité spécifique introduite pour contourner
le fait qu'il est illégale d'écrire en PHP le code suivant:

    [php]
    $this
        ->if($object = new object($valueAtKey0 = uniqid()))
        ->and(unset($array[0])
        ->then
            ->sizeOf($object)
                ->isZero()
    ;

Le langage génère en effet dans ce cas l'erreur fatale:

    [shell]
    Parse error: syntax error, unexpected 'unset' (T_UNSET), expecting ')'

Il n'est en effet pas possible d'utiliser unset() comme argument d'une fonction.

Pour résoudre ce problème, le mot-clef when est capable d'interpréter l'éventuelle fonction anonyme
qui lui est passée en argument, ce qui permet d'écrire le test précédent de la manière suivante:

    [php]
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

Bien évidemment, si when ne reçoit pas de fonction anonyme en argument, il se comporte exactement
comme if, and et then, à savoir qu'il ne fait absolument rien fonctionnellement parlant.

### assert

Enfin, il existe le mot-clef assert qui a également un fonctionnement un peu particulier.

Pour illustrer son fonctionnement, le test suivant va être utilisé:

    [php]
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

Le test précédent présente un inconvénient en terme de maintenance, car si le développeur a besoin
d'intercaler un ou plusieurs nouveaux appels à bar::doSomething() entre les deux appels déjà
effectués, il sera obligé de mettre à jour en conséquence la valeur de l'argument passé à exactly().

Pour remédier à ce problème, il est possible de recourir à la méthode resetCalls() du contrôleur du
bouchon de la manière suivante:

    [php]
    $this
        ->if($foo = new \mock\foo())
        ->and($bar = new bar($foo))
        ->and($bar->doSomething())
        ->then
            ->mock($foo)
                ->call('doOtherThing')
                    ->once()
        ->if($foo->getMockController()->resetCalls())
        ->and($bar->setValue(uniqid())
        ->then
            ->mock($foo)
                ->call('doOtherThing')
                    ->once()
    ;

En effet, la méthode resetCalls() efface la mémoire du contrôleur et il est donc possible d'écrire
l'assertion suivante comme si le bouchon n'avait jamais été utilisé.

Le mot-clef assert permet de se passer de l'appel explicite à resetCalls() et de plus il provoque
l'effacement de la mémoire de l'ensemble des adaptateurs et des contrôleurs de bouchon définis au
moment de son utilisation.

Grâce à lui, il est donc possible d'écrire le test précédent d'une façon plus simple et plus
lisible, d'autant qu'il est possible de passer une chaîne de caractère à assert afin d'expliquer le
rôle des assertions suivantes:

    [php]
    $this
        ->assert('Foo is empty')
            ->if($foo = new \mock\foo())
            ->and($bar = new bar($foo))
            ->and($bar->doSomething())
            ->then
                ->mock($foo)
                    ->call('doOtherThing')
                        ->once()
        ->assert('Foo has a value')
            ->if($bar->setValue(uniqid())
            ->then
                ->mock($foo)
                    ->call('doOtherThing')
                        ->once()
    ;

La chaîne de caractères sera de plus reprise dans les messages générés par atoum si l'une des
assertions ne passent pas avec succès.




## Le mode loop

Lorsqu'un développeur fait du développement piloté par les tests, il travaille de la manière
suivante:

- il commence par créer le test correspondant à ce qu'il veut développer ;
- il exécute le test qu'il vient de créer ;
- il écrit le code permettant au test de passer avec succès ;
- il modifie ou complète son test et repart à l'étape 2.

Concrètement, cela signifie qu'il doit:

* créer son code dans son éditeur favori ;
* quitter son éditeur pour utiliser une console afin d'exécuter son test ;
* revenir à son éditeur pour écrire le code permettant au test de passer avec succès ;
* revenir à la console afin de relancer l'exécution de son test ;
* revenir à son éditeur afin de modifier ou compléter son test ;

Il y a donc bien un cycle qui se répétera tant que la fonctionnalité n'aura pas été développé
dans son intégralité.

Cependant, ce cycle est complexe et impose de nombreux allers-retours entre plusieurs logiciels,
ainsi que la saisie récurrente d'une même commande dans le terminal afin de lancer l'exécution des
tests unitaires.

atoum propose le mode "loop" disponible via les arguments -l ou --loop, qui permet au développeur de
ne pas avoir à relancer manuellement les tests et permet donc de fluidifier le processus de
développement.

Dans ce mode, atoum commence par exécuter une première fois les tests qui lui sont demandés.

Une fois les tests terminés, si les tests ont été passé avec succès par le code, atoum se met
simplement en attente :

    [shell]
    php tests/units/classes/adapter.php -l
    > atoum version DEVELOPMENT by Frédéric Hardy (/Users/fch/Atoum)
    > PHP path: /usr/local/bin/php
    > PHP version:
    => PHP 5.3.8 (cli) (built: Sep 21 2011 23:14:37)
    => Copyright (c) 1997-2011 The PHP Group
    => Zend Engine v2.3.0, Copyright (c) 1998-2011 Zend Technologies
    =>     with Xdebug v2.1.1, Copyright (c) 2002-2011, by Derick Rethans
    > mageekguy\atoum\tests\units\adapter...
    [S___________________________________________________________][1/1]
    => Test duration: 0.02 second.
    => Memory usage: 0.25 Mb.
    > Total test duration: 0.02 second.
    > Total test memory usage: 0.25 Mb.
    > Code coverage value: 100.00%
    > Running duration: 0.16 second.
    Success (1 test, 0 method, 2 assertions, 0 error, 0 exception) !
    Press <Enter> to reexecute, press any other key to stop...

Si le développeur presse une autre touche que Enter, atoum se terminera.

Dans le cas contraire, atoum ré-exécutera à nouveau les mêmes tests, sans que le développeur n'ait à
faire une autre action.

Dans le cas ou le code ne passe pas les tests avec succès, c'est à dire si des assertions ne sont
pas vérifiées ou si il y a eu des erreurs ou des exceptions, atoum se met également en attente:

    [shell]
    php tests/units/classes/adapter.php -l
    > atoum version DEVELOPMENT by Frédéric Hardy (/Users/fch/Atoum)
    > PHP path: /usr/local/bin/php
    > PHP version:
    => PHP 5.3.8 (cli) (built: Sep 21 2011 23:14:37)
    => Copyright (c) 1997-2011 The PHP Group
    => Zend Engine v2.3.0, Copyright (c) 1998-2011 Zend Technologies
    =>     with Xdebug v2.1.1, Copyright (c) 2002-2011, by Derick Rethans
    > mageekguy\atoum\tests\units\adapter...
    [F___________________________________________________________][1/1]
    => Test duration: 0.00 second.
    => Memory usage: 0.00 Mb.
    > Total test duration: 0.00 second.
    > Total test memory usage: 0.00 Mb.
    > Running duration: 0.17 second.
    Failure (1 test, 0 method, 1 failure, 0 error, 0 exception) !
    > There is 1 failure:
    => mageekguy\atoum\tests\units\adapter::test__call():
    In file /Users/fch/Atoum/tests/units/classes/adapter.php on line 17, mageekguy\atoum\asserters\string::isEqualTo() failed: strings are not equals
    -Reference
    +Data
    @@ -1 +1 @@
    -string(13) "4ea0354cd717c"
    +string(32) "19798c230d5462b3bdae194f364feffa"
    Press <Enter> to reexecute, press any other key to stop...

Tout comme dans le cas ou tout s'est bien passé, si le développeur presse une autre touche que
Enter, atoum se terminera.

Cependant, s'il presse la touche Enter, au lieu de rejouer les mêmes tests comme dans le cas ou les
tests ont été passés avec succès, atoum n'exécutera que les tests en échec, au lieu de les rejouer
dans leur intégralité.

Le développeur pourra alors dépiler les problèmes et rejouer les tests en erreur autant de fois que
nécessaire simplement en appuyant sur Enter.

De plus, une fois que tous les tests en échec passeront à nouveau avec succès, atoum exécutera
automatiquement la totalité de la suite de tests afin de détecter les éventuelles régressions
introduite par la ou les corrections effectuées par le développeur.

Bien évidemment, le mode "loop" ne prend en compte que
[le ou les fichiers de tests unitaires lancés](#fichiers-a-executer) par atoum.




## Le mode debug

Parfois, un test ne passe pas et il est difficile d'en découvrir la raison.

Dans ce cas, l'une des techniques possibles pour remédier au problème est de tracer le comportement
du code concerné, soit directement au cœur de la classe testée à l'aide de fonctions du type de
var_dump() ou print_r(), soit au niveau du test unitaire.

Et il se trouve que atoum dispose d'un certain nombre d'outils pour faciliter la tâche du
développeur dans ce dernier contexte.

Ces outils ne sont cependant actif que lorsque atoum est exécuté à l'aide de l'argument --debug,
afin que l'exécution des tests unitaires ne soit pas perturbée par les instructions relatives au
déboggage hors de ce contexte.

Lorsque l'argument --debug est utilisé, trois méthodes peuvent être activée:

* dump() qui permet de connaître le contenu d'une variable ;
* stop() qui permet d'arrêter l'exécution d'un test ;
* executeOnFailure() qui permet de définir une fonction anonyme ou une fermeture lexicale qui ne
sera exécutée qu'en cas d'échec d'une assertion.

Ces trois méthodes s'intègrent parfaitement dans l'interface fluide qui caractérise atoum.

### dump

La méthode dump() peut s'utiliser de la manière suivante:

    [php]
    $this
        ->if($foo = new foo())
        ->then
            ->object($foo->setBar($bar = new bar()))
                ->isIdenticalTo($foo)
            ->dump($foo->getBar())
    ;

Lors de l'exécution du test, le retour de la méthode foo::getBar() sera affiché sur la sortie
standard.

Il est également possible de passer plusieurs arguments à dump(), de la manière suivante:

    [php]
    $this
        ->if($foo = new foo())
        ->then
            ->object($foo->setBar($bar = new bar()))
                ->isIdenticalTo($foo)
            ->dump($foo->getBar(), $bar)
    ;

### stop

L'utilisation de la méthode stop() est également très simple:

    [php]
    $this
        ->if($foo = new foo())
        ->then
            ->object($foo->setBar($bar = new bar()))
                ->isIdenticalTo($foo)
            ->stop() // le test s'arrêtera ici si --debug est utilisé
            ->object($foo->getBar())
                ->isIdenticalTo($bar)
    ;

### executeOnFailure

La méthode executeOnFailure() est très puissante et tout aussi simple à utiliser.

Elle prend en effet en argument une fonction anonyme qui sera exécutée si et seulement si l'une des
assertions composant le test n'est pas vérifiée. Elle s'utilise de la manière suivante:

    [php]
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

Dans l'exemple précédent, contrairement à dump() qui provoque systématiquement l'affichage sur la
sortie standard le contenu des variables qui lui sont passées en argument, la fonction anonyme
passée en argument ne provoquera l'affichage du contenu de la variable foo que si l'une des
assertions suivantes est en échec.

Bien évidement, il est possible de faire appel plusieurs fois à executeOnFailure() dans une même
méthode de test pour définir plusieurs fonctions anonymes différentes devant être exécutées en cas
d'échec du test.





## Les méthodes d'initialisation

Lorsqu'il exécute les méthodes de test d'une classe, atoum suit le processus suivant:

- il exécute la méthode setUp() de la classe de test ;
- il lance un sous-processus PHP pour exécuter chaque méthode de test ;
- dans le sous-processus PHP, avant d'exécuter la méthode de test, il exécute la méthode
beforeTestMethod() de la classe de test ;
- dans le sous-processus PHP, il exécute la méthode de test ;
- dans le sous-processus PHP, il exécute la méthode afterTestMethod() de la classe de test ;
- une fois le sous-processus PHP terminé, il exécute la méthode tearDown()de la classe de test ;

Les méthodes setUp() et tearDown() permettent donc respectivement d'initialiser et de nettoyer
l'environnement de test pour l'ensemble des méthodes de test de la classe exécutée, à la différence
des méthodes beforeTestMethod() et afterTestMethod().

Ces deux méthodes permettent en effet respectivement d'initialiser et de nettoyer l'environnement
d'exécution des tests individuellement pour chacune des méthodes de test de la classe, puisqu'elles
sont exécutées dans le même sous-processus, au contraire de setUp() et tearDown().

C'est d'ailleurs la raison pour laquelle les méthodes beforeTestMethod() et afterTestMethod()
acceptent comme argument le nom de la méthode de test exécutée, afin de pouvoir ajuster les
traitements en conséquence.

    [php]
    <?php
    namespace vendor\project\tests\units;

    use
        mageekguy\atoum,
        vendor\project
    ;

    require __DIR__ . '/mageekguy.atoum.phar';

    class bankAccount extends atoum
    {
        public function setUp()
        {
            // Exécutée **avant l'ensemble** des méthodes de test.
            // Initialisation globale.
        }

        public function beforeTestMethod($method)
        {
            // Exécutée **avant chaque** méthode de test.

            switch ($method)
            {
                case 'testGetOwner':
                    // Initialisation pour testGetOwner().
                break;

                case 'testGetOperations':
                    // Initialisation pour testGetOperations().
                break;         
            }
        }

        public function testGetOwner()
        {
            ...
        }

        public function testGetOperations()
        {
            ...
        }

        public function afterTestMethod($method)
        {
            // Exécutée **après chaque** méthode de test.

            switch ($method)
            {
                case 'testGetOwner':
                    // Nettoyage pour testGetOwner().
                break;

                case 'testGetOperations':
                    // Nettoyage pour testGetOperations().
                break;         
            }
        }

        public function tearDown()
        {
            // Exécutée après l'ensemble des méthodes de test.
            // Nettoyage global.
        }
    }

Par défaut, les méthodes setUp(), beforeTestMethod(), afterTestMethod() et tearDown() ne font
absolument rien.

Il est donc de la responsabilité du programmeur de les surcharger lorsque c'est nécessaire dans les
classes de test concernées.




## Fournisseurs de données

Pour vous aider à tester efficacement vos classes, atoum met à votre disposition des fournisseurs de
données (data provider en anglais).

Un fournisseur de données est une méthode d'une classe de test chargée de générer des arguments pour
une méthode de test, arguments qui seront utilisés par ladite méthode pour valider des assertions.

La définition du fournisseur de données qui doit être utilisé par une méthode de test se fait grâce
à l'annotation @dataProvider appliquée à la méthode de test concernée, de la manière suivante:

    [php]
    class calculator extends atoum
    {
        /**
         * @dataProvider sumDataProvider
         */
        // Veillez à définir le bon nombre d'arguments
        public function testSum($a, $b)
        {
            $this
                ->if($calculator = new project\calculator())
                ->then
                    ->integer($calculator->sum($a, $b))->isEqualTo($a + $b)
            ; 
        }

        ...
    }

Évidemment, il ne faut pas oublier de définir, au niveau de la méthode de test,
les arguments correspondant à ceux qui seront retournés par le fournisseur de données.
Si ce n'est pas le cas, atoum générera une erreur lors de l'exécution des tests.

Une fois l'annotation définie, il n'y a plus qu'à créer la méthode correspondante:

    [php]
    class calculator extends atoum
    {
        ...

        // Fournisseur de données de testSum().
        public function sumDataProvider()
        {
            return array(
                array( 1, 1),
                array( 1, 2),
                array(-1, 1),
                array(-1, 2),
            );
        }
    }

Lors de l'exécution des tests, atoum appellera la méthode de test testSum() successivement avec les
arguments (1, 1), (1, 2), (-1, 1) et (-1, 2) renvoyés par la méthode sumDataProvider().

**Note**: attention, l'isolation des tests ne sera pas utilisée dans ce contexte,
ce qui veut dire que chacun des appels successifs à la méthode testSum() sera réalisé dans le même
processus PHP.

**Note**: un fournisseur de données peut au choix retourner un tableau ou bien un itérateur.





## Les bouchons

atoum dispose d'un système de bouchonnage (mock en anglais) puissant et simple à mettre en œuvre.

### À partir d'une interface ou d'une classe existante

Il y a plusieurs manière de créer un bouchon à partir d'une interface ou d'une classe (abstraite ou
non).

la plus simple est de créer un objet dont le nom absolu est préfixé par \mock:

    [php]
    // création d'un bouchon de l'interface \Countable
    $countableMock = new \mock\Countable;
    
    // création d'un bouchon de la classe abstraite
    // \Vendor\Project\AbstractClass
    $vendorAppMock = new \mock\Vendor\Project\AbstractClass;
    
    // création d'un bouchon de la classe \StdClass
    $stdObject     = new \mock\StdClass;

Si vous désirez changer le nom de la classe ou son espace de nom, vous devez utiliser le
mockGenerator.

Sa méthode generate prend 3 paramètres:

* le nom de l'interface ou de la classe à bouchonner ;
* le nouvel espace de nom, optionnel ;
* le nouveau nom de la classe, optionnel.

    [php]
    // création d'un bouchon de l'interface \Countable vers \MyMock\Countable
    // on ne change que l'espace de nom
    $this->mockGenerator->generate('\Countable', '\MyMock');
    $countableMock = new \myMock\Countable;
    
    // création d'un bouchon de la classe abstraite
    // \Vendor\Project\AbstractClass vers \MyMock\AClass
    // on change l'espace de nom et le nom de la classe
    $this->mockGenerator->generate('\Countable', '\MyMock', 'AClass');
    $vendorAppMock = new \mock\Vendor\Project\AbstractClass;
    
    // création d'un bouchon de la classe \StdClass vers \mock\OneClass
    // on ne change que le nom de la classe
    $this->mockGenerator->generate('\StdClass', null, 'OneClass');
    $stdObject     = new \mock\OneClass;

**Note**: si vous n'utilisez que le premier argument et ne changer ni l'espace de nom, ni le nom de
la classe, alors c'est équivalent à la première solution.

    [php]
    $countableMock = new \mock\Countable;

    // est équivalent à:

    $this->mockGenerator->generate('\Countable');   // inutile
    $countableMock = new \mock\Countable;


### À partir de rien

Vous pouvez également créer un bouchon qui ne soit pas lié à une interface ou une classe (abstraite
ou non) existante.

Pour cela, et bien faite comme si elle existait !

En effet, le code suivant fonctionne parfaitement:

    [php]
    $firstMockedObject  = new \mock\MyUnknownClass;
    $secondMockedObject = new \mock\My\Unknown\Class;

### Modifier le comportement d'un bouchon

Un fois le bouchon créé et instancié, il est souvent utile de pouvoir modifier le comportement de
ses méthodes.

Pour cela, il faut passer par son contrôleur en utilisant la méthode getMockController().

**Note**: vous ne pouvez redéfinir que les méthodes publiques.

    [php]
    $databaseClient = new \mock\Database\Client();

    // redéfinie la méthode connect
    $databaseClient->getMockController()->connect = function() {};


    // redéfinie la méthode select
    $databaseClient->getMockController()->select = function() {
        return array();
    };

    // redéfinie la méthode query avec des arguments
    $databaseClient->getMockController()->query = function(Query $query) {
        switch($query->type) {
            case Query::SELECT:
                return array();
            break;

            default;
                return null;
        }
    };

**Note**: vous pouvez définir directement une valeur à retourner systématiquement

    [php]
    // indique que la méthode query retourne systématiquement un tableau vide
    $databaseClient->getMockController()->query = array();

    // équivalent à:
    $databaseClient->getMockController()->query = function() {
        return array();
    };


### Cas particulier du constructeur

Pour bouchonner le constructeur d'une classe, il faut:

* créer une instance de la classe \atoum\mock\controller avant d'appeler le constructeur du
bouchon ;
* définir via ce contrôleur le comportement du constructeur du bouchon à l'aide d'une fonction
anonyme ;
* appeler sur le contrôleur la méthode injectInNextMockInstance().

    [php]
    $controller = new \atoum\mock\controller();
    $controller->__construct = function() {};
    $controller->injectInNextMockInstance();

    $databaseClient = new \mock\Database\Client();


### Tester un bouchon

atoum vous permet de vérifier qu'un bouchon a été utilisé correctement.

    [php]
    $databaseClient = new \mock\Database\Client();
    $databaseClient->getMockController()->connect = function() {};
    $databaseClient->getMockController()->query   = array();

    $bankAccount = new \Vendor\Project\Bank\Account();
    $this
        // utilisation du bouchon via un autre objet
        ->array($bankAccount->getOperation($databaseClient))
            ->isEmpty()

        // test du bouchon
        ->mock($databaseClient)
            ->call('query')
                ->once()        // vérifie que la méthode query
                                // n'a été appelé qu'une seule fois
    ;

**Note**: reportez-vous à la documentation sur l'assertion [mock](#mock) pour obtenir plus
d'informations sur les tests des bouchons.