[Home](https://github.com/Romakita/ts-express-decorators/wiki) > Testing

> Testing module is in version alpha! Certain functionality are likely to change in the future!

This section help you to test your service. It's highly recommended to test your application with Mocha + Chai, but you can use Jasmine or other unit test framework.

## Installation

Install the latest version of Mocha and Chai:

```typescript
npm install --save-dev mocha chai @types/mocha @types/chai
```
Then to use the tools to test your service/controller you need to add these line in your `tsconfig.json`:
```json
{
  "paths":{
    "ts-express-decorators/testing": [
      "node_modules/ts-express-decorators/dts/testing",
      "node_modules/ts-express-decorators/lib/testing"
    ]
  }
}
```

With this configuration, you will be able to use the import of the test module:
```typescript
import {inject} from "ts-express-decorators/testing";
```
## Testing services

TsExpressDecorators are bundled with a testing module `ts-express-decorators/testing`. This module provide a function `inject()` to inject your services collected via annotation `@Service()`.

Example of unit test for the `ParseService`:

```typescript
import {expect} from "chai";
import {inject} from "ts-express-decorators/testing";
import ParseService from "../src/services/parse";

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
import {inject} from "ts-express-decorators/testing";
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

