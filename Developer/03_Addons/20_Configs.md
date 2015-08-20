The configs directory for an addon can store any config file needed for your addon and the [Config Object](/Developer/Core/Configs) can be used to load and retrieve the config data during processing. 

There are however some set configs that are recognised by the core and are handled slightly differently.

## Routes

A `routes.php` file can be created within your configs directory to store any routes needed for that addon. The way routes are loaded and handled are exactly the same as defined [here](/Developer/Core/Router) and [here](/Developer/App/Routes) with one small difference with the route attachment.

When defining the route details an extra element can be passed into the array to define the addon so the router can provide the dispatcher the correct path for finding the controller / action.

The element in question is `addon` and is defined as a sub element of the `values` array which contains the `sub-folder` (usually 'admin' or 'client') and the controller and action on individual routes.

As this is usually used for defining the routes for the whole addon the `addon` element is usually define on the main route attach method rather then all the individual routes.

Here is an example from the Support Desk addon showing the `addon` element being defined.

    $admin_routes = App::get('configs')->getRoutes('admin');

    App::get('router')->attach('/admin', array(
        'name_prefix' => 'admin-',
        'values' => array(
            'sub-folder' => 'admin',
            'addon' => 'support_desk'
        ),
        'params' => array(
            'id' => '(\d+)'
        ),

        'routes' => $admin_routes

    ));

For information on the two methods used above please see [this page](/Developer/App/Routes).

## Hooks

As part of the registering of which addons are installed, the system will check for a `hooks.php` file within the `configs` directory and simply include it.

This file can be used to define any hooks the addon needs to listen for during processing. For information on how to setup listeners check out [this page](/Developer/Core/Hooks) on the hook system.

Below is a quick example however of the `hooks.php` file being used in the Support Desk to listen for the automation calls so it can cleanup and close any tickets.

    <?php

    App::get('hooks')->startListening(
        'automation-begin',
        'ticket-cleanup',
        '\Addon\SupportDesk\Libraries\Automation::cleanup'
    );

