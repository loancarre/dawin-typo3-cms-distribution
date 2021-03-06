# TYPO3-CMS installation

With [composer](https://getcomposer.org/) and [typo3_console](https://packagist.org/packages/helhum/typo3-console)

## Prerequisites

- PHP >= 7.1
- MySQL >= 5.7
- Apache >= 2.1 or Nginx (if available)
- A database user with granted access to **an empty database**

## Development

This distribution is provided with a [laradock](http://laradock.io/) configuration to run TYPO3 with docker.

## Installation

### Using docker

    git clone --recursive https://github.com/pitchart/dawin-typo3-cms-distribution.git cms
    cd cms/laradock-dawin-t3/
    make install-dev

The TYPO3 website is available at http://localhost/ and the backend at http://localhost/typo3/

### Using system environment

You must [download composer binaries](https://getcomposer.org/download/) before to start installation.

#### Clone the repository

    git clone --recursive https://github.com/pitchart/dawin-typo3-cms-distribution.git cms
    cd cms

#### Install vendors with composer

    composer install

#### Install TYPO3 with typo3_console

    php vendor/bin/typo3cms install:setup

#### Post installation commands

##### Activate an extension

    php vendor/bin/typo3cms extension:activate **extension_key**

##### Flush cache

    php vendor/bin/typo3cms cache:flush

##### Launch a PHP server

    php -S localhost:<port> -t web/

The TYPO3 website is available at http://localhost:8080/ and the backend at http://localhost:8080/typo3/

## TIPS

### Adding your extension's class loading informations

    // composer.json
    "autoload": {
      "psr-4": {
        "MyVendor\\MySitePackage\\": "web/typo3conf/ext/my_site_package/Classes"
      }
    }

### composer.json template for an extension

    {
      "name": "my-vendor/my-extension-key",
      "type": "typo3-cms-extension",
      "version": "x.x.x",
      "description": "...",
      "require": {
        "typo3/cms-core": "^8.7"
      },
      "replace": {
          "my_extension_key": "self.version",
          "my-vendor/my-extension-key": "self.version"
      },
      "autoload": {
        "psr-4": {
          "MyVendor\\MyExtensionKey\\": "Classes/"
        }
      }
    }
    
### Show php error in development mode

By default Typo3 replaces php errors by a message in order to avoid stopping the page's loading if a php error has occured.
In development mode you need to deactivate this mode.

In List Go to the root page (Page next to earth), then edit the template record. 
In the field setup add the following code:

    config.contentObjectExceptionHandler = 0
    
https://docs.typo3.org/typo3cms/TyposcriptReference/Setup/Config/Index.html#contentobjectexceptionhandler

## References
- [TYPO3 extensions available for composer usage](https://composer.typo3.org/satis.html)
- [TYPO3 Documentations](https://docs.typo3.org/typo3cms/References.html)
- [Composer and TYPO3 7LTS](https://usetypo3.com/typo3-and-composer.html)
- [How to configure class loading for extensions in TYPO3 7LTS](http://insight.helhum.io/post/130876393595/how-to-configure-class-loading-for-extensions-in)
- [Add a Git repository as a package using Composer for PHP](http://www.andrew-kirkpatrick.com/2012/10/add-a-git-repository-as-a-package-using-composer-for-php/)
- [Typo3 viewhelpers documentation](https://fluidtypo3.org/viewhelpers.html)
