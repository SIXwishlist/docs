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

The paginate method is a static method located within AppModel which provides each Model the ability to produce a paginated list. 

Given the page number and any required conditions the method will retrieve the required rows and also produce and assign to the template all the required info for the page numbers to be generated.

    /**
     * create a paginated array of data
     *
     * @param   int         Number of items per page
     * @param   int         Page number
     * @param   array       Conditions to search on
     * @param   string|bool Field to sort on
     * @param   string      Sort direction
     * @param   string      Route name to use for paging links
     * @param   array       array of data to be passed to route generator
     * @return  array       array of results
     */
    public static function paginate(
    	$per_page, 
    	$page, 
    	$conditions = array(), 
    	$sort_by = false, 
    	$sort_order = 'desc', 
    	$route = null, 
    	$params = array()
    )
    
### Conditions

The conditions array doesn't map directly to Eloquent style where conditions. Instead a more basic but slightly more verbose array format is used.

Currently on 3 types of "where" statement are supported, `where`, `orWhere` and `whereIn`, omitting the `type` element will default to `where`.

    array(
        array(
            'column' => 'my_col',
            'operator' => '=',
            'value' => 'foobar'
        ),
        array(
            'type' => 'or',
            'column' => 'my_col2',
            'operator' => '>',
            'value' => 10
        ),
        array(
            'type' => 'in',
            'column' => 'my_col3',
            'value' => array(
                1, 3, 6, 33, 45, 56, 99
            )
        )
    )

### Route

If no route is passed then paginate will assume it's an Admin based page and try to use the following route name.

`admin-lowercasemodelname-paging`

For sections that have paging, two routes are generally created, one for no paging (i.e. page 1) and also for all routes that have an actual page number.

    'taxlevel' => array(
        'path' => '/tax-levels/',
        'values' => array(
            'controller' => 'TaxLevelsController',
            'action' => 'index'
        )
    ),
    'taxlevel-paging' => array(
        'params' => array(
            'page' => '(\d+)'
        ),
        'path' => '/tax-levels/{:page}/',
        'values' => array(
            'controller' => 'TaxLevelsController',
            'action' => 'index'
        )
    ),
    
n.b. The above routes would be prefixed with 'admin-' as described in the [Application Routes](/Developer/App/Routes) documentation.
    
#### Examples

Some example model names and the route name that would be used.

Model Name | Route Name
---------- | ----------
Staff | admin-staff-paging
TaxLevel | admin-taxlevel-paging
EmailTemplate | admin-emailtemplate-paging

