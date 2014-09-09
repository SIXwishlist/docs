The dispatcher class behaves like a standard dispatcher. It takes the output from the matched route from the [Router](/Developer/Core/Router) and processes this to determine which controller to load.

There are some extra functions available to use however which will be useful from time to time.

## Getters

The dispatcher provides two getter methods to provide information about the route the dispatcher is processing and also the page number (for use on listing pages with paging).

First is the getRoute() method, this simply returns the router object that the dispatcher receives and is the direct output from the `Router->match()` method.

    /**
     * return the route we are loading
     *
     * @return  object  route object returned from the router for the route we are loading
     */
    public function getRoute()
    {
        return $this->route;
    }
    
Next is the `getPageNumber()`. The router can handle the processing of page numbers if passed in the correct format (a route with the 'page' parameter set, see `/whsuite/app/configs/routes/admin/domainextension.php` and look for the domainextension-paging route.

    /**
     * return the page number
     *
     * @return  int  page number found in URL, if none exists '1' will be returned
     */
    public function getPageNumber()
    {
        $page_number = 1;
        if (isset($this->route->page_number) && $this->route->page_number > 0) {

            $page_number = $this->route->page_number;
        }

        return $page_number;
    }
    
If no page number is found it will default to 1 indicating the first page.

## Inline Requests

The last method that will be of use is the `load()` method. This method will allow you to perform inline requests to another URL. After setting all the data it will use Aura/HTTP package to perform a request to the given URL and return it's output.

    /**
     * dispatch to the given route and return data
     *
     * @param   string  the url to dispatch to
     * @param   string  (optional) method request constant to use (see aura/http/src/aura/http/message/request.php for options)
     * @param   mixed   (optional) any data to set, can be string / array / file resource (only used in post/put method)
     * @param   mixed   (optional) authentication: array('type' => 'AUTH_BASIC|AUTH_DIGEST', 'username' => '', 'password' => '')
     *                  (see aura/http/src/aura/http/message/request.php for more info)
     * @return  mixed
     */
    public function load($url, $method = 'GET', $data = array(), $auth = null)
    
In a simple example, the method could be used like so

	$json_results = \App::get('dispatcher')->load('/my-api/something.json');
	
This would set the json data from something.json into $json_results allowing you to then perform a `json_decode()` and process as needed.