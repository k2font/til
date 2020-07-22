# Arrow Function(アロー関数)
### 概要
- https://developer.mozilla.org/ja/docs/Web/JavaScript/Reference/Functions/Arrow_functions
- `function`を簡潔に書きたい、という動機から生まれた記法。

```javascript
// Arrow Function
const test = () => {}

// 上記と同様の記法
function test() {

}
```

### Arrow Functionを使う利点
- Arrow Functionは以下のような利点がある
1. 簡潔に書ける
2. `this`を束縛しない

### 記法いろいろ
- 単一の命令しかない場合、`{}`を取り外した上で`return`も省略できる。

```javascript
// Arrow Function
const test1 = (num) => {
  return num * num;
}

// 上記と同じ記法
const test2 = (num) => num * num;

// 以下は正しい動作をしない
// num^2がreturnされない。
const test3 = (num) => {
  num * num;
}
```

```bash
$ test1(24)
576

$ test2(24)
576

$ test3(24)
undefined
```