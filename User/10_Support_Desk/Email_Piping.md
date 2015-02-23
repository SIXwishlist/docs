This will be expanded in due course with further examples of how to setup it up on specific systems. In the mean time please refer to the manuals for your control panel on how to setup email piping / forwarding.

## Path

To setup email piping you need to direct the emails to the piping script within the Support Desk addon.

If you do not know the full absolute path to WHSuite installation, create a file in the root of your WHSuite installation called `path.php` and copy the following code into this file.

	<?php echo dirname(__FILE__); ?>
	
Navigate to this file in your browser and you should get a path shown on the screen. 
	
	/Applications/MAMP/htdocs/myhost.com/www
	
Note this path and delete the file (leaving it on your server could cause a security risk).

Now we have the main path to the WHSuite root, we can build the rest of the path to the piping script which is located inside the Support Desk addon. The full path to the piping script should look something similar to this.

	/Applications/MAMP/htdocs/myhost.com/www/whsuite/app/addons/support_desk/piping/script.php

To run the piping script, the control panel needs to know that we intend to run a php script, this is done with the following path, this loads the php executable on your server.

	/usr/bin/php
	
This is the default location for most servers, other servers may have moved the executable, in those cases please contact your host for more information.

The full command to your php executable should be followed by a space and then the full path to the piping script above, altogether the command for your email piping should look similar to.

	/usr/bin/php /Applications/MAMP/htdocs/myhost.com/www/whsuite/app/addons/support_desk/piping/script.php
	
## Support Departments

After setting up the piping script above, you will then need to make sure any departments that you want to be able to "pipe" emails to have the piping option turned on in the Support Departments settings. Once turned on, you will also need to specify the email address that is to be used to receive the Support Tickets for that department.




	
	