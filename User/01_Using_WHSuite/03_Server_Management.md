WHSuite's server management area is located under the 'Products' sidebar menu item. When you visit the server management page, you'll have the option of managing and adding both servers and server groups.

##Server Groups
A server group is used to define a type of server. For example if you offered cPanel and Plesk Linux hosting options in one location, you'd have two server groups. One for cPanel and one for Plesk. 

Each server group can be assigned one server module (e.g cPanel, Plesk, DirectAdmin, etc), and all servers in that group are then presumed to run on that module.

Within a group you can enable Auto-Fill servers. When this is enabled, WHSuite will use the deployment priority level of each server to determine where new accounts should be activated. By doing this you can ensure accounts are automatically distributed across your servers without the need to constantly change which server to use when setting up customers hosting services.

##Server Management
Once you've added your servers to a group, each server is given it's own management page. From here you can view the server details. Selected server modules also include the ability to perform extra actions on the server. These are provided under the 'Manage' tab. 

As an example our included cPanel module allows you to reboot the server, as well as restart a selection of system services. You can also view the server load averages to get an idea of how the server is performing.