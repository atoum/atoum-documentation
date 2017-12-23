.. _function-anchor:

function
********

It's the assertion dedicated to the :ref:`native function <mock-native-function>` that were mocked.

.. _function-wasCalled:

wasCalled
=========

``wasCalled`` checks that the mocked function was called.

.. _function-wasCalledWithArguments:

wasCalledWithArguments
======================

``wasCalledWithArguments`` allow to check some of the arguments from the call to the mocked function.

.. _function-wasCalledWithIdenticalArguments:

wasCalledWithIdenticalArguments
===============================

``wasCalledWithIdenticalArguments`` allow to check all the arguments from the call to the mocked function.

.. _function-wasCalledWithoutAnyArgument:

wasCalledWithoutAnyArgument
===========================

``wasCalledWithoutAnyArgument`` validated that the call to the mocked function was made without arguments.


.. _function-count_all:

Count call
==========

If you want you can do one more assertion by counting the number of call.

.. code-block:: php

	<?php
	public function testMethodWithAnErrorLog()
	{
		$this->function->error_log = true;

		$this
			->if($this->newTestedInstance())
			->and($this->testedInstance->methodWithAnErrorLog($notExcepted = uniqid()))
			->then
				->function('error_log')
					->wasCalledWithArguments('Value ' . $notExcepted . ' is not excepted here')
						->once()
		;
	}

Here, we assert that our mocked function was called once, with the given arguments.

.. _function-after:

after
`````

``after`` checks if the mocked function has been called after the one passed as parameter.


.. hint::
   ``after`` is the same as the one on the ``mock`` asserter.
   For more information, refer to the documentation of :ref:`mock::after <mock-after>`

.. _function-at-least-once:

atLeastOnce
```````````

``atLeastOnce`` check if mocked function has been called at least once.

.. hint::
   ``atLeastOnce`` is the same as the one on the ``mock`` asserter.
   For more information, refer to the documentation of :ref:`mock::atLeastOnce <at-least-once>`

.. _function-before:

before
``````

``before`` checks if the mocked function has been called before the one passed as parameter.

.. hint::
   ``before`` is the same as the one on the ``mock`` asserter.
   For more information, refer to the documentation of :ref:`mock::before <mock-before>`

.. _function-exactly-anchor:

exactly
```````

``exactly`` check that the mocked function has been called a specific number of times.

.. hint::
   ``exactly`` is the same as the one on the ``mock`` asserter.
   For more information, refer to the documentation of :ref:`mock::exactly <exactly-anchor>`

.. _function-never-anchor:

never
`````

``never`` check that the mocked function has never been called.

.. hint::
   ``never`` is the same as the one on the ``mock`` asserter.
   For more information, refer to the documentation of :ref:`mock::never <never-anchor>`

.. _function-once-twice-thrice:

once/twice/thrice
`````````````````
This asserters check that the mocked function has been called exactly:

* once
* twice
* thrice

.. hint::
   ``once`` is the same as the one on the ``mock`` asserter.
   For more information, refer to the documentation of :ref:`mock::once/twice/thrice <once-twice-thrice>`
