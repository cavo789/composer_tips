# Adding local packages

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
