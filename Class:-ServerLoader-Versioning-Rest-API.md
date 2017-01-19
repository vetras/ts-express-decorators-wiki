[Home](https://github.com/Romakita/ts-express-decorators/wiki) > [ServerLoader](https://github.com/Romakita/ts-express-decorators/wiki/Class:-ServerLoader) > Versioning Rest API

> This feature is available only for TsExpressDecorators v1.3.x on higher.

TsExpressDecorator provide the possibility to mount multiple Rest path instead of the default path `/rest` that is settled with method [`ServerLoader.setEndpoint('/rest')`](https://github.com/Romakita/ts-express-decorators/wiki/Class:-ServerLoader----API#serverloadersetendpointendpoint-serverloader). 
TsExpressDecorators v1.3.x introduce the new method `ServerLoader.mount(endpoint, controllersDir)` and let you to versioning your REST API like this:

```typescript
import * as Express from "express";
import {ServerLoader, IServerLifecycle} from "ts-express-decorators";
import Path = require("path");

export class Server extends ServerLoader implements IServerLifecycle {

    constructor() {
        super();

        const appPath: string = Path.resolve(__dirname);

        this.mount('rest/', appPath + "/controllers/**/**.js") 
            .mount('rest/v1/', appPath + "/v1/controllers/**/**.js") 
            .createHttpServer(8000)
            .createHttpsServer({
                port: 8080
            });

    }
}
```