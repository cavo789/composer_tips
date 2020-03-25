![Banner](images/banner.png)

# Composer tips

> Some tips & tricks for Composer

[Official documentation](https://getcomposer.org/doc/)

* [Adding local packages](#adding-local-packages)
* [Troubleshooting](#troubleshooting)
  * [Self-update](#self-update)
  * [Diagnose](#diagnose)
  * [Composer is slow](#composer-is-slow)
    * [Disable Xdebug](#disable-xdebug)
    * [Get more informations](#get-more-informations)
* [License](#license)

## Adding local packages

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

Curious? Go to your project, navigate in the `vendor` folder and you'll see that the package is a symbolic link to your local package.

Nice thing: any changes in your package is therefore immediately done in your project. **Really handy when you develop the package and the project that uses it at the same time.**

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

* `repositories.packagist.org.url`: make sure you're using `https`. If not, you can force it by running `composer config --global repo.packagist composer https://packagist.org`.

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
