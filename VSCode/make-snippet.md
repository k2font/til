# スニペットを作成する
### 概要
- VSCodeはスニペットを登録できる
- スニペット...たとえばCSVファイルを読み込む処理を `csv-reader` と入力するだけで一発で表示できる機能
- VSCodeはこの処理を自前でカスタマイズできる

### 登録方法
- コマンドパレット `⌘ + Shift + P` を開いて、 `Configure User Snippets` と入力する
- 登録したい言語を選択する
- `*.json`にスニペットを登録する

```json
"CSVリーダー": {
  "description": "CSVリーダーを追加",
  "prefix": "csv-reader",
  "body": [
    "${1:csvReader} := csv.NewReader(${2:reader})",
    "for {",
    "\trecord, err := $1.Read()",
    "\tif err != nil {",
    "\t\tbreak",
    "\t}",
    "\t$0",
    "}",
  ]
}
```

- descriptionにはこのスニペットの説明を記載する
- prefixには、実際にショートカットとして使用される文字列を追記する
- bodyはprefixに紐づくスニペット。`\t`はタブを表す
- これ書くのめんどくさすぎない???
- 簡単に書けるツールがあれば超便利だな〜。