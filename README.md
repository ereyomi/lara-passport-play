![Lara-passport-play demo](Lara-passport-play.gif)

## End point call
```PHP
	http://127.0.0.1:8000/api/register
	http://127.0.0.1:8000/api/login
```

## Sqlite Configuration

1. After creating a new SQLite database using a command such as

```bash
	> touch database/database.sqlite
``` 
 
you can easily configure your environment variables to point to this newly created database

a. by using the database's absolute path:


```bash
	DB_CONNECTION=sqlite
	DB_DATABASE=/absolute/path/to/database.sqlite
```

b. Remove DB_DATABASE=... from .env and use the path in the config/database.php
- set 'default' => env('DB_CONNECTION', 'sqlite'), make sure this exist

```PHP
	'connections' => [

        'sqlite' => [
            'driver' => 'sqlite',
            'url' => env('DATABASE_URL'),
            'database' => env('DB_DATABASE', database_path('database.sqlite')),
            'prefix' => '',
            'foreign_key_constraints' => env('DB_FOREIGN_KEYS', true),
        ],
		...
```
		
- move your database.sqlite to 'database' folder in your app root folder.

if you run into any problem concerning connecting to database, run the following commands

- php artisan config:cache

- php artisan config:clear

- restart server: php artisan server




##Laravel Passport Setup / Installation

1. Download and install [git](https://git-scm.com/)

2. Clone project

```bash
    > git clone https://github.com/ereyomi/libraryApi
```

3. cd project-name

4. Install Dependencies

```bash
    > npm install (optional)
    > composer require
```
5. Copy the .env.example file and rename it into the .env file (For this you can run the following command)

```bash
	> copy .env.example .env
```
6. Run the following command to generate a new key

```bash
	> php artisan key:generate
```
7. Migrate DataBase

```bash
	> php artisan migrate
	> php artisan db:seed (start up with dummy data) (optional: for this project)
	> php artisan migrate:fresh --seed (this is the combination of the above commands)
```
8. Run project

```bash
    > php artisan serve 
```
9. if you run into any problem concerning connecting to database, run the following commands

```bash
	> php artisan config:cache
	> php artisan config:clear
	> restart server: php artisan server
```
10. Set the right permissions on all directories and files in your project by simply running (Optional)

```bash
	> chmod 755 -R nameofyourproject/
	> chmod -R o+w nameofyourproject/storage
```

##To use DB_DATABASE

In the .env file use absolute  path 
e.g

	DB_DATABASE=C:\xampp\htdocs\larapoll\database\database.sqlite


###Installation and SetUp

Thanks to [Laravel Passport Docs](https://laravel.com/docs/7.x/passport) 

1. To get started, install Passport via the Composer package manager:

```bash
    > composer require laravel/passport
```


2. The Passport service provider registers its own database migration directory with the framework, so you should migrate your database after installing the package. The Passport migrations will create the tables your application needs to store clients and access tokens:


```bash
    > php artisan migrate
```

3. Next, you should run the `passport:install` command. This command will create the encryption keys needed to generate secure access tokens. In addition, the command will create "personal access" and "password grant" clients which will be used to generate access tokens:


```bash
    > php artisan passport:install
```

4. After running the `passport:install` command, add the `Laravel\Passport\HasApiTokens` trait to your `App\User model`. This trait will provide a few helper methods to your model which allow you to inspect the authenticated user's token and scopes:


```PHP
	<?php

		namespace App;

		use Illuminate\Foundation\Auth\User as Authenticatable;
		use Illuminate\Notifications\Notifiable;
		use Laravel\Passport\HasApiTokens;

		class User extends Authenticatable
		{
		    use HasApiTokens, Notifiable;
		}
```

4. Next, you should call the `Passport::routes` method within the `boot` method of your `AuthServiceProvider`. This method will register the routes necessary to issue access tokens and revoke access tokens, clients, and personal access tokens:

```PHP
	<?php

		namespace App\Providers;

		use Illuminate\Foundation\Support\Providers\AuthServiceProvider as ServiceProvider;
		use Illuminate\Support\Facades\Gate;
		use Laravel\Passport\Passport;

		class AuthServiceProvider extends ServiceProvider
		{
		    /**
		     * The policy mappings for the application.
		     *
		     * @var array
		     */
		    protected $policies = [
		        'App\Model' => 'App\Policies\ModelPolicy',
		    ];

		    /**
		     * Register any authentication / authorization services.
		     *
		     * @return void
		     */
		    public function boot()
		    {
		        $this->registerPolicies();

		        Passport::routes();
		    }
		}
```

5. Finally, in your `config/auth.php` configuration file, you should set the driver option of the api authentication guard to `passport`. This will instruct your application to use Passport's `TokenGuard` when authenticating incoming API requests:

```PHP
	'guards' => [
	    'web' => [
	        'driver' => 'session',
	        'provider' => 'users',
	    ],

	    'api' => [
	        'driver' => 'passport',
	        'provider' => 'users',
	    ],
	],
```
