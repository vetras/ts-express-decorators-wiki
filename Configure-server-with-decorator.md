> Since v1.4.x you can use the decorator @ServerSettings to configure the server.

```typescript
import * as Express from "express";
import {ServerLoader, ServerSettings} from "ts-express-decorators";
import Path = require("path");

@ServerSettings({
   rootDir: Path.resolve(__dirname)
})
export class Server extends ServerLoader {
    static Initialize = (): Promise<any> => new Server().start();
}

Server.Initialize();
```