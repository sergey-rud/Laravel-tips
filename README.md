### Laravel tips, Development pattern

* All models should be placed in “models” folder
* Each class, method and property should have phpDoc description
* Class description should contains only **magic** fields and methods
* Each DB migration script should have Table Engine declaration
* Strings should be placed in single quotes instead of double quotes for both PHP and JavaScript. HTML attributes and CSS should use double quotes only.
* Classes and methods shouldn’t be used in the Views
* Model method shouldn’t use global scope, eg $_SESSION, $_SERVER, $_COOKIE, etc. Please use Controllers to retrieve such info and pass it to Model method **as arguments**.
