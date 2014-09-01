ZF2 Application Skeleton
===========================

Introduction
------------
This is a simple, skeleton application using the ZF2 MVC layer and module
systems. This application is meant to be used as a starting place for those
looking to develop new site or module on top of my Cornerstone base
application.

This skeleton application can be used for developing new independent,
composable Module or it can be used to create a skeleton for a Site. In fact,
every module you develop using Cornerstone should be a module and an
independent site for testing.

I have separated the application skeleton from the Cornerstone module itself so
that updates to what I consider the base application can be composed in without
any need for manual merging. For instance if one of the built in Cornerstone
routes changes, you simply run composer update and pull in the latest changes.

Requirements
------------
As noted in the composer file, this module requires Zend Framework 2.2 or
higher. In addition, it requires `oakensoul/Cornerstone`, though realistically
any Module providing an "Application" would work. Though, if you were going
to do that, you should probably just use the ZF2 Skeleton Application that
already exists.

Installation with Composer
--------------------------
The initial creation of a project should be done by performing a clone using
composer; make sure to answer "Yes" when it prompts `Do you want to remove the
existing VCS (.git, .svn..) history?`.
```
composer create-project -sstable oakensoul/application-skeleton path/to/install
```

Generate a Configuration File
------------------------------------------
Create a config file in config/autoload/<environment>.json. Use the
example_environment.json.dist file as a base if needed. Note, you should likely
add that config file to your .gitignore if it contains any production settings.

Generate Your Virtual Host
--------------------------
To generate a virtual host you need to run the packaged cli script. Note, this
depends on the oakensoul/Cornerstone module to run.
```
./console.php application build-vhost --env=<environment>
```

Update the `module` Folder
--------------------------
If you are creating a new module using this Application skeleton, simply rename
the ApplicationBlueprint folder to match the name of your new module. Note, you
will need to update various references to it throughout the code.

```
$ cd module/
$ mv ApplicationBlueprint NewModuleName
```

Namespace Updates for `Module.php` and `IndexController.php`
```
namespace ApplicationBlueprint;
```

becomes

```
namespace NewModuleName;
```

Update Configuration Files
--------------------------
### config/application.config.php

Update the `$modules` array and change `ApplicationBlueprint` to your module name (e.g., NewModuleName).
```
$modules = array (
    'Application',
    'ApplicationBlueprint'
);
$application_config['modules'] = $modules;
```

becomes

```
$modules = array (
    'Application',
    'NewModuleName'
);
$application_config['modules'] = $modules;
```

### config/autoload/global.php
Change the value of the `Region` key to match your module slug
Change the value of the `Domain` key to match your domain

For example, when I test any of my modules I tend to set them up as:
  http://vagrant.modulename.application-blueprint.com/

```
return array (
    'Installation' => array (
        'Vhost' => array (
            'Server' => array (
                'Domain' => 'application-blueprint',
                'Region' => 'vagrant.www.'
            )
        )
    )
);
```

becomes

```
return array (
    'Installation' => array (
        'Vhost' => array (
            'Server' => array (
                'Domain' => 'oakensoul',
                'Region' => 'vagrant.module-name.'
            )
        )
    )
);
```

Or, update it to match your new sites region and domain:
```
return array (
    'Installation' => array (
        'Vhost' => array (
            'Server' => array (
                'Domain' => 'oakensoul',
                'Region' => 'www.'
            )
        )
    )
);
```

### module/<module name>/config/module.config.php
#### Add routes

Add any routes you would like as necessary. Check out the Cornerstone
documentation for available routes to extend from as well as some
built in values you can set to enforce the https scheme, set
default theme and localization, etc.

#### Update Views (If Needed)
You typically will not need to update the view data. If necessary, you would place your changes in this block:
```
$view_manager = array (
    'template_path_stack' => array (
        __DIR__ . '/../view'
    )
);
```

### module/<module name>/Module.php

#### Update Invokables
```
$invokables['Application\Controller\Index'] = 'ApplicationBlueprint\Http\Controller\IndexController';
```

becomes

```
$invokables['Application\Controller\Index'] = 'NewModuleName\Http\Controller\IndexController';
```

Note: In the example, we are overloading the route Application\Controller\Index
to resolve to our module's landing page. This can be helpful for testing
modules that don't normally have controllers and routes, specifically libraries.

If you are going to use this method as I have with a few of my modules, make
sure that your final "Site" module is the last to overload that Invokable. At
the very least you can point it back at the Cornerstone index controller.

Add additional invokables and factories as necessary.

Update/Create Automated Tests
-----------------------------
All PHPUnit based tests are located under the `test/phpunit` directory with CasperJS tests
located under the `test/caspserjs` directory. Update or create tests as you develop your module.

Documentation
-------------
Please update your module documentation, everyone loves great documentation!

## Contributors

Collaborators:
* [@Oakensoul](https://github.com/oakensoul)


## Contributing

* [Getting Started](doc/CONTRIBUTING.md)
* [Bug Reports](doc/CONTRIBUTING.md#bug-reports)
* [Feature Requests](doc/CONTRIBUTING.md#feature-requests)
* [Pull Requests](doc/CONTRIBUTING.md#pull-requests)

# LICENSE
This module is licensed using the Apache-2.0 License:

```
Copyright (c) 2013, Robert G. Johnson Jr. @Oakensoul and Project Contributors
```