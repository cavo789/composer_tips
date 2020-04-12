# Display the config

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
