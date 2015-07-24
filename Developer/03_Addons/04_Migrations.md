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

### createTable()

### dropTable()

### alterTable()

### $date

This can be used for populate data into tables where created_at / updated_at dates are needed. The variable is populated with "todays" date in the format of `YYYY-MM-DD HH:MM:SS`.

This variable can be accessed anywhere with the `up` or `down` methods with the simple `$this->date`.

### $db_prefix


