.. Documentation reST http://rest-sphinx-memo.readthedocs.org/en/latest/ReST.html
   In reST, the headers should be highlighted by = *-^ ' ~':.'"# and the order does not matter.
   Nevertheless, in the doc atoum, here's the selected order: #, *, =,-, ', ^, ',:,., ' "
   If you have more than 4 levels, split your file into multiple files.

.. _home:

What is atoum?
==============

atoum is a unit test framework like PHPUnit or SimpleTest, but it has a few advantages over these:

* It's modern and uses the innovations of latest PHP versions;
* It's simple and easy to learn;
* It's intuitive because its syntax is close to the English natural language;
* despite constant changes to atoum, backward compatibility is one of the priorities of its developers.

You can find more information on the `official website <http://atoum.org/>`_.

.. toctree::
   :numbered:
   :maxdepth: 2

   start_with_atoum
   installation
   first_test
   running_tests
   how_to_write_test_cases
   asserters
   mocking_systems
   engine
   mode-loop
   mode-debug
   fine_tuning
   configuration_bootstraping
   annotations
   option_cli
   cookbook
   extensions
   ide
   faq
   contribute
   licences
