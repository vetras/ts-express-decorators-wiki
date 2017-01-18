You can use [ts-httpexceptions](https://github.com/Romakita/ts-httpexceptions) or similar module to throw an http exception.
All exception are intercepted by the Global errors handler and are sent to the client browser.

Here an example:
```typescript
import {Controller, Get, PathParams} from "ts-express-decorators";
import {BadRequest} from "ts-httpexceptions";
import * as Express from "express";

@Controller("/calendars")
export class CalendarCtrl {

    @Get("/:id")
    public get(
        @PathParams("id") id: number
    ): any {
    
        if (isNaN(+id)) {
            throw(new BadRequest("Not a number"));
        }
       
       return {id: id};
    }
}
```
If `id` parameter is not an number, the method throw a Bad Request. This will produce a response with status code 400 and "Not a number" message.
