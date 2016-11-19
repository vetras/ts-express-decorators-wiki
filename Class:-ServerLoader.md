ServerLoader provider all method to instanciate an ExpressServer. ServerLoader can't be instanciated directly. You must create your own Server class inherited the ServerLoader class.

It provide some features :

 * Middleware settings,
 * ComponentScan: import all controllers or services under a folder,
 * GlobalErrorHandler: Error management (GlobalErrorHandler),
 * Authentification strategy.

## API
### new ServerLoader()

Create new instance of ServerLoader. `ServerLoader` patch the [http.IncomingMessage](https://nodejs.org/api/http.html#http_class_http_incomingmessage) to add the method `$tryAuth`.
> For this reason, online one instance of ServerLoader can be constructed.

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