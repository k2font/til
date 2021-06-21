# WebSocketとは?
### 概要
- リアルタイムな双方向通信を可能とするプロトコル。
- 永続的な接続を介してBrowserとサーバーでデータのやり取りを行う方法を提供する。
- オンラインゲームやチャットアプリなどに使用される。

### WebSocketを利用する
- 以下のように`WebSocket`を`new`すると、WebSocketコネクションが接続される。
```javascript
let socket = new WebSocket("ws://k2font.com/path/to/file");
```
- ただし、上記の例の`/path/to/file`下でクライアントサイドのWebSocketイベントを受け取るサーバサイドアプリケーションが動作している必要がある。

#### `ws://`ではなく`wss://`を使用することが望ましい
- `ws`は`http`と同じく暗号化されていない通信。平文なので、攻撃者に通信を盗聴される恐れがある。
- `wss`は`https`と同じくTLSで送信側データの暗号化を行うため、盗聴による通信の盗み見を防ぐことができる(正確には通信の傍受がされたとしてもメッセージの中身まで見ることができない)

#### WebSocketを使用したサンプルアプリケーション
- サーバーにWebSocketのコネクションを受けられるアプリケーションが存在することを仮定している
```javascript
// クライアントサイドのWebSocketアプリケーション

const WebSocket = require('ws');
let socket = new WebSocket("wss://domainname.com/path/to/server");

socket.onopen = (e) => {
  console.log('[Open] Connection!');
  console.log('Sending to server');
  socket.send("My name is Sho.");
};

socket.onmessage = (event) => {
  console.log(`[message] data is received! ${event.data}`);
}

socket.onclose = (event) => {
  if(event.wasClean) {
    console.log(`[close] Connection close cleanly, code=${event.code}`);
  } else {
    console.log('[close] Connection killed by server.');
  }
}

socket.onerror = (error) => {
  console.log(error);
}
```

- 基本的に以下の4つのイベントが存在する
  - open: 接続が確立された
  - message: データを受け取った
  - close: 切断された
  - error: エラーが起きた

### WebSocket接続の段取り
- `new WebSocket(url)`が実行されると、以下の段取りでWebSocketの接続を行う
  1. HTTPリクエストを送信
  2. HTTPレスポンスを受領
  3. WebSocketプロトコルに切り替え相互接続
- これをOpening Handshakeと呼ぶ
  - https://triple-underscore.github.io/RFC6455-ja.html#section-1.3

- 1のリクエストは以下の通り。
- `Connection: Upgrade`は「プロトコルを変更するけど大丈夫？」をサーバに伝えることを指している
```
GET /game
Host: k2font.com
Origin: https://k2font.com
Connection: Upgrade
Upgrade: websocket
Sec-WebSocket-Key: jfls8/jfask74fd2k33fuwega==
Sec-WebSocket-Version: 13
```

- 2のレスポンスは以下の通り
- WebSocket接続を行う場合、レスポンスは必ず101を返す必要がある。
```
101 Switching Protocols
Upgrade: websocket
Connection: Upgrade
Sec-WebSocket-Accept: hsBlbuDTkk24srzEOTBUlZAlC2g=
```
- このレスポンスを受け取った後に、WebSocket接続が開始される。