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
[ConverterService](https://github.com/Romakita/ts-express-decorators/wiki/Converters) | This service contain all default and custom converters defined with `@Converter()`.
RequestService | Provide methods to get header, bodyParams, queryParams, pathParams or Session data from an expression.
ParseService | Parse an expression and get a data from an Object.
RouteService | Get all routes collected by `ts-express-decorators`.


