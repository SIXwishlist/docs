A standard gateway is a gateway such as Paypal or Stripe. These are usually gateways in which WHSuite passes the details to gateway, the gateway then takes the money and tells WHSuite that it was taken successfully allowing WHSuite to then process the order.

The following documentation is for someone who wished to integrate a Payment Gateway like one mentioned above into WHSuite and assumes that you are already familiar with the generic addon documentation.

## Details File

The details file for a payment gateway addon should look something like this, taking note of the `type` element and `gateway_type` element within `$details`

    <?php
    namespace Addon\Paypal;

    class PaypalDetails extends \App\Libraries\AddonDetails
    {
        /**
         * addon details
         */
        protected static $details = array(
            'name' => 'Paypal Payment',
            'description' => 'Paypal payment gateway.',
            'author' => array(
                'name' => 'WHSuite Dev Team',
                'email' => 'info@whsuite.com'
            ),
            'website' => 'http://www.whsuite.com',
            'version' => '1.0.0',
            'license' => 'http://whsuite.com/license/ The WHSuite License Agreement',
            'type' => 'gateway',
            'gateway_type' => 'standard'
        );
    }

## Library File

The library file for a gateway is the main file from which you build up the transaction / payment.

The name of this file should be a `snake_case` version of the addon name (this should match the folder name). The actual class that resides within this file should be a `CamelCase` version of the addon name (this should match the namespace for the addon).

The file should also implement `\App\Libraries\Interfaces\Gateway\PaymentInterface`, this ensures all the required methods are set within the class.

