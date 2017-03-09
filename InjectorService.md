[Home](https://github.com/Romakita/ts-express-decorators/wiki) > [API References](https://github.com/Romakita/ts-express-decorators/wiki/API-references) > InjectorService

> This service is available since v1.3.x

This service contain all services collected by `@Service` or services declared manually with `InjectorService.factory()` or `InjectorService.service()`.

## Methods

#### `InjectorService.invoke<T>(target, locals = new Map<Function, any>(), designParamTypes?: any[]): T`

Invoke the class and inject all services that required by the class constructor.

**Parameters**

Param | Type | Details
---|---|---
target | `string|number` | The injectable class to invoke. Class parameters are injected according constructor signature.
locals | `Map<Function, any>` | Optional object. If preset then any argument Class are read from this object first, before the `InjectorService` is consulted.
designParamTypes | `any[]` | List of injectable types

** Return**
The class constructed.

**Example**
```typescript
import {InjectorService} from "ts-express-decorators";
import MyService from "./services";

class OtherService {
   constructor(injectorService: InjectorService) {
      const myService = injectorService<MyService>(MyService); 
   } 
}
```

***