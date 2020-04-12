# openssl extension is required

If you get the following error when f.i. running a `composer update`, just think to enable the `openssl` extension in your `php.ini`.

```bash
[Composer\Exception\NoSslException]
  The openssl extension is required for SSL/TLS protection but is not available. If you can not enable the openssl ex
  tension, you can disable this error, at your own risk, by setting the 'disable-tls' option to true.
```

Under Dos, run `php --info | more` and pay attention to the `Loaded Configuration File` configuration item. You'll see there the name of the loaded `php.ini`.

Open that file with Notepad, search for the `extension=openssl` line and remove the `;` at the first position.

Save and close.
