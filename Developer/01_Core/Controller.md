The core comes with a base controller that should be inheritted by **all Application / Addon controllers.**

*nb. This controller is actually already inheritted by the AppController and other Base Controllers. See the [App Controllers](/Developer/App/Controllers)*

## The Setup

The core controller setups some of the core classes and adds them to the controller object and also to the View.

### View

The View class (for more information see [View Documentation](/Developer/Core/View)), is setup to be accessible via

	$this->view

from within a controller and is also added to the view templates as
	
	$view
	
allowing you to use the View class to fetch templates as "elements".

### Assets

The Assets class (for more information see [Assets Documentation](/Developer/Core/Assets)), is setup to be accessible via

	$this->assets
	
from within a controller and is also added to the view templates as

	$assets
	
allowing you to add and load css / js / image files from within the template.

### Router

The Router class (for more information see [Router Documentation](/Developer/Core/Router)), is added to the templates and is accessible via

	$router
	
allowing you to use the router generate function to generate the routes for your links.

*nb. Currently this object is not added to the controller, it is currently accessed via \App::get('router') this will be amended in a later version so it is accessible via `$this->router`.*

## OnLoad

The constructor for the controller is the method which handles the setup above. To prevent errors with developers needing to perform setup operations inside the constructor and forgetting to use
	
	parent::__construct();
	
therefore breaking the controllers, we introduced the "onLoad" method. Simply the constructor checks to see if an onLoad method exists, if it does it runs the method.

This means there is never a need override the construct method meaning the controller at a system level will still function.

Inheritted controllers however may have their own onLoad methods so calling parent::onLoad(); from within your onLoad method is a **must!**

*nb. This may seem like duplication, however incorrectly overriding constructors will break the system for definite. Incorrectly overriding onLoads may mean the system will still work but just not 100% as expected.*

	