# styled-componentsとは?
### 概要
- Reactにおいてスタイリングを行う際に取られる1手法「CSS-in-JS」の考えに則ったライブラリのこと

<img width="1342" alt="スクリーンショット 2020-07-24 17 46 33" src="https://user-images.githubusercontent.com/6561417/88374979-af3d9c00-cdd5-11ea-87a7-71c99151576a.png">

### なぜstyled-componentsを使用するの?

- ReactでCSSを指定するには、以下の3つの方法がある
  1. インラインスタイルの使用
  2. CSS Modulesの使用
  3. `*.css`ファイルとクラス名によるスタイリング
  4. CSS-in-JSを用いる
- 1や2の方法は、Reactを用いない従来のCSSとは異なる書き方を強いられる。また、これらの方法では疑似セレクタやメディアクエリ等モダンなCSSの機能を使用することが難しい。ましてやインラインスタイルは公式ドキュメントにおいて以下のように取り上げられており、殆どの場合は使用すること自体をよく検討する必要がある。
  > Some examples in the documentation use style for convenience, but using the style attribute as the primary means of styling elements is generally not recommended.
  https://ja.reactjs.org/docs/dom-elements.html#style
- 3の方法は一般的に使用される方法であり、React公式ドキュメントでも初心者に対して推奨されている方法である。しかし、CSSにおいてクラス名は常にグローバルであるため、プロジェクトファイルが肥大化するとクラス名の衝突による意図しないオーバーライドが発生する。これにより、命名規則の再考やCSS設計そのものの見直しが強いられる。
- 4にあるCSS-in-JSを用いることで、1や2、3で見られる問題に悩まされること無くスタイリングを行うことができる。
- styled-componentsを使用すると、タグ付きテンプレートリテラルとCSSの力を利用して、従来のCSSと同じ書き方でスタイルを設定することができる。styled-componentsはCSS-in-JSのひとつである。
- CSS-in-JSの考えに則ったライブラリはほかにもたくさんある。技術的な比較は以下のページを参照のこと。
  - 参考: https://github.com/MicheleBertoli/css-in-js