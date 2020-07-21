# よく使うtsconfig.jsonの設定

```jsonc
{
  "compilerOptions": {
    "target": "ES2017",  // コンパイル後のjavascriptのバージョンを指定する
    "module": "commonjs", // npmモジュールを使用する
    "esModuleInterop": true, // npmモジュールでもimport文を使用できるようにする
    "sourceMap": true, // ソースマップを出力する
    // ※ソースマップ: JavaScriptとTypeScriptを結びつけておく定義ファイル。
    // JavaScriptを実行しながらTypeScriptをデバッグすることができるようになる

    "outDir": "./out", // コンパイル後のJavaScriptファイルの出力先を指定する
    "strict": true, // すべてのコードで型定義を強制する
    "baseUrl": "./", // TypeScriptの絶対参照時のベースパス
    "forceConsistentCasingInFileNames": true 
  }
}

```