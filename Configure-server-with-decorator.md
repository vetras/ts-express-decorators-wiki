[Home](https://github.com/Romakita/ts-express-decorators/wiki) > [ServerLoader](https://github.com/Romakita/ts-express-decorators/wiki/Class:-ServerLoader) > Configure server with decorator

> Since v1.4.x you can use the decorator `@ServerSettings` to configure the server.

`@ServerSettings` let you to configure quickly your server via decorator. This decorator take your configuration and merge it with the default server configuration.

The default configuration is as follow:
```json
{
  "rootDir": "path/to/root/project",
  "env": "development",
  "port": 8080,
  "httpsPort": 8000,
  "endpointUrl": "/rest",
  "uploadDir": "${rootDir}/uploads",
  "mount": {
    "/rest": "${rootDir}/controllers/**/*.js"
  },
  "componentsScan": [
    "${rootDir}/middlewares/**/*.js",
    "${rootDir}/services/**/*.js",
    "${rootDir}/converters/**/*.js"
  ]
}
```

You can customize your configuration as follow:
```typescript
import * as Express from "express";
import {ServerLoader, ServerSettings} from "ts-express-decorators";
import Path = require("path");

@ServerSettings({
   rootDir: Path.resolve(__dirname),
   mount: {
     "/rest": "${rootDir}/controllers/current/**/*.js",
     "/rest/v1": "${rootDir}/controllers/v1/**/*.js"
   }
})
export class Server extends ServerLoader {
    static Initialize = (): Promise<any> => new Server().start();
}

Server.Initialize();
```