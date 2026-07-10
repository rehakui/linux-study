TIL（2026-07-10）
AlmaLinuxをMacからSSHで操作する環境を構築
学んだこと
VirtualBoxのコンソールは起動・障害対応用として利用する。
普段のLinux操作はMacからSSH接続して行う。
SSH接続中はMacではなくAlmaLinux上のbashが動作している。
MacのプロンプトとAlmaLinuxのプロンプトを見れば、どちらを操作しているか判断できる。
SSH公開鍵認証

Mac側

~/.ssh/
├── config
├── id_ed25519
└── id_ed25519.pub

AlmaLinux側

~/.ssh/
└── authorized_keys

公開鍵をコピー

ssh-copy-id almalinux

これでパスワードなしで

ssh almalinux

が利用できるようになった。

VirtualBoxのネットワーク構成

最終構成

Adapter1 : NAT
    └─ インターネット接続

Adapter2 : Host-only
    └─ MacとのSSH接続
IPアドレス
enp0s3
10.0.2.15
NAT用
インターネット接続
enp0s8
192.168.56.2
Host-only
MacからSSH接続するための固定IP

Macの~/.ssh/config

Host almalinux
    HostName 192.168.56.2
    User haruka
Docker

Dockerのインストールは完了。

サービス状態

systemctl status docker
active (running)

起動は成功している。

現在の課題

permission denied while trying to connect to the Docker daemon socket

→ dockerグループへの追加を次回実施する。

気付き
「今どのOSを操作しているか」を常に意識することが重要。
SSH接続中でもMacのターミナルを使っているだけで、実際にコマンドを実行しているのはAlmaLinux。
ネットワークは最初に安定させると、その後のDockerやLPICの学習がスムーズになる。
次回
harukaをdockerグループへ追加
docker ps
docker run hello-world
Docker Composeの動作確認
