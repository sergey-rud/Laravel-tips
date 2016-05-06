### Laravel tips, Development pattern

* Code style: PSR1/PSR2 with blank line before return statement (use CTRL+ALT+L before commit)
* Class names MUST be declared in `StudlyCaps`.
* Class constants MUST be declared in all upper case with underscore separators.
* Method names MUST be declared in `camelCase`.
* Property names MUST be declared in `camelCase`.
* Property names MUST start with an initial underscore if they are private.
* Always use `elseif` instead of `else if`.
* When declaring public class members specify `public` keyword explicitly.
* All models should be placed in “models” folder
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
* Each DB migration script should have Table Engine declaration

Example:
```php
Schema::create('users', function ($table) {
   $table->engine = 'InnoDB';
   $table->increments('id');
});
```
* Strings should be placed in single quotes instead of double quotes for both PHP and JavaScript. HTML attributes and CSS should use double quotes only.
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
* Use short array syntax only.
