# this
### そもそもthisとは?
- あるプロパティがどのオブジェクトに属しているか、もしくはあるオブジェクトがどのリソース(?)に属しているかを示すもの。

```javascript
const test = {
  prop: 42,
  func: function() {
    return this.prop; // このオブジェクトにおいてtest.propを指す
  },
};

const prop = 32;

console.log(test.func()); // 42
// 32を出力することはない
```

### thisが指すもの
#### グローバルオブジェクト
- すべての関数の外側はグローバルコンテキストを指す。
- グローバルコンテキストでは必ず`this`は`window`を指す

```javascript
console.log(this === window); // window
```

- グローバルに初期化された変数は`window`に属している

```javascript
let a = 42;
console.log(window.a);
```

#### 関数
- 関数の呼び出し方により異なる
##### `strict`モードではない時
- 以下のコードではthisの値を指定していないため、`this`は`window`を指す
- `strict`モードではないとき、`this`は規定でグローバルオブジェクトを指す

```javascript
function f1() {
  return this;
}

console.log(f1() === window); // true
```

##### `strict`モードの時
- `strict`モードの場合、実行コンテキストで`this`の値が指定されていないと`undefined`を返す

```javascript
function f2() {
  return this;
}

console.log(f2() === undefined); // true
```

- `call()`や`apply()`を使用すると、関数の呼び出し時に指定したオブジェクトを割り当てることができる

```javascript

```

#### アロー関数
- 

#### クラスメソッド
- 