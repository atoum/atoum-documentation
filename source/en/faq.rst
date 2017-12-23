.. _faq:

FAQ
###

.. _faq_error_log:

If you get an unknow error check if you have an error_log ?
***********************************************************
If you use `error_log`, you will meet "Error UNKNOWN in" error from atoum. To avoid this, just use a :ref:`mock of the native function <mock-native-function>` `error_log`

.. code-block:: php

	namespace Foo
	{
		class TestErrorLog
		{
		    public function runErrorLog()
		    {
		        error_log('message');
		        return true;
		    }
		}
	}

	namespace Foo\test\unit
	{
		class TestErrorLog extends \atoum
		{
		    public function testRunErrorLog()
		    {
				$this->function->error_log = true;
				$this->newTestedInstance;
				$this->boolean($this->testedInstance->runErrorLog())->isTrue;
				$this->function('error_log')->wasCalled()->once();
		    }
		}
	}


.. _faq_ogo:

Is atoum always named atoum ?
*****************************
No, at start, atoum was named ogo. When you write PHP on an azerty keyboard, then you switch on key to the left, you write ogo.

.. _faq_license:

What's the atoum license ?
**************************
atoum is released under the BSD-3-Clause License. See the bundled `LICENSE <https://github.com/atoum/atoum/blob/master/LICENSE>`_ file for details.

What's the roadmap ?
********************
The easier way to find it, is to check the `milestone tags <https://github.com/atoum/atoum/milestones>`_ on github.
