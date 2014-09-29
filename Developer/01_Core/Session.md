The Session class is basically a wrapper for the Aura Session class. This is to make accessing the session data easier. Here we only cover our methods / overriding changes, for full documentation see the Aura Session 1.x branch documents [here](https://github.com/auraphp/Aura.Session/tree/develop).

# Session Data

If you need to store data in the session to use between numerous pages then you can set data to session using the following code.

	\App::get('session')->setData('unique-name', 'value');

To then access the data use the following code. This function will return `null` if the given unique name does not exist in the session variable.

	\App::get('session')->getData('unique-name');

# Flash Data

If you need to store data in the session to use only on the next page such as a confirmation / error messages, use the following 'Flash Data' functions.

	\App::get('session')->setFlash('unique-name', 'value');

To then access the data use the following code.

	\App::get('session')->getFlash('unique-name');

Given the nature of flash data, once a flash data variable as been retrieved, it will be deleted from the session. If you need to check a flash variable has been set before getting it the follow code can be used and it will return a boolean value as to whether the variable exists or not.

	\App::get('session')->hasFlash('unique-name');

# CSRF Functionality

For full information on the CSRF functionality the session handler provides see the [Aura Session readme](https://github.com/auraphp/Aura.Session). The session handler uses function overloading allowing you to run the CSRF code straight from App::get('session').

The Aura Session readme gives this as an example to get a CSRF token.

	$csrf_value = $session->getCsrfToken()->getValue()

To do the same using our Session Handler you can do the following.

	$csrf_value = \App::get('session')->getCsrfToken()->getValue()
