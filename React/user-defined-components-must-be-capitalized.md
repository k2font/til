# ユーザ定義のコンポーネント名は大文字で始める

### 問題
- 以下のようなコンポーネントがあるとする

```jsx
import React from 'react';

const testComponent = () => {
  // 何らかの処理
}

function App() {
  return (
    <div className="App">
      <testComponent />
    </div>
  )
}

export default App;
```

- このコンポーネントはレンダリングエラーを起こす。エラーメッセージは以下。

```bash
Property 'testComponent' does not exist on type 'JSX.IntrinsicElements'.
```

### 解決法
- コンポーネント名の先頭を大文字に書き換える

```jsx
import React from 'react';

const TestComponent = () => {
  // 何らかの処理
}

function App() {
  return (
    <div className="App">
      <TestComponent />
    </div>
  )
}

export default App;
```

### なぜ先頭は大文字にする必要がある?
- 先頭を小文字の名前に指定した場合、組み込みのコンポーネントを参照する
- そのため、先頭を小文字に指定したコンポーネントは、ユーザが定義したJavaScriptファイルにあるコンポーネントではなく、HTML組み込みのコンポーネントを参照しに行く
- 組み込みコンポーネントに`testComponent`のような名前のコンポーネントは存在しないため、レンダリングエラーを引き起こす。
- 先頭を大文字にすると、期待通りに動作する。

### 参考
- https://ja.reactjs.org/docs/jsx-in-depth.html#user-defined-components-must-be-capitalized