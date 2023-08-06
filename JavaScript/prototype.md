# prototype
## 前置き
- 空のオブジェクトを定義しただけなのに、そのオブジェクトにはメソッドが定義されている
- 例えば以下のようなコードは実行できる。
  ```js
  const obj = {};
  obj.toString();
  ```
- これはなぜ？
  - 理由は `Object.prototype` に `toString()` メソッドが定義されているため

## prototypeとは
- JavaScriptに存在するすべてのオブジェクトの元となるオブジェクト
  - FunctionとかArrayの親オブジェクトがObject
- `Object.prototype` とは、Objectのプロトタイプオブジェクト
  - `Object.prototype` には `toString()` や `valueOf()` などのメソッドが定義されている
  - つまり、`Object.prototype` に定義されているメソッドは、すべてのオブジェクトが使えるメソッドであり、`{}` と宣言しただけのオブジェクトでも `.toString()` が使用できる
  - このようなメソッドをプロトタイプメソッドと呼ぶ
  - ちなみに、空のオブジェクトや配列等のオブジェクトがプロトタイプメソッドを利用できることを、プロトタイプチェーンと呼ぶ