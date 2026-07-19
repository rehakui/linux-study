# パッケージ管理とSSH接続

2026-07-19

## 学習内容

- Section5
  - 演習テスト1

- Section6
  - 44. パッケージの基本
  - 45. aptコマンド
  - 46. Debian形式パッケージ（dpkg・apt）
  - 47. yumコマンド
  - 48. RPMコマンド
  - 小テスト8

---

## 問われた内容・学んだこと

### パッケージ管理とは

Linuxではソフトウェアを「パッケージ」という単位で管理する。

パッケージ管理システムを利用することで、

- インストール
- 更新
- 削除
- 依存関係の解決

を簡単に行える。

---

### 依存関係

あるソフトウェアが動作するために、他のパッケージが必要になることを**依存関係**という。

例

```
vim
 └── vim-common
 └── vim-filesystem
```

パッケージ管理ツールは必要なパッケージを自動でインストールしてくれる。

---

### 競合関係

同時にインストールできないパッケージ同士を**競合関係**という。

パッケージ管理ツールは競合も自動で判定する。

---

## aptコマンド（Debian・Ubuntu）

パッケージ更新

```bash
sudo apt update
```

パッケージのアップグレード

```bash
sudo apt upgrade
```

パッケージのインストール

```bash
sudo apt install パッケージ名
```

パッケージの削除

```bash
sudo apt remove パッケージ名
```

---

## Debian形式パッケージ

Debian系では

- dpkg
- apt

を利用する。

### dpkg

ローカルパッケージを操作する。

```bash
dpkg -i package.deb
```

### apt

依存関係も含めて管理できる。

---

## yumコマンド（AlmaLinux・CentOS）

パッケージ情報

```bash
yum info bash
```

インストール済み一覧

```bash
yum list
```

リポジトリ一覧

```bash
yum repolist
```

インストール

```bash
sudo yum install パッケージ名
```

削除

```bash
sudo yum remove パッケージ名
```

---

## RPMコマンド

RPM形式のパッケージを直接操作する。

インストール済み検索

```bash
rpm -qa
```

パッケージ情報

```bash
rpm -qi bash
```

インストールファイル一覧

```bash
rpm -ql bash
```

---

## SSH接続の確認

SSH設定ファイル

```bash
~/.ssh/config
```

設定例

```text
Host almalinux
    HostName 192.168.56.2
    User haruka

Host ubuntu
    HostName localhost
    Port 2222
    User haruka
```

接続

```bash
ssh ubuntu
```

---

## SSH公開鍵認証

公開鍵をサーバへコピー

```bash
ssh-copy-id ubuntu
```

コピー後はパスワード入力なしでログインできることを確認した。

---

## 実践

### AlmaLinux

- `yum info`
- `yum list`
- `yum repolist`
- `/etc/yum.conf`の確認

を実際に実行した。

---

### Ubuntu

SSH設定ファイルを編集し、

```bash
ssh ubuntu
```

だけで接続できるように設定した。

また、

```bash
ssh-copy-id ubuntu
```

で公開鍵認証を設定し、SSHログインを確認した。

---

## 気づき

- aptはDebian系、yumはRed Hat系で利用するパッケージ管理ツールである。
- yumは依存関係を考慮してパッケージを管理できるため便利だった。
- rpmコマンドはパッケージそのものの情報確認に適している。
- SSHのconfigを設定すると長い接続コマンドを書く必要がなくなり、接続が非常に楽になった。
- `ssh-copy-id`を利用すると公開鍵認証を簡単に設定でき、パスワード入力を省略できることを確認した。
