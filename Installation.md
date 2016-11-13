You can get the latest release using npm:

```batch
$ npm install --save ts-express-decorators express@4
```

> **Important!** TsExpressDecorators requires Node >= 4, Express >= 4, TypeScript >= 2.0 and 
the `experimentalDecorators`, `emitDecoratorMetadata`, `types` and `lib` compilation 
options in your `tsconfig.json` file.

```json
{
  "compilerOptions": {
    "target": "es5",
    "lib": ["es6", "dom"],
    "types": ["reflect-metadata"],
    "module": "commonjs",
    "moduleResolution": "node",
    "experimentalDecorators":true,
    "emitDecoratorMetadata": true,
    "sourceMap": true,
    "declaration": false
  },
  "exclude": [
    "node_modules"
  ]
}
```

> **Note** : target can be set to es2015/ES6. You can use pure es6 API with `ts-express-decorators/es6`.
