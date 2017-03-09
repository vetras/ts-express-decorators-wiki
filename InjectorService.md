[Home](https://github.com/Romakita/ts-express-decorators/wiki) > [API References](https://github.com/Romakita/ts-express-decorators/wiki/API-references) > InjectorService

> This service is available since v1.3.x

This service contain all services collected by `@Service` or services declared manually with `InjectorService.factory()` or `InjectorService.service()`.

## Methods

#### `InjectorService.invoke<T>(target [, locals [, designParamTypes]]): T`

Invoke the class and inject all services that required by the class constructor.

**Parameters**

Param | Type | Details
---|---|---
target | `string|number` | The injectable class to invoke. Class parameters are injected according constructor signature.
locals | `Map<Function, any>` | Optional object. If preset then any argument Class are read from this object first, before the `InjectorService` is consulted.
designParamTypes | `any[]` | Optional object. List of injectable types.

**Return**

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

#### `InjectorService.invokeMethod(method, options): any`

Invoke a class method and inject service.

**Parameters**

Param | Type | Details
---|---|---
method | `*` | The injectable method to invoke. Method parameters are injected according method signature.
options | `IInjectableMethod | any[]` | Object to configure the invocation. 

IInjectableMethod options:

 * **target**: Optional. The class instance.
 * **methodName**: `string` Optional. The method name.
 * **designParamTypes**: `any[]` Optional. List of injectable types.
 * **locals**: `Map<Function, any>` Optional. If preset then any argument Class are read from this object first, before the `InjectorService` is consulted. 

**Return**

The returned result by the method.

**Example**
```typescript
import {InjectorService} from "ts-express-decorators";

class MyService {
   constructor(injectorService: InjectorService) {

      injectorService.invokeMethod(this.method, {
          this,
          methodName: 'method'
      });
   } 
   
   method(otherService: OtherService) {}
}
```
***

#### `InjectorService.get<T>(target): T`

Get a service already constructed.

**Parameters**

Param | Type | Details
---|---|---
target | `*` | The service class.

**Return**

The class already constructed

**Example**
```typescript
import {InjectorService} from "ts-express-decorators";
import MyService from "./services";

class OtherService {
   constructor(injectorService: InjectorService) {

      const myService = injectorService.get<MyService>(MyService);

   }
}
```

***

#### `InjectorService.has(target): boolean`

Check if the service exists in `InjectorService`.

**Parameters**

Param | Type | Details
---|---|---
target | `*` | The service class.

**Example**
```typescript
import {InjectorService} from "ts-express-decorators";
import MyService from "./services";

class OtherService {
   constructor(injectorService: InjectorService) {

      const exists = injectorService.has(MyService); // true or false

   }
}
```
