# Laravel tips. Development pattern

##Laravel tips
* Each DB migration script should have Table Engine declaration

Example:
```php
Schema::create('users', function ($table) {
   $table->engine = 'InnoDB';
   $table->increments('id');
});
```
* Use PDO Connection Options:
```php
return array(

    /* other settings removed for brevity */
    'connections' => array(


        'mysql' => array(
            'driver'    => 'mysql',
            'host'      => 'localhost',
            'database'  => 'database',
            'username'  => 'root',
            'password'  => '',
            'charset'   => 'utf8',
            'collation' => 'utf8_unicode_ci',
            'prefix'    => '',
            'options'   => array(
                 PDO::MYSQL_ATTR_INIT_COMMAND => "SET time_zone = '+00:00'",
            ),
        ),
    )
)
```

##Development pattern

### Code Style
* Code style: PSR1/PSR2 with blank line before return statement (use CTRL+ALT+L before commit)
* Strings should be placed in single quotes instead of double quotes for both PHP and JavaScript. HTML attributes and CSS should use double quotes only.
* Use short array syntax only.
* Class names MUST be declared in `StudlyCaps`.
* Method and Property names MUST be declared in `camelCase`.
* Always use `elseif` instead of `else if`.
* When declaring public class members specify `public` keyword explicitly.
* For better readability there should be no blank lines between property declarations and two blank lines
  between property and method declaration sections. One blank line should be added between the different visibility groups.

Example:

```php
<?php
class Foo
{
    public $publicProp1;
    public $publicProp2;

    protected $protectedProp;

    private $_privateProp;


    public function someMethod()
    {
        // ...
    }
}
```
### File structure
* All models should be placed in “models” folder

### Comments
* Each class, method and property should have phpDoc description
* Class description should contains only **magic** fields and methods

Bad example:
```php
/**
 * @property null|int $price // it shouldn't be here, because it's NOT magic property
 * @property string $name
 * @property ProductUser[]|\Illuminate\Database\Eloquent\Collection $productUsers
 */
class Product
{
    public $price = null;
    
    /*
     * other code ...
     */
}
```

Good example:
```php
/**
 * @property string $name
 * @property ProductUser[]|\Illuminate\Database\Eloquent\Collection $productUsers
 */
class Product
{
    /**
     * @var null|int
     */
    public $price = null;
    
    /*
     * other code ...
     */
}
```
### Logic

* Classes and methods shouldn’t be used in the Views (controller should handle all logic and pass simple array to view)
* Model method shouldn’t use global scope, eg $_SESSION, $_SERVER, $_COOKIE, etc. Please use Controllers to retrieve such info and pass it to Model method **as arguments**.

Bad example:
```php
class UploadHandler
{
    public function upload($filename)
    {
        
        //uploading file
        //...
        //end uploading file
        
        //save log
        $log = new UploadLog();
        $log->user()->associate(Auth::user()); // it's sad, because we use $_SESSION here
        $log->filename = $filename;
        $log->save();
    }
}
```
Good example:
```php
class UploadHandler
{
    public function upload($filename, $user) // new argument here
    {

        //uploading file
        //...
        //end uploading file

        //save log
        $log = new UploadLog();
        $log->user()->associate($user); // now it's cool
        $log->filename = $filename;
        $log->save();
    }
}
```
* All models should extend custom BaseModel, all controllers should extend custom BaseController.
* BaseModel and BaseController shouldn't ovveride default Framework behavior, however it may extend functionality by adding new methods.
* Changing type of an existing variable is considered as a bad practice. Try not to write such code unless it is really necessary.
```php
public function save(Transaction $transaction, $argument2 = 100)
{
    $transaction = new Connection; // bad
    $argument2 = 200; // good
}
```

###Additional rules

###### `=== []` vs `empty()`

Use `empty()` where possible.


###### `=== null` vs `is_null()`

Use `=== null` or `empty()` where possible.
