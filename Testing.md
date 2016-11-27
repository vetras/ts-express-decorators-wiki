> Testing module is in version alpha! Certain functionality are likely to change in the future!

This section help you to test your service. It's highly recommended to test your application with Mocha + Chai, but you can use Jasmine or other unit test framework.

## Configuration
In your `tsconfig.json`, you must add `paths` settings:

```typescript
{
  "compilerOptions": {
    "baseUrl":".",
    "target": "es5",
    "lib": ["es6", "dom"],
    "types": [
      "reflect-metadata",
      "mocha",
      "chai"
    ],
    "module": "commonjs",
    "moduleResolution": "node",
    "experimentalDecorators":true,
    "emitDecoratorMetadata": true,
    "sourceMap": true,
    "declaration": false,
    "paths":{
      "ts-express-decorators/testing": [
        "node_modules/ts-express-decorators/dts/testing",
        "node_modules/ts-express-decorators/lib/testing"
      ]
    }
  },
  "exclude": [
    "node_modules"
  ]
}
```
> Theses lines enable Typescript to use testing as a typescript module from another directory.

## Testing services

TsExpressDecorators are bundled with a testing module `ts-express-decorators/testing`. This module provide a function `inject()` to inject your services collected via annotation `@Service()`.

Example of unit test for the `ParseService`:

```typescript
import {expect} from "chai";
import {inject} from "ts-express-decorators/testing";
import ParseService from './parse';

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

Testing asynchrone method is also possible with `Done` function:

```typescript
import {expect} from "chai";
import {inject, Done} from "ts-express-decorators/testing";
import DbService from '../src/services/db';

describe('DbService :', () => {

    it('should data from db', inject([DbService, Done], (dbService: dbService, done) => {
        
        dbService
           .getData()
           .then((data) => {
               expect(data).to.be.an('object');
               done();
           });

    }));
});
```

