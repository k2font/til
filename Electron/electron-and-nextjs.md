# ElectronとNext.jsを組み合わせる
- 非常に簡単なコマンドで組み合わせが完了する。
- ついでにTypeScriptも組み合わせることができる。

## 設定手順
### サンプルプロジェクトを使ってNext.jsアプリケーションをセットアップする
- 以下のコマンドを実行する

```bash
$ npx create-next-app --example with-electron-typescript ${YOUR_APP_NAME}
$ cd ${YOUR_APP_NAME}
```

### npm run build + npm run dev
- あとはbuildして実行する

```bash
$ npm run build
$ npm run dev
```

## ページの一部を修正する
- メインプロセスは `electron-src` を指す。一方でレンダラープロセスは `renderer` を指す。
- よって、ページの見た目を修正したかったら `renderer` を修正する。
- では、`renderer/pages/index.tsx` の一部を変更する

```tsx
import { useEffect } from 'react'
import Link from 'next/link'
import Layout from '../components/Layout'

const IndexPage = () => {
  useEffect(() => {
    // add a listener to 'message' channel
    global.ipcRenderer.addListener('message', (_event, args) => {
      alert(args)
    })
  }, [])

  const onSayHiClick = () => {
    global.ipcRenderer.send('message', 'hi from next')
  }

  return (
    <Layout title="Home | Next.js + TypeScript + Electron Example">
      <h1>Hello Next.js 👋</h1>
      <button onClick={onSayHiClick}>Say hi to electron</button>
      <p>
        <Link href="/about">
          <a>About</a>
        </Link>
      </p>
      <!-- 以下を追加 -->
      <p>
        これはElectronとNext.jsのテストページです。
      </p>
    </Layout>
  )
}

export default IndexPage
```

- そのまま保存すると、自動的にページが更新される。Next.jsの便利機能。

## アプリケーションのパッケージング
```
$ npm run dist
```

- `electron-builder` が起動して、ビルドが始まる
- ビルドされたアプリケーションの名前は、 `package.json` の `productName` に記載された名前が指定される。