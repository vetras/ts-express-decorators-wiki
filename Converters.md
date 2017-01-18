> Since v1.3.0, TsExpessDecorators support the serialization/deserialization.

The decorator `@JsonProperty` let you to customize serialization and deserialization property when `ts-express-decorators` send data as JSON.

All following decorators use `@JsonProperty` metadata to deserialize a Plain Object JavaScript to his Model:

`@PathParams(expression?: string)`, `@BodyParams(expression?: string)`, `@CookiesParams(expression?: string)`, `@QueryParams(expression?: string)`, `@Session(expression?: string)`.

## Example
### Define a model:

`@JsonProperty()` let you decorate an attribut. By default, no parameters are required to use it. But in some cases, we need to configure explicitly the JSON attribut name mapped to the class attribut. Here an example of different use cases with `@JsonProperty()`:

```typescript
class EventModel {

    @JsonProperty()
    name: string;
     
    @JsonProperty('startDate')
    startDate: Date;

    @JsonProperty({name: 'end-date'})
    endDate: Date;

    @JsonProperty({use: Task})
    tasks: TaskModel[];
}

class TaskModel {
     subject: string;
     rate: number;
}
```
> Theses ES6 collections can be used : Map and Set.
> Map will be serialized as an object and Set as an array.
> By default `Date`, `Array`, `Map` and `Set` have a default custom `Converter` allready embded. But you can override theses (see next part).

For the `Array`, you must  add the '{use: baseType}' option to the decorator. TypeClass will be used to deserialize each item in the collection stored on the attribut source.

### Use your model on a Controller:

```typescript
@Controller("/")
export class EventCtrl {

     @Post("/")
     save(@BodyParams() event: EventModel){
          console.log(event instanceof EventModel); // true
          return event; // event will be serialized according to your annotation on EventModel class.
     } 

     //OR
     @Post("/")
     save(@BodyParams('event') event: EventModel) {
          console.log(event instanceof EventModel); // true
          return event; // event will be serialized according to your annotation on EventModel class.
     }
}
```
## Customize serialization/deserialization
### With @Converter

`@Converter(...targetTypes)` let you to define some converters for a certain type/Classe. It usefull for a generic conversion.

#### Simple type
Here an example to create a custom converter for the Date type:

```typescript
import {IConverter, Converter} from "ts-express-decorators";

@Converter(Date)
export class DateConverter implements IConverter {

    // Called when a date string will be deserialized from POJ
    deserialize(data: string): Date {
        return new Date(data);
    }
 
    // Called when a Date object will be serialized to POJ
    serialize(object: Date): any {
        return object.toISOString();
    }
}
```

#### Collection (Array, Map, Set)
 
For a collection, the converter is a little more complex because we need to know the base type to use when we deserialize an `Array`, `Map` or `Set` from Plain Object JavaScript. To do that, the deserialize method have two more parameters:

* `target` parameter is equal to Array, the type given to the `@Converter(Array)`. `target` parameter isn't necessary if we have only one `targetType` for a class Converter (example: `@Converter(Array, Set, Map)`). 
* `baseType` is the type given to `@JsonProperty({use: baseType})`. It's this type that will be used.

##### Array converter

```typescript
import {ConverterService, Converter, IConverter} from "ts-express-decorators";

@Converter(Array)
export class ArrayConverter implements IConverter {

    constructor(private converterService: ConverterService) {}

    deserialize<T>(data: any[], target: any, baseType?: T): T[] {
        return (data as Array<any>).map(item =>
            this.converterService.deserialize(item, baseType)
        );
    }

    serialize(data: any[]) {
        return (data as Array<any>).map(item =>
            this.converterService.serialize(item)
        );
    }
}
```

> In this example, we use the `ConverterService` to delegate the deserialization for each item. But you can implement your own deserialization/serialization strategy.

##### Map converter
```typescript
import {ConverterService, Converter, IConverter} from "ts-express-decorators";

@Converter(Map)
export class MapConverter implements IConverter {
    constructor(private converterService: ConverterService) {}

    deserialize<T>(data: any, target: any, baseType: T): Map<string, T> {

        const obj = new Map<string, T>();

        Object.keys(data).forEach(key  => {

            obj.set(key, <T>this.converterService.deserialize(data[key], baseType));

        });

        return obj;
    }

    serialize<T>(data: Map<string, T>): any {
        const obj = {};

        data.forEach((value, key) =>
            obj[key] = this.converterService.serialize(value)
        );

        return obj;
    }
}
```

> In this example, we use the `ConverterService` to delegate the deserialization for each item. But you can implement your own deserialization/serialization strategy.

##### Set converter

```typescript
import {ConverterService, Converter, IConverter} from "ts-express-decorators";

@Converter(Set)
export class SetConverter implements IConverter {
    constructor(private converterService: ConverterService) {}

    deserialize<T>(data: any, target: any, baseType: T): Set<T> {
        const obj = new Set<T>();

        Object.keys(data).forEach(key => {

            obj.add(<T>this.converterService.deserialize(data[key], baseType));

        });

        return obj;

    }

    serialize<T>(data: Set<T>): any[] {
        const array = [];

        data.forEach((value) =>
            array.push(this.converterService.serialize(value))
        );

        return array;
    }
}
```

> In this example, we use the `ConverterService` to delegate the deserialization for each item. But you can implement your own deserialization/serialization strategy.
