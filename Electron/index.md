# Electron
## Electronとは
- Webアプリケーションを開発する技術を使っtえネイティブアプリケーションを開発できるライブラリ
- SlackやTwitchが採用している

## Hello, World!
### 必要な環境
- Node.jsが必要。下記のコマンドを実行してバージョンが表示されるかを確認する。

```bash
$ node -v
$ npm -v
```

### まずは雛形を作成する
- npmを初期化する

```bash
$ npm init
```

- `package.json` を見てみる
- Electronのエントリーポイントは `main` となる。
  - つまり、この部分を弄ると起点となるコードを変更できる。
  - プロジェクトの都合に合わせて適宜書き換えよう。

```jsonc
{
  "name": "electron-hello-world",
  "version": "1.0.0",
  "description": "",
  "main": "index.js", // <- エントリーポイント
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "author": "",
  "license": "ISC"
}

```

- アプリケーションの `devDependencies` に `electron` パッケージを追加する

```bash
$ npm install --save-dev electron
```

- `package.json` のscriptsにコマンドを足す

```json
"scripts": {
  "start": "electron ."
},
```

### メインプロセスの実行
- Electronの `main.js` はメインプロセスを制御する
- メインプロセスは以下を制御する
  - アプリのライフサイクルの制御
  - ネイティブインターフェースの表示
  - 特権的操作の実行
  - レンダラープロセスの管理
  - ...などなど

### ウェブページの作成
- アプリケーション自体が読み込むウェブページを作成する必要がある
- `index.html` を作成する

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta http-equiv="X-UA-Compatible" content="IE=edge">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Document</title>
</head>
<body>
  <h1>aaa</h1>
</body>
</html>
```

### ブラウザウィンドウでウェブページを開く
- HTMLファイルをブラウザウィンドウに描画するために、以下の2つのelectronモジュールを使用する
  - `app` モジュール
    - アプリケーションのイベントライフサイクルを管理する
  - `BrowserWindow` モジュール
    - アプリケーションウインドウを作成して管理する
- `main.js` の先頭で必要なモジュールをインポートし、createWindowで `BrowserWindow` を初期化する
  - 最後に `createWindow()` を実行する
    - `app` モジュールの `ready`イベントが発生した後にブラウザウィンドウを作成できる。
    - `ready`イベントを待ち受けるには `whenReady()` を使用する

```js
const { app, BrowserWindow } = require("electron");

const createWindow = () => {
  const window = new BrowserWindow({
    width: 800,
    height: 600
  });

  window.loadFile("index.html");
}

app.whenReady().then(() => {
  createWindow();
});
```

### アプリケーションを実行する
- `$ npm start` を実行することで、htmlファイルをレンダリングできる。