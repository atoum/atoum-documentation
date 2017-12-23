.. _asserters_collection:

Asserters collection
####################

..
.. .. sidebar:: TOC
..
..  .. contents::
..    :depth: 2
..    :local:

To write more explicit and less wordy tests, atoum provide several asserters who give access to specific assertions related to tested var.

As atoum different asserters are specializations of manipulated items, asserters inherits from asserters they specialize.
It help keep consistency between asserters and force to use same assertion names.

This is the asserters inheritance tree:

.. code-block:: shell

    -- asserter (abstract)
        |-- error
        |-- mock
        |-- stream
        |-- variable
        |   |-- array
        |   |    `-- castToArray
        |   |-- boolean
        |   |-- class
        |   |    `-- testedClass
        |   |-- integer
        |   |   |-- float
        |   |   `-- sizeOf
        |   |-- object
        |   |   |-- dateInterval
        |   |   |-- dateTime
        |   |   |   `-- mysqlDateTime
        |   |   |-- exception
        |   |   `-- iterator
        |   |       `-- generator
        |   |-- resource
        |   `-- string
        |       |-- castToString
        |       |-- hash
        |       |-- output
        |       `-- utf8String
        `-- function


.. note::
    The general asserter/assertion syntaxe is:
    ``$this->[asserter]($value)->[assertion];``

.. note::
    Most of the assertions are fluent, as you will see below.

.. note::
	At the end of this chapter you will find several :ref:`tips & tricks<asserter_tips>` related to assertion and asserter, don't forget to read it!

.. please keep this ordered in an alphabetic way thanks

.. include:: asserters/afterDestructionOf.inc.rst
.. include:: asserters/array.inc.rst
.. include:: asserters/boolean.inc.rst
.. include:: asserters/castToArray.inc.rst
.. include:: asserters/castToString.inc.rst
.. include:: asserters/class.inc.rst
.. include:: asserters/dateInterval.inc.rst
.. include:: asserters/dateTime.inc.rst
.. include:: asserters/error.inc.rst
.. include:: asserters/exception.inc.rst
.. include:: asserters/extension.inc.rst
.. include:: asserters/float.inc.rst
.. include:: asserters/function.inc.rst
.. include:: asserters/generator.inc.rst
.. include:: asserters/hash.inc.rst
.. include:: asserters/integer.inc.rst
.. include:: asserters/mock.inc.rst
.. include:: asserters/mysqlDateTime.inc.rst
.. include:: asserters/object.inc.rst
.. include:: asserters/output.inc.rst
.. include:: asserters/resource.inc.rst
.. include:: asserters/sizeOf.inc.rst
.. include:: asserters/stream.inc.rst
.. include:: asserters/string.inc.rst
.. include:: asserters/utf8String.inc.rst
.. include:: asserters/variable.inc.rst

.. include:: asserters/ztips.inc.rst
