Controllers within WHSuite act like Controllers do in most MVC based frameworks, calling to models to get data and then processing and handing to the Views for display.

All Controllers extend a Base Controller, which is used to provide common functionality for related Controllers.

All Controllers are also sorted into folders for better organisation and grouping of related items. These folders are referenced when attaching Routes to Aura Router. WHSuite Controllers are grouped into four folders

* admin
* base
* client
* installer

## Base Controllers

The purpose of Base Controllers is to provide common functionality for multiple Controllers to save rewriting code.

WHSuite comes with 6 Base Controllers, one of which acts as a "Base Controller" for the other Base Controllers.

### AppController

AppController is the main Base Controller which all other Base Controllers should extend, this Controller itself extends the System Controller. Within this AppController contains the bulk of our [Scaffolding System](/Developer/App/Scaffolding), which allows you to easily setup a simple CRUD system used on numerous sections of WHSuite.

This Controller also setups some common functionality with Flash Messages, Translations and Breadcrumbs.

    if (App::get('session')->hasFlash('success')) {
        \App\Libraries\Message::set(App::get('session')->getFlash('success'), 'success');
    }

    if (App::get('session')->hasFlash('error')) {
        \App\Libraries\Message::set(App::get('session')->getFlash('error'), 'fail');
    }
    
    $this->lang = App::get('translation');

### AdminController

* Extended by all Controllers within Admin Controllers Folder
* Sets up the admin theme set.
* Sets up admin authentication.
* Automatically protects any methods from viewing unless logged in as an Admin. 

### ClientController

* Extended by all Controllers within Client Controllers Folder
* Sets up the client theme set.
* Sets up client authentication.
	* If logged in, will setup the clients language / currency settings as well.

### GatewayBaseController

* Extended by Gateways (e.g. Paypal, Stripe)
* Provides a common `returnButton` method for changing payment buttons on new order page based upon chosen payment method.

### InstallerController

* Extended by the Installation Controllers.
* Common methods for checking PHP extensions, migrations and WHSuite installation version.

### WidgetsController

* Extended by any Dashboard Widgets / Shortcuts
* Currently no extra functionality but common functionality will be implemented in the future to make Shortcuts / Widgets easier to render.

## Scaffolding

The scaffolding methods are all found within AppController. These methods are designed to enable quick and easy setup of simple CRUD based sections. Simply override the desired methods in your Controller, making sure naming conventions are followed and powerful CRUD options will be made available.

For the full documention, see the [Scaffolding Documentation](/Developer/App/Scaffolding).