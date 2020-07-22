# filter関数内のアロー関数の仕様について
### 問題
- 以下の2つの関数は何を出力する?

```javascript
function test() {
  let tasks = [
    {
      done: false
    },
    {
      done: true
    }
  ];
  return tasks.filter((task) => !task.done);
}
```

```javascript
function test() {
  let tasks = [
    {
      done: false
    },
    {
      done: true
    }
  ];
  return tasks.filter((task) => {
    !task.done
  });
}
```

### 答え
- 前者は...
```json
[
  {
    "done": false
  }
]
```

- 後者は...

```json
[]
```

### なぜ?
- filter関数に指定するcallbackはtrueもしくはfalseを返す必要がある。callbackに与えられた引数のArrayに対して、trueを返した場合は残り、falseを返した場合は取り除かれる。
  - 念の為。filter関数は非破壊。ターゲットとなる変数に対し変更を加えない。
  - 返り値は、callback関数がtrueを吐き出した値で構成された新しいArrayが返る。

- つまり、2つ目の例は`{}`内で値を`return`すればよい。

```javascript
function test() {
  let tasks = [
    {
      done: false
    },
    {
      done: true
    }
  ];
  return tasks.filter((task) => {
    return !task.done
  });
}
```

- 結果は以下の通り。正しく動作した！

```json
[
  {
    "done": false
  }
]
```

- 参考: https://developer.mozilla.org/ja/docs/Web/JavaScript/Reference/Global_Objects/Array/filter

### 素朴な疑問
- なぜ`{}`で囲われないArrow関数は`return`がいらないの?
  - Arrow Functionは単一の命令のみ記載する場合、`{}`を取り除いた上で`return`を省略できる仕様だから。
- こちらを参照 👉 [Arrow Functionの仕様](/JavaScript/arrow-function.md)