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
  'use strict';
  return this;
}

console.log(f2() === undefined); // true
```

- `call()`や`apply()`を使用すると、関数の呼び出し時に指定したオブジェクトを割り当てることができる

```javascript
// このオブジェクトはobjに属する
const obj = {a: 'Custom'};

// このオブジェクトはグローバルオブジェクトなので、windowに属する
const a = 'Global';

function whatsThis() {
  return this.a;
}

console.log(whatsThis()); // 'Global'
console.log(whatsThis.call(obj)); // 'Custom'
console.log(whatsThis.apply(obj)); // 'Custom'
```

- `call()`と`apply()`の違い
  - `call()`は引数リストを受け取る。`apply()`は引数の配列を受け取る。

```javascript
function add(c, d) {
  return this.a + this.b + c + d;
}

const obj = {a: 1, b: 2};

// callは引数をそのまま受けとる
console.log(add.call(obj, 5 , 7)); // 15。1 + 2 + 5 + 7を行う

// applyは引数の配列を受け取る
console.log(add.apply(obj, [5, 7])); // 15

// call, applyどちらも最初の引数ではthisが何を指すのかを指定する
```

- bindメソッドを用いた場合の挙動について
  - [こちら](/JavaScript/bind-function-usage.md)を参照してください

#### アロー関数
- アロー関数では、そのアロー関数が宣言されたコンテキストが`this`として設定される。
- 以下の`foo()`内の`this`は、`foo()`がグローバルオブジェクトであるため、`this`は`window`が指定される。
```javascript
var globalObject = this;
var foo = () => this;
console.log(foo() === globalObject); // true
```

- アロー関数内で宣言された`this`の特徴として、何があっても宣言された時点での値が`this`に割り当てられる。
- そのため、アロー関数は`call()`や`apply()`、`bind()`によるthisの値の指定を受け付けない

```javascript
var globalObject = this;
// ここで決定したthisの値はいかなる場合に置いても変更されることはない
var foo = () => this;
console.log(foo() === globalObject); // true

// fooをオブジェクトのメソッドとして呼び出す。
// しかし、foo()が返すthisはobjを指すのではなく、windowを指す。
// なぜなら、アロー関数内のthisに格納される値は、宣言された時点で決定するから
// つまり、オブジェクトとして再定義をしたとしても、thisの値はグローバルオブジェクトを指し続ける。
const obj = {func: foo};

console.log(obj.func() === globalObject); // true

// callでobjを割り当てることを試みるが、thisはグローバルオブジェクトを指し占めす
console.log(foo.call(obj) === globalObject); // true

// bindでobjを割り当てるが、こちらもグローバルオブジェクトを指す
foo = foo.bind(obj);
console.log(foo() === globalObject); // true
```

#### オブジェクトのメソッド
- 