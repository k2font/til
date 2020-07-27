# 分割代入
### 概要
- 配列から指定した添字の値を簡単に書く方法のこと

### 配列の分割代入
- 以下のような、配列にアクセスして値を異なる変数に格納するコードがあるとする。
```javascript
const array = [1, 2, 3, 4, 5];
const v1 = array[0]; // 1
const v2 = array[1]; // 2
const v3 = array[2]; // 3
const v4 = array[2]; // 4... のはずが添え字ミスで3
const v5 = array[4]; // 5
```

- 縦に連なり見づらくきれいではない。コーディングもめんどくさい。記載ミスも発生する。
- 上記のようなコードは、以下のように書くことができる。
- このような書き方を **分割代入** という。

```javascript
const array = [1, 2, 3, 4, 5];
const [v1, v2, v3, v4 ,v5] = array; // スッキリ書けた！
const [, a, , b] = array; // 特定の値だけ絞って取り出すことも可能。
```

- 「1と2、その他全部」のようなざっくりとした代入もできる。

```javascript
const array = [1, 2, 3, 4, 5];
const [v1, v2, ...rest] = array; // restには[3, 4, 5]が入る
```

### オブジェクトの分割代入
- 分割代入はオブジェクトに対しても実行可能。

```javascript
const obj = {
  a: 1,
  b: 2,
  c: 3
}

const {a, b, c} = obj;
const {c} = obj; // ある固定の値のみ取得することも可能
```

- 取得した値に別名を付けたいときは`:`をつける。

```javascript
const {a: obj1, b: obj2, c: obj3} = obj;
```

- たとえば、以下のような2つの`import`文は同義である。これは、前者がオブジェクトの分割代入を行っているためである。

```javascript
import { Module1 } from "Modules1"; // クラスというオブジェクトの分割代入を行っている
import Module1 from "Modules1"; // 上記と同じ書き方
```