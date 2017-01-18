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

* `@Response()`: Express.Response service.
* `@RequestService()`: Express.Request service.
* `@Next()`: Express.NextFunction service.
* `@PathParams(expression?: string)`: Get a parameters on `Express.Request.params` attribut.
* `@BodyParams(expression?: string)`: Get a parameters on `Express.Request.body` attribut.
* `@CookiesParams(expression?: string)`: Get a parameters on `Express.Request.cookies` attribut.
* `@QueryParams(expression?: string)`: Get a parameters on `Express.Request.query` attribut.
* `@Session(expression?: string)`: Get a parameters on `Express.Request.query` attribut.
* `@Header(expression: string)`: Inject request header parameters.
* `@Required()`: Set a required flag on parameters.