# Laravel tips

* Each DB migration script should have Table Engine declaration

Example:
```php
Schema::create('users', function ($table) {
   $table->engine = 'InnoDB'; //that's right way
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
            'strict'    => true, // set true here
            'timezone'  => '+00:00', // set UTC timezone here
        ],

    ],
```
* All models should be placed in `models` folder
* Do not use reserved Eloquent ORM fields as names for DB columns. For example, do not use next DB column names: `table`, `guarded`, `hidden`, `visible`, `fillable`, etc. This leads to unexpected behavior during filtration collections (Illuminate/Database/Eloquent/Collection).
* On laravel/app/Console/Kernel.php when you would like to code schedule() method - do not place any code outside of the call() method of \Illuminate\Console\Scheduling\Schedule object. 

Bad example:
```php
    protected function schedule(Schedule $schedule)
    {
        $user = User::find(1); // shouldn't be here
        
         $schedule->call(function () use ($user) {
             $user->changePassword();
         })->cron($user->getCron()); // $user->getCron() shouldn't be here
    }
```
Good example:
```php
    protected function schedule(Schedule $schedule)
    {
        $schedule->call(function () {
            $user = User::find(1);
            
            if ($user->isItTimeToChangePassword()) {
                $user->changePassword();
            }
        })->everyMinute();
    }
```

* Use `.gitignore.example` from this repo to create or modify `.gitignore` in the root Framework folder.
