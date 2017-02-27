[Home](https://github.com/Romakita/ts-express-decorators/wiki) > Services

## IOC

> Since v1.1.0, TsExpessDecorators support the service injection (IOC). 

The decorator `@Service()` declare a new service can be injected in other service or controller on there `constructor()`.
All services annotated with `@Service()` are constructed one time.

`@Service()` use the `reflect-metadata` to collect and inject service on controllers or other services. 

### Settings Server

In first place, you must adding the `services` folder in your server settings like here :
 
```typescript
import * as Express from "express";
import {ServerLoader, IServerLifecycle} from "ts-express-decorators";
import Path = require("path");

export class Server extends ServerLoader implements IServerLifecycle {
    /**
     * In your constructor set the global endpoint and configure the folder to scan the controllers.
     * You can start the http and https server.
     */
    constructor() {
        super();

        const appPath: string = Path.resolve(__dirname);
        
        this.setEndpoint("/rest")                       // Declare your endpoint
            .scan(appPath + "/controllers/**/**.js")    // Declare the directory that contains your controllers
            .scan(appPath + "/services/**/**.js")    // Declare the directory that contains your services
            .createHttpServer(8000)
            .createHttpsServer({
                port: 8080
            });

    }
}       
```
In second place, create a new file in your services folder. Create a new Class definition and add the `@Service()` annotation on your class.

Example :
```typescript
@Service()
export default class FooService {
    constructor() {
    
    }
}
```

Finally, inject the service to another service:
```typescript
import FooService from "./FooService";

@Service()
export default class MyService {
    constructor(private fooService: FooService) {
    
    }
}
```
Or to another controller: 

```typescript
import MyService from "./MyService";

@Controller('/rest') 
class MyController {
    constructor(private myService: MyService){
    
    }
}  
```

## Services available

Some services are provided by `ts-express-decorators`. Theses services are follows:

Service Name | Description
--- | ---
[ControllerService](https://github.com/Romakita/ts-express-decorators/wiki/Controllers) | This service contain all controllers defined with `@Controller`. 
[ConverterService](https://github.com/Romakita/ts-express-decorators/wiki/Converters) | This service contain all default and custom converters defined with `@Converter()`.
[ExpressApplication](https://github.com/Romakita/ts-express-decorators/wiki/ExpressApplication) | This service contain the instance of `Express.Application`.
[InjectorService](https://github.com/Romakita/ts-express-decorators/wiki/InjectorService) | This service contain all services collected by `@Service` or services declared manually with `InjectorService.factory()` or `InjectorService.service()`.
[MiddlewareService](https://github.com/Romakita/ts-express-decorators/wiki/Middlewares) | This service contain all default and custom converters defined with `@Middleware()` or `@MiddlewareError()`.
ParseService | Parse an expression and get a data from an Object.
RequestService | Provide methods to get header, bodyParams, queryParams, pathParams or Session data from an expression.
[RouterController](https://github.com/Romakita/ts-express-decorators/wiki/RouterController) | Provide the Express.Router instance of the class declared as `@Controller`.
[ServerSettings](https://github.com/Romakita/ts-express-decorators/wiki/ServerSettings) | Provide the server configuration.


