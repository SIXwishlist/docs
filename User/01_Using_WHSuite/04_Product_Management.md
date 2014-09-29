Products in WHSuite are split into **Product Groups**. This allows you to show related products grouped together on your order forms.

Product groups have a few of their own settings, which will expand over time. For now, you can perform actions such as setting the display order and visibility of a group (and all it's products).

When you choose to add a product to a group, you first select the product type. WHSuite supports three major product types of of the box:

- Hosting
- Domains
- Other

These selections are purely for our addon system to determine what addon types to make available to the product. For example if you select Domain as the product type, you'll be asked to pick a domain extension, whereas if you pick Hosting, you'll be given different options.

'Other' product types are able to use generic product details, as well as any custom fields that get added in by an addon package. A good example of this would be something like an addon that allows you to sell SSL certificates - this would be an 'other' addon.

All product types have the ability to automatically be created, suspended, terminated, etc through their individual product type add ons.

With hosting products, you are given the option of selecting the server group to assign the product to. This then works out what server module to use for the product.

So if you sold both Plesk and cPanel hosting, and wanted to create a product for your cPanel hosting plans, you'd select the server group that has your cPanel server inside of it. The product details page would then automatically load up your list of plans on your cPanel servers.

##Managing Products
When products are created, you can click the 'Manage Product' link next to any of them to modify the product's details. This includes both details such as the product name and description and more advanced options such as stock levels and multi-currency pricing.

We've split the management of products into a tabbed view to keep each set of options as organised as possible:

####Basic Details
The basic details tab is where you can enter the product name, description, active/inactive status, visibility, which email template to use when emailing the client their purchase details, the stock level (optional) and the sort order when listing the products.

####Hosting/Domain Details
The hosting or domain details tab only applies to hosting and domain product types, and shows fields specific to the product type selected. With hosting this would include the server group and bandwidth/diskspace settings.

For domain's it includes things such as the domain extension, and whois privacy options.

####Pricing Details
The pricing details tab will look different depending on if the product a a hosting, domain or 'other' type. You'll use this to set the pricing of the product in each currency you accept.

For domains you'll be able to set the register, renewal, transfer and restore pricing. For hosting you'll be able to set the price, a setup fee, bandwidth overage fees and diskspace overage fees.

Individual pricing periods can be enabled and disabled per-currency using the 'Enabled' checkbox on the right of each row.

####Billing Details
The billing details tab is where you can set things such as how many days after a late payment to suspend the purchase. You can also enable tax collection and change which email templates are used to notify your customers of suspension or termination of the hosting service.
