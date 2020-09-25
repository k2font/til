# `git pull`時にマージコミットを作らないようにする

### 前提
- ちゃんと理解するには`merge` `fetch` `rebase`を理解しておく必要があるよ
  - めっちゃかんたんに説明すると...
    - `git fetch`: リモート追跡ブランチにリモートリポジトリからブランチを引っ張ってくる(ローカルのブランチにマージしない)
      - リモート追跡ブランチとは `origin/master`みたいなoriginと名の付くもの
    - `git merge`: ブランチAとブランチBの変更を統合する
    - `git rebase`: 実行したブランチのコミットを一旦取り除く + 保存し、`master`ブランチの先頭に移動する

### 概要
- 軽率に `git pull` を実行するとマージコミットができることがある
  - localの `master` ブランチのコミット一覧が、リモートリポジトリの `remote/origin/master` に存在しないときにマージコミットが生まれる。
  - 普通は発生しないかもしれない。しかし、 リモートの `master` ブランチの変更を取り込まずにローカルで`git merge`とか変なことすると発生する可能性がある

### じゃあどうするか
- 以下のコマンドを叩く
  
	`git fetch && git rebase origin/master`
  - そもそも`git pull`は`git fetch` + `git merge`のような挙動を取る。
  - `git push`の対は`git pull`ではない。正しくは`git fetch`
- もしくは以下でOK

	`git pull --rebase`
	- `git fetch` + `git rebase` を行ってくれるコマンド

- `rebase`すると履歴がきれいになる

### 参考リンク
- https://kray.jp/blog/git-pull-rebase/