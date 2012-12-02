# Cookbook

## Usage with symfony 1.4

If you want to use atoum inside a symfony 1.4 project, the sfAtoumPlugin is available at the
following adress : 
[https://github.com/atoum/sfAtoumPlugin](https://github.com/atoum/sfAtoumPlugin).

## Installation

There is many ways to install the plugin in your project : 

* via composer
* via git submodules

### Using composer

Add this in your composer.json :

    [shell]
    "require"     : {
      "atoum/sfAtoumPlugin": "*"
    },


After a `php composer.phar update` the plugin should be in the plugin folder and atoum
in the vendor folder.

Then in your ProjectConfiguration file you have to activate the plugin and define the atoum path.

    [php]
    sfConfig::set('sf_atoum_path', dirname(__FILE__) . '/../vendor/atoum/atoum');

    if (sfConfig::get('sf_environment') != 'prod')
    {
      $this->enablePlugins('sfAtoumPlugin');
    }

### Using a git submodule

Install atoum as a submodule

    [shell]
    git submodule add git://github.com/atoum/atoum.git lib/vendor/atoum

Install sfAtoumPlugin as a git submodule

    [shell]
    git submodule add git://github.com/atoum/sfAtoumPlugin.git plugins/sfAtoumPlugin

Add the plugin in your ProjectConfiguration file

    [php]
    if (sfConfig::get('sf_environment') != 'prod')
    {
      $this->enablePlugins('sfAtoumPlugin');
    }


## Write tests

Tests must include the bootstrap :

    [php]
    require_once __DIR__ . '/../../../../plugins/sfAtoumPlugin/bootstrap/unit.php';


## Launch tests

You can launch tests with this symfony command : 

    [shell]
    ./symfony atoum:test


You can pass a configuration any atoum configuration, so for example, you can pass
a configuration file like this : 

    [php]
    php symfony atoum:test -c config/atoum/hudson.php


