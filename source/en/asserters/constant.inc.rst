.. _constant-anchor:

constant
********

It's the assertion dedicated to the constant.

.. _constant-isEqualTo:

isEqualTo
=========

``isEqualTo`` checks that the given constant is equal to the given value.

.. code-block:: php

	<?php
	define('YOLO', 'yolo');

	namespace Foo\test\unit
	{
		class Bar extends \atoum
		{
		    public function testWithConstant()
		    {
				$this->constant(YOLO)
					->isEqualTo('yolo');
				$this->string(YOLO)
					->isEqualTo('yolo');
		    }
		}
	}
