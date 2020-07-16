# 404ページを追加する
- https://www.gatsbyjs.org/docs/add-404-page/
- 公式ドキュメントはだいぶ不親切
- カスタム404ページは `src/pages/404.js` に配置されているコンポーネントから生成される
- `gatsby-config.js` への追加記述は不要
- `gatsby develop` で開発サーバを立ち上げている場合は以下のような見た目

<img width="1486" alt="スクリーンショット 2020-07-16 12 21 39" src="https://user-images.githubusercontent.com/6561417/87622846-f5a05480-c75e-11ea-8cad-de147be67a5c.png">


- `gatsby build` して `gatsby serve` するとちゃんと表示される。

<img width="1486" alt="スクリーンショット 2020-07-16 12 21 48" src="https://user-images.githubusercontent.com/6561417/87622856-fa650880-c75e-11ea-9814-bd10513f3d1a.png">