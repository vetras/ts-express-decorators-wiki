### Class decorators
## Decorators references
### Class decorators

* [`@Controller(route: string, ...ctrlsNamesDependencies?: string[])`](https://github.com/Romakita/ts-express-decorators/wiki/Controllers) : Declare a new controller with his Rest path.
* [`@Service()`](https://github.com/Romakita/ts-express-decorators/wiki/Services) : Declare a new Service that can be injected in other Service or Controller.
* [`@Converter(classOrType: any)`](https://github.com/Romakita/ts-express-decorators/wiki/Converters) : Declare a new serializer/deserializer when a class/type is deserialized from JSON and vice versa.

### Method decorators

* `@All(route)`: Intercept all request for a given route.
* `@Get(route)`: Intercept request with GET Http verb for a given route.
* `@Post(route)`: Intercept request with POST Http verb for a given route.
* `@Put(route)`: Intercept request with PUT Http verb for a given route.
* `@Delete(route)`: Intercept request with DELETE Http verb for a given route.
* `@Head(route)`: Intercept request with HEAD Http verb for a given route.
* `@Patch(route)`: Intercept request with PATCH Http verb for a given route.
* `@Authenticated()`: Call the `Server.isAuthenticated` method to check if the user is authenticated.
* `@Use(...middlewares: any[])`: Set a custom middleware or custom Http method.
* `@ResponseView(viewPath: string, options: any)`: Render viewPath file using the method return data

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