Addon views work slightly differently to how views work within the main system.

System views will always be prefixed with the selected theme that is set. Addon views will not due to the nature of the addon not knowing what themes will be installed.

As a result an addon should define it's own views structure based on what sections are available within it (admin / client etc...).

## Overriding

In order for maximum flexability addon views can still be overridden into the main theme however should you need to customise the addon to fit your theme.

For more information on overriding views and the paths at which to place the template files to enable the automatic overriding, see the page [here](/Developer/Core/View). See the section entitled "Template Paths" for information on where to place the template files.