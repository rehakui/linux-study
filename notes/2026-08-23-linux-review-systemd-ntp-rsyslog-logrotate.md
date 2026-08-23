# TIL - 2026-08-23 Linux復習

## 今日の復習内容

### 1. SysVinit と systemd の違い

SysVinit では、起動処理を主に「ランレベル」「rcスクリプト」「シンボリックリンク」で管理する。

```text
init
↓
/etc/inittab
↓
/etc/rc.d/rc.sysinit
↓
/etc/rc.d/rc
↓
/etc/rc.d/rc3.d/
↓
Sxxサービス名
↓
/etc/rc.d/init.d/配下の実体
```

`rc3.d` などにある `Sxx...` は実体ではなく、`init.d` 配下の起動スクリプトへのシンボリックリンクになっている。

現在の AlmaLinux では主に systemd を使うため、SysVinit 時代と同じ `/etc/rc.d/rc3.d/` などが存在しない場合がある。

systemd では概念的に次のように管理する。

```text
systemd
↓
target
↓
.service
↓
デーモン
```

重要な違いは、SysVinit が主に「起動順序」をスクリプト名などで管理していたのに対し、systemd は Unit 同士の依存関係を使って管理する点。

---

### 2. `/var` の役割

`/var` は variable の略で、システム稼働中に内容が変化するデータを置く。

主な例:

```text
/var/log    ログ
/var/lib    サービスの状態データ
/var/cache  キャッシュ
/var/spool  処理待ちデータ
/var/tmp    一時ファイル
```

覚え方:

```text
/etc   = 設定
/usr   = プログラムや共有データ
/var   = 動作中に増減・変化するデータ
/home  = ユーザーのデータ
```

---

### 3. `rpm` と `grep` の使い方

次のように書くと、

```bash
grep -i chrony /usr/bin/rpm
```

`/usr/bin/rpm` という実行ファイルの中から文字列 `chrony` を探すことになる。

インストール済みパッケージの中から `chrony` を探したい場合は、

```bash
rpm -qa | grep -i chrony
```

とする。

意味:

```text
rpm -qa
↓
インストール済みRPMパッケージを全部表示
↓
grep -i chrony
↓
chrony を含む行だけ表示
```

個別に確認するなら、

```bash
rpm -q chrony
```

でもよい。

---

### 4. ntpd と chronyd

NTP は「ネットワーク経由で時刻を同期する仕組み」。

`ntpd` と `chronyd` は、その NTP を利用する別々のデーモン。

```text
NTP
├── ntpd
└── chronyd
```

同じ Linux 上で両方を同時に時刻同期担当として動かすのではなく、通常はどちらか一方を使う。

一方で、両者は NTP という共通プロトコルを使うため、別マシン間では互換性がある。

例:

```text
NTPサーバー: ntpd
        ↓ NTP
NTPクライアント: chronyd
```

逆も可能。

---

### 5. chrony の構成

名前が似ているので区別する。

```text
chrony   = パッケージ・ソフトウェア一式
chronyd  = 実際に常駐して時刻同期するデーモン
chronyc  = chronyd の状態確認・操作を行うコマンド
```

chronyd の設定ファイル:

```text
/etc/chrony.conf
```

参照する NTP サーバーは `server` や `pool` で指定する。

例:

```text
server ntp.example.com iburst
```

`iburst` は初回同期を高速化するためのオプション。

---

### 6. chronyd の設定変更の流れ

基本の流れは次の通り。

```text
1. 設定を見る
   cat /etc/chrony.conf

2. 実際の参照先を見る
   chronyc sources

3. 設定を変更する
   vi /etc/chrony.conf

4. 設定を反映する
   systemctl restart chronyd

5. サービスの状態を確認する
   systemctl status chronyd

6. 参照先が変わったか確認する
   chronyc sources
```

設定ファイルを書き換えただけでは、すでに動作中のデーモンが自動で新設定を使うとは限らない。

そのため、再起動などで設定を反映する。

---

### 7. systemd / systemctl / service / daemon の関係

関係は次のように整理できる。

```text
systemd
↓ 管理
chronyd.service
↓ 起動
chronyd
```

- `systemd`
  - Linux のサービス管理を行う親玉
  - PID 1 として動作する
- `chronyd.service`
  - systemd に「chronyd をどう起動するか」を伝える Unit 設定
- `chronyd`
  - 実際に時刻同期をするデーモン
- `/etc/chrony.conf`
  - chronyd 自身の動作設定

`systemctl` は systemd に命令を出すためのコマンド。

```bash
systemctl start chronyd
systemctl stop chronyd
systemctl restart chronyd
systemctl status chronyd
systemctl enable chronyd
systemctl disable chronyd
```

覚え方:

```text
systemctl = systemd を操作する窓口
```

---

### 8. `TZ` と `/etc/localtime`

`/etc/localtime` はシステム全体のデフォルトのタイムゾーン設定で、永続的。

環境変数 `TZ` はプロセス単位で一時的にタイムゾーンを上書きできる。

例:

```bash
TZ=UTC date
```

この場合、その `date` コマンドでは `/etc/localtime` より `TZ=UTC` が優先される。

重要なのは「優先順位」と「永続性」は別ということ。

```text
優先順位:
TZ > /etc/localtime

永続性:
TZ < /etc/localtime
```

覚え方:

```text
/etc/localtime = デフォルト設定
TZ             = 一時的な上書き
```

---

### 9. syslog と rsyslog

従来の syslog を改良し、信頼性を高めたものが rsyslog。

教材では、

```text
syslog
- UDP
- 信頼性を確保しない
- 通信速度が速い

rsyslog
- TCP
- 信頼性を確保
- 通信速度はUDPより遅い
```

という整理。

rsyslog の設定ファイル:

```text
/etc/rsyslog.conf
```

rsyslogd は設定をもとにログの送り先を決める。

---

### 10. rsyslog のフィルタリング

rsyslog にはログを選別する仕組みがある。

基本的な考え方:

```text
ファシリティ.プライオリティ    アクション
```

- ファシリティ
  - 何のサービス・発信源のログか
- プライオリティ
  - ログの重要度
- アクション
  - どこへ出力するか

例:

```text
authpriv.*    /var/log/secure
```

これは、

```text
認証関係のログ
+
すべての重要度
↓
/var/log/secure に出力
```

という意味。

---

### 11. rsyslog とデータベース保存について

今日確認した教材では、rsyslog の特徴として以下は説明されていた。

- TCP に対応
- 信頼性を高めている
- ログをファイルへ保存
- 端末へ出力
- 別サーバーへ転送
- ファシリティやプライオリティでログを選別

一方、「rsyslog はデータベースにログを保存できる」という点は、今日確認した教材には明示されていなかった。

教材内で「独自のデータベースに保存」と明記されていたのは systemd-journald の方。

そのため、小テストで rsyslog の「データベース保存」が正解になっていた点は、教材だけでは導きにくい内容だった。

---

### 12. ログローテーション

ログファイルを放置すると、

```text
messages
```

がどんどん大きくなってしまう。

そこでログローテーションを使う。

例:

```text
現在:
messages

1回目:
messages    → messages.1
新しい messages を作成

2回目:
messages.1  → messages.2
messages    → messages.1
新しい messages を作成
```

世代管理すると、

```text
messages
messages.1
messages.2
messages.3
...
```

となる。

ただし、単にリネームするだけではディスク使用量は減らない。

肥大化対策になる本当の理由は、

- 一定世代を超えた古いログを削除する
- 古いログを圧縮することがある
- 現在のログを新しいファイルへ切り替える

から。

要するに、

```text
ログローテーション
= 世代管理 + 古いログの削除/圧縮
```

と理解する。

---

## 今日の要点

今日の復習で特に重要だったもの。

```text
systemctl
= systemd を操作するコマンド

chrony / chronyd / chronyc
= パッケージ / デーモン / 操作コマンド

/etc/chrony.conf
= chronyd の設定

TZ
= /etc/localtime を一時的に上書きできる

rsyslog
= ログを条件で振り分けられる

ログローテーション
= 古いログを世代管理し、最終的に削除・圧縮する
```

