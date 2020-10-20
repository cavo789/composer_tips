# Composer is slow

## Things to do

### Make sure to use the latest version of Composer

> [https://getcomposer.org/doc/03-cli.md#self-update-selfupdate-](https://getcomposer.org/doc/03-cli.md#self-update-selfupdate-)

Don't use an old version, upgrade your current version of Composer by running

```bash
composer self-update
```

### Disable xDebug

> [https://getcomposer.org/doc/articles/troubleshooting.md#xdebug-impact-on-composer](https://getcomposer.org/doc/articles/troubleshooting.md#xdebug-impact-on-composer)

When xDebug is enabled on CLI, composer will run in a non-optimized way.

To detect if xDebug is enable, run `php -m | findstr xdebug` on the command prompt. If you don't see `xdebug` as the result of the command, it means that the xDebug module isn't loaded and this is fine.

### Use https instead http

Prefer `https://packagist.org` on `http`. Run the next statement and make sure you're using the secured protocol:

```bash
composer config --editor --global
```

### Install prestissimo

> [https://github.com/hirak/prestissimo](https://github.com/hirak/prestissimo)

Once installed, `Prestissimo` will start during any `composer install` or `composer update` statement and will download, in parallel, any needed librairies. This done, composer will start to make his job; loading everything from the cache.

`Prestissimo` will thus speed up the process of downloading packages.

```bash
composer global require hirak/prestissimo
```

* Make sure curl is loaded in your `php.ini` (run `php --ini` to determine the full path of that file),
* If you get the `SSL certificate problem: unable to get local issuer certificate:` when running `composer update` (see https://github.com/composer/composer/issues/6855):
   * Download the `cacert.pem` file from [https://curl.haxx.se/ca/cacert.pem](https://curl.haxx.se/ca/cacert.pem),
   * Save the file on your disk (`C:\Composer\cacert.pem` f.i.),
   * With your File Explorer, right-click on the download file and make sure the file isn't blocked,
   * Edit `php.ini` and add `curl.cainfo = "C:\Composer\cacert.pem"`,
   * Restart your apache server (optional for CLI scripts)

## Update only the required library

If you wish to only update one library, just run the following command:

```bash
composer update vendor/package --with-dependencies
```

## Get more informations

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
