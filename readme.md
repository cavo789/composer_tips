![Banner](images/banner.png)

# Composer tips

> Some tips & tricks for Composer

* [Official documentation](https://getcomposer.org/doc/)
* [Composer cheat sheet](https://composer.json.jolicode.com/)

* [Adding local packages](#adding-local-packages)
* [Update on dev, install on prod](#update-on-dev-install-on-prod)
* [Get the list of outdated packages without installing them](#get-the-list-of-outdated-packages-without-installing-them)
* [Troubleshooting](#troubleshooting)
  * [Self-update](#self-update)
  * [Config](#config)
  * [Diagnose](#diagnose)
  * [Composer is slow](#composer-is-slow)
    * [Disable Xdebug](#disable-xdebug)
    * [Get more informations](#get-more-informations)
* [License](#license)

## Adding local packages

> [Official doc](https://getcomposer.org/doc/05-repositories.md#path)

Imagine you've a tree structure like this:

```text
c:\christophe\repositories\
    |-- pandoc                  your package
        |-- composer.json           "name": "avonture/pandoc"
    |-- marknotes               your project
        |-- composer.json
```

The `pandoc` package defined a package called `avonture/pandoc` and you wish to use it in the `marknotes` project.

**So, in short, you want to use a local package in your project.**

In your project `marknotes\composer.json` file, change two settings:

1. In the list of repositories, add a `path` entry and set the `url` to the relative path to the package so:

```json
  "repositories": [
    {
      "type": "path",
      "url": "../pandoc/"
    }
  ]
```

2. Then add your package in the `require` list like this:

```json
  "require": {
    "avonture/pandoc": "dev-master"
  }
```

Now, run a `composer install` to install that new package in your project. 

Curious? Go to your project, navigate in the `vendor` folder and you'll see that the package is a *NTFS junction* (similar to a symbolic link) to your local package. We can force a symlink like below but, then, `composer install` should be started from a DOS Prompt under Admin privileges.

```json
  "repositories": [
    {
      "type": "path",
      "url": "../pandoc/",
      "options": {
        "symlink": true
      }
    }
  ]
```

Nice thing: any changes in your package is therefore immediately done in your project. **Really handy when you develop the package and the project that uses it at the same time.**

## Get information's about installed packages

Show information about installed packages.

```bash
composer info
```

Returns:

```text
doctrine/instantiator              1.2.0   A small, lightweight utility to instantiate objects in PHP without invoking their constructors
myclabs/deep-copy                  1.9.1   Create deep copies (clones) of your objects
phar-io/manifest                   1.0.3   Component for reading phar.io manifest information from a PHP Archive (PHAR)
phar-io/version                    2.0.1   Library for handling version information and constraints
phpdocumentor/reflection-common    1.0.1   Common reflection classes used by phpdocumentor to reflect the code structure
phpdocumentor/reflection-docblock  4.3.1   With this component, a library can provide support for annotations via DocBlocks or otherwise retrieve information that is embedded in a DocBlock.
phpdocumentor/type-resolver        0.4.0
phpspec/prophecy                   1.8.1   Highly opinionated mocking framework for PHP 5.3+
phpunit/php-code-coverage          7.0.6   Library that provides collection, processing, and rendering functionality for PHP code coverage information.
phpunit/php-file-iterator          2.0.2   FilterIterator implementation that filters files based on a list of suffixes.
phpunit/php-text-template          1.2.1   Simple template engine.
phpunit/php-timer                  2.1.2   Utility class for timing
phpunit/php-token-stream           3.0.2   Wrapper around PHP's tokenizer extension.
phpunit/phpunit                    8.2.4   The PHP Unit Testing framework.
sebastian/code-unit-reverse-lookup 1.0.1   Looks up which function or method a line of code belongs to
sebastian/comparator               3.0.2   Provides the functionality to compare PHP values for equality
sebastian/diff                     3.0.2   Diff implementation
sebastian/environment              4.2.2   Provides functionality to handle HHVM/PHP environments
sebastian/exporter                 3.1.0   Provides the functionality to export PHP variables for visualization
sebastian/global-state             3.0.0   Snapshotting of global state
sebastian/object-enumerator        3.0.3   Traverses array structures and object graphs to enumerate all referenced objects
sebastian/object-reflector         1.1.1   Allows reflection of object attributes, including inherited and non-public ones
sebastian/recursion-context        3.0.0   Provides functionality to recursively process PHP variables
sebastian/resource-operations      2.0.1   Provides a list of PHP built-in functions that operate on resources
sebastian/type                     1.1.3   Collection of value objects that represent the types of the PHP type system
sebastian/version                  2.0.1   Library that helps with managing the version number of Git-hosted PHP projects
symfony/polyfill-ctype             v1.11.0 Symfony polyfill for ctype functions
theseer/tokenizer                  1.1.3   A small library for converting tokenized PHP source code into XML and potentially other formats
webmozart/assert                   1.4.0   Assertions to validate method input/output with nice error messages.
```

## Update on dev, install on prod

On the production server, to make sure to install exactly the same dependencies than on your last dev installation, prefer to use `composer install`. Composer will then install the same version as defined in the `composer.lock` file and not, like with `composer update` the last available version for all your dependencies.

To make sure your `composer.lock` file is up-to-date before going to production, run a `composer validate` so `composer.lock` is refreshed.

`composer install` will retrieve version information's from `composer.lock`.

## Get the list of outdated packages without installing them

Shows a list of installed packages that have updates available

```bash
composer outdated
```

Output will looks like:

```text
doctrine/instantiator             1.2.0   dev-master 6a1471d A small, lightweight utility to instantiate objects in PHP without invoking their constructors
myclabs/deep-copy                 1.9.1   2.x-dev 0ef5985    Create deep copies (clones) of your objects
phar-io/manifest                  1.0.3   dev-master 3d94e3b Component for reading phar.io manifest information from a PHP Archive (PHAR)
phar-io/version                   2.0.1   dev-master 45a2ec5 Library for handling version information and constraints
phpdocumentor/reflection-common   1.0.1   dev-master b0843c8 Common reflection classes used by phpdocumentor to reflect the code structure
phpdocumentor/reflection-docblock 4.3.1   dev-master cd72d39 With this component, a library can provide support for annotations via DocBlocks or otherwise retrieve information that is embedded in a DocBlock.
phpdocumentor/type-resolver       0.4.0   dev-master d6b5291
phpspec/prophecy                  1.8.1   dev-master 451c3cd Highly opinionated mocking framework for PHP 5.3+
phpunit/php-code-coverage         7.0.6   7.0.10             Library that provides collection, processing, and rendering functionality for PHP code coverage information.
phpunit/php-token-stream          3.0.2   3.1.1              Wrapper around PHP's tokenizer extension.
phpunit/phpunit                   8.2.4   8.5.x-dev fb668be  The PHP Unit Testing framework.
sebastian/environment             4.2.2   4.2.3              Provides functionality to handle HHVM/PHP environments
sebastian/exporter                3.1.0   3.1.2              Provides the functionality to export PHP variables for visualization
symfony/polyfill-ctype            v1.11.0 dev-master 4719fa9 Symfony polyfill for ctype functions
theseer/tokenizer                 1.1.3   dev-master 901c88d A small library for converting tokenized PHP source code into XML and potentially other formats
webmozart/assert                  1.4.0   dev-master e3be5e4 Assertions to validate method input/output with nice error messages.
```

## Troubleshooting

### Self-update

Make sure you're using the latest version of Composer.

```bash
composer self-update
```

### Config

Display the list of configuration options of Composer.

```bash
composer config --list
```

In the output, make sure that:

* `repositories.packagist.org.url`: make sure you're using `https`. If not, you can force it by running `composer config --global repo.packagist composer https://packagist.org`
* `github-protocols`: we can force `https` and avoid timeout f.i. if `ssh` can't be used on your network. Force `https` by running `composer config --global github-protocols https` (note: seems to be only for `GitHub.com`)

```text
[repositories.packagist.org.type] composer
[repositories.packagist.org.url] https?://repo.packagist.org
[repositories.packagist.org.allow_ssl_downgrade] true
[process-timeout] 300
[use-include-path] false
[preferred-install] auto
[notify-on-install] true
[github-protocols] [https, ssh]
[vendor-dir] vendor (C:\Christophe\Repository\Sample/vendor)
[bin-dir] {$vendor-dir}/bin (C:\Christophe\Repository\Sample/vendor/bin)
[cache-dir] C:/Users/Christophe/AppData/Local/Composer
[data-dir] C:/Users/Christophe/AppData/Roaming/Composer
[cache-files-dir] {$cache-dir}/files (C:/Users/Christophe/AppData/Local/Composer/files)
[cache-repo-dir] {$cache-dir}/repo (C:/Users/Christophe/AppData/Local/Composer/repo)
[cache-vcs-dir] {$cache-dir}/vcs (C:/Users/Christophe/AppData/Local/Composer/vcs)
[cache-ttl] 15552000
[cache-files-ttl] 15552000
[cache-files-maxsize] 300MiB (314572800)
[bin-compat] auto
[discard-changes] false
[autoloader-suffix]
[sort-packages] false
[optimize-autoloader] false
[classmap-authoritative] false
[apcu-autoloader] false
[prepend-autoloader] true
[github-domains] [github.com]
[bitbucket-expose-hostname] true
[disable-tls] false
[secure-http] true
[cafile]
[capath]
[github-expose-hostname] true
[gitlab-domains] [gitlab.com]
[store-auths] prompt
[archive-format] tar
[archive-dir] .
[htaccess-protect] true
[use-github-api] true
[lock] true
[home] C:/Users/Christophe/AppData/Roaming/Composer
```

### Diagnose

Checks common errors to help debugging problems ([official doc](https://getcomposer.org/doc/03-cli.md#diagnose))

```bash
composer diagnose
```

### Composer is slow

#### Disable Xdebug

When Xdebug is enabled on CLI, composer will run in a non-optimized way.

To detect if Xdebug is enable, run `php -m | findstr xdebug` on the command prompt. If you don't see `xdebug` as the result of the command, it means that the xdebug module isn't loaded and this is fine.

#### Get more informations

Here is how to detect the slowest part of the update action.

* `-vvv`: Increase the verbosity of messages: 1 for normal output, 2 for more verbose output and 3 for debug
* `--profile`: Display timing and memory usage information

```bash
composer update -vvv --profile
```

The output of such statement will looks like below and will allow to detect bottlenecks.

```text
Reading ./composer.json
Loading config file ./composer.json
Checked CA file C:\Users\Christophe\AppData\Local\Temp\opeB01A.tmp: valid
Executing command (C:\Christophe\Repository\Sample): git branch --no-color --no-abbrev -v
Reading C:/Users/Christophe/AppData/Roaming/Composer/composer.json
Loading config file C:/Users/Christophe/AppData/Roaming/Composer/composer.json
Reading C:\Christophe\Repository\Sample/vendor/composer/installed.json
Reading C:/Users/Christophe/AppData/Roaming/Composer/vendor/composer/installed.json
Loading plugin PackageVersions\Installer
Running 1.10.1 (2020-03-13 20:34:27) with PHP 7.2.18 on Windows NT / 10.0
[8.3MiB/0.00s] Loading composer repositories with package information
[8.6MiB/0.01s] Downloading https://repo.packagist.org/packages.json
[8.6MiB/0.26s] Writing C:/Users/Christophe/AppData/Local/Composer/repo/https---repo.packagist.org/packages.json into cache
[8.6MiB/0.26s] Updating dependencies (including require-dev)
[8.6MiB/0.27s] Reading C:/Users/Christophe/AppData/Local/Composer/repo/https---repo.packagist.org/p-provider-2013.json from cache
[12.3MiB/0.28s] Reading C:/Users/Christophe/AppData/Local/Composer/repo/https---repo.packagist.org/p-provider-2014.json from cache
[19.9MiB/0.30s] Reading C:/Users/Christophe/AppData/Local/Composer/repo/https---repo.packagist.org/p-provider-2015.json from cache
[33.8MiB/0.34s] Reading C:/Users/Christophe/AppData/Local/Composer/repo/https---repo.packagist.org/p-provider-2016.json from cache
[51.3MiB/0.39s] Reading C:/Users/Christophe/AppData/Local/Composer/repo/https---repo.packagist.org/p-provider-2017.json from cache
[74.1MiB/0.46s] Reading C:/Users/Christophe/AppData/Local/Composer/repo/https---repo.packagist.org/p-provider-2018.json from cache
[103.3MiB/0.53s] Reading C:/Users/Christophe/AppData/Local/Composer/repo/https---repo.packagist.org/p-provider-2019.json from cache
[107.6MiB/0.56s] Reading C:/Users/Christophe/AppData/Local/Composer/repo/https---repo.packagist.org/p-provider-2019-04.json from cache
[118.9MiB/0.60s] Reading C:/Users/Christophe/AppData/Local/Composer/repo/https---repo.packagist.org/p-provider-2019-07.json from cache
[128.9MiB/0.64s] Reading C:/Users/Christophe/AppData/Local/Composer/repo/https---repo.packagist.org/p-provider-2019-10.json from cache
[141.7MiB/0.70s] Reading C:/Users/Christophe/AppData/Local/Composer/repo/https---repo.packagist.org/p-provider-2020-01.json from cache
[170.3MiB/0.74s] Reading C:/Users/Christophe/AppData/Local/Composer/repo/https---repo.packagist.org/p-provider-archived.json from cache
[167.1MiB/0.77s] Downloading http://repo.packagist.org/p/provider-latest%24f04717b5e6119ce3f6998519bfa59aeaed232a726afb9bfc5d9fc1fcc817919e.json
[175.3MiB/1.33s] Writing C:/Users/Christophe/AppData/Local/Composer/repo/https---repo.packagist.org/p-provider-latest.json into cache
[173.5MiB/1.35s] Reading C:/Users/Christophe/AppData/Local/Composer/repo/https---repo.packagist.org/provider-avonture$pandoc.json from cache
[173.9MiB/1.35s] Resolving dependencies through SAT
[173.9MiB/1.35s] Looking at all rules.
[173.9MiB/1.36s]
[173.9MiB/1.36s] Dependency resolution completed in 0.000 seconds
[173.8MiB/1.36s] Analyzed 62 packages to resolve dependencies
[173.8MiB/1.36s] Analyzed 64 rules to resolve dependencies
[173.8MiB/1.36s] Nothing to install or update
[173.7MiB/1.36s] Reading C:\Christophe\Repository\Sample/vendor/composer/installed.json
[173.7MiB/1.36s] Executing command (C:\Christophe\Repository\Sample\vendor\avonture\pandoc): git log -n1 --pretty=%ct "7c8f30ce42448a1225e8061476f0f7a0a91cebdd"
[173.7MiB/1.56s] Reading ./composer.lock
[173.7MiB/1.56s] Generating autoload files
[173.6MiB/1.60s] Memory usage: 173.61MiB (peak: 192.02MiB), time: 1.6s
``` 

## License

[MIT](LICENSE)
