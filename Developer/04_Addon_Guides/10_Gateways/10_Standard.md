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

## Addon Setup

As part of the setup for the addon, an initial [migration](/Developer/Addons/Migrations) will be required to setup the addon and make it available for selection by a user when paying an invoice.

When an addon is installed, the addon installer will automatically add a row to the `addons` MySQL table to log that the addon was installed and what version is installed. Within the migration however we need to add a row to the `gateways` MySQL table. This let's the WHSuite know that this addon is actually a payment gateway. 

### Migration Up

Use the example code below to add a row to the gateway table.

    $Gateway = new \Gateway();
    $Gateway->slug = 'example'; // addon directory name
    $Gateway->name = 'Example'; // visual display name
    $Gateway->addon_id = $addon_id; // don't modify this line
    $Gateway->is_merchant = 0; // don't modify this line
    $Gateway->process_cc = 0; // don't modify this line
    $Gateway->process_ach = 0; // don't modify this line
    $Gateway->is_active = 1; // don't modify this line
    $Gateway->sort = 10; // don't modify this line
    $Gateway->save(); // don't modify this line
    
### Migration Down    

Use the following example code in the down method of your initial migration to remove the gateway row.

    // Remove gateway record
    $Gateway = \Gateway::where('addon_id', '=', $addon_id)
        ->where('slug', '=', 'example') // addon directory name
        ->first();

    \GatewayCurrency::where('gateway_id', '=', $Gateway->id)
        ->delete();

    $Gateway->delete();
    
The gateway currency will usually be set by the user once they have installed your addon. This allows the admin to make certain gateways only available to certain currencies.

### Extras

You can also then use the `up()` and `down()` methods on the initial migration to install any required settings fields that may be needed for the gateway such as account usernames or Oauth connection details.

## Library File

The library file for a gateway is the main file from which you build up the transaction / payment.

The name of this file should be a `snake_case` version of the addon name (this should match the folder name). The actual class that resides within this file should be a `CamelCase` version of the addon name (this should match the namespace for the addon).

The file should also implement `\App\Libraries\Interfaces\Gateway\PaymentInterface`, this ensures all the required methods are set within the class.

Below is a example to work from:

    <?php
    namespace Addon\Example\Libraries;

    use App\Libraries\Interfaces\Gateway\PaymentInterface;

    class Example implements PaymentInterface
    {
        /**
         * setup a payment
         * setup params, account names / amount etc..
         *
         * @param   array   array of data in order to perform the transaction
         * @param   bool    Indicator of whether we're setting up for check return
         * @return  bool    Not required. Returning false can stop transaction process however
         */
        public function setup($data, $returnSetup = false);

        /**
         * process the payment
         *
         * @param   array   array of data in order to perform the transaction
         * @return  bool
         */
        public function process($data);

        /**
         * check the return of a payment
         *
         * @param   array   array of data in order to perform the transaction
         * @return  bool|string
         */
        public function checkReturn($data);
    }

Other methods may be defined within the class should you need to access other objects or wish to split code out into various methods to promote DRY coding principles.

The methods above are the required methods however that our payment library interfaces with to make the payment.

### setup()

The `setup()` method is generally used to setup the connection to the gateway or initiated the gateways SDK and setup the amounts due. 

It accepts 2 parameters, the first being the data array passed into the payment library, this will generally contain IDs for the invoice / client / currency and also the total due. Below is an example of what `$data` may contain:

    $data = array(
        'invoice_id' => $invoice->id,
        'client_id' => $client->id,
        'currency_id' => $currency->id,
        'total_due' => $total_due,
        'data' => $extraData
    );

In the instance above taken from the "Invoice Payment" Screen from within the client area, the `$extraData` contains the posted form data.

The `setup()` method is called both when loading up the library to take payment and also again when we load the library to validate the return from the payment gateway. `$returnSetup` is a way of identifying whether we are setting up to take payment or to check the return. If `$returnSetup === true` then we are setting up to check the return from a gateway.

The return from `setup()` can also be used to stop the processing should something not be present that is needed. Simply return boolean `false` to cancel the payment. **Any other return value and the transaction shall proceed as normal**.

### process()

The `process()` method is used to kick start the actual taking of payment. It accepts 1 parameter which will be the same `$data` array that was passed to `setup()`.

Within this method it generally wants to get the payment from the gateway and immediately return a boolean `true` or `false` depending what the gateway says. This process works for a gateway such as Stripe.

If you are working with a gateway that works more like Paypal, the `process()` method would not complete as the `process()` method would redirect to Paypal ready for Paypal to take the payment.

### checkReturn()

If this method is needed then generally you will need to create a custom controller for your addon that will receive the return value from the gateway.

Within the controller (examples in the next section), the payments library will need to be loaded to check the return value from the gateway, the payments library will then call to the gateway library you have created and call this `checkReturn()` method.

The method accepts 1 parameter which is passed into the payment library, generally this will again be an array of Invoice / Currency / Client IDs to be processed.

The return value from this method should be a boolean `false` or it can be a string of the transaction reference depending on the results of checking the gateways return value.

## Custom Controller

As mentioned above, if you are dealing with a Paypal style gateway, in which you redirect to the gateway to take payment before navigating back to your site to process the return value of the gateway, you will generally need to create a custom controller to accept this return value.

Some snippets / methods that you will need to process the return value and finalise payments are show below

### Checking the return value
	
	// $data should contain any data needed to process the payment
	// such as invoice id, currency id or client id
   	$addon = Addon::where('directory', '=', 'example')
        ->where('is_gateway', '=', 1)
        ->with('Gateway')
        ->first();

    $transactionRef = \App\Libraries\Payments::checkPayment($addon, $data);

### Checking Transaction Reference

If the gateway has been setup to return gateway return tokens / references, this method can be used to check to make sure the transaction isn't logged twice, incase of double click events or other random anomalies.

	\App\Libraries\Payments::referenceExists($invoiceId, $transactionRef)

This method returns a simple boolean `true` / `false` depending on whether the reference already exists.

### Finalise and log Payment

The following method should only be called once you are certain the payment has been completed successfully as this will log the transaction row and mark the invoice as paid.

   	$addon = Addon::where('directory', '=', 'example')
        ->where('is_gateway', '=', 1)
        ->with('Gateway')
        ->first();
    \App\Libraries\Payments::finalisePayment($invoiceId, $transactionRef, $addon);

## Full Controller Example

Below is a full example of the custom controller used within the Paypal. The following controller contains 2 methods. `payCancel()` is used when a user manually cancels the transaction from Paypal. `payReturn()` should receive every other return from Paypal (both successful and failed transactions).

    <?php

    class PaypalController extends ClientController
    {
        /**
         * handle the return from paypal and process accordingly
         *
         */
        public function payReturn($invoice_id = 0)
        {
            // get the invoice
            $invoice = Invoice::find($invoice_id);

            if (is_object($invoice)) {

                $total_due = $invoice->total - $invoice->total_paid;

                $gateway_data = array(
                    'invoice_id' => $invoice->id,
                    'client_id' => $invoice->client_id,
                    'currency_id' => $invoice->currency_id,
                    'total_due' => $total_due
                );

                $addon = Addon::where('directory', '=', 'paypal')
                    ->where('is_gateway', '=', 1)
                    ->with('Gateway')
                    ->first();

                $transaction_ref = \App\Libraries\Payments::checkPayment($addon, $gateway_data);

                if ($transaction_ref !== false) {

                    // check we haven't already logged this transaction (JIC)
                    if (! \App\Libraries\Payments::referenceExists($invoice_id, $transaction_ref)) {

                        // not found, add the transaction row
                        \App\Libraries\Payments::finalisePayment($invoice_id, $transaction_ref, $addon);
                    }

                    \App::get('session')->setFlash('success', \App::get('translation')->get('payment_successfully_taken'));
                    return header("Location: ".App::get('router')->generate('client-manage-invoice', array('id' => $invoice_id)));
                }
            }

            \App::get('session')->setFlash('error', \App::get('translation')->get('error_taking_payment'));
            return header("Location: ".App::get('router')->generate('client-manage-invoice', array('id' => $invoice_id)));
        }

        /**
         * payment was cancelled display errors
         */
        public function payCancel($invoice_id = 0)
        {
            \App::get('session')->setFlash('error', \App::get('translation')->get('error_taking_payment'));
            return header("Location: ".App::get('router')->generate('client-manage-invoice', array('id' => $invoice_id)));
        }
    }

