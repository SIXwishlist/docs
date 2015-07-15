There are 4 methods available for customising the index listing page.

* indexToolbar
* indexColumns
* indexActions
* indexBreadcrumb

## indexToolbar

This method is used to show sub menu items on the toolbar above the listing table. In most cases this toolbar shows at least a link to the "add" section of the module. Below is an example indexToolbar method from the Client Notes admin controller.

    protected function indexToolbar()
    {
        $route = \App::get('dispatcher')->getRoute();
        return array(
            array(
                'url_route' => 'admin-client-profile',
                'link_class' => '',
                'icon' => 'fa fa-list-ul',
                'label' => 'manage_client',
                'route_params' => array(
                    'id' => $route->values['id']
                )
            ),
            array(
                'url_route' => 'admin-clientnote-add',
                'link_class' => '',
                'icon' => 'fa fa-plus',
                'label' => 'new_note',
                'route_params' => array(
                    'id' => $route->values['id']
                )
            )
        );
    }
    
`url_route` is simply the route name as discussed [here](/Developer/Core/Router) and [here](/Developer/App/Routes).

`icon` is a font awesome icon to use prefixed to the link text. See [here](http://fortawesome.github.io/Font-Awesome/icons/) for the list of icons available.

`label` is the language slug to be used. This will be auto translated into the correct language by the template. You can just use a hardcoded language string in this `label` field should you not wish to use the translations as the translation system will default to showing the text entered if not translation string is found.

`route_params` is simply an array that gets passed to the router containing any parameters needed to generate the url. In the case above this is for the ID of the client so we can associate the note entered with the correct client.

## indexColumns

This is the main method used to define which fields should be shown in the listing table. This method is fairly clever in that it will allow you to define not just columns on the current model but other related model data (in different relationship types as well). Below is an example from the admin Staffs Controller.

    protected function indexColumns()
    {
        return array(
            array(
                'field' => array(
                    'first_name',
                    'last_name'
                ),
                'label' => 'name',
            ),
            array(
                'field' => 'email'
            ),
            array(
                'field' => 'StaffGroup.name',
                'label' => 'Groups',
                'seperator' => ' / '
            ),
            array(
                'field' => 'activated',
                'label' => 'status',
                'option_labels' => array(
                    '0' => '<span class="label label-danger">' . $this->lang->get('inactive') . '</span>',
                    '1' => '<span class="label label-success">' . $this->lang->get('active') . '</span>'
                )
            ),
            array(
                'field' => 'last_login'
            ),
            array(
                'action' => 'edit',
                'label' => null
            ),
            array(
                'action' => 'delete',
                'label' => null
            )
        );
    }
    
Usually `field` is the only parameter needed to define. The type of the field can usually be worked out based on the schema and the field type of the column in the database. In some cases where it is needed a `type` element can be added to describe the field type required.

Advanced `field` features included being able to define it as an array and actually define multiple fields in the instance of the name above. The Scaffolding will simply get the fields defined and join them together for displaying.

Another advanced feature for the `field` option is the ability to define in dot notation related models and a field from that model. As long as the [convention of naming the relationship methods](/Developer/App/Models) on a model then simply chain the model names until you get the one that has the field you require. The example above simply shows how to retrieve `StaffGroup` names that the staff member belongs to. The scaffolding is clever also in that it can handle multiple return results, like the example above we could return as many groups as they Staff Member is part of.

By defining `seperator` you can customise how fields are joined together. If not defined, `seperator` will default to a single space.

If no `label` column is defined then WHSuite will take the field name and use that as the language slug to try and find a translation. If a label is defined then it will still **Set `label` to null if you don't want to display a label at all.**

The `action` element value links to the action buttons (edit / delete etc...) that will be described in `indexActions`, see below for more info on these. 

`option_labels` allow you to define the values and labels for anything with multiple options or for checkbox off / on values.

## indexActions

This method is used to define all the actions available and describe how the button should look. These action buttons are then referenced within `indexColumns`.

    protected function indexActions()
    {
        return array(
            'edit' => array(
                'url_route' => 'admin-staff-edit',
                'link_class' => 'btn btn-primary btn-small pull-right',
                'icon' => 'fa fa-pencil',
                'label' => 'edit',
                'params' => array('id')
            ),
            'delete' => array(
                'url_route' => 'admin-staff-delete',
                'link_class' => 'btn btn-danger btn-small pull-right',
                'icon' => 'fa fa-times',
                'label' => 'delete',
                'params' => array('id')
            )
        );
    }
    
The `key` for each element needs to be a unique string which is used as the reference point within the `action` label of `indexColumns` as mentioned above.

`url_route` is simply the route name as discussed [here](/Developer/Core/Router) and [here](/Developer/App/Routes).

`link_class` is an element which is used to place any css classes onto the `a` tag.

`icon` is a font awesome icon to use prefixed to the link text. See [here](http://fortawesome.github.io/Font-Awesome/icons/) for the list of icons available.

`label` is the language slug to be used. This will be auto translated into the correct language by the template. You can just use a hardcoded language string in this `label` field should you not wish to use the translations as the translation system will default to showing the text entered if not translation string is found.

`params` is an array defining what fields need to be pulled from the main models data set, this data is then passed to the router for use in generating the link for the button. **Currently related data cannot be retrieved.**

## indexBreadcrumb

If you are dealing with a standard CRUD setup for the scaffolding then generally you would have no need to override this method. 

    /**
     * generate the index breadcrumb trail, can override without redefining whole index functon
     *
     * @param string $model - the model name we are dealing with, used to generate the routes
     * @param string $page_title - the page title of the current page
     */
    protected function indexBreadcrumb($model, $page_title)
    {
        // prefix - either admin or client
        $prefix = $this->getSection();

        $breadcrumb = App::get('breadcrumbs');
        $breadcrumb->add($this->lang->get('dashboard'), $prefix . '-home');
        $breadcrumb->add($page_title);
        $breadcrumb->build();
    }
    
No data needs to be returned from this method however the `build` method on the breadcrumb class needs to be called otherwise no breadcrumb will display.

This will simply show a link to the WHSuite dashboard and then an unclickable link to the current page you are on.