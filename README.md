## Route It

A fast, flexible router which can be used to quickly build RESTful web apps.

- [License](#license)
- [Author](#author)
- [Requirements](#requirements)
- [Installation](#installation)
- [Usage](#usage)

## License

This project is open source and available under the [MIT License](LICENSE).

## Author

<img src="https://cdn1.onbayfront.com/bfm/brand/bfm-logo.svg" alt="Bayfront Media" width="250" />

- [Bayfront Media homepage](https://www.bayfrontmedia.com?utm_source=github&amp;utm_medium=direct)
- [Bayfront Media GitHub](https://github.com/bayfrontmedia)

## Requirements

* PHP `^8.0`
* ctype PHP extension

## Installation

```
composer require bayfrontmedia/route-it
```

## Usage

### Start using Route It

Default options:

```
use Bayfront\RouteIt\Router;

$options = [
    'automapping_enabled' => false,
    'automapping_namespace' => '',
    'automapping_route_prefix' => '',
    'class_namespace' => '',
    'files_root_path' => '',
    'force_lowercase_url' => false
];

$router = new Router($options);
```

### Force lowercase

When `force_lowercase_url` is enabled in the `$options` array, all incoming requests which contain uppercase characters will be redirected via a 301 redirect to their lowercase counterpart.
Query parameters will not be affected.

### Automapping

When automapping is enabled, Route It will automatically attempt to dispatch the incoming request in the form of `class/method/optional-parameter` without having to define each individual route.

The incoming request must match the automapping route prefix, and classes must exist in the automapping namespace as defined in the `$options` array.

For example:

 ```
 use Bayfront\RouteIt\Router;
 
 $options = [
     'automapping_enabled' => true,
     'automapping_namespace' => 'App\\Pages',
     'automapping_route_prefix' => '/app'
 ];
 
 $router = new Router($options);
 ```

Using the above example, incoming requests would be automapped like so:

| Endpoint                | Class               | Method | Parameter     |
|-------------------------|---------------------|--------|---------------|
| `/app`                  | App\Pages\Home      | index  |               |
| `/app/customers`        | App\Pages\Customers | index  |               |
| `/app/customers/edit`   | App\Pages\Customers | edit   |               |
| `/app/customers/edit/5` | App\Pages\Customers | edit   | `['id' => 5]` |

Understandably, automapping does not provide a solution for every scenario, in which case, routes can be added.

### Named routes

Named routes are helpful when using hyperlinks in controllers, views or templates.
By referencing a named route instead of a specific URL, the hyperlink will stay up-to-date with your defined routes.

When dispatching a request using the [dispatch](#dispatch) , [dispatchTo](#dispatchto), or [dispatchToFallback](#dispatchtofallback) methods,
Route It will automatically insert an array of all named routes to the destination as a parameter with the key of `routes`,
unless otherwise specified.

See documentation for these methods for more information.

### Wildcards

Wildcards can be used when defining a route in order to dynamically define a path, and send its value to the destination as a parameter. The syntax is `{type:name}`, where `type` is the wildcard type, and `name` is the name of the parameter which is to be passed to the destination.

Wildcards include:

- `*`: Any non-whitespace character in this segment of the request
- `alpha`: Any alphabetic characters in this segment of the request (see [ctype_alpha](https://www.php.net/manual/en/function.ctype-alpha.php))
- `num`: Any numeric characters in this segment of the request (see [ctype_digit](https://www.php.net/manual/en/function.ctype-digit.php))
- `alphanum`: Any alphanumeric characters in this segment of the request (see [ctype_alnum](https://www.php.net/manual/en/function.ctype-alnum.php))
- `**`: Everything else that may exist on the request path (catch-all)
- `?`: Optionally existing in this segment of the request (can only be used at the last segment)

For examples, see [addRoute](#addroute).

### Public methods

- [setHost](#sethost)
- [getHost](#gethost)
- [setRoutePrefix](#setrouteprefix)
- [getRoutePrefix](#getrouteprefix)
- [addFallback](#addfallback)
- [getFallbacks](#getfallbacks)
- [addRedirect](#addredirect)
- [getRedirects](#getredirects)
- [addRoute](#addroute)
- [any](#any)
- [connect](#connect)
- [delete](#delete)
- [get](#get)
- [head](#head)
- [options](#options)
- [patch](#patch)
- [post](#post)
- [put](#put)
- [trace](#trace)
- [getRoutes](#getroutes)
- [addNamedRoute](#addnamedroute)
- [getNamedRoutes](#getnamedroutes)
- [getNamedRoute](#getnamedroute)
- [getResolvedParameters](#getresolvedparameters)
- [resolve](#resolve)
- [dispatch](#dispatch)
- [dispatchTo](#dispatchto)
- [dispatchToFallback](#dispatchtofallback)

<hr />

### setHost

**Description:**

Sets the hostname for defined routes.

**Parameters:**

- `$host` (string)

**Returns:**

- (self)

**Example:**

```
$router->setHost('example.com');

$router->addRoute('GET', '/login', function () {
 
    // Destination for example.com/login

});

$router->setHost('subdomain.example.com');

$router->addRoute('GET', '/login', function () {
 
    // Destination for subdomain.example.com/login

});
```

<hr />

### getHost

**Description:**

Retrieves the hostname for defined routes.

**Parameters:**

- None

**Returns:**

- (string)

<hr />

### setRoutePrefix

**Description:**

Sets the route prefix for defined routes.

**Parameters:**

- `$prefix` (string)

**Returns:**

- (self)

**Example:**

```
$router->setHost('example.com')->setRoutePrefix('app');

$router->addRoute('GET', '/login', function () {
 
    // Destination for example.com/app/login

});
```

<hr />

### getRoutePrefix

**Description:**

Retrieves the route prefix for defined routes.

**Parameters:**

- None

**Returns:**

- (string)

<hr />

### addFallback

**Description:**

Adds a fallback destination for given request method(s) when no route can be found.
The response will be sent with a `404` HTTP status code.

**Parameters:**

- `$methods` (string|array): Request method(s) for which this fallback is valid, or "ANY"
- `$destination` (mixed)
- `$params = []` (array): Parameters to pass to the destination

**Returns:**

- (self)

**Example:**

```
$router->addFallback('ANY', function () {
    echo '404 page not found';
});
```

For more destination examples, see [addRoute](#addroute).

<hr />

### getFallbacks

**Description:**

Returns array of defined fallbacks.

**Parameters:**

- None

**Returns:**

- (array)

<hr />

### addRedirect

**Description:**

Adds a redirect. Wildcards can be used in the path.

**Parameters:**

- `$methods` (string|array): Request method(s) for which this redirect is valid, or "ANY"
- `$path` (string): Request path
- `$destination` (string): Can be an internal path or fully qualified URL
- `$status = 302` (int): HTTP status code used with the redirect

**Returns:**

- (self)

**Example:**

```
// Internal temporary redirect (will not redirect to self)

$router->addRedirect('ANY', '{**:path}', '/under-construction', 302);

// Fully qualified URL

$router->addRedirect('GET', '/documentation', 'https://www.example.com/new-documentation');
```

For more path definition examples, see [addRoute](#addroute).

<hr />

### getRedirects

**Description:**

Returns array of defined redirects.

**Parameters:**

- None

**Returns:**

- (array)

<hr />

### addRoute

**Description:**

Adds a defined route. Wildcards can be used in the path.

Destinations can be a callable function, a named route,  a file, or a `$class->method()`. 
Each route can have its own predefined parameter(s), and parameters can also be defined dynamically by a "wildcard".

**Parameters:**

- `$methods` (string|array): Request method(s) for which this route is valid, or "ANY"
- `$path` (string): Request path
- `$destination` (mixed)
- `$params = []` (array): Parameters to pass to the destination
- `$name = NULL` (string|null): An optional name to assign to this route

**NOTE:** 
Names should not be assigned to routes which include wildcards in the path.
Named routes are only intended to define specific URL's.
Named route names must be unique, as names which already exist are overwritten.

**Returns:**

- (self)

**Example:**

```
$router

    // Callable

    ->addRoute('GET', '/customers', function () {

        echo 'Customers';

    })

    // Callable with a wildcard parameter

    ->addRoute('GET', '/customers/{num:id}', function ($params) {

        echo 'Customer id: ' . $params['id'];

    })

    // Callable with optional wildcard (if existing in the request path, it will overwrite the defined parameter)

    ->addRoute('GET', '/optional/{?:name}', function ($params) {

        echo 'Hello, ' . $params['name'] . '! This is a callable with an optional parameter.';

    }, [
        'name' => 'John'
    ])

    // Callable, saving as a named route

    ->addRoute('GET', '/login', function($params) {

        // Login

    }, [], 'login')

    // To a named route

    ->addRoute('GET', '/oldlogin', 'login')

    // To a file from the files_root_path as defined in the options array

    ->addRoute('GET', '/file', '@filename.html')

    // To a class:method from the class_namespace as defined in the options array

    ->addRoute('GET', '/class', 'TestClass:index', ['greeting' => 'Hello!']);
```

<hr />

### any

**Description:**

Adds route for ANY request method.

Equivalent of calling `addRoute('ANY'...)`

**Parameters:**

- `$path` (string): Request path
- `$destination` (mixed)
- `$params = []` (array): Parameters to pass to the destination
- `$name = NULL` (string|null): An optional name to assign to this route

**NOTE:** Names should not be assigned to routes which include wildcards in the path. Named routes are only intended to define specific URL's.

**Returns:**

- (self)

**Example:**

```
$router->any('/customers', 'Controller:anyMethod');
```

<hr />

### connect

**Description:**

Adds route for CONNECT request method.

Equivalent of calling `addRoute('CONNECT'...)`

**Parameters:**

- `$path` (string): Request path
- `$destination` (mixed)
- `$params = []` (array): Parameters to pass to the destination
- `$name = NULL` (string|null): An optional name to assign to this route

**NOTE:** Names should not be assigned to routes which include wildcards in the path. Named routes are only intended to define specific URL's.

**Returns:**

- (self)

**Example:**

```
$router->connect('/customers', 'Controller:connectMethod');
```

<hr />

### delete

**Description:**

Adds route for DELETE request method.

Equivalent of calling `addRoute('DELETE'...)`

**Parameters:**

- `$path` (string): Request path
- `$destination` (mixed)
- `$params = []` (array): Parameters to pass to the destination
- `$name = NULL` (string|null): An optional name to assign to this route

**NOTE:** Names should not be assigned to routes which include wildcards in the path. Named routes are only intended to define specific URL's.

**Returns:**

- (self)

**Example:**

```
$router->delete('/customers', 'Controller:deleteMethod');
```

<hr />

### get

**Description:**

Adds route for GET request method.

Equivalent of calling `addRoute('GET'...)`

**Parameters:**

- `$path` (string): Request path
- `$destination` (mixed)
- `$params = []` (array): Parameters to pass to the destination
- `$name = NULL` (string|null): An optional name to assign to this route

**NOTE:** Names should not be assigned to routes which include wildcards in the path. Named routes are only intended to define specific URL's.

**Returns:**

- (self)

**Example:**

```
$router->get('/customers', 'Controller:getMethod');
```

<hr />

### head

**Description:**

Adds route for HEAD request method.

Equivalent of calling `addRoute('HEAD'...)`

**Parameters:**

- `$path` (string): Request path
- `$destination` (mixed)
- `$params = []` (array): Parameters to pass to the destination
- `$name = NULL` (string|null): An optional name to assign to this route

**NOTE:** Names should not be assigned to routes which include wildcards in the path. Named routes are only intended to define specific URL's.

**Returns:**

- (self)

**Example:**

```
$router->head('/customers', 'Controller:headMethod');
```

<hr />

### options

**Description:**

Adds route for OPTIONS request method.

Equivalent of calling `addRoute('OPTIONS'...)`

**Parameters:**

- `$path` (string): Request path
- `$destination` (mixed)
- `$params = []` (array): Parameters to pass to the destination
- `$name = NULL` (string|null): An optional name to assign to this route

**NOTE:** Names should not be assigned to routes which include wildcards in the path. Named routes are only intended to define specific URL's.

**Returns:**

- (self)

**Example:**

```
$router->options('/customers', 'Controller:optionsMethod');
```

<hr />

### patch

**Description:**

Adds route for PATCH request method.

Equivalent of calling `addRoute('PATCH'...)`

**Parameters:**

- `$path` (string): Request path
- `$destination` (mixed)
- `$params = []` (array): Parameters to pass to the destination
- `$name = NULL` (string|null): An optional name to assign to this route

**NOTE:** Names should not be assigned to routes which include wildcards in the path. Named routes are only intended to define specific URL's.

**Returns:**

- (self)

**Example:**

```
$router->patch('/customers', 'Controller:patchMethod');
```

<hr />

### post

**Description:**

Adds route for POST request method.

Equivalent of calling `addRoute('POST'...)`

**Parameters:**

- `$path` (string): Request path
- `$destination` (mixed)
- `$params = []` (array): Parameters to pass to the destination
- `$name = NULL` (string|null): An optional name to assign to this route

**NOTE:** Names should not be assigned to routes which include wildcards in the path. Named routes are only intended to define specific URL's.

**Returns:**

- (self)

**Example:**

```
$router->post('/customers', 'Controller:postMethod');
```

<hr />

### put

**Description:**

Adds route for PUT request method.

Equivalent of calling `addRoute('PUT'...)`

**Parameters:**

- `$path` (string): Request path
- `$destination` (mixed)
- `$params = []` (array): Parameters to pass to the destination
- `$name = NULL` (string|null): An optional name to assign to this route

**NOTE:** Names should not be assigned to routes which include wildcards in the path. Named routes are only intended to define specific URL's.

**Returns:**

- (self)

**Example:**

```
$router->put('/customers', 'Controller:putMethod');
```

<hr />

### trace

**Description:**

Adds route for TRACE request method.

Equivalent of calling `addRoute('TRACE'...)`

**Parameters:**

- `$path` (string): Request path
- `$destination` (mixed)
- `$params = []` (array): Parameters to pass to the destination
- `$name = NULL` (string|null): An optional name to assign to this route

**NOTE:** Names should not be assigned to routes which include wildcards in the path. Named routes are only intended to define specific URL's.

**Returns:**

- (self)

**Example:**

```
$router->trace('/customers', 'Controller:traceMethod');
```

<hr />

### getRoutes

**Description:**

Returns array of defined routes.

**Parameters:**

- None

**Returns:**

- (array)

<hr />

### addNamedRoute

**Description:**

Adds a specific path as a named route.

This is helpful when wanting to reference a URL that is not defined as a route.

**Parameters:**

- `$path` (string): Request path
- `$name` (string): Name to assign to this route

**Returns:**

- (self)

**Example:**

```
$router->setHost('example.com')->setPrefix('app');

$router->addNamedRoute('/assets/css', 'css');

// Returns: https://example.com/app/assets/css
echo $router->getNamedRoute('login'); 
```

<hr />

### getNamedRoutes

**Description:**

Returns array of named routes.

**Parameters:**

- None

**Returns:**

- (array)

<hr />

### getNamedRoute

**Description:**

Returns URL of a named route.

**Parameters:**

- `$name` (string)
- `$default = ''` (string): Default value to return if named route does not exist

**Returns:**

- (string)

<hr />

### getResolvedParameters

**Description:**

Get array of all parameters present for the current route, once resolved/dispatched.

**Parameters:**

- None

**Returns:**

- (array)

<hr />

### resolve

**Description:**

Resolves the incoming HTTP request by searching for a matching redirect, route, automapped location, or fallback.
Destination-specific parameters will overwrite global parameters of the same key.

Returned array:

- `redirect`: Contains keys `type`, `destination` (URL), and `status` (HTTP status code)
- `route`: Contains keys `type`, `destination` (defined route), and `params`
- `automap`: Contains keys `type`, `destination` (as class:method), and `params`
- `fallback`: Contains keys `type`, `destination` (defined fallback), `params` and `status`

A `DispatchException` will be thrown if the request is unable to be resolved.

**Parameters:**

- `$name` (string)
- `$default = ''` (string): Default value to return if named route does not exist

**Returns:**

- (array)

<hr />

### dispatch

**Description:**

Resolves and dispatches the incoming HTTP request.

Destination-specific parameters will overwrite global parameters of the same key.

**Parameters:**

- `$params = []` (array): Global parameters to pass to all destinations 

**Returns:**

- (mixed)

**Throws:**

- `Bayfront\RouteIt\DispatchException`

**Example:**

```
use Bayfront\RouteIt\DispatchException;
use Bayfront\RouteIt\Router;

$options = [
    'automapping_enabled' => false,
    'automapping_namespace' => '',
    'automapping_route_prefix' => '',
    'class_namespace' => '',
    'files_root_path' => '',
    'force_lowercase_url' => false
];

$router = new Router($options);

// Add routes here

try {

    $router->dispatch();

} catch (DispatchException $e) {
    die($e->getMessage());
}
```

### dispatchTo

**Description:**

Dispatches to a specific destination.

Destinations can be a callable function, a named route, a file, or a `$class->method()`.

**Parameters:**

- `$destination` (mixed)
- `$params = []` (array): Parameters to pass to the destination

**Returns:**

- (mixed)

**Throws:**

- `Bayfront\RouteIt\DispatchException`

**Example:**

```
try {

    $router->dispatchTo('TestClass:index');

} catch (DispatchException $e) {
    die($e->getMessage());
}
```

### dispatchToFallback

**Description:**

Dispatches to fallback for current request method, or throws exception.

Fallback-specific parameters defined using the `addFallback` method will overwrite these parameters of the same key.

**Parameters:**

- `$params = []` (array): Parameters to pass to the destination

**Returns:**

- (mixed)

**Throws:**

- `Bayfront\RouteIt\DispatchException`

**Example:**

```
try {

    $router->dispatchToFallback();

} catch (DispatchException $e) {
    die($e->getMessage());
}
```

### redirect

**Description:**

Redirects to a given URL using a given status code.

**Parameters:**

- `$url` (string): Fully qualified URL
- `$status = 302`: HTTP status code to return

**Returns:**

- (void)

**Throws:**

- `Bayfront\RouteIt\DispatchException`

**Example:**

```
try {

    $router->redirect('https://www.example.com);

} catch (DispatchException $e) {
    die($e->getMessage());
}
```