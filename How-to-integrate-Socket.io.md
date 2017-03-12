[Home](https://github.com/Romakita/ts-express-decorators/wiki) > [Example](https://github.com/Romakita/ts-express-decorators/wiki/Example) > Socket.io

## Installation
Run this command:
```typescript
npm install --save socket.io
```
If you want type checking for Socket.io, run this command:
```typescript
npm install --save-dev @types/socket.io
```

## Example 

Use the `$onReady` hook to create your Socket server:
```typescript
import {ServerLoader, ServerSettings, Inject} 
import SocketService from "./services/SocketService";

class Server extends ServerLoader {

    @Inject()
    public $onReady(socketService: SocketService): void {
        
          socketService.createServer(this.httpServer);

    }
}
```
Wrap socket.io into the a Service:
```typescript
import * as Http from "http";

@Service()
export default class SocketService {
      private io: SocketIO.Server;

      constructor() {
          
      }

      public emit(...args) => SocketService.io.emit(...args);

      static createServer(httpServer: Http.Server)  {
           SocketService.io = SocketIO(httpServer);
           // Then create your socket app...
      }
}
```