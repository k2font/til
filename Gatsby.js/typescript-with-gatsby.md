# TypeScriptとGatsby.js
- https://www.gatsbyjs.org/docs/typescript/
- Gatsby.jsはTypeScriptをデフォルトでサポートしている
- `gatsby-plugin-typescript` プラグインが必要だが、これはインストールせずとも存在する
- あとは `.js` と `.tsx` にするだけで大丈夫。

### 注意
- ビルド中に型チェックを実行しない
- 公式ドキュメントには `Visual Studio Code is very good in this regard.` とだけ書かれていてよくわからない。追加調査する。
- `gatsby-personal-starter-blog` を用いた場合、クラスコンポーネントで構成されいる。このまま `.tsx` に変更するだけでも動作するが、関数コンポーネントに書き直したほうがTypeScriptの扱いが楽(Hooksで機能追加したい場合には関数コンポーネントにする必要がある)。
- もし関数コンポーネントに変更するには、propsの型を `pageProps` にする必要がある

```jsx
import React from "react"
import { PageProps } from "gatsby"
export default function IndexRoute(props: PageProps) {
  return (
    <>
      <h1>Path:</h1>
      <p>{props.path}</p>
    </>
  )
}
```