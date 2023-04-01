# php-router-swagger
Swagger Plugin for eru123/router

## Basic Usage

```php
<?php

// Include the vendor/autoload.php file.
require_once __DIR__ . '/vendor/autoload.php';

// Import the required classes.
use eru123\swagger\Swagger;
use eru123\router\Router;

// Define Swagger path and it's json file path.
$swagger = Swagger::build([
    '/docs' => [
        '/v1' => __DIR__ . '/swaggerv1.json',
        '/v2' => __DIR__ . '/swaggerv2.json',
        '/v3' => [
            '-dev' => __DIR__ . '/swaggerv3-dev.json',
            '-prod' => __DIR__ . '/swaggerv3-prod.json',
        ]
    ]
]);

// Create a new Router instance for the API path and add the Swagger instance as a child.
$api = new Router();
$api->base('/api');
$api->child($swagger);

// Create the main Router instance
$router = new Router();

// Add a fallback route to return a 404 json response.
$router->fallback('/', function () {
    return [
        'error' => '404 Not Found'
    ];
});

// Add the API router as a child of the main router.
$router->child($api);

// Run the router.
$router->run();
```

### Produced Routes
The example code above will produce the following routes:
 - /api/docs/v1
 - /api/docs/v2
 - /api/docs/v3-dev
 - /api/docs/v3-prod

For each of these routes, the file specified in the Swagger::build() method will be served as a json file in these paths as `/swagger.json` (e.g. `/api/docs/v1/swagger.json`).