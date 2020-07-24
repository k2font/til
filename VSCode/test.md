# テストをVSCodeで行う方法
### 概要
- ソフトウェア開発で行うテストをVSCodeのインタフェース上で行うことが可能
- Test Explorer UIをインストールすることで、テストUIを使用することができる

<img width="752" alt="スクリーンショット 2020-07-22 16 10 15" src="https://user-images.githubusercontent.com/6561417/88145809-f8ef8080-cc35-11ea-9a13-aa8f8fb5406d.png">

- Test Explorer UIをインストールすることで、サイドバーに以下のアイコンが表示される

<img width="65" alt="スクリーンショット 2020-07-22 16 10 43" src="https://user-images.githubusercontent.com/6561417/88145982-37853b00-cc36-11ea-8416-bf4285bfd874.png">

### mochaを用いたテストをVSコードで行う方法
1. 拡張機能`mocha`をインストールする
2. `.vscode/settings.json`に設定を記載する

```jsonc
{
  // 注意: ./node_modules/.bin/mochaを指定するとテスト時にエラーが出る
  "mochaExplorer.mochaPath": "./node_modules/mocha",
  "mochaExplorer.files": "tests/model/task/*",
  "mochaExplorer.require": [
    "ts-node/register",
  ],
  "mochaExplorer.cwd": "./",
  "mochaExplorer.env": {
    "NODE_PATH": "./",
  },

  "editor.codeLens": true,
  "testExplorer.codeLens": true
}
```

3. テストタブを押し、再生ボタンをクリック
4. テスト実行

<img width="1005" alt="テスト実行画面" src="https://user-images.githubusercontent.com/6561417/88146228-8fbc3d00-cc36-11ea-8128-45011075d6a1.png">

