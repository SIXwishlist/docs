# Process

Currently assets are processed through a PHP request and returned by setting the content type. This is something we are looking to amend at a later date to make WHSuite more efficient. The functionality below for setting and processing assets won't change however.

# Accessing

To access the assets handler, use one of the following methods:

	\App::get('assets'); // from anywhere
	$this->assets // from inside a controller
	$assets // from inside a view
	
# Paths

All assets are loaded from a base directory, then depending on whether you've called an image, script or style depends which subdirectory is used.

As default all assets are loaded from 

	/whsuite/app/themes/THEME_NAME/assets/SUB_DIR
	
When calling an asset you can use subdirectories to further organise your files.

Addon syntax is also available which works in the same way as the [View](/Developer/Core/View) class, or see below for examples.

This will search the following directories for the asset.

	/whsuite/app/themes/THEME_NAME/ADDON_NAME/assets/SUB_DIR
	/whsuite/app/addons/ADDON_NAME/assets/SUB_DIR
	
# Images

To create an image and return the image along with the img tag use the following code:
   
	echo $assets->image('email.png', array(
    	'render' => true,
    	'alt' => 'Email'
	));


Notice the render => true, as part of the options array in the second param. This causes the function to return a html tag, omit this element or set to false to just return the path (useful for background-image: url() ). The second param can also contain any other data you wish to pass to the image tag, alt, class, id, data variables etc...

You can load the image from an addon using the standard addon notation
    
	echo $assets->image('support_desk::email.png', array(
    	'render' => false
	));


# Javascript

To add javascript from within a controller / library / other code, use the following function:
  
	$this->assets->addScript('jquery.slider.js');

Or you can pass in an array to the function and set multiple files at once:
   
	$this->assets->addScript(array(
    	'jquery.slider.js',
    	'jquery.bxSlider.js',
    	'support::load-ticket.js'
    	'knowledgebase::faqs.js'
	));


If you are within a template you can also add a script using the following function, this will return the path to the script rendered in a script tag.
    
	echo $assets->script('main.js');

<sup>nb. currently you cannot pass in an array and have it render multiple script tags in one call.</sup> 

This function does also accept a second parameter which will add extra attributes to the script tag.

# Stylesheets

To add a stylesheet from within a controller / library / other code, use the following function:
    
	$this->assets->addStyle('main.css');

This will default to a media of 'screen'. If you wish to add a stylesheet with a different 'media' setting on the link tag see the following example, which will also demonstrate adding multiple stylesheets in one function call:
    
	$this->assets->addStyle(array(
    	array(
        	'file' => 'reset.css'
    	),
    	'main.css',
    	array(
        	'file' => 'gallery.css',
        	'media' => 'print'
    	),
    	'support.css'
	));

As usual, addon syntax of `addon::file_name.css` can be used for files.

If you are within a template you can also add a stylesheet using the following function, this will return the path to the script rendered in a link tag.
  
	echo $assets->style('test.css');

<sup>nb. currently you cannot pass in an array and have it render multiple style tags in one call.</sup> 

This function does also accept a second parameter which will add extra attributes to the link tag, allowing you to define the media settings and any other attributes you need to modify.
 
	echo $assets->style('support::test.css', array(
    	'media' => 'print'
	));