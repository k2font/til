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
- フレームワークとしてExpress.jsを用いる
```javascript
var app = require('express')();
var http = require('http').createServer(app);
var io = require('socket.io')(http);

app.get('/', (req, res) => {
  res.sendFile(__dirname + '/index.html');
});

io.on('connection', (socket) => {
  io.emit('login message', "user is in." + Date());

  console.log('a user connected.');

  socket.on('disconnect', () => {
    io.emit('logout message', "user is disconnected!");
  });

  socket.on('chat message', (msg) => {
    io.emit('chat message', msg);
  });
});

http.listen(3000, () => {
  console.log('listening on *:3000');
});
```

- `io.on('connection', _callback)`で接続イベントの監視。コールバック関数の引数には`socket`を指定する。
- `socket.on('disconnect', _callback)`で切断イベントの監視。
- `socket.on('<event message>', _callback)`でイベントのリッスン。
- `io.emit('<event message>', _callback);`でイベントメッセージの配信。

#### クライアントサイド
```html
<!doctype html>
<html>
  <head>
    <title>Socket.IO chat</title>
    <style>
      * { margin: 0; padding: 0; box-sizing: border-box; }
      body { font: 13px Helvetica, Arial; }
      form { background: #000; padding: 3px; position: fixed; bottom: 0; width: 100%; }
      form input { border: 0; padding: 10px; width: 90%; margin-right: 0.5%; }
      form button { width: 9%; background: rgb(108, 197, 226); border: none; padding: 10px; }
      #messages { list-style-type: none; margin: 0; padding: 0; }
      #messages li { padding: 5px 10px; }
      #messages li:nth-child(odd) { background: #eee; }
    </style>
  </head>
  <body>
    <ul id="messages"></ul>
    <form action="">
      <input id="m" autocomplete="off" /><button>Send</button>
    </form>
  </body>
  <script src="/socket.io/socket.io.js"></script>
  <script src="https://code.jquery.com/jquery-3.4.1.min.js"></script>
  <script>
    $(function() {
      var socket = io();
      $('form').submit(function(e) {
        e.preventDefault();
        socket.emit('chat message', $('#m').val());
        $('#m').val('');
        return false;
      });
      socket.on('chat message', function(msg) {
        console.log("chat message");
        $('#messages').append($('<li>').text(msg));
      });
      socket.on('login message', function(msg) {
        console.log("login message");
        $('#messages').append($('<li>').text(msg));
      });
      socket.on('logout message', function(msg) {
        console.log("logout message");
        $('#messages').append($('<li>').text(msg));
      });
    });
  </script>
</html>
```

- `socket.on('<event message>', _callback)`でイベントのリッスン。
  - イベントメッセージに応じてクライアントアプリケーションの挙動を変化させている。
  - 上記サンプルだと、たとえば`chat message`を受信すると他ユーザからの発話を表示する

### 注意点
- socket.ioは純粋なWebSocketではないため、socket.io <-> WebSocketのような通信を行うことが出来ない。クライアントがsocket.io製であるならば、サーバもsocket.io製でなくてはいけない。