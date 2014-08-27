The App class is the central class for the core and holds the registry of objects used throughout the system.

As the framework handles the startup process the main functions of use will be those relating to using the registry and also addon checks.

## Adding to Registry

Use the following function to add an object to the registry:

    /**
     * add item to the registry
     *
     * @static
     * @param string - identifying key for retrieval later on
     * @param mixed - item to add to the registry
     * @param bool - overwrite item if key is already taken? (optional:default = true)
     * @return bool - usually true (unless $overwrite = false and $key already exists or $key is empty)
     */
    public static function add($key, $item, $overwrite=true)
    
This can be used in the follow way:

	$my_class = new \Vendor\MyClass($my_var);
	\App::add('myclass', $my_class);
	
As default, this will obviously overwrite any object already stored with the key of 'myClass', simple set the third parameter to false if you wish to protect objects. The function will return **false** should an object with that key already exist.

### Object initiate and add

The App class also provides a factory function which will start up any object with any passed parameters and then add it to the registry.

    /**
     * load the given class and add to App
     * will be added with the key of lowercase classname
     *
     * @param   string  class to load (including namespaces)
     * @param   mixed   optional data to pass to class constructor
     * @return  mixed   bool on false, object on load
     */
    public static function factory()
    
Using the same example with a our MyClass object, you would be able to use the factory in the following way:

	\App::factory('\Vendor\MyClass', $my_var);
	
The class name is used as the key for the registry and is turned to lowercase before adding, so it will be the exact same key as the above example when using standard **add** method.

## Retrieving from Registry

To retrieve an object from the registry you use the simple "getter".

    /**
     * retrieve an object from the registry
     *
     * @static
     * @param string - key to find and retrieve
     * @return mixed - the item stored / null if it's not set
     */
    public static function &get($key)

The key to be passed, is the key specified within the add or the class name when using the factory method.

	$my_class = \App::get('myclass');
	
The getter returns the objects as reference, allowing you to   
a) chain methods straight from the getter  
b) affect the object without having to re-add to the registry.

	$my_class = new MyClass();
	$my_class->setFoo('bar');
	
	\App::add('myclass', $my_class);

	echo \App::get('myclass')->getFoo(); // will return 'bar'

	\App::get('myclass')->setFoo('foo-bar');
	
	echo \App::get('myclass')->getFoo(); // will return 'foo-bar'

### Checking the Registry