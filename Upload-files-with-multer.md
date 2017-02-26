> Since v1.4.x, TsExpessDecorators support file uploading with Multer

### Installation

Before using the `@MultipartFile()` you must install [multer](https://github.com/expressjs/multer) module on your project:
```bash
npm install multer --save
```
You can install multer's file definitions via npm:
```bash
npm install @types/multer --save-dev
```
### Configure the File upload directory

By default the directory used is `${projetRoot}/uploads`. You can configure another directory on your `ServerLoader` settings.

```typescript
const appPath = Path.resolve(__dirname);

export class Server extends ServerLoader implements IServerLifecycle {

    constructor() {
        super();

        this.settings.uploadDir = `${appPath}/custom-dir`;

        this
            .mount('/rest', `${appPath}/controllers/**/**.js`)
            .createHttpServer(8000)
            .createHttpsServer({
                port: 8080
            });

    }  
}
```

### Example 
TsExpressDecorators use multer to handler file uploads. Single file can be injected like this:

```typescript
@Controller('/')
class MyCtrl {
     @Post('/file')
     private uploadFile(@MultipartFile() file: Multer.File) {

     }
}
```
For multiple files, just add Array type annotation like this:
```typescript
@Controller('/')
class MyCtrl {
  @Post('/files')
     private uploadFile(@MultipartFile() files: Multer.File[]) {

     }
}
```
