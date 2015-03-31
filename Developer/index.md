This developer manual will help get you started with developing addons and interacting with WHSuite.

### System Overview

WHSuite uses a custom framework based on the MVC design pattern that contains built in framework functionality and then 3rd party composer based packages along with bespoke composer packages built specifically for WHSuite.  
The framework also supports addons which themselves are structured around MVC.

### The Core

The main core of the framework is the App class which contains the system registry. Any object that may be required more then once during a process may be added to the registry with a key. This object can then be retrieved again and again without having to reinitiate the object.

The App Class is also responsible for handling the "registering" of addons and loading that addons configs and / or hooks.

For full documentation on the built in framework functionality view the core section of the documentation.

### Composer Packages

The main 3rd party libraries we use are listed below, each 3rd party library may require other libraries / packages for it's own internal use, these are not documented here.

* Aura Router ([1.X branch](https://github.com/auraphp/Aura.Router/tree/develop))
* Aura Session ([1.X branch](https://github.com/auraphp/Aura.Session/tree/develop))
* Aura HTTP ([1.X branch](https://github.com/auraphp/Aura.Http))
* Laravel Eloquent ([4.1.X branch](https://github.com/illuminate/database))
* Laravel Validation ([4.1.X branch](http://github.com/illuminate/validation))
* Symfony Filesystem ([2.3.X branch](https://github.com/symfony/Filesystem)
* Symfony Finder ([2.3.X branch](https://github.com/symfony/Finder))
* Symfony Translation ([2.4.X branch](https://github.com/symfony/Translation))
* Cartalyst Sentry ([2.X branch](https://github.com/cartalyst/sentry))
* Omnipay ([2.X branch](https://github.com/omnipay/omnipay))

Some of these packages are used inside a custom "wrapper" class in order extend default functionality allowing us to maximise the use for these packages.  
This list is correct as of Version 1.0.0 Stable, this list may be subject to change during continued development.

The following is a list of some of the main custom packages WHSuite uses.

* **Input** - Static class used for handling GET / POST data. Has built in CSRF checks when used in conjunction with the Forms package.
* **Forms** - Form building functions for use within templates to generate forms. Can use the Input class to automatically populate the fields.
* **Utilities** - Static class with useful methods
* **Breadcrumb** - Breadcrumb trail builder
* **Email** - Wrapping class for Swift Mailer
* **Money** - Currency conversions
* **Settings** - Loading settings from database
* **Translations** - Language translations
* **Custom Fields** - Allows custom fields on any section
* **Migrations** - Database migrations
