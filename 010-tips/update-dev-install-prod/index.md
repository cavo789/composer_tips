# Update on dev, install on prod

On the production server, to make sure to install exactly the same dependencies than on your last dev installation, prefer to use `composer install`. Composer will then install the same version as defined in the `composer.lock` file and not, like with `composer update` the last available version for all your dependencies.

To make sure your `composer.lock` file is up-to-date before going to production, run a `composer validate` so `composer.lock` is refreshed.

`composer install` will retrieve version information's from `composer.lock`.
