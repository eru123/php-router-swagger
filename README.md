# php-router-swagger
Swagger Plugin for eru123/router

## Basic Usage

```php
<?php

require_once __DIR__ . '/vendor/autoload.php';

use eru123\swagger\Swagger;
use eru123\router\Router;

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

/** 
 * This will create the following routes:
 *  - /docs/v1
 *  - /docs/v2
 *  - /docs/v3-dev
 *  - /docs/v3-prod
 */

$api = new Router();
$api->base('/api');
$api->child($swagger);

/**
 * /api is used as base path, so now it will 
 * be prepended to all of its children. 
 * The following routes will be created:
 *  - /api/docs/v1
 *  - /api/docs/v2
 *  - /api/docs/v3-dev
 *  - /api/docs/v3-prod
 */

$router = new Router();
$router->fallback('/', function () {
    return [
        'error' => '404 Not Found'
    ];
});
$router->child($api);
$router->run();
```