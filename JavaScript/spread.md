# スプレッド構文
### 概要
- 配列や文字列等のオブジェクトを指定の場所で繰り返し参照することができる
- 反復的に呼び出される場合にスプレッド構文を用いて配列を展開する
- IE未対応なので注意。

```javascript
const sum = (x, y, z) => {
  return x + y + z;
};

const array = [1, 2, 3];

console.log(sum(...array)); // 6

// スプレッド構文を用いない書き方は以下
console.log(sum.apply(null, array)); // 6
```

- `new`でコンストラクタを呼びだす場合にも有効に利用できる

```javascript
const array = [1994, 2, 1];
const birthday = new Date(...array);
// Tue Mar 01 1994 00:00:00 GMT+0900 (日本標準時)
```

### `concat`の代用として使用する
- 2つ以上の配列を連結する時、`concat`が用いられる
- これをスプレッド構文を用いることでより簡潔に記述することができる

```javascript
const array1 = [1, 2, 3];
const array2 = [4, 5, 6];

const cat = [...array1, ...array2]; // [1, 2, 3, 4, 5, 6]

const array3 = [7, ...cat, 8, 9]; // このように書いて連結することもできる！
// [7, 1, 2, 3, 4, 5, 6, 8, 9]
```

### `slice`の代用として使用する
- 配列自体を複製する事ができる

```javascript
const array1 = [100, 200, 300];
const array2 = [...array1]; // array1がそのまま複製される
```

### オブジェクトリテラルでのスプレッド構文を使用する方法
- ES2018で新たに書くことができるようになった構文。
- オブジェクトのコピーやマージが可能

```javascript
const obj1 = {foo: "bar", x: 42};
const obj2 = {foo: "baz", y: 13};

const cloneObj = {...obj1}; // {foo: "bar", x: 42}
const mergeObj1 = {...obj1, ...obj2}; // {foo: "baz", x: 42, y: 13}
const mergeObj2 = {...obj2, ...obj1}; // {foo: "bar", x: 42, y: 13}
// 後に読み込まれたオブジェクトが、すでに存在するプロパティの値を上書きする
```