For loading Routes within the system we have developed a method as part of our [Config Handler](/Developer/Core/Configs) to loop over the given directory and include all the files it finds automatically. These files contain all the routes for the system, this method allows us to group certain routes together and generally make maintaining all the routes within the system (and addons) easier.

## How it works

The method, when used will look for a `routes` folder at the same level as the file that ran the method.

e.g.

	app/configs/routes.php
	app/configs/routes/

As part of the method call, a parameter must be passed, this is generally either `admin` or `client` as we separate our routes based on admin or client side.

The method will then look for another subdirectory named after the passed parameter. If it exists, the method will loop the folder and include any file it finds. Any file that contains routes should define a `$routes` variable with an array of the routes.

    $routes = array(
        'announcement' => array(
            'path' => '/announcements/',
            'values' => array(
                'controller' => 'AnnouncementController',
                'action' => 'index'
            )
        ),
        'announcement-paging' => array(
            'path' => '/announcements/{:id}/',
            'values' => array(
                'controller' => 'AnnouncementController',
                'action' => 'index'
            )
        ),
        ...
    );
    
The method will then attach and return all the routes it finds as an array, allowing us to pass the full array of routes to Aura Router.

    $admin_routes = App::get('configs')->getRoutes('admin');

    App::get('router')->attach('/admin', array(
        'name_prefix' => 'admin-',
        'values' => array(
            'sub-folder' => 'admin'
        ),
        'params' => array(
            'id' => '(\d+)'
        ),

        'routes' => $admin_routes
    ));
    
## Addons

As the method work on a relative basis, the method can also be used for loading routes for addons, again a sub routes folder is used to separate between admin and client routes.

## Route Name Prefixes

As stated, main WHSuite routes are split between admin / client. These routes all have a prefix prepended to them by Aura with the use of the `name_prefix` parameter used in the above example.

So with the above examples, the Announcement routes are from `app/configs/routes/admin/announcements.php` and as a result are part of the admin routes. This means to generate the route url for this route, the name for each route is called with the `admin-` prefix.

	announcement -> admin-announcement
	announcement-paging -> admin-announcement-paging

For the client side announcement routes found within `app/configs/routes/client/announcements.php` the prefix would be `client-`

	announcement -> client-announcement
	announcement-paging -> client-announcement-paging
	
Ths functionality is part of Aura Router and not custom to WHSuite.

## Known Quirks / Future Amendments

Currently a sub-folder must be passed to the method which means within the routes folder, at least one sub folder must be present, generally this is not an issue as there is almost always a separation of admin / client.

This is something we will look to amend in the future as some addons created may not require a sub-folder or having a sub-folder may be excessive.
