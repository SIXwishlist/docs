There are 6 methods that can be used for generating the add / edit scaffolding pages, these are:

* formToolbar
* formFields
* formBreadcrumb
* processData
* getExtraData
* afterSave

There is also a static variable which can be defined on the controller should you wish to override the form scaffolding method and return the save result rather then redirect after saving.

## formToolbar

This method is used to define what items to show on the toolbar above the main form. This is exactly the same as the [indexToolbar](/Developer/App/Scaffolding/Listing) method. For most CRUD instances the toolbar doesn't have to change between listing / form so as default, `formToolbar` simply calls back to `indexToolbar`.

## formFields

This method defines exactly what fields are required on the form. At a minimum the fields need to be defined in a `ModelName.fieldName` format. For the scaffolding to work correctly the Models primary key field should always be included in this array (usually an `id` field).

    protected function formFields()
    {
        $fields = array(
            'Currency.id',
            'Currency.code' => array(
                'label' => 'currency_code'
            ),
            'Currency.prefix',
            'Currency.suffix',
            'Currency.decimals',
            'Currency.decimal_point',
            'Currency.thousand_separator',
            'Currency.conversion_rate',
            'Currency.auto_update'
        );

        return $fields;
    }
    
With nothing else set other then the Model and field name, WHSuite will try to automatically work out what field type it needs to be based on the schema for the model. As with the indexColumns the label for the field will be taken from the field name and passed through the translation methods.

If you want to customise it more, you can pass in the `type` which corresponds to `input` tag types, `textareas` or `select` boxes. 

`label` can be passed to manually set what language slug should be  passed to the translation methods.

`options` element can be defined to pass any options for select boxes. Alternatively this can be done automatically by assigning the array of options to the templates with the same variable name as the field name. e.g. `Staff.language_id` would look for `$language_id` within the template for the array of languages to populate the drop down with.

As part of the form builder which generates the html form fields, some elements are available to inject content into the field layout. `before`, `between` and `after` are elements available.

    'Staff.confirm_password' => array(
        'type' => 'password',
        'after' => '<span class="help-block">'.\App::get('translation')->get('password_leave_blank').'</span>'
    ),
    
The default layout for form fields is:

	<div class="form-group">
		-- Before Elements Injected Here --
		<label for="dataStaffConfirmPassword">Confirm Password</label>
		-- Between Elements Injected Here --
		<input type="password" class="form-control" name="data[Staff][confirm_password]" id="dataStaffConfirmPassword">
		-- After Elements Injected Here --
	</div>

You can also cutomise the input and the wrapping div by using some of the following elements

* `class` - defines the class on the form field.
* `wrap_class` - defines the class on the wrapping div.
* `label` - if passed as an array, instead of a string, with a sub element of `label` for the label slug, you can then define extra attributes such as `class`, `id` to be placed on the label tag. Alternatively, pass false to not show a label.
* `wrap` - defines which tag to use for the wrapper tag, defaults to 'div'. Set to false to not use a wrapping tag.

## formBreadcrumb

As with the indexBreadcrumb method, this method will auto populate for most standard CRUD setups. It will as default populate with a dashboard link, link to the listing page and finally a link to the current add / edit page.

As with indexBreadcrumb the method needs to finish with the `build` method being called on the Breadcrumb class.

    /**
     * generate the form breadcrumb trail, can override without redefining whole form functon
     *
     * @param string $model - the model name we are dealing with, used to generate the routes
     * @param string $page_title - the page title of the current page
     */
    protected function formBreadcrumb($model, $page_title)
    {
        // prefix - either admin or client
        $prefix = $this->getSection();

        $breadcrumb = App::get('breadcrumbs');
        $breadcrumb->add($this->lang->get('dashboard'), $prefix . '-home');
        $breadcrumb->add($this->lang->get(strtolower($model) . '_management'), $prefix . '-' . strtolower($model));
        $breadcrumb->add($page_title);
        $breadcrumb->build();
    }

## processData

The processData method is applied to every field that is being saved. This can be used to format data ready for inserting into the database.

