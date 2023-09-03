# Hooksについて
## はじめに
- React公式ドキュメントのリファレンスを読んで理解した内容を記載する

## Hooksの種類
### State Hooks
- 入力値や状態を保持するためのHooks
- `useState` もしくは `useReducer` を宣言することで定義出来る


```tsx
function ImageGallery() {
  const [index, setIndex] = useState(0);
}
```

#### Reducer
- Reducer関数を用いると、直接イベントハンドラをコンポーネント内に定義するかわりに、ユーザの行動に応じた処理を別の場所にまとめることが出来る
- Reducerを使ってイベントハンドラを発行する場合、処理を直接実行するのではなく、データの塊をdispatchする

```tsx
function handleAddTask(text) {
  dispatch({
    type: 'added',
    id: nextId++,
    text: text,
  });
}

function handleChangeTask(task) {
  dispatch({
    type: 'changed',
    task: task,
  });
}

function handleDeleteTask(taskId) {
  dispatch({
    type: 'deleted',
    id: taskId,
  });
}
```

- Reducer関数をセットアップして、最後に `useReducer` でコンポーネントから呼び出せば完成。
- ただし、if文の羅列でReducer関数のメンテナンスコストが上がる可能性も高いので、利用タイミングはよく考えること。

### Context Hooks
- 親コンポーネントから子コンポーネントへのデータの受け渡しを行うためのHooks
- `useContext` を宣言することで定義出来る

```tsx
function Button() {
  const theme = useContext(ThemeContext);
  // ...
}
```

#### Context
- 親コンポーネントが、その下にあるツリーのどのコンポーネントに対してもデータを利用できるようにするもの
  - 普通、親コンポーネントから子コンポーネントにデータを渡すためには、propsを受け渡す
  - しかし、多層のコンポーネントをまたいでデータを渡すと、propsを受け渡すコンポーネントが増えてしまう
  - Contextを用いると、この問題が解決できる

### Ref Hooks
- DOMノードやタイムアウトID等の情報を保持するのに便利なHooks
- (利用イメージが良くわからない)
  - Reactのエスケープハッチとして使われる模様
- あとで読む: https://react.dev/learn/referencing-values-with-refs

### Effect Hooks
- 
