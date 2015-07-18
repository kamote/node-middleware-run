# middleware-run

This module allows you to run middleware from within a route or on it's own. The function returns a promise with the string next value `route` or `next`. If an error is thrown or `next(new Error())` is called within the chain it will be caught by the promise.

```
npm i middleware-run --save
```

# Usage

#### [`express-promise-router`](https://github.com/alex-whitney/express-promise-router)

```javascript
var promiseRouter = require("express-promise-router")()
promiseRouter.get("/hello", function(req, res){
  return middlewareRun(req, res, [
    middlewareSetTest("foo"),
    middlewareSetTest("bar")
  ])
})
```

#### Express Router

```javascript
var expressRouter = require("express").Router()
expressRouter.get("/hello", function(req, res, next){
  return middlewareRun(req, res, [
    middlewareSetTest("foo"),
    middlewareNextRoute,
    middlewareSetTest("bar")
  ]).then(function(val){
    if(val == "next") return next()
    return next(val)
  }).catch(next)
})
```

#### Without Router

```javascript
var httpMocks = require('node-mocks-http')
var req  = httpMocks.createRequest({})
var res = httpMocks.createResponse()
middlewareRun(req, res, [
  middlewareSetTest("foo"),
  middlewareNextRoute,
  middlewareSetTest("bar")
]).then(function(value){
  console.log(req.test) //foo
})
```
