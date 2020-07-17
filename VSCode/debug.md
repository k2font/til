# デバッグ機能
### 初期設定
- `/.vscode/launch.json` を作成する
- デバッグビューで「launch.jsonを作成する」ボタンを押すと自動的に作成される

<img width="333" alt="スクリーンショット 2020-07-17 11 09 43" src="https://user-images.githubusercontent.com/6561417/87740978-6bb7c080-c81e-11ea-8201-06029881b258.png">


### デバッグを行う
- 再生ボタン `Run Debugging` を押すとデバッグが開始される
- デバッグ中はウインドウ下部の色が変わる
  - 色が変わらない場合は `launch.json` の設定が間違っているので見直す

<img width="1279" alt="スクリーンショット 2020-07-17 11 12 01" src="https://user-images.githubusercontent.com/6561417/87740984-6e1a1a80-c81e-11ea-8513-2db114439301.png">

- 標準入力を受け付けるためには、 `launch.json` のconfigを設定変更する。`externalConsole` を `true` にする。
```json
{
  "version": "0.2.0",
  "configurations": [
    {
      "name": "g++ - アクティブ ファイルのビルドとデバッグ",
      "type": "cppdbg",
      "request": "launch",
      "program": "${fileDirname}/${fileBasenameNoExtension}",
      "args": [],
      "stopAtEntry": false,
      "cwd": "${workspaceFolder}",
      "environment": [],
      "externalConsole": true, // この設定をtrueにすると別ウインドウでターミナルが立ち上がり、標準入力を受け付けるようになる
      "MIMode": "lldb",
      "preLaunchTask": "C/C++: g++ build active file"
    }
  ]
}
```

- ブレークポイントを設定し、ステップ実行して変数の中身を見るなどデバッグを行う
- 使いこなせるかどうかはk2font次第