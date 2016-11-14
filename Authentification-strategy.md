The annotation `@Authentification` use a `ServerLoader.isAuthenticated()` method to check the authentification strategy.
You can configure this method by adding an `isAuthenticated()` method on your `Server` class.

```typescript
import * as Express from "express";
import {ServerLoader} from "ts-express-decorators";
import Path = require("path");

export class Server extends ServerLoader {
    //...
    /**
     * Set here your authentification strategy.
     * @param request
     * @param response
     * @param next
     * @returns {boolean}
     */
    public isAuthenticated(request: Express.Request, response: Express.Response, next: Express.NextFunction): boolean {

        return true;
    }
}
```