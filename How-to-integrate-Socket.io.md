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
import {ServerLoader, ServerSettings, Inject} from "ts-express-decorators";
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
import {Service} from "ts-express-decorators";

@Service()
export default class SocketService {
      private io: SocketIO.Server;
      private stacks = [];

      constructor() {
          
      }
      /**
       * Store all callbacks that will be adding to socket.io instance.
       */
      public onConnection(callback: Function): SocketService {
          this.stacks.push(callback);
          return this;
      }

      public emit(...args) => this.io.emit(...args);

      createServer(httpServer: Http.Server)  {
           this.io = SocketIO(httpServer);

           // Map all callbacks to this connection events.
           this.stacks.forEach(cb => this.io.on('connection', cb));
      }
}
```

Finally inject your service to another service or controller : 

```typescript
import {Controller} from "ts-express-decorators";
import SocketService from "../services/SocketService";

@Controller('/')
export class MySocketCtrl {
    
   constructor(private socketService: SocketService) {
       socketService.onConnection(this.onConnection);
   }
   
   private onConnection = (socket) => {
      // write your code :)
   }
}
```
