> Since v1.3.0, TsExpessDecorators support the serialization/deserialization.

The decorator `@JsonProperty` let you to customize serialization and deserialization property when `ts-express-decorators` send data as JSON.

All following decorators use `@JsonProperty` metadata to deserialize a Plain Object JavaScript to his Model:

* `@PathParams(expression?: string)`,
* `@BodyParams(expression?: string)`,
* `@CookiesParams(expression?: string)`,
* `@QueryParams(expression?: string)`,
* `@Session(expression?: string)`.

# Example

1. Define a model and add `@JsonProperty` on some attributes:

```typescript
class EventModel {

    @JsonProperty({name: 'Name'})
    name: string;
     
    @JsonProperty()
    startDate: Date;

    @JsonProperty()
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
> By default `Date`, `Array`, `Map` and `Set` have a custom `Converter` (see next part for more information).

2. Use your model on a Controller:
```typescript
@Controller("/")
export class EventCtrl {

     @Post("/")
     save(@BodyParams() event: EventModel){
          console.log(event instanceof EventModel); // true
     } 

     //OR
     @Post("/")
     save(@BodyParams('event') event: EventModel) {
          console.log(event instanceof EventModel); // true
     }
}
```

> Page being written