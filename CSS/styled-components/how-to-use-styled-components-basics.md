# styled-componentsの使い方(基礎)
### ことはじめ
- styled-componentsを用いたスタイリングを行うには、タグ付きテンプレートリテラルを用いて要素にスタイルをつけていく

```tsx
import React from "react";
import styled from "styled-components";

export default function Window() {
  return (
    <Wrapper>
      <Title>CSS完全に理解した</Title>
    </Wrapper>
  );
}

const Title = styled.h1`
  font-size: 1.5em;
  text-align: center;
  color: palevioletred;
`;

const Wrapper = styled.section`
  padding: 2em;
  background: papayawhip;
`
```

- 上記の例では、着色され文字の大きさが指定された`h1`タグを`Title`として生成し、関数コンポーネント内に配置されている。

### propsの値に応じてスタイリングを変更する
- コンポーネントに渡されるpropsの値に応じてスタイルを変化させることができる

```tsx
import React from 'react';
import styled from 'styled-components'

interface Props {
  data: string;
  primary?: any;
}

const Button = styled.button`
  background: ${(props: Props) => props.primary ? "palevioletred" : "white"};
  color: ${(props: Props) => props.primary ? "white" : "palevioletred"};

  font-size: 1em;
  border-radius: 3px;
  border: 2px solid palevioletred;
  margin: 0 1em;
  padding: 1em 2em;
`;

const Btn = (props: Props) => {
  console.log(props);
  return (
    <div>
      <Button {...props}>{props.data}</Button>
    </div>
  );
}

export default Btn;
```

- 上記の例では、タグ付きテンプレートスタイルの中に`${}`で囲まれた三項演算子が存在する事がわかる。`props.primary`の値に応じて背景色と文字色が変わるコンポーネントを作成している。
  - なお、`props.primary`はOptionであることを`interface`で指定している。
- `props`は関数コンポーネントから`Button`にわたす。渡すときは[スプレッド構文]()を用いる。

- 上記のコンポーネントを用いる場合は、呼び出し先のコンポーネントで以下のように以下のように以下のように書く

```html
<Btn data="これはstyled-componentsで作られたボタンです"></Btn>
<Btn primary data="これはstyled-componentsで作られたボタンです"></Btn>
```

### コンポーネントをオーバーライドする
- `styled()`でコンポーネントをオーバーライドすることができる。

```typescript
const Button = styled.button`
  background: ${(props: Props) => props.primary ? "palevioletred" : "white"};
  color: ${(props: Props) => props.primary ? "white" : "palevioletred"};

  font-size: 1em;
  border-radius: 3px;
  border: 2px solid palevioletred;
  margin: 0 1em;
  padding: 1em 2em;
`;

const TomatoBtn = styled(Button)`
  color: tomato;
  border-color: tomato;
`
```

### `as`を使ってレンダリングする要素を動的に変更する
- 用途が異なるコンポーネントごとに同じスタイルを割り当てたい時、`as`は有効に活用できる
- たとえば、スタイルは同じだけど、ボタンとアンカーリンクの2種類の要素を作成したい時、以下のように記述する

```typescript
import React from 'react';
import styled from 'styled-components'

interface Props {
  data: string;
  primary?: any;
}

const Button = styled.button`
  background: ${(props: Props) => props.primary ? "palevioletred" : "white"};
  color: ${(props: Props) => props.primary ? "white" : "palevioletred"};

  font-size: 1em;
  border-radius: 3px;
  border: 2px solid palevioletred;
  margin: 0 1em;
  padding: 1em 2em;
`;

const TomatoBtn = styled(Button)`
  color: tomato;
  border-color: tomato;
`

const Btn = (props: Props) => {
  console.log(props);
  return (
    <div>
      <TomatoBtn as="a" href="/" {...props}>{props.data}</TomatoBtn>
      <Button {...props}>{props.data}</Button>
    </div>
  );
}

export default Btn;
```

- 上記の例は`<a>`タグを指定して`TomatoBtn`をアンカーリンクにしている

- `as`にはカスタムコンポーネントも指定することができる

```typescript
import React from 'react';
import styled from 'styled-components'

interface Props {
  data: string;
  primary?: any;
}

const Button = styled.button`
  background: ${(props: Props) => props.primary ? "palevioletred" : "white"};
  color: ${(props: Props) => props.primary ? "white" : "palevioletred"};

  font-size: 1em;
  border-radius: 3px;
  border: 2px solid palevioletred;
  margin: 0 1em;
  padding: 1em 2em;
`;

const ReversedButton = (props: any) => <Button {...props} children={props.children.split('').reverse()} />

const Btn = (props: Props) => {
  console.log(props);
  return (
    <div>
      <Button as={ReversedButton} {...props}>Custom Button with Normal Button styles</Button>
      <Button {...props}>{props.data}</Button>
    </div>
  );
}

export default Btn;
```

- 続き: https://styled-components.com/docs/basics#styling-any-component