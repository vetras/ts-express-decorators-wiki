> This document is available only for `TsExpressDecorators` v1.2.x on higher.

ServerLoader provider all method to instantiate an ExpressServer. ServerLoader can't be instantiated directly. You must create your own Server class inherited the ServerLoader class.

It let you manage lifecycle server like middlewares configuration, authentification strategy or global error interception (see [Lifecycle](#lifecycle)).

## API
### new ServerLoader()

Create new instance of ServerLoader. `ServerLoader` patch the [http.IncomingMessage](https://nodejs.org/api/http.html#http_class_http_incomingmessage) to add the method `$tryAuth`.
> For this reason, online one instance of ServerLoader can be constructed.

Then configure all folders that you want import in your `Server` with [`Server.scan()`](#serverloaderscanglobpattern-serverloader) method.

Example:
```typescript
import {ServerLoader, Server, IServerLifecycle} from "ts-express-decorators";
import Path = require("path");

export class Server extends ServerLoader implements IServerLifecycle {

    constructor() {
        super();

        let appPath = Path.resolve(__dirname);
        
        this.setEndpoint('/rest')
            .scan(appPath + "/controllers/**/**.js")
            .createHttpServer(8000)
            .createHttpsServer({
                port: 8080
            });

    }

    static Initialize(): Bluebird<any> {
     
        return new Server()
            .start()
            .then(() => {
                console.log('Server started...');
            });
    }
}
```

> `IServerLifecycle` provide interface to implement quickly the right methods to hook [lifecycle's phase](#lifecycle). 

#### ServerLoader.createHttpServer(port): ServerLoader
**port**: `string|number`

Create a new HTTP server with the provided `port`.

#### ServerLoader.createHttpsServer(httpsOptions): ServerLoader
**httpsOptions**: `IHTTPSServerOptions`

Create a new HTTPs server.

`httpsOptions` <IHTTPSServerOptions>:

* `port` &lt;number&gt;: Port number,
* `key` &lt;string&gt; | &lt;string[]&gt; | [&lt;Buffer&gt;](https://nodejs.org/api/buffer.html#buffer_class_buffer) | &lt;Object[]&gt;: The private key of the server in PEM format. To support multiple keys using different algorithms an array can be provided either as a plain array of key strings or an array of objects in the format `{pem: key, passphrase: passphrase}`. This option is required for ciphers that make use of private keys.
* `passphrase` &lt;string&gt; A string containing the passphrase for the private key or pfx.
* `cert` &lt;string&gt; | &lt;string[]&gt; | [&lt;Buffer&gt;](https://nodejs.org/api/buffer.html#buffer_class_buffer) | [&lt;Buffer[]&gt;](https://nodejs.org/api/buffer.html#buffer_class_buffer): A string, Buffer, array of strings, or array of Buffers containing the certificate key of the server in PEM format. (Required)
* `ca` &lt;string&gt; | &lt;string[]&gt; | [&lt;Buffer&gt;](https://nodejs.org/api/buffer.html#buffer_class_buffer) | [&lt;Buffer[]&gt;](https://nodejs.org/api/buffer.html#buffer_class_buffer): A string, Buffer, array of strings, or array of Buffers of trusted certificates in PEM format. If this is omitted several well known "root" CAs (like VeriSign) will be used. These are used to authorize connections.

See more info on [httpsOptions](https://nodejs.org/api/tls.html#tls_tls_createserver_options_secureconnectionlistener).

#### get ServerLoader.expressApp: [Express.Application](http://expressjs.com/fr/4x/api.html#app)

Return the current instance of [Express.Application](http://expressjs.com/fr/4x/api.html#app).

#### ServerLoader.setEndpoint(endpoint): ServerLoader
**endpoint**: `string`

Configure the global endpoint path for all collected controller.

#### ServerLoader.scan(globPattern): ServerLoader
**globPattern**: `string`

Scan and imports all files matching the pattern. See the document on the [Glob pattern](https://www.npmjs.com/package/glob) for more information.

Example:
```typescript
import {ServerLoader} from "ts-express-decorators";
import Path = require("path");

export class Server extends ServerLoader {

    constructor() {
        super();

        let appPath = Path.resolve(__dirname);
        
        this.scan(appPath + "/controllers/**/**.js")
            .scan(appPath + "/services/**/**.js")

    }
}
```
Theses pattern scan all files in the directories `controllers`, `services` recursively.

#### ServerLoader.start(): Promise
Start the express server.

### Lifecycle
ServerLoader lifecycle let you intercept a phase.

Lifecycle phases:

* `$onMountingMiddlewares`: Configure all express middleware on this phase,
* `$onReady`: Called when httpServer and/or httpsServer are ready,
* `$onAuth`: Called when an Endpoint require an authentification strategy before access to the endpoint method,
* `$onError`: Called when an error is intercepted by Express or TsExpressDecorators.

#### ServerLoader.$onMountingMiddlewares(): void

Some middlewares are required to work with all decorators as follow:

* `cookie-parser` are required to use `@CookieParams`,
* `body-parser` are require to use `@BodyParams`,
* [`method-override`](https://github.com/expressjs/method-override)

Example of middlewares configuration:
```typescript
class Server extends ServerLoader implements IServerLifecycle {

    public $onMountingMiddlewares():  {

        let morgan = require('morgan'),
            cookieParser = require('cookie-parser'),
            bodyParser = require('body-parser'),
            compress = require('compression'),
            methodOverride = require('method-override'),
            session = require('express-session');

        this
            .use(morgan('dev'))
            .use(ServerLoader.AcceptMime("application/json"))
            .use(bodyParser.json())
            .use(bodyParser.urlencoded({
                extended: true
            }))
            .use(cookieParser())
            .use(compress({}))
            .use(methodOverride());

        return this;
    }
}
```

#### ServerLoader.$onReady(): void

#### ServerLoader.$onAuth(request, response, next): void

#### ServerLoader.$onError(error, request, response, next): void