Below is the doc block for the method.

    /**
     * processData
     *
     * Function called as the form data is assigned to the model.
     * useful for any processing needed on the field
     *
     * @param string $field - the field name we are dealing with
     * @param mixed $data - the data we are assigning to the model
     * @param object $main_model - the main model we are saving (passed by reference so we can affect it!)
     * @return mixed - data to assign to the model
     */
    protected function processData($field, $data, &$main_model)
    
Below is an example of it in use within the support desk departments, in this situation it is used to prevent the staff group link being updated as this is handled within another callback method and to also format the email piping settings associated with a department.

    protected function processData($field, $data, &$main_model)
    {
        if ($field == 'StaffGroup') {
            $data = false;
        } elseif (Str::contains($field, 'piping_settings_')) {
            $data = false;
        } elseif ($field == 'piping') {
            if ($data == 1) {
                $department = PostInput::get('data.SupportDepartment');
                $settings = array();

                foreach ($department as $key => $value) {
                    if (Str::contains($key, 'piping_settings_')) {
                        $key = str_replace('piping_settings_', '', $key);
                        $settings[$key] = $value;
                    }
                }
                $main_model->piping_settings = json_encode($settings);
            } else {
                $main_model->piping_settings = '';
            }
        }
        return $data;
    }
    
## getExtraData

This method is called after the main model data has been loaded but just before anything is added to the view. This can be used to load any extra data needed for a form. Data such as lists for drop downs or radio buttons etc...

Below is an example of it in use within the knowledge base articles admin area for retrieving the categories that we can assign the article to.

    protected function getExtraData($model)
    {
        $this->view->set(
            'kb_category_id',
            KbCategory::formattedList(
                'id',
                'name',
                array(
                    array(
                        'column' => 'is_active',
                        'operator' => '=',
                        'value' => 1
                    )
                ),
                'sort',
                'asc'
            )
        );
    }
    
The `$model` variable passed as a parameter is a copy of the main model which can be used to retrieve the extra data if you need to filter on any of the data the main model contains.

## afterSave

This method is a controller method that is called after the main model has saved successfully. The method accepts one parameter which is the main model that has just been saved with this variable being passed by reference.

    /**
     * afterSave callback
     *
     * Called once the save was successful
     *
     * @param object $main_model - the main model we are saving (passed by reference so we can affect it!)
     */
    protected function afterSave(&$main_model) { }
    
This can be used to do any processing needed after the main model has saved, from saving things such as related data or uploading files.

Below is an example of the `afterSave` callback being used by the support desks admin support ticket controller. In this instance it's used to attach the post to the actual ticket that has just been created and to then upload any files that were attached during the posting.

    protected function afterSave(&$main_model)
    {
        parent::afterSave($main_model);

        $SupportPost = new SupportPost;
        $SupportPost->body = PostInput::get('data.SupportTicket.SupportPost.body');
        $SupportPost->staff_id = $this->admin_user->id;

        $SupportPost = $main_model->SupportPost()->save($SupportPost);

        if (\App::checkInstalledAddon('uploader')) {

            // process any uploads
            \Addon\Uploader\Libraries\Process::uploads('SupportPost', $SupportPost);
        }

        // ticket notification
        \Addon\SupportDesk\Libraries\Notifications::clientNewReply($main_model->id);
    }
    
## return\_on\_success

On a controller, the static variable `self::$return_on_success` can be defined. This can be used if you wish to override the method and perform a different redirect to the one the scaffolding provides.

An example of it in use can be found in the client side support ticket controller as part of the support desk. In standard operation the scaffolding would redirect back the normal listing. Within the support desk however we wanted to be able redirect to a different location if the user is a guest user and the post was successful.

The following code is a condensed example from the support ticket controller mentioned above.

    public function form($id = null)
    {
        // if we've posted, setup guest account
    	
    	 // define the $return_on_success var to true
    	 // and called parent form to activate the scaffolding
        self::$return_on_success = true;
        $return = parent::form($id);

        if ($return) {
            // the post was successful
            // check if it was a guest user or not
            // and redirect to the appropriate place.
        }
    }
    

