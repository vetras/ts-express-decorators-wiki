## Decorators
### Class decorators
Signature | Example | Description
--- | --- | ---
[`@Controller(route: string, ...ctrlsNamesDependencies?: string[])`](https://github.com/Romakita/ts-express-decorators/wiki/Controllers) | `@Controller('/rest/calendars') class MyController` | Declare a new controller with his Rest path. His methods annotated will be collected to build the routing list. This routing listing will be built with the `express.Router` object.
[`@Service()`](https://github.com/Romakita/ts-express-decorators/wiki/Services) |  `@Service() class Service` | Declare a new Service that can be injected in other `Service` or `Controller`.
[`@Converter(...targetTypes: any[])`](https://github.com/Romakita/ts-express-decorators/wiki/Converters) | `@Service() class Service` Declare a new serializer/deserializer when a class/type is deserialized from JSON and vice versa.

### Method decorators

Signature | Example | Description | Express analogue
--- | --- | --- | ---
`@All(route)` | `@All('/calendars') all()` | Intercept all request for a given route. | `router.all('/calendars', all)`
`@Get(route)` | `@Get('/calendars') get()` | Intercept request with GET Http verb for a given route. | `router.get('/calendars', get)`
`@Post(route)` | `@Post('/calendars') post()` | Intercept request with POST Http verb for a given route. | `router.post('/user', post)`
`@Put(route)` | `@Put('/calendars') put()` | Intercept request with PUT Http verb for a given route. | `router.put('/calendars', put)`
`@Delete(route)` | `@Delete('/calendars') delete()` | Intercept request with DELETE Http verb for a given route. | `router.delete('/calendars', delete)`
`@Head(route)` | `@Head('/calendars') head()` | Intercept request with HEAD Http verb for a given route. | `router.head('/calendars', head)`
`@Patch(route)` | `@Patch('/calendars') patch()` | Intercept request with PATCH Http verb for a given route. | `router.patch('/calendars', patch)`
`@Use(...middlewares: any[])` | `@Use(middleware) method()` |  Set a custom middleware or custom Http method. | `router.use(middleware, method)` 
`@Authenticated(options?)` | `@Autenthicated() @Get('/calendars') get()` | Call the `Server.isAuthenticated` method to check if the user is authenticated. | `router.get('/calendars', authenticatedMiddleware, get)`
`@ResponseView(viewPath: string, options: any)` | `@get('/calendars') @ResponseView('calendars.html') patch()` | Render `viewPath` file using the method return data.

### Parameter Decorators
Signature | Example | Description | Express analogue
--- | --- | --- | ---
`@Request()` | `get(@Request() request: Request) {}` | Inject the `Express.Request` service. | `function(request, response) {}`
`@Response()` | `get(@Response() response: Response) {}` | Inject the `Express.Response` service. | `function(request, response) {}`
`@Next()` | `get(@Next() response: NextFunction) {}` | Inject the `Express.NextFunction` service. | `function(request, response, next) {}`
`@PathParams(expression?: string, useClass?: any)` | `get(@PathParam("id") id: string) {}` |  Get a parameters on `Express.request.params` attribut. | `request.params.id`
`@BodyParams(expression?: string, useClass?: any)` | `get(@BodyParam() calendar: CalendarModel) {}` | Inject a parameters on `Express.request.body` attribut. | `request.body`
`@CookiesParams(expression?: string, useClass?: any)` | `get(@CookiesParams("cook") cook: string) {}` | Get a parameters on `Express.request.cookies` attribut. | `request.cookies.cook`
`@QueryParams(expression?: string, useClass?: any)` | `get(@QueryParams("id") id: string) {}` | Get a parameters on `Express.request.query` attribut. | `request.query.id`
`@Session(expression?: string, useClass?: any)` | `get(@Session() context: Context) {}` | Get a parameters on `Express.request.session` attribut. | `request.session`
`@Header(key: string)` | `get(@Header("x-token") token: string) {}` | Inject request header parameters. | `request.get('x-token')`
`@Required()` | `get(@QueryParams("id") @Required() id: string) {} | Set a required flag on a parameter. | 

> Note : `useClass` parameters is only required if you want to deserialize a collection type. (See [converters page](https://github.com/Romakita/ts-express-decorators/wiki/converters)).

## Services

Some services are provided by `ts-express-decorators`. Theses services are follows:

Service Name | Description
--- | ---
[ConverterService](https://github.com/Romakita/ts-express-decorators/wiki/Converters) | This service contain all default and custom converters defined with `@Converter()`.
RequestService | Provide methods to get header, bodyParams, queryParams, pathParams or Session data from an expression.
ParseService | Parse an expression and get a data from an Object.