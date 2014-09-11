WHSuite framework employs a simple "listener" style hook system.

This page is purely for the syntax of setting up hooks, for the best practises of using the hooks during addon development see [this page](/Developer/Addons/Hooks), or for a full list of the hook points available see [this page](/Developer/App/Hook_Locations).

## Listening

To listen for a hook, simply call the `startListening()` method of the hook class.

    /**
     * add a listener
     *
     * @param   string  event we are listening for
     * @param   string  unique name for this listener - so we can access again if needed
     * @param   string  callback function
     */
    public function startListening($event, $name, $callback)
    
This could be used in the following way to perform some start up action once we know what route we are to load.

	\App::get('hooks')->startListening('post_route', 'load_sidebar', '\Sidebar::load');
	
This would listen for the moment we find the route we are loading and then it would run the static load method in a class called Sidebar. 

If for some reason during runtime it's decided that a listener is no longer needed then you can remove the listener with the `stopListening()` method.

    /**
     * stop listening
     *
     * @param   string  event we are listening for
     * @param   string  unique name for this listener - so we can access again if needed
     */
    public function stopListening($event, $name)
    
Using the above example, this would be used in the following way

	\App::get('hooks')->stopListening('post_route', 'load_sidebar');
	
*nb. Some hook events may return a variable to your listening callback method, as the above example, the route object that the dispatcher will be using would be passed to \Sidebar::load(). These are documented along with the hook locations [here](/Developer/App/Hook_Locations)*

## Dispatching 

If you wish to setup your own hooks for your addon, you need to use the `callListeners()` method.

    /**
     * call all listeners
     *
     * @param   string  event to call
     * @param   mixed   multiple variables can be passed after the event name
     */
    public function callListeners()
    
The first parameter must be defined, this is the 'event name' which becomes the first parameter of the startListening / stopListening methods.

Any other parameters passed after the first parameter become the values passed to each of the listener callbacks during hook dispatch.