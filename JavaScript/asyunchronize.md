# 非同期処理
- 非同期処理なくしてJavaScriptは語れない

### そもそも同期処理とは
- 同期処理は、プログラムを律儀に上から順番に実行していく方法である
- たとえば以下のようなプログラムである

```javascript
const a = 99;
const n = 3;
let res = Math.pow(a, n);
console.log(res); // 970299
```

- 上記のプログラムは上から変数を宣言、代入しながらaのn乗を算出している。これは上から順番に実行される同期処理プログラムの例である。このプログラムはWebブラウザでの実行時に悪い影響を及ぼさない。
- 次のような場合に、同期処理に嫌気が指すことになる。

```javascript
function blocktime(timeout) {
  const startTime = Date.now();
  while(true) {
    const diffTime = Date.now() - startTime;
    if(diffTime >= timeout) return;
  }
}

console.log("処理を開始");
blocktime(1000);
console.log("この行が呼ばれるまで1秒かかりました");
```

- 上記の例では、指定したミリ秒だけ`while`文を回すプログラムである。
- `while`文の実行中はメインスレッドを専有することになるため、ブラウザがフリーズする。これはWebアプリとしては致命的となる。
- これにより、メインスレッドを専有せずにプログラムを賢く実行していく仕組みが必要になる。それが非同期処理。
  - 注意: 非同期処理もメインスレッドで実行されるが、同期処理と挙動が異なる。

### 非同期処理の書き方3選
- [コールバック関数](/JavaScript/callback-function.md)
- [Promise](/JavaScript/promise.md)
- [async/await](/JavaScript/async-await.md)