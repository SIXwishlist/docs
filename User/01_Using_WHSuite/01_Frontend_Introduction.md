The frontend of WHSuite handles all interactions with both new and existing clients. 

When someone visits the frontend and they are not logged in, they are presented with the option to login, create an account or place an order. An editable welcome message is also show. 

Of course you can completely re-style the client area by creating a new theme or modifying an existing client theme. We recommend not modifying any of the themes that come with WHSuite, and instead advise that you duplicate them and give them a new name. In the event of a missing template file, WHSuite will attempt to load up the file from the default client area theme as a fail-safe. 

##Creating An Account
When a user goes to create a new client account, they are required to provide their email address, a password and their billing address. They also have the option of selecting their language and currency should you support them.

Finally they can choose weather or not they'd like emails that are sent by your WHSuite installation to be in HTML or Plaintext. All of these settings can be modified by the client at any time from their profile area.

##Client Area
Once a client logs in, they are presented with the client dashboard. 

The dashboard shows them their address, account credit balance, any announcements relevent to them and a brief overview of their services and invoices.

Our addon system allows for certain areas of the dashboard to be extended. As an  example, when you enable the official WHSuite support desk addon, an additional tab is added to the services/invoices table to show the client their recent support tickets.

In the top navigation menu the client can click the 'My Account' link to open up a list of options. These include:

- Overview
- My Services
- My Invoices
- Manage Billing Details
- Payment History
- Profile 
- Logout

The 'My Services' page lists their active and past services, and where applicable allows them to view and manage the service.

The 'My Invoices' page lists all the clients past and present invoices, with the option to view them and download a PDF copy of the invoice.

The 'Manage Billing Details' page lists any payment methods the client has stored if you've enabled credit card and/or ach account storage. The client can also manually add funds to their account from here, providing a great way of letting people 'pre-pay' for service renewals.

The 'Payment History' page lists all transactions the client has ever made. WHSuite keeps meticulous records of every payment that is recorded, allowing clients and staff to refer back to transactions at any time.

The 'Profile' page allows your client to modify their billing and contact details, as well as update their preferences. If you've added any custom client fields and made them visible/editable to clients, they will be able to ammend those here.

Finally the 'Logout' link will kill the active login session and return the client to the login page.

##Orders
The order system in WHSuite allows for hosting, domain and 'other' based products. 

Both hosting and 'other' products are shown as list items, with a HTML based description of the product on offer. Domain products work slightly differently, and will allow for a live domain availability check, as  well as a list of all domain extensions you offer.

Certain domain extensions require additional details from the client, as do some hosting services. WHSuite allows addons to create these custom fields on the purchase page for each product type, allowing for a consistent purchase experience.

When a client is ready to complete an order, they are taken to the checkout. Upon confirming an order, they are automatically directed to their first invoice. Once this is paid by the client, WHSuite will optionally set the new service up where applicable. The client is then emailed any details about their new service.

