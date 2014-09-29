# Accessing

The config handler is registered automatically during bootstrap and can be accessed via the `configs` key in the App registry.

	\App::get('configs');

# Getting Data

The config handler uses dot notation in order to make navigating to the config items you want much easier. In main app configs the first element will always be the filename for the config, with each following element being the keys of the array.

	// from database.php config file
	return array(
    	'mysql' => array(
        	'host' => 'localhost',
        	'user' => 'root',
        	'pass' => 'root',
        	'name' => 'whsuite',
        	'prefix' => 'whs_'
    	)
	);

To retrieve the database name from this config, the following code would be used.

	App::get('configs')->get('database.mysql.name');

# Setting Data

All the main config data should automatically lazy loaded during bootstrapping. If, however, you wish to add other data during runtime to the config you can do so with the following function. Again dot notation is used for the $key to determine where in the config the $value needs to be placed.

	$postgresql = array(
		'host' => 'localhost',
		'user' => 'root',
		'pass' => 'password',
		'name' => 'db_name'
	);
	\App::get('configs')->set('database.postgresql', $postgresql);

A third parameter can also be passed, a boolean value on whether or not to overwrite any existing data. The default value for this is true, so if you need to set some data, only if it hasn't already been set, pass a 3rd parameter with a value of false.

# Addons

The config handler can also be used for addon data if you do not wish to store that in the Settings database table.

In order to associate data with an addon, store the data using an addon prefix, much like when calling addon [views](/Developer/Core/Views).

To set some postgresql database connection details that are specific for the support desk, you would set that data like this:

	$postgresql = array(
		'host' => 'localhost',
		'user' => 'root',
		'pass' => 'password',
		'name' => 'db_name'
	);
	\App::get('configs')->set('support_desk::database.postgresql', $postgresql);

Consequently to retrieve the data, you would use the same addon prefix and the same `get()` method as above.

	\App::get('configs')->get('support_desk::database.postgresql); // would return the full array of host, user, pass, name
	\App::get('configs')->get('support_desk::database.postgresql.user'); // would return the username 'root'


# Misc. Functions

If you wish to return the whole config array in one variable, the config handler has a built in export function.

	\App::get('configs')->export();

