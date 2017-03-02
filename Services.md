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
import {ServerLoader} from "ts-express-decorators";
import Path = require("path");
const rootDir = Path.resolve(__dirname);

@ServerSettings({
   rootDir,
   mount: {
      '/rest': `${rootDir}/controllers/**/**.js`
   },
   componentsScan: [
       `${rootDir}/services/**/**.js`
   ]
})
export class Server extends ServerLoader {
   
   $onInit() { // Injector isn't initialized at this step.
      MyService.configure(...);
   }
   
   @Inject()
   $onMountingMiddlewares(myService: MyService) {
       this.use(myService.getMiddleware())
   }
   
   @Inject()
   $onReady(myService: MyService) {
       
   }
}       
```
In second place, create a new file in your services folder. Create a new Class definition and add the `@Service()` annotation on your class.

Example :
```typescript
@Service()
export default class MyService {
    constructor() {
    
    }
    
    static configure(options: any) {
       
    }

    public getMiddleware() {
       return (request, response, next) => next();    
    }
}
```

Finally, inject the service to another service:
```typescript
import MyService from "./MyService";

@Service()
export default class FooService {
    constructor(private myService: myService) {
    
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

## Factory - Add a service already constructed 
> This feature is available since v1.4.0

This example show you how you can add a service already constructed like a npm module.

```typescript
// MyFooFactory.ts
import {InjectorService} from "ts-express-decorators";

export interface IMyFooFactory {
   getFoo(): string;
}

export type MyFooFactory = IMyFooFactory;
export const MyFooFactory = Symbol("MyFooFactory");

InjectorService.factory(MyFooFactory, {
     getFoo:  () => "test"
});
```

```typescript
// otherservice.ts
import {MyFooFactory} from "./FooFactory.ts";

@Service()
export default class OtherService {
     constructor(@Inject(MyFooFactory) myFooFactory: MyFooFactory){
           console.log(myFooFactory.getFoo()); /// "test"
     }
}
```
> Note: TypeScript transform and store `MyFooFactory` as `Function` type in the metadata. So to inject a Factory, you must use the `@Inject(type)` decorator.

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


