# ブラウザの仕組み
## はじめに
- ここでは、アドレスバーに「google.com」と入力してからブラウザの画面にページが表示されるまでに何が起きているかをまとめる

## 対象Webブラウザ
- オープンソースブラウザ
- Chrome, Firefox, Safariが対象。

## Webブラウザの主な機能
- ユーザの選択したリソースをウインドウに描画する。
  - 最も基本的な機能はHTMLとCSSを元にした描画と装飾。
- リソースはHTML, 画像, (最近はあまり見ないけど)XMLが該当する。PDFなどのタイプも存在する
- リソースの場所はURIを使用してユーザが指定する。

## ブラウザのUI
- どのブラウザもUIは共通している。
- 標準規格が規定されているわけではなく、それぞれのブラウザのいい部分を互いに ~~盗み合って~~ オマージュしているため、UIが似通っている。
- 今日のブラウザがどのようなUIとなっているかは言わずもがな。たとえばChromeを見てみよう。

## ブラウザの上位構造
- ブラウザの主な構成要素は以下の通り。

### ユーザインタフェース
- アドレスバーとか戻るボタンなど。
- ブラウザの中で、要求したページを表示する部分以外の部分を指す。

### ブラウザエンジン
- 不明。あとで調べる

### レンダリングエンジン
- 要求されたコンテンツの表示を担当する。
- 要求されたHTMLとCSSを構造解析して、描画と装飾を施す。

### ネットワーキング
- HTTPリクエストを作成する部分。

### UIバックエンド
- 不明

### JavaScriptインタプリタ
- JavaScriptのコードの解析と実行に使用する

### データストレージ
- Cookieなどのデータをハードディスク / SSDに保存しておく必要がある。
- 現在ではウェブデータベースが存在する。

## レンダリングエンジンのお仕事
- 

# 参考文献
https://www.html5rocks.com/ja/tutorials/internals/howbrowserswork/