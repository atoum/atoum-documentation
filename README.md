# Documentation of atoum
This documentation is available on [docs.atoum.org](http://docs.atoum.org).

## Building the documentation
To build this documentation, follow this instruction :

1. Clone git://github.com/marmotz/sia.git & git clone git://github.com/marmotz/LinksChecker.git inside the current project
1. Run <code>composer install</code> on sia project
1. Create output directory (output/fr & output/en)
1. Launch the documentation generator : <code>./sia/bin/sia -i fr -o output/fr -t atoum</code>
1. Check dead link
1. Enjoy

Or simply run

	git clone git://github.com/marmotz/sia.git
	git clone git://github.com/marmotz/LinksChecker.git
	composer install -d sia
	mkdir -p output/fr output/en
	./sia/bin/sia -i fr -o output/fr -t atoum
	./sia/bin/sia -i en -o output/en -t atoum
	./LinksChecker/check output/fr/chapitre1.html && ./LinksChecker/check output/en/chapter1.html
