# Promise
### Promiseとは
- ES5で存在したエラーファーストコールバックという考え方をビルトインオブジェクトに昇華させたもの。
  - エラーファーストコールバックは、ES5の組み込みの命令ではなく暗黙のルールであるため、エラーファーストコールバックに従わなくても正しく動くコードを書くことができる。
  - 書き方を統一するために、ES2015より新しいオブジェクトとして登場した。

### 宣言の仕方
- `new Promise()`する。
- 非同期処理が成功したときは`resolve`を呼ぶ。失敗したら`reject`が呼ばれる。

```javascript
const executor_test = (resolve, reject) => {

};

const promise_test = new Promise(executor_test);

// promise_test関数はPromiseインスタンスを返す
promise_test().then(()=> {
    // 非同期処理が成功したときの処理(executorがresolveが返却された時)
}).catch(() => {
    // 非同期処理が失敗したときの処理(executorがrejectが返却された時)
});
```

- 以下の例は上記の具体例。
- promiseオブジェクト`Promise`を返す。この`Promise`は非同期処理に成功するので、`resolve`が呼ばれ「Promiseが発行されたよ」→「resolveされたから呼ばれたよ」と出力する。

```javascript
const promise = new Promise((resolve, reject) => {
  console.log("Promiseが発行されたよ");
  resolve(); // こっちが呼ばれる
  reject();
});

const onFulfilled = () => {
  console.log("resolveされたから呼ばれたよ");
};

const onRejected = () => {
  console.log("rejectされたから呼ばれたよ");
};

promise.then(onFulfilled, onRejected);
```

### `then()`と`catch()`
- 結構ややこしいよ！
- 関数が返す`Promise`の状態に応じて利用されるメソッドが変わる
  - `then()`: `resolve`および`reject`が渡されると呼び出されるメソッド。`resolve`が渡されると第一引数の関数が、`reject`が渡されると第二引数の関数が呼び出される。
    - なお、第二引数はオプションであり、渡さなくてもいい。
    - もし`reject`のみ引数として与えたい場合、第一引数には`undefined`を書く。
  - `cathc()`: `reject`が渡されると呼び出されるメソッド。`reject`のみ渡されるとわかっている場合、`then()`だと第一引数に`undefined`を与える必要がある。スッキリ書きたいときのために使用する。
- 以下の例では`dummyFetch()`という関数が`Promise`を返却する。Promise内のif文の分岐(=非同期処理)が評価されると、返却するメソッドが決定する。
  - たとえば`/success/*`という値が`path`に入ると、Promiseは`resolve`であるオブジェクト``{ body: `Response body of ${path}` }``を返す。`resolve`が渡されたので、`then()`では`onFulfilled`が呼び出される。
  - 逆に`/failure/**`が`path`に入ると、Promiseは`reject`であるErrorを返す。それをそれを受け取った`then`関数は`onReject`を呼び出す。

```javascript
function dummyFetch(path) {
  return new Promise((resolve, reject) => {
    setTimeout(() => {
      if(path.startsWith("/success")) resolve({ body: `Response body of ${path}` });
      else reject(new Error("NOT FOUND"));
    }, 1000 * Math.random());
  });
}

const onFulfilled = (response) => {
  console.log(response);
}

const onReject = (error) => {
  console.log(error);
}

dummyFetch("/success/data").then(onFulfilled, onReject);

dummyFetch("/failure/data").then(onFulfilled, onReject);
// 2つ目のdummyFetchは、以下の書き方でも問題ない。
// dummyFetch("/failure/data").then(undefined, onReject);
// dummyFetch("/failure/data").catch(onReject);
```

- 結論、慣れるまで書き続けること。
- つまり、発行したPromiseはPromise内で`resolve`か`reject`を呼び出せば、あとは`then`が処理してくれる。
  - `try .. catch`を使わなくても、Promiseが実行したメソッドに応じて自動的にエラーハンドリングを行ってくれる。最高！

### Promiseの状態(Fulfilled, Rejected, Pending)
- Promiseインスタンスは、内部的に次の3つの状態が存在する。
  - Fulfilled: `resolve`した状態。この場合、thenの第一引数が呼ばれる。
  - Rejected: `reject`した状態。この場合、thenの第二引数かcatchが呼ばれる。
  - Pending: FulfilledでもRejectedでもない状態。これは`resolve`も`reject`もしていないので、どちらの状態になるか決めかねている。
- 基本、FilfilledかRejectedの状態になれば、以降Promiseの状態は変化することはない。また、APIで操作できるものでもないため、むりやりいじることも不可能。
  - つまり、Pending→Fulfilledとなるか、Pending→Rejectedとなるかの2択しかない。Fulfilled→Pendingにもならないし、Rejected→Fulfilledとなることもできない。