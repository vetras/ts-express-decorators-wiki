> Since v1.4.x, TsExpessDecorators support Middlewares decorators

`@Middleware()` is similar to the Express middleware with the difference that it is a class and you can use the IoC to inject other services on his constructor. 

All middlewares decorated by `@Middleware` or `@MiddlewareError` have one method named `use()`. This method can use all parameters decorators as you could see with the [Controllers](https://github.com/Romakita/ts-express-decorators/wiki/Controllers). (See all [parameters decorators ](https://github.com/Romakita/ts-express-decorators/wiki/API-references#parameter-decorators)).In addition, you have this specifics parameters decorators for middlewares.

* [Installation]()
*

## Installation

In first place, you must adding the `middlewares` folder on `componentsScan` attribute in your server settings as follow :
 
```typescript
import * as Express from "express";
import {ServerLoader} from "ts-express-decorators";
import Path = require("path");
const rootDir = Path.resolve(__dirname);

@ServerSettings({
   rootDir,
   mount: {
      '/rest': `${rootDir}/controllers/**/**.js`
   },
   componentsScan: [
       `${rootDir}/services/**/**.js`,
       `${rootDir}/middlewares/**/**.js`
   ]
})
export class Server extends ServerLoader {
   
}       
```
In second place, create a new file in your middlewares folder. Create a new Class definition and add the `@Middleware()` or `@MiddlewareError()` annotations on your class.

You have different use case to declare and use a middlewares. Theses uses are following:

 * Global Middleware, this middleware can be used on [ServerLoader](https://github.com/Romakita/ts-express-decorators/wiki/Class:-ServerLoader),
 * Global MiddlewareError, this middleware error can be used on [ServerLoader](https://github.com/Romakita/ts-express-decorators/wiki/Class:-ServerLoader),
 * Endpoint Middleware, this middleware can be used on a controller method,
 * Endpoint Middleware Error, this middleware can be used on a controller method.

## Specifics decorators

Signature | Example | Description | Express analogue
--- | --- | --- | ---
`@Err()` | `useMethod(@Err() err: any) {}` | Inject the `Express.Err` service. (Decorator for middleware).| `function(err, request, response, next) {}`
`@ResponseData()` | `useMethod(@ResponseData() data: any)` | Provide the data returned by the previous middlewares.
`@EndpointInfo()` | `useMethod(@EndpointInfo() endpoint: Endpoint)` | Provide the endpoint settings.

## Declare middleware

Global middlewares and Endpoint middlewares are almost similar but Global middleware cannot use the `@EndpointInfo` decorator.

### Global

Global middlewares let you to manage request and response on [`ServerLoader`](https://github.com/Romakita/ts-express-decorators/wiki/Class:-ServerLoader).

In first, create your middleware:
```typescript
import {IMiddleware, Middleware, ResponseData, Response, ServerSettingsService} from "ts-express-decorators";
import {NotAcceptable} from "ts-httpexceptions";

@Middleware()
export default class GlobalAcceptMimesMiddleware implements IMiddleware {
   
   private mimes: string[];
   
   constructor(private serverSettingsService: ServerSettingsService) {
      this.mimes = this.serverSettings.get('acceptMimes');
   }

   use(@Request() request) {

        this.mimes.forEach((mime) => {
            if (!request.accepts(mime)) {
                throw new NotAcceptable(mime);
            }
        });
   }
}
```

The finally, add your middleware in `ServerLoader`:

```typescript
@ServerSettings({
   rootDir,
   mount: {
      '/rest': `${rootDir}/controllers/**/**.js`
   },
   componentsScan: [
       `${rootDir}/services/**/**.js`,
       `${rootDir}/middlewares/**/**.js`
   ],
   acceptMimes: ['application/json']  // add your custom configuration here
})
export class Server extends ServerLoader {
   $onMountingMiddlewares() {
       this.use(GlobalAcceptMimeMiddleware);
   }
}       
```

## Global errors middlewares

### Global MiddlewareError

### Middleware for Endpoint

### MiddlewareError for Endpoint