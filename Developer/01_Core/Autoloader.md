## Autoloading

As well as composer autoloading for any packages located within the Vendor directory, the framework comes bundled with an autoloading class which can handle autoloading for any framework based items such as libraries, models and any addon based class.  
  
The framework uses [spl_autoload_register](php.net/spl_autoload_register spl_autoload_register) to set the loadClass method as the method to use during autoloading.

### Models

Models within the system are loaded into the global namespace automatically meaning you do not have to prefix models with any namespace / vendor nor do you have to **"include"** the model file itself. This does have downsides in that the system and addons cannot have any models share the same name otherwise conflicts will arise. 

*nb. This implementation may be modified in the future.*

Autoloading of the models works by ***"registering"*** the model directories, this is done automatically during startup. Registering simple logs the model name and the model file path for use by the loadClass method to be able to load in the model before it is used.

### Namespaces

If the loadClass method fails to find the class as a model it will fail back on to a namespace based method to try to find and load the class.

All namespaced classes that are non composer based must be prefixed with one of the following:

* \Core 
* \App
* \Addon

The Core prefix is reserved for all the framework specific files only and resolves to /whsuite/system/.  
The App prefix is reserved for the application specific files and resolves to /whsuite/app/.

The Addon prefix is the main prefix to be used by 3rd party developers when developing their addons. The prefix resolves to the addon root /whsuite/app/addons/. Directly following the Addon prefix should be the camel cased version of the addon name.

e.g.

* support_desk -> SupportDesk
* directadmin -> Directadmin
* knowledge_base -> KnowledgeBase

Each addon should therefore have a unique namespace prefix.

After each prefix (whether it's core, app or addon) each sub-namespace will need to be either the class filename camel cased (as above addon examples) or a sub directory (again camel cased)leading to the class filename.

#### Examples

Namespaced Class | File Location
---------------- | -------------
\App\Libraries\ClientAuth | doc\_root/whsuite/app/libraries/client\_auth.php
\Core\Exceptions\PageNotFoundException | doc\_root/whsuite/system/exceptions/page_not_found_exception.php
\Addon\SupportDesk\Libraries\NotificationsHelper | doc\_root/whsuite/app/addons/support\_desk/libraries/notifications\_helper.php
