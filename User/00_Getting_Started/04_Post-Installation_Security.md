Once you've installed WHSuite, there's a few additional steps you can take to increase the security of your installation. We highly recommending following this guide before using WHSuite in a production environment.

##1. Restricting system files
WHSuite allows you to move a majority of it's files out of your public_html/httpd/httpdocs directory, and into the directory above. This ensures that people can't browse to the directories in an attempt to circumvent security.

As default your directory layout will look something like this:


> /home/username/public\_html/<br>
/home/username/public\_html/whsuite/<br>
/home/username/public\_html/inc/<br>
/home/username/public\_html/index.php<br>
/home/username/public\_html/assets.php<br>

Move the whsuite directoy into the /home/username/ directory. 

Once this has been done, using your code or text editor, open the following file:
>/home/username/public\_html/inc/inc.php

At the top of the file you'll see a variable that allows you to set your whsuite directory location:

	// ----------------------------------------------------------------------
	// WHSUITE Folder Location - relative to the index.php / asset.php files
	// ----------------------------------------------------------------------
	$whs_dir = '';

As you've moved the whsuite directory up one level, you can modify this variable as follows:

	// ----------------------------------------------------------------------
	// WHSUITE Folder Location - relative to the index.php / asset.php files
	// ----------------------------------------------------------------------
	$whs_dir = '../';

##2. Renaming the admin directory
Bots and hackers will routienly scan websites for a directory called 'admin'. Because of this, it's good practice to rename the admin path to better conceal the location and reduce the possibility of attack.

WHSuite uses a 'routing system', which allows for simple editing of any URI path in the system. To proceed with renaming the admin path, open up the following file in your code or text editor:

> /home/username/whsuite/app/configs/routes.php

Near the top of this file you'll find this line:

	App::get('router')->attach('/admin', array(
	
You can modify the '/admin' part to your own directory name. We recommend something unique that people wont guess. This should not contain any trailing slashes or additional directories, and must start with a forward slash.


##3. Enable System Passphrases
WHSuite makes use of AES and RSA encryption methods for some areas of it's database. The system areas that are encrypted using RSA allow for a system passphrase to be used. 

Using a passphrase means that once the data is stored, it's impossible to retrieve it without the passphrase being provided. At no point is the passphrase stored on the server, making it a highly secure data storage method.

A good example for when this should be used is if you store customer credit cards or bank account details. This does however mean that automated cron payments will no longer work, as the system will not be able to decrypt the card data.

If you choose to use passphrases for your encryption (highly recommended), you can set a passphrase by logging into the admin area, then browsing to the System Settings page. From there, select the Passphrase Settings option in the sidebar.

**Passphrases are not stored anywhere and can never be retrieved if lost. Be sure to use something memorable. Loosing your passphrase will result in data loss.**

##4. Forcing SSL redirection
We highly recommend running WHSuite with SSL. If you are doing so, you can modify your htaccess file to forcefull redirect users to the HTTPS address.

An example of how to do this in your htaccess file would be:

    RewriteEngine On 
    RewriteCond %{SERVER_PORT} 80 
    RewriteRule ^(.*)$ https://yourhostingsite.com/$1 [R,L] 
