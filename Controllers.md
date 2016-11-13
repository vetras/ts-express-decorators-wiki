> Since v1.1.0 a controller are instanciated for each incoming request.

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
