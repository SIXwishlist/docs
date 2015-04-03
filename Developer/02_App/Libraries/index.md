Libraries within WHSuite are helpers that are used for specific purposes but are not necessarily needed all the time and are therefore only loaded when needed.

## File Location

All App specific libraries should go within the `whsuite/app/libraries` folder, any libraries belonging to an addon should go within a libraries folder within that addon.

## Naming

All App Libraries should have the namespace `App\Libraries`, this should be defined at the top of the file.

	<?php
	namespace App\Libraries;
	
Class names should all be CamelCased with the filename should be a snake_cased version of the class name.

ClassName | file_name
--------- | ----------
LanguageHelper | language_helper.php
AddonHelper | addon_helper.php
Message | message.php

Below is an example class definition for the LanguageHelper

    <?php // language_helper.php

    namespace App\Libraries;

    class LanguageHelper { }
    
## Usage

Library class can either be accessed directly through the namespace

	$isWHSuiteGermanInstalled = \App\Libraries\LanguageHelper::isInstalled(2, 0);
	
	$LangImporter = new \App\Libraries\LanguageImporter('JSON_STRING');
		
Or by using the [App::factory method](/Developer/Core/App_Class)

	$LangImporter = App::factory('\App\Libraries\LanguageImporter');
	
	