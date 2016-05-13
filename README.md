# Laravel tips

* Each DB migration script should have Table Engine declaration

Example:
```php
Schema::create('users', function ($table) {
   $table->engine = 'InnoDB';
   $table->increments('id');
});
```
* Always set DB `timezone` manually, also use `strict` MySQL mode:
```php
    'connections' => [

        'mysql' => [
            'driver'    => 'mysql',
            'host'      => env('DB_HOST', 'localhost'),
            'database'  => env('DB_DATABASE', 'forge'),
            'username'  => env('DB_USERNAME', 'forge'),
            'password'  => env('DB_PASSWORD', ''),
            'charset'   => 'utf8',
            'collation' => 'utf8_unicode_ci',
            'prefix'    => '',
            'strict'    => true,
            'timezone'  => 'UTC',
        ],

    ],
```
* All models should be placed in `models` folder
* Do not use reserved Eloquent ORM fields as names for DB columns. For example, do not use next DB column names: `table`, `guarded`, `hidden`, `visible`, `fillable`, etc. This leads to unexpected behavior during filtration collections (Illuminate/Database/Eloquent/Collection).
