# Extra

* [24 Tips for Using Composer Efficiently](https://blog.martinhujer.cz/17-tips-for-using-composer-efficiently/)
  * *You should commit `composer.lock` to git in **applications** and not for **libraries**.* (tips #5 and #6)
  * *Sort packages in `require` and `require-dev` by name to reduce merge conflicts.* (tip #8)
  * *You can define other types of dependencies in composer.json*: specify that you're using f.i. `ext-pdo_mysql` so everyone who will install your project will immediately be aware of this (tip #12)
