[Home](https://github.com/Romakita/ts-express-decorators/wiki) > Testing

> Testing module is in version alpha! Certain functionality are likely to change in the future!

This section help you to test your service. It's highly recommended to test your application with Mocha + Chai, but you can use Jasmine or other unit test framework.

## Sections

* [Testing services]()
* [Testing controllers]()
* [Testing converters]()
* [Testing middlewares]()

## Testing services

TsExpressDecorators are bundled with a testing module `ts-express-decorators/testing`. This module provide a function `inject()` to inject your services collected via annotation `@Service()`.

Example of unit test for the `ParseService`:

```typescript
import {expect} from "chai";
import {inject} from "ts-express-decorators/testing";
import ParseService from "ts-express-decorators";

describe('ParseService :', () => {

    it('should clone object', () => {

        const source = {};

        expect(ParseService.clone(source)).not.to.be.equal(source);

    });

    it('should evaluate expression with a scope and return value', inject([ParseService], (parserService: ParseService) => {

        expect(parserService.eval("test", {
            test: "yes"
        })).to.equal("yes");

    }));
});
```

Testing asynchronous method is also possible with `Done` function:

```typescript
import {expect} from "chai";
import {inject} from "ts-express-decorators/testing";
import DbService from '../src/services/db';

describe('DbService :', () => {

    it('should data from db', inject([DbService, Done], (dbService: dbService, done: Done) => {
        
        dbService
           .getData()
           .then((data) => {
               expect(data).to.be.an('object');
               done();
           });

    }));
});
```

## Testing converters

`Converters` let you to customize how [ConverterService](https://github.com/Romakita/ts-express-decorators/wiki/Converters) will deserialize a data for one or more types. This example show you the unit testing for the Array type. 

The converter implementation in:
```typescript
import ConverterService from "../services/converter";
import {Converter} from "../decorators/converter";
import {IConverter} from "../interfaces/Converter";
import {isArrayOrArrayClass} from "../utils/utils";

@Converter(Array)
export class ArrayConverter implements IConverter {

    constructor(private converterService: ConverterService) {}

    deserialize<T>(data: any, target: any, baseType?: T): T[] {

        if (isArrayOrArrayClass(data)) {
            return (data as Array<any>).map(item =>
                this.converterService.deserialize(item, baseType)
            );
        }

        return [data];
    }

    serialize(data: any[]) {
        return (data as Array<any>).map(item =>
            this.converterService.serialize(item)
        );
    }
}

```
And the unit testing:
```typescript
import {expect} from "chai";
import {inject} from 'ts-express-decorators/testing';
import {ConverterService} from "ts-express-decorators/src";

describe("ArrayConverter :", () => {

    it('should convert data', inject([ConverterService], (converterService: ConverterService) => {

        const arrayConverter = converterService.getConverter(Array);

        expect(!!arrayConverter).to.be.true;

        expect(arrayConverter.deserialize(1)).to.be.an('array');
        expect(arrayConverter.deserialize([1])).to.be.an('array');

    }));

});
```

