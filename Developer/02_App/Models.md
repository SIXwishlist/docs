All application models are placed within the whsuite/app/models folder and are available in the global namespace. All models should extend the AppModel also located within this folder.

## AppModel

The AppModel is used as a base model for all our other models to extend. AppModel itself extends [Laravels Eloquent Model](http://laravel.com/docs/4.2/eloquent).

We use AppModel to place common functionality that all our models share, this includes

* Modified saved method which calls hooks after calling Eloquents save
* Modified delete method which calls hooks after calling Eloquents delete.
* Custom paginate method which from one method can generate a paginated list of data
* Handling custom fields if set

## Validation

Validation is handled primarly by [Laravels Validation Package](http://laravel.com/docs/4.2/validation), our wrapper package simply allows us to define the rules on a model and process them into the format Laravel Validation requires.

Validation rules can be defined on a model by specifying an static variable named rules.

	public static $rules = array();
	
The array key for each element must be the field name you wish to validate on with the value being a pipe separated list of rules to apply, these are done in exactly the same format as Laravels package expects. 

Any rule that requires additional parameters, these should be separated from the rule with a colon.

### Example

Below is a full example of validation used on our Client Model.

    public static $rules = array(
        'first_name' => 'required|max:255',
        'last_name' => 'required|max:255',
        'company' => 'max:255',
        'email' => 'email|required',
        'password' => 'same:confirm_password',
        'confirm_password' => '',
        'html_emails' => 'integer|max:1',
        'address1' => 'max:150|required',
        'address2' => 'max:150',
        'city' => 'max:150|required',
        'state' => 'max:150|required',
        'postcode' => 'max:50|required',
        'country' => 'max:255|required',
        'phone' => 'max:25|required',
        'currency_id' => 'integer',
        'status' => 'integer',
        'language_id' => 'integer',
        'is_taxexempt' => 'integer|max:1',
        'first_ip' => 'max:46',
        'first_hostname' => 'max:255',
        'last_ip' => 'max:46',
        'last_hostname' => 'max:255',
        'last_login' => 'max:255',
        'activated' => 'integer|max:1'
    );
    
## Relationships

As our models are extending from Laravels Eloquent Package, full documentation for defining relationships can be found [here](http://laravel.com/docs/4.2/eloquent#relationships). 

### Naming Convention

The name convention we use for the relationship methods is simple, the method names for a relationship match the foreign models name.

#### Examples
	
	// Staff Model
	public function StaffGroup()
	{
		return $this->belongsToMany('StaffGroup');
	}
	
	// Invoice Model
	public function InvoiceItem()
	{
		return $this->hasMany('InvoiceItem');
	}

	// LanguagePhrase	
	public function Language()
	{
		return $this->belongsTo('Language');
	}
	
## Paginate

The paginate method is a static method located within AppModel which provides each Model the ability to produce a paginated list. Given the conditions and sort orders and a few other misc settings (some of which can be inferred by the method by following correct naming conventions), the method will retrieve the required rows given the page number and also produce and assign to the template all the required info for the page numbers to be generated.


