> Since v1.4.x, TsExpessDecorators support Middlewares decorators

### Installation

In first place, you must adding the `middlewares` folder on `componentsScan` attribut in your server settings as follow :
 
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
       `${rootDir}/middlewares/**/**.js`,
   ]
})
export class Server extends ServerLoader {
   
}       
```
In second place, create a new file in your middlewares folder. Create a new Class definition and add the `@Middleware()` or `@MiddlewareError()` annotations on your class.

You have differents use case to declare and use a middlewares. Theses uses are following:

 * Global Middleware, this middleware can be used on [ServerLoader](),
 * Global MiddlewareError, this middleware error can be used on [ServerLoader](),
 * Endpoint Middleware, this middleware can be used on a controller method,
 * Endpoint Middleware Error, this middleware can be used on a controller method.

### Global Middleware

### Global MiddlewareError

### Middleware for Endpoint

### MiddlewareError for Endpoint