WHSuite comes with a built in basic migration system. This system simply provides an `up` method and a `down` method to allow you to write the migration to do whatever you need it to.

While not strictly an accurate migration system, this method does provide the ability for a migration to handle both the modification of the table and the prepopulation of data.

## Creation 

### Naming

All migrations must reside in a `migrations` folder located within the root of your addon directory. Migration filenames must also be created in a way that makes them order correctly so they can be run in the correct order. The easiest way to do that is follow the same naming structure we have done for our releases.

`migrationYYYY_MM_DD_HHMMSS_versionX.php`

By using this format for your migration filenames it will ensure that each migration is run in the correct order.

Assuming the format above is used for the filename the class name would need to be

`Migration2014_06_07_125314_version1`

### Namespace

When creating a migration always make sure that you define a namespace for the migration. As mentioned [here](/Developer/Core/autoloader) all addons have their own namespace which maps to the addon root directory `\Addon\AddonName`. From here simply CamelCase any subfolders to navigate to a class and make it available via  namespacing. As the migrations reside in a folder named `migrations` the full namespace (once you've swapped out your addon name) would be:

	namespace \Addon\AddonName\Migrations;
	
### Extending

To provide some common functionality for creating migrations a BaseMigration exists for all migrations to extend. This can be found at the namespace `\App\Libraries\BaseMigration`. Make sure any migration you create extends this class.

### Up / Down

The two functions that do the work are the `up` method and the `down` method. Both methods accept 1 parameter which contains the `$addon_id`. This is the ID number the addon is given within our addons table, allowing you to then create tables and prepopulate data / language strings with the addon.

    /**
     * migration 'up' function to install items
     *
     * @param   int     addon_id
     */
    public function up($addon_id)
    {
    }
    
    /**
     * migration 'down' function to delete items
     *
     * @param   int     addon_id
     */
    public function down($addon_id)
    {
    }
    
### Full Example

Below is a full template example for a migration

    <?php 
    // stored = whsuite/app/addons/addon_name/migrations
    // filename = migration2015_07_24_011156.php
    namespace Addon\AddonName\Migrations;

    class Migration2015_07_24_011156_version1 extends \App\Libraries\BaseMigration
    {
        /**
         * migration 'up' function to install items
         *
         * @param   int     addon_id
         */
        public function up($addon_id)
        {
        }

        /**
         * migration 'down' function to delete items
         *
         * @param   int     addon_id
         */
        public function down($addon_id)
        {
        }
    }
    
## Helper Methods / Attributes

As mentioned above, the BaseMigration file provides some common used helper methods and attributes. These are mainly convience wrappers around [Laravels Schema Builder](URL).


### Altering the Schema

The following methods are essentially convience wrappers around Laravels built in methods for managing the Schema. Our wrapper method essentially makes it easier to access (due to WHSuite not using Laravel in full, only the Eloquent ORM system) and also automatically prepends the set database prefix onto the given table name.


### createTable()

The createTable method is essentially a wrapper around `Schema::create()`, but simply prepending the database prefix as mentioned above.

The method declaration is exactly the same so you can simply use our method exactly as the [Laravel Documentation](http://laravel.com/docs/4.2/schema#creating-and-dropping-tables) says to use the `Schema::create` method.

    /**
     * wrapper for the schema create method
     * to auto prefix tables
     *
     * @param   string      Table name to create
     * @param   function    Anonymouse function containing the schema instructions
     */
    public function createTable($table_name, $function)

### dropTable()

As with createTable above, `dropTable` is a wrapper to simply prepend the database prefix automatically. Again the method declaration is exactly the same so you can use it in the same way as the `Schema::drop` method described [here](http://laravel.com/docs/4.2/schema#creating-and-dropping-tables)

    /**
     * wrapper for the schema create method
     * to auto prefix tables
     *
     * @param   string      Table name to drop
     */
    public function dropTable($table_name)

### alterTable()

As with the methods above, `alterTable` is a wrapper to prepend the database prefix. The method declaration is exactly the same as `Schema::table` method and is used for modifying the tables structure (to rename see, `renameTable()` below) as described in the [Laravel Documentation](http://laravel.com/docs/4.2/schema#adding-columns)

    /**
     * wrapper for the schema table method to perform column alterations
     *
     * @param   string          Table name
     * @param   function    Anonymouse function containing the schema instructions
     */
    public function alterTable($table_name, $function)

### renameTable()

**THIS METHOD IS TO BE ADDED BETA 4.1**

As above, this method simply a wrapper to prepend the database prefix. This is a wrapper for `Schema::rename()` and shares exactly the same method declaration so can be used in the same way as shown [here](http://laravel.com/docs/4.2/schema#creating-and-dropping-tables).

    /**
     * wrapper for the schema rename method to rename database tables
     *
     * @param   string          Old table name
     * @param   string          New table name
     */
    public function renameTable($old_table, $new_table)


### $date

This can be used for populate data into tables where created_at / updated_at dates are needed. The variable is populated with "todays" date in the format of `YYYY-MM-DD HH:MM:SS`.

This variable can be accessed anywhere with the `up` or `down` methods with the simple `$this->date`.

### $db_prefix

Should it be needed the database prefix that is set within `app/configs/database.php` can be accessed via `$this->db_prefix`. However with the convience wrapper methods mentioned above use of this will likely be limited.
