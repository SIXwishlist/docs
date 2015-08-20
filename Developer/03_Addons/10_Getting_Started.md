## Naming

All WHSuite specific addons should be placed within the `whsuite/app/addons` folder. All addons should be created in the snake_case format.

* support_desk
* cpanel
* knowledge_base

Following this naming convention will mean that the addon root directory will be available at the following namespace automatically: `\Addon\CamelCasedAddonName\`

Folder Name | Namespace Path
------------| --------------
support_desk| \Addon\SupportDesk\
cpanel | \Addon\Cpanel\
knowledge_base | \Addon\KnowledgeBase\


## Folder Structure

As WHSuite is built upon the use of namespaces aside from the reserved folders any folder structure is available as folders / class will be accessible via the namespace.

Below is a list of reserved folders that WHSuite will expect to contain Addon specific classes that it should be aware of (controllers / models / views / migrations etc...)

* assets
	* css
	* img
	* js
* configs
* controllers
* languages
* libraries (not fully reserved but the main convention)
* migrations
* models
* views
