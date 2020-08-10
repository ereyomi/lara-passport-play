###Installation
To get started, install Passport via the Composer package manager:

```bash
    > composer require laravel/passport
```


The Passport service provider registers its own database migration directory with the framework, so you should migrate your database after installing the package. The Passport migrations will create the tables your application needs to store clients and access tokens:


```bash
    > php artisan migrate
```
