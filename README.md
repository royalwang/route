# Route - A fast route implemented by php
[![StyleCI](https://styleci.io/repos/84079657/shield?branch=master)](https://styleci.io/repos/84079657)
[![Build Status](https://travis-ci.org/zqhong/route.svg?branch=master)](https://travis-ci.org/zqhong/route)
[![Scrutinizer Code Quality](https://scrutinizer-ci.com/g/zqhong/route/badges/quality-score.png?b=master)](https://scrutinizer-ci.com/g/zqhong/route/?branch=master)

`zqhong/route` is inspred by [nikic/FastRoute](https://github.com/nikic/FastRoute). But it is faster than `FastRoute`.

* [README English](README.md)
* [README 简体中文](README_ZH_CN.md)

---

# Install
```
$ composer require -vvv "zqhong/route:dev-master"
```

# Example
```
<?php

use zqhong\route\Helpers\Arr;
use zqhong\route\RouteCollector;
use zqhong\route\RouteDispatcher;

require dirname(__DIR__) . "/vendor/autoload.php";

function getUser($uid)
{
    echo "Your uid: " . $uid;
}

/** @var RouteDispatcher $routeDispatcher */
$routeDispatcher = dispatcher(function (RouteCollector $r) {
    $r->addRoute('GET', '/user/{id:\d+}', 'getUser');
});

$httpMethod = 'GET';
$uri = '/user/1';
$routeInfo = $routeDispatcher->dispatch($httpMethod, $uri);

if (Arr::getValue($routeInfo, 'isFound')) {
    $handler = Arr::getValue($routeInfo, 'handler');
    $params = Arr::getValue($routeInfo, 'params');
    call_user_func_array($handler, $params);
} else {
    echo '404 NOT FOUND';
}

```

Using CURL send request：
```
// return 404 NOT FOUND
$ curl http://example.com/?r=ops

// return Your uid: 1
$ curl http://example.com/?=/user/1
```

---

# Benchmark
## Environment
* OS：Ubuntu 16.04 LTS（Vultr 4Cores 8G memory）
* PHP：7.0.4
* Apache：2.4.18

## Result
![](http://ww1.sinaimg.cn/large/ce744de6gy1fdgwdy9aghj20pc0df3yh)
```
nikic_route(v1.2)
Requests per second:    3527.98 [#/sec] (mean)

symfony route(v3.2)
Requests per second:    5193.17 [#/sec] (mean)

zqhong route(dev-master)
Requests per second:    5923.56 [#/sec] (mean)
```

Benchmark test code and result in `benchmark` folder.

---

# Document
[How it works](http://nikic.github.io/2014/02/18/Fast-request-routing-using-regular-expressions.html)