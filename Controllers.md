[Home](https://github.com/Romakita/ts-express-decorators/wiki) > Controllers

> Since v1.1.0 a controller are instanciated for each incoming request.

## Sections

* [Response and Request](#response-and-request)
* [PathParams, BodyParams, QueryParams](#pathparams-bodyparams-queryparams)
* [Header](#header)
* [Use promise](#use-promise)
* [Custom middleware](#custom-middleware)
* [Controller dependencies](#controller-dependencies)

### Response and Request

You can use decorator to inject `Express.RequestService`, `Express.Response` and 
`Express.NextFunction` services instead of the classic call provided by Express API.
Just use decorator `RequestService`, `Response` and `Next` on your method parameters like this :

```typescript
import {Controller, Get, Response, Request, Next} from "ts-express-decorators";
import * as Express from "express";

interface ICalendar{
    id: string;
    name: string;
}

@Controller("/calendars")
export class CalendarCtrl {

    @Get("/:id")
    public get(
        @Request() request: Express.Request, 
        @Response() response: Express.Response,
        @Next() next: Express.NextFunction
    ): void {
    
        setTimeout(() => {
            response.send(200, {id: request.params.id, name: "test"});
            next();
        });

    }
}
```

### PathParams, BodyParams, QueryParams

`@PathParams` decorator provide you a quick access to an attribute `Express.request.params`.

```typescript
import {Controller, Get, Response, PathParams} from "ts-express-decorators";
import * as Express from "express";

@Controller("/calendars")
export class CalendarCtrl {

    @Get("/:id")
    public get(
        @PathParams("id") id: number, 
        @Response() response: Express.Response,
        @Next() next: Express.NextFunction
    ): void {
    
        setTimeout(() => {
            response.send(200, {id: id, name: "test"});
            next();
        });

    }
}
```

Same decorator are available to get other params. Use `BodyParams` 
(with the right HTTP verb `@Post`, `@Put`, etc...), `QueryParams` or `CookiesParams` 
to get parameters send by the client. 

### Header

`@Header` decorator provide you a quick access to the `Express.request.get()`

```typescript
import {Controller, Get, Response, PathParams} from "ts-express-decorators";
import * as Express from "express";

@Controller("/calendars")
export class CalendarCtrl {

    @Get("/:id")
    public get(
        @Header("x-token") token: string,
        @PathParams("id") id: number
    ): any {
    
        console.log("token", token);
        return {id: id};
    }
}
```

### Use promise

TsExpressDecorators support Promise API to send a response. Just return a promise
in your method and the controller will be waiting your promised response before 
send a response to the client.

```typescript
import {Controller, Get, Response, Request} from "ts-express-decorators";
import * as Express from "express";

interface ICalendar{
    id: string;
    name: string;
}

@Controller("/calendars")
export class CalendarCtrl {

    @Get("/:id")
    public get(
        @Request() request: Express.Request, 
        @Response() response: Express.Response
    ): Promise<ICalendar> {
    
        return new Promise<ICalendar>((resolve: Function, reject: Function) => {
        
            // you can modify status with response.status(202);

            resolve({
                id: id,
                name: "test"
            });
        });

    }
}

```

### Custom middleware

`@Use()` decoratore let you to add custom middleware on a method. 

```typescript
import {Controller, Get, PathParams} from "ts-express-decorators";
import {BadRequest} from "httpexceptions";
import * as Express from "express";

@Controller("/calendars")
export class CalendarCtrl {

    @Get("/:id")
    @Use(CalendarCtrl.middleware)
    public get(
        @PathParams("id") id: number
    ): any {
      
       
       return {id: id};
    }
    
    static middleware(request, response, next) {
    
        console.log(request.params.id); 
        next();
    }
}
```

**Note** : Middle can't use injectable service like the `CalendarCtrl.get()` method actually. 

### Controller dependencies

A controller can depend to other controllers. Dependencies let you manage each Controller as Express Router module.

```typescript

@Controller("/events")
export class EventCtrl {
 ...
}

@Controller("/calendars", EventCtrl)
export class CalendarCtrl {
 ...
}

@Controller("/rest")
export class RestCtrl{
    constructor(
        routeService: RouteService
    ){
        routeService.printRoutes();
    }
}
```

In this example, CalendarCtrl has EventCtrl as dependencies. When all Controllers are built, the recorded routes will be as follows :

* /rest
* /rest/calendars
* /rest/calendars/events