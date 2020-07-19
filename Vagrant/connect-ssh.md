# VagrantでホストしたVMにSSHで接続する
- `vagrant ssh` でもいいが、たとえばVSCodeのリモートアクセス機能を用いてコードを書きたい時などに普通に `ssh` したいと思う時がある
- `vagrant ssh-config`の情報を用いて`ssh`すればOK。

### 手順
1. `Vagrantfile`の以下行のコメントアウトを外す
2. `config.vm.network "private_network", ip: "192.168.33.10"`
3. `vagrant ssh-config`でsshの接続情報を表示する
4. `vagrant ssh-config --host 192.168.33.10 >> ~/.ssh/config`を実行して、sshのconfigに**vagrantのssh-config**を設定する。
5. `ssh 192.168.33.10`する

### vagrant ssh-configは何を表示しているのか？
- ssh構成ファイルを出力するコマンド
- https://www.vagrantup.com/docs/cli/ssh_config.html