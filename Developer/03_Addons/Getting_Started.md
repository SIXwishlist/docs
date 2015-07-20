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

## Details File

A details file is required for each addon, this contains information stored within a class about the Addon that the addon system can read without having to connect to a database or install the addon first.

The file name must be the addon folder name with `_details` appended and saved as a PHP file.

Folder Name | Details Filename
----------- | ----------------
support_desk | support_desk_details.php
cpanel | cpanel_details.php
knowledge_base | knowledge_base_details.php

The class name must be a `CamelCased` version of the file name.

Filename | Class name
----------- | ----------------
support_desk_details.php | SupportDeskDetails
cpanel_details.php | CpanelDetails
knowledge_base_details.php | KnowledgeBaseDetails

Below is an example of the details file from the Support Desk addon.

    <?php
    namespace Addon\SupportDesk;

    class SupportDeskDetails extends \App\Libraries\AddonDetails
    {
        /**
         * addon details
         */
        protected static $details = array(
            'name' => 'Support Desk',
            'description' => 'Support Desk / Ticket system.',
            'author' => array(
                'name' => 'WHSuite Dev Team',
                'email' => 'info@whsuite.com'
            ),
            'website' => 'http://www.whsuite.com',
            'version' => '1.0.0',
            'license' => 'http://whsuite.com/license/ The WHSuite License Agreement',
            'type' => 'module'
        );
    }

The `name`, `description` and `version` attributes are currently used on the addon management listing page. 

**NOTE:** The version number from here is also stored upon installing an addon, the stored version is then compared against the version within this file so WHSuite knows when to display an 'Update' button for the user run any update migrations. For best results use a [SemVer](http://semver.org] approach to your version numbering.

The `author`, `website` and `license` fields are currently not in use yet but will be used when a WHSuite addon market place is introduced.

The final `type` element is used to define the type of addon it is, some options flag certain fields in our database and enable certain actions. **NOTE** Some actions still have to be done manually within a migration, this will be recitified in a later version.

### Addon Types

The types available are:

* module
	* These are fully fledge addons that provide a full extra section to the website such as a Support Desk, Knowledge Base, Content Management System.
* plugin
	* These are tiny addons that will usually use callbacks on the models or provide code to be called within controller actions to hook into existing work flow. The uploader addon is an example of this.
* server
	* These are server addons that help provide the account setup when someone purchases a hosting product. Cpanel, Directadmin, Plesk are examples of these.
* registrar
	* These are the domain addons that provide the domain registration when a domain is purchases through WHSuite, examples of these are enom and logicboxes.
* gateway
	* These are all the payment gateways that are available for use by the customer. Examples of these include Authorizenet, Paypal and Stripe.
	* These addons require another element to be defined of `gateway_type`

### Gateway Types

There are currently two supported gateway types.

#### Merchant

Merchant gateways require more setup within the initial migration of the addon (we hope to add more functionality to make this easier in the future) to define what it can handle.

Merchange gateways usually handle more complex ACH and CC payments. They can also provide the ability to store the CC or ACH details so we don't have to ask the customer each time. These are generally used for recurring monthly billing payments.

Details on what needs to be added to enable the ACH / CC storage will be mentioned within the Migrations and the Gateway documentation.

#### Standard

Standard gateways are ones such as Paypal / Stripe / OfflinePayment. These ones provide the simple standard transaction integration and are usually used for one off type payments. Most of the work is handled by the gateways system and our addon simply tells the gateway "who we are" and how much to charge.

These gateways will then send some kind of notification back to tell us when payment has complicated.






