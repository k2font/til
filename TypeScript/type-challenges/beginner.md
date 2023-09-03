# 初級
## Pick
### 問題
```txt
組み込みの型ユーティリティPick<T, K>を使用せず、TからKのプロパティを抽出する型を実装してください。
```

### 解法メモ
#### 解答
```ts
interface Todo {
  title: string
  description: string
  completed: boolean
}

type MyPick<T, K extends keyof T| string | number | symbol> = {
  [P in K]: P extends keyof T ? T[P] : never
}

type TodoPreview = MyPick<Todo, 'title' | 'completed'>

const todo: TodoPreview = {
  title: 'Clean room',
  completed: false,
}
```

- この問題を特にあたり前提となる知識は以下の通り
  - ジェネリクス
  - Conditional Types
  - never型

#### ジェネリクス
- 型をパラメータで受け取れる機能
```ts
type User<T> = {
  name: T
}

type UserString = User<string>
type UserNumber = User<number>
```

- `T` は `extends` をつけることで、設定できる型を制限することが出来る
- 例えば `T extends string` の場合、 `T` は `string` 型(もしくは`string`を継承した型)しか設定できなくなる。

#### Conditional Types
- 条件によって型を変更できる機能
```ts
type Foo<T> = T extends string ? string | number
```

- 上記の例では、`string`型を渡したら`string`型と評価されるが、それ以外の型を渡すと`number`型と評価される

#### never型
- 発生しない値の型
- 存在しない方を無いものとして扱える
```ts
type Exclute<T, U> = T extends U ? never : T
```

- 上記の例では、TがUを継承している場合は`never`型、そうじゃないなら`T`型になる。
- これを使うと以下のような結果になる
  - booleanを継承する型`boolean`型が渡された場合は`never`型が返却されるため、結果は`string | number`型となる。
```ts
type Foo = Exclude<string | number | boolean, boolean>
// type Foo = string | number
```
