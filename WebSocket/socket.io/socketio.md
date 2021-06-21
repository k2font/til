# socket.io
### 概要
- [WebSocket](/WebSocket/whats-websocket.md)がJavaScript/Node.jsで簡単に利用できるライブラリ
- サーバーサイド専用のsocket.ioとクライアントサイド専用のsocket.io-clientがある
  - https://github.com/socketio/socket.io
  - https://github.com/socketio/socket.io-client
- クライアントからサーバへWebSocket接続が可能か試み、出来ないようであればHTTPロングポーリングに変更する賢いライブラリ。

- socket.ioを使うと、クライアントサイドにおける以下の生WebSocketコードが...

```javascript
let WebSocket = require('ws');
const socket = new WebSocket('wss://...'); // WebSocket接続を試みる

socket.onopen(() => {
  socket.send('hello!!!!');
});

socket.onmessage((data) => {
  console.log(data);
});
```

以下のように読みやすくになる。  
(※要CDNによるsocket.io読み込み `<script src="/socket.io/socket.io.js"></script>`)

```javascript
const socket = io("wss://...");

socket.on('connect', () => {
  socket.send('hello!!!!');
});

socket.on('message', (data) => {
  console.log(data);
});
```

- WebSocket接続を試みる手順は[こちら](/WebSocket/whats-websocket.md)を参照

### 導入方法
```shell
$ yarn add socket.io
```

### Hello World
#### サーバーサイド
- フレームワークとしてExpress.jsを用い、TypeScriptで書いた
```typescript
import express from "express";
import type { Request, Response } from "express";
import cookieParser from "cookie-parser";
import logger from "morgan";
import http from "http";
import { Server } from "socket.io";
import type { Socket } from "socket.io";

const app = express();
const server = http.createServer(app);

const io = new Server(server);

app.use(logger('dev'));
app.use(express.json());
app.use(express.urlencoded({ extended: false }));
app.use(cookieParser());

app.get("/", (req: Request, res: Response) => {
  res.sendFile(__dirname + "/index.html");
});

io.on("connection", (socket: Socket) => {
  console.log("a user connected!");

  socket.on("chat message", (message: string) => {
    console.log("message: " + message);
  });

  socket.on("disconnect", () => {
    console.log("user disconnected!");
  })
});

server.listen(3000, () => {
  console.log("listening on 3000");
});
```

- `io.on('connection', _callback)`で接続イベントの監視。コールバック関数の引数には`socket`を指定する。
- `socket.on('disconnect', _callback)`で切断イベントの監視。
- `socket.on('<event message>', _callback)`でイベントのリッスン。
- `io.emit('<event message>', _callback);`でイベントメッセージの配信。

#### クライアントサイド
```html
<!DOCTYPE html>
<html>
  <head>
    <title>Socket.IO chat</title>
    <style>
      body { margin: 0; padding-bottom: 3rem; font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, Helvetica, Arial, sans-serif; }

      #form { background: rgba(0, 0, 0, 0.15); padding: 0.25rem; position: fixed; bottom: 0; left: 0; right: 0; display: flex; height: 3rem; box-sizing: border-box; backdrop-filter: blur(10px); }
      #input { border: none; padding: 0 1rem; flex-grow: 1; border-radius: 2rem; margin: 0.25rem; }
      #input:focus { outline: none; }
      #form > button { background: #333; border: none; padding: 0 1rem; margin: 0.25rem; border-radius: 3px; outline: none; color: #fff; }

      #messages { list-style-type: none; margin: 0; padding: 0; }
      #messages > li { padding: 0.5rem 1rem; }
      #messages > li:nth-child(odd) { background: #efefef; }
    </style>
  </head>
  <body>
    <ul id="messages"></ul>
    <form id="form" action="">
      <input id="input" autocomplete="off" /><button>Send</button>
    </form>

    <script src="/socket.io/socket.io.js"></script>
    <script>
      const socket = io();

      const form = document.getElementById("form");
      const input = document.getElementById("input");

      form.addEventListener("submit", (err) => {
        err.preventDefault();
        if(input.value) {
          socket.emit("chat message", input.value);
          input.value = "";
        }
      });
    </script>
  </body>
</html>
```

- `socket.on('<event message>', _callback)`でイベントのリッスン。
  - イベントメッセージに応じてクライアントアプリケーションの挙動を変化させている。
  - 上記サンプルだと、たとえば`chat message`を受信すると他ユーザからの発話を表示する

### 注意点
- socket.ioは純粋なWebSocketではないため、socket.io <-> WebSocketのような通信を行うことが出来ない。クライアントがsocket.io製であるならば、サーバもsocket.io製でなくてはいけない。