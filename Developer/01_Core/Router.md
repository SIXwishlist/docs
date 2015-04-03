The Router class is basically a wrapper for the Aura Router class. Here we only cover our methods / overriding changes, for full documentation see the Aura Router 1.x branch documents [here](https://github.com/auraphp/Aura.Router/tree/develop).

## Loading Routes

To attach routes to the router an array containing the routes must be passed into the route. For organisational and ease of use purposes, we wanted to separate all our routes into different files. 

As a result we have the `loadRoutes()` method which during bootstrap finds and includes in all the active route files ready for the router to process the incoming route.

## Attaching Routes

We have also overridden the `attach()` method. This is done to simply prepend the custom PHP constant `URL_PREFIX`. This contains either a forward slash (/) if WHSuite is installed in the root of your domain or a forward slash followed by the sub-directory WHSuite is stored in.

eg.

* www.example.com -> URL_PREFIX = /
* www.example.com/billing -> URL_PREFIX = /billing

As soon as URL_PREFIX is prepended, it is handed back to the Aura Router attach method.

## Matching Routes

We redefined the match method in order to do some further processing of the values returned.

Aura Router supports 'extra' parameters in the URL. When we're dealing with paging our page number comes through as an extra parameter with a value of page-<number>. We simply process the extra parameters to determine if a page number is set, if so we set it in a separate property on the router object that was returned from the Aura Router match method and then we simply return that object as it would normally expect.

This page number is the value that is then accessible via the `getPageNumber()` method on the [dispatcher class](/Developer/Core/Dispatcher).

## Magic Call

We make use of PHP magic `__call()` method as well in order to allow us to pass any methods that don't exist in our router to the Aura Router to handle if they exist in there.

    /**
     * function overloading to prevent having to use App::get('router')->router->add();
     * can just do App::get('router')->add();
     *
     * @param   string  method name we are trying to load
     * @param   array   array of params to pass to the method
     * @retun   mixed   return of the method
     */
    public function __call($name, $params)
    {
        if (method_exists($this->router, $name)) {

            $method_reflection = new \ReflectionMethod($this->router, $name);

            return $method_reflection->invokeArgs($this->router, $params);

        } else {

            throw new \Exception('Fatal Error: Function '.$name.' does not exist!');
        }
    }