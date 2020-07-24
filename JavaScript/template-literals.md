# テンプレートリテラル
### 概要
- 組み込み式を使える文字列リテラル。
- 文字列に `${props.data.name}`のような式を含めることで、文字列を可変値として扱うことができるようになる。
- バッククォートでくくる。
- `$`と`{}`で囲まれた部分をプレースホルダと呼ぶ

### 書き方
```javascript
const literal = `私は ${props.data.name} です。`;
```

### タグ付きテンプレートリテラル
- テンプレートリテラルにはタグをつけることができる。
- このタグは関数(タグ関数と呼ぶ)として置くことができ、与えられたテンプレートリテラルを評価することができる。
- 与えられたタグ関数はテンプレートリテラルをパースした値を引数に取る。

- 例。以下のコードはTypeScriptです。

```typescript
let person: string = 'shoichiro';

// タグ関数
function myTag(strings: string[], personExp: string): string {
  let str0: string = strings[0]; // "私は"
  let str1: string = strings[1]; // "です。"

  let personStr: number;
  if (personExp.length > 6){
    personStr = '名前が長い男';
  } else {
    personStr = '名前が短い男';
  }

  return `${str0}${personStr}${str1}`;
}

let output: string = myTag`私は ${ person } です。`; // タグ付き

console.log(output);
// 私は名前が長い男です
```

