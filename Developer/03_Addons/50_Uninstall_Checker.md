To help prevent unnecessary errors with uninstalling an addon and having other data and addons rely on the first addon, you have the option of creating method within the [Addon Details File] (/Developer/Addons/Details_File) which will be run upon uninstalling an addon.

This method allows you to check any related data to see if you want to allow the addon to be uninstalled. A simple boolean `true` / `false` result determines whether or not the uninstall can proceed.

As default, this method is defined with `App\Libraries\AddonDetails`, the file which all details files should extend, and it simply returns true. To change this, simply override the method within your addons details file.

    /**
     * get the addon details
     *
     * @param   int $addon_id   The addons ID within WHSuite database
     * @return  bool
     */
    public function uninstallCheck($addon_id)
    {
        return true;
    }
    
As stated within the DocBlock, the method receives one parameter which is the addons ID number within the `addons` table. This is the ID number which will be potentially linked with data across the database.

## Domain Registrar Addons

If you have created an addon which links to a domain registrars API then within `\App\Libraries\AddonHelper` is a generic method for use by Domain Registrar addons to check whether that registrar is in use, `domainAddonUninstallCheck`. 

This method is used by the `Enom` and `Logicboxes` addons and examples of its use can be found within the respective details file for each addon.

From the Enom addon details file:

    /**
     * get the addon details
     *
     * @param   int $addon_id   The addons ID within WHSuite database
     * @return  bool
     */
    public function uninstallCheck($addon_id)
    {
        return $this->addon_helper->domainAddonUninstallCheck($addon_id);
    }
	
## Hosting Addons

If you have created an addon which links to a control panel for setting up hosting account packages, within `\App\Libraries\AddonHelper` is a generic method for use within Hosting Addons to check whether the addon is in use with any Server Groups via a Server Addon, `hostingAddonUninstallCheck`.

The methid is currently used out the box by the `Cpanel`, `DirectAdmin` and `Plesk` addons and examples of its use can be found within the respective details file for each addon.

From the Cpanel addon details file:

    /**
     * get the addon details
     *
     * @param   int $addon_id   The addons ID within WHSuite database
     * @return  bool
     */
    public function uninstallCheck($addon_id)
    {
        return $this->addon_helper->hostingAddonUninstallCheck($addon_id);
    }