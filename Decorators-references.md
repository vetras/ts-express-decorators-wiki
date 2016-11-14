### Class decorators

* `@Controller(route: string, ...ctrlsNamesDepedencies?: string[])` : Declare a new controller with his Rest path. 
* `@Service()` : Declare a new Service that can be injected in other Service or Controller.

### Method decorators

* `@All(route)`: Intercept all request for a given route.
* `@Get(route)`: Intercept request with GET http verb for a given route.
* `@Post(route)`: Intercept request with POST http verb for a given route.
* `@Put(route)`: Intercept request with PUT http verb for a given route.
* `@Delete(route)`: Intercept request with DELETE http verb for a given route.
* `@Head(route)`: Intercept request with HEAD http verb for a given route.
* `@Patch(route)`: Intercept request with PATCH http verb for a given route.
* `@Authenticated()`: Call the `Server.isAuthenticated` method to check if the user is authenticated.
* `@Use(...middlewares: any[])`: Set a custom middleware.

### Parameter Decorators

* `@Response()`: Express.Response service.
* `@RequestService()`: Express.Request service.
* `@Next()`: Express.NextFunction service.
* `@PathParams(expression: string)`: Get a parameters on Express.Request.params attribut.
* `@BodyParams(expression: string)`: Get a parameters on Express.Request.body attribut.
* `@CookiesParams(expression: string)`: Get a parameters on Express.Request.cookies attribut.
* `@QueryParams(expression: string)`: Get a parameters on Express.Request.query attribut.
* `@Required()`: Set a required flag on parameters.