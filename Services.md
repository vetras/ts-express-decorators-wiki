> Since v1.1.0, TsExpessDecorators support the service injection (IOC). 

The decorator `@Service()` declare a new service can be injected in other service or controller on there `constructor()`.
All services annotated with `@Service()` are constructed one time.

`@Service()` use the `reflect-metadata` to collect and inject service on controllers or other services. 

### Settings Server

In first place, you must adding the `services` folder in your server settings like here :
 
```typescript
import * as Express from "express";
import {ServerLoader} from "ts-express-decorators";
import Path = require("path");

export class Server extends ServerLoader {
    /**
     * In your constructor set the global endpoint and configure the folder to scan the controllers.
     * You can start the http and https server.
     */
    constructor() {
        super();

        let appPath: string = Path.resolve(__dirname);
        
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
