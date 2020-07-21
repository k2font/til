# リモート開発機能
### 概要
- クラウドサーバやDockerコンテナ内のリソースをVSCodeから編集することができる
- たとえばVirtualBox内のLinux(ゲストOS)とmacOS(ホストOS)間の共有ディレクトリを作成して、そのディレクトリを `code .`で開いて... みたいな面倒な操作は必要なくなる。超便利。
- Dockerコンテナ上の開発を行う場合は非常に協力。いちいちVSCode CLIをコンテナにインストールする必要がなくなる。

### 設定方法
- コマンドパレットに`Remote-SSH connect`と入力し。`Remote-SSH: Connect to Host...`を選択する
- `~/.ssh/config`に登録されている接続先が表示されるので、好きなホストを選択する
- 画面左下に以下の表示がされれば接続完

<img width="208" alt="スクリーンショット 2020-07-20 9 45 56" src="https://user-images.githubusercontent.com/6561417/87889831-854f4700-ca6e-11ea-9f20-869a2ab74500.png">

- SSH接続後の状態は以下の通り

<img width="1051" alt="スクリーンショット 2020-07-20 9 48 11" src="https://user-images.githubusercontent.com/6561417/87889833-87190a80-ca6e-11ea-8fe5-bad34f77fdd6.png">

- あとはローカルのディレクトリ下での開発と同じように開発できる。Happy Hacking!