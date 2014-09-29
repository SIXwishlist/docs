The View class uses standard PHP to process the templates so all PHP functions can be used within templates.

# Setting Data

Assuming controllers have been setup correctly and are extending a base controller, accessing the view class is simple. To set data to a template you can do it in either of two ways.

    $this->view->set('var_name', 'var_value');

    $this->view->set(array(
        'var1' => 'value1',
        'var2' => 'value2'
        'var3' => 'value3'
    ));


All the data set above is then accessible from within a template in the following ways

    echo $var_name;
    echo $var1;
    echo $var2;
    echo $var3;

# Parsing Templates

To parse a template from within a controller there are two options. `fetch()` or `display()`. These are both accessible via `$this->view` from within a controller or `\App::get('view')` elsewhere in the system.

`fetch()` will parse the template and return the template as a string from the function. `display()` will fetch the template as a string then instead of returning as a string, it will display the html.

	$template = $this->view->fetch('template.php');
	$this->view->display('template.php');

You can also access the view class from within a template. This allows you to parse 'element' templates which may only contain a small section of reusable code that is used on many templates such as, headers / footers / menus etc...
To access the view class from within a template, use the following code.

	$view->set('var', 'value');
	$template = $view->fetch('snippet.php', array('var' => 'value'));


# Template Paths

As default, all templates are parsed from the root of the views directory from within a theme, e.g. `/whsuite/app/theme/THEME_NAME/views/`

Templates can also be placed into sub folders to aid with organisation and as such this can be reflected when calling the fetch or display function. 

	$template = $this->view->fetch('users/login.php');
	// This will check the following path /whsuite/app/themes/THEME_NAME/views/users/login.php

If you are developing an addon, the syntax for calling an addon's template is slightly different. Call them using the following code

	$template = $this->view->fetch('addon_name::template.php');
	$template = $this->view->fetch('addon_name::sub-folder/template.php'); // sub-folders can also be used 


To allow maximum flexibility with templating when the View class is dealing with an addons template it will look in two places for the template. First it will check to see if you have overwritten the default template to match your theme / design, if not it will then fallback and load the template shipped with the addon.<br />
For the following code

	$template1 = $this->view->fetch('support_desk::ticket_list.php');
	$template2 = $this->view->fetch('support_desk::admin/view_ticket.php');

The following paths will be checked for an existing template.

	/whsuite/app/themes/THEME_NAME/support_desk/views/ticket_list.php
	/whsuite/app/addons/support_desk/views/ticket_list.php

	/whsuite/app/themes/THEME_NAME/support_desk/views/admin/view_ticket.php
	/whsuite/app/addons/support_desk/views/admin/view_ticket.php

If a template cannot be found, an error will be thrown.

# Misc. Functions

There are some misc functions within the View class which shouldn't be needed to use as the framework will handle this but they are related to the setting/getting of the theme directory and the setting and getting of the theme.

	App::get('view')->setThemeDir('/path/to/themes/root');
	$theme_dir = App::get('view')->getThemeDir();

	App::get('view')->setTheme('my_theme');
	$theme = App::get('view')->getTheme();


The handling of the setting the theme directory is done by the 'system bootstrap' so for the most part that function should never need to be used unless you are developing a theme changing addon.