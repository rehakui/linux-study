# Linux学習メモ — 2026-08-30

## 1. シェル変数・環境変数

### `export` と `declare`
| コマンド | 役割 |
|---|---|
| `VAR=value` | シェル変数を設定 |
| `declare VAR=value` | 変数を定義・属性を設定 |
| `export VAR=value` | 環境変数として子プロセスへ引き継ぐ |
| `declare -x VAR=value` | `export` と同様に export 属性を付ける |

- `export`：子プロセスへ渡したい変数に使う
- `declare`：整数・配列・読み取り専用など、変数の属性設定に使う

### `env`
一時的に環境変数を設定してコマンドを実行できる。

```bash
env LANG=C command
```

---

## 2. 正規表現とシェルのメタキャラクタ

| 意味 | シェル | 正規表現 |
|---|---|---|
| 0文字以上の任意の文字列 | `*` | `.*` |
| 任意の1文字 | `?` | `.` |
| a,b,c のどれか1文字 | `[abc]` | `[abc]` |
| a,b,c 以外の1文字 | `[!abc]` | `[^abc]` |
| 複数候補 | `{abc,xyz}` | `(abc\|xyz)` ※EREでは `(abc|xyz)` |
| 行頭 | - | `^` |
| 行末 | - | `$` |
| 特殊文字の意味を打ち消す | `\` | `\` |

### 覚え方
- 正規表現の `.` = 任意の **1文字**
- `*` = 直前の要素を **0回以上**
- `+` = 直前の要素を **1回以上**

したがって、

```text
.*  = 0文字以上の任意の文字列
.+  = 1文字以上の任意の文字列
```

---

## 3. シェルスクリプトの実行方法

| 実行方法 | 実行場所 | 現在のシェルへの影響 |
|---|---|---|
| `bash script.sh` | 新しい bash | 残らない |
| `source script.sh` | 現在のシェル | 残る |
| `. script.sh` | 現在のシェル | 残る |
| `./script.sh` | 別プロセス | 残らない |

### `source` と `.`
Bashではほぼ同じ。

- `source`：Bash系でよく使う
- `.`：POSIX標準で移植性が高い

### `./` の意味
`./` は **カレントディレクトリ**。

```bash
./test.sh
```

は「現在のディレクトリにある `test.sh` を実行する」という意味。

相対パス・絶対パスも指定できる。

```bash
../test.sh
/home/user/test.sh
```

---

## 4. `PATH`

`PATH` は、**コマンドを探すディレクトリの一覧**。

例：

```bash
/home/haruka/.local/bin:/home/haruka/bin:/usr/local/bin:/usr/bin:/usr/local/sbin:/usr/sbin
```

`:` 区切りで、左から順番に検索される。

```bash
ls
```

と入力すると、シェルは `PATH` 内から `ls` を探す。

カレントディレクトリ `.` が `PATH` に含まれていない場合、

```bash
test.sh
```

ではなく、

```bash
./test.sh
```

と指定する。

---

## 5. `test` コマンド

### 文字列
| オプション | 意味 | 覚え方 |
|---|---|---|
| `-z STRING` | 長さが0なら真 | z = zero |
| `-n STRING` | 長さが0でなければ真 | n = non-zero |

例：

```bash
name=""

if test -z "$name"; then
    echo "空です"
fi
```

---

## 6. 関数

例：

```bash
function entername()
{
    echo -n "enter your name: "
    read name
}
```

呼び出し：

```bash
entername
```

while内で再び `entername` を呼ぶことで、次の入力を受け取り `$name` を更新できる。

---

## 7. `$0` と `$#`

### `bash args.sh`
```text
$0 = args.sh
$# = 引数の個数
```

### `. args.sh`
現在のシェルで実行するため、

```text
$0 = -bash
```

のように、現在のシェル名のまま。

例：

```bash
bash args.sh aaa bbb
```

```text
$0 = args.sh
$# = 2
$1 = aaa
$2 = bbb
```

---

## 8. `exec`

`exec` は、**現在のシェルプロセスを指定したコマンドのプロセスに置き換える**。

```bash
exec ./function.sh
```

通常：

```text
bash
└─ function.sh
```

`exec`：

```text
bash
↓ 置き換え
function.sh
```

- 新しい子プロセスを追加するのではない
- 元のシェルは残らない
- スクリプト終了後、ログインシェルならそのままログアウトすることがある

覚え方：

> exec = 今のプロセスを別のプログラムに入れ替える

---

## 9. プロセス・ジョブ・シグナル

### 基本用語
- 実行中のプログラムを管理する基本単位：**プロセス**

### ジョブ
バックグラウンドのジョブをフォアグラウンドへ戻す：

```bash
fg
```

### シグナル
```text
SIGSTOP = 停止
SIGCONT = 再開
```

`CONT` = **continue**

### `kill` 系
| コマンド | 指定方法 |
|---|---|
| `kill` | PID |
| `pkill` | プロセス名・パターン |
| `killall` | 同じ名前のプロセスすべて |

---

## 10. システム時刻・タイムゾーン

### システムクロック表示・設定
```bash
date
```

### デフォルトのタイムゾーン設定
```text
/etc/localtime
```

---

## 11. SysV init

### `/etc/rc.d/rc[N].d`
- `K` で始まるファイル：そのランレベルで **停止**
- `S` で始まるファイル：そのランレベルで **起動**

覚え方：

```text
K = Kill
S = Start
```

### デーモン起動用
**initスクリプト**

代表的な配置先：

```text
/etc/rc.d/init.d
```

---

## 12. ログ管理

### `logger`
タグを付ける：

```bash
logger -t TAG message
```

### `rsyslog`
特徴：
- ファイルへログ保存
- TCP / UDP 対応
- データベースへのログ保存が可能
- フィルタリング機能あり

### `journald`
ログを表示：

```bash
journalctl
```

優先度で絞り込み：

```bash
journalctl -p err
```

---

## 13. 定期実行

定期的にジョブを実行する仕組み：

```text
cron
```

---

## 14. RPM

指定したパッケージを照会するモード：

```bash
-q
--query
```

---

## 15. ファイル操作

1つのファイルを複数ファイルに分割：

```bash
split
```

---

## 16. ネットワーク・DNS

### インターネット上で使用するIPアドレス
**グローバルIPアドレス**

### DNS設定ファイル
```text
/etc/resolv.conf
```

---

## 17. FHS

`/` ファイルシステムに必要な管理用コマンドを含むディレクトリ：

```text
/sbin
```

---

## 18. ユーザー・パスワード管理

### `chage`
| オプション | 意味 | 覚え方 |
|---|---|---|
| `-I` | パスワード期限切れ後、無効化までの日数 | Inactive |
| `-E` | アカウント有効期限 | Expire |
| `-d` | パスワード最終変更日 | day / `--lastday` |

### 一時的に別ユーザーへ切り替え
```bash
su
```

現在のログインセッションを保ったまま別ユーザーへ切り替えられる。

---

## 19. `gpg`

`gpg` は OpenPGP の総合ツール。

主な用途：
- 暗号化
- 復号
- 電子署名
- 署名検証
- 鍵生成
- 鍵のインポート / エクスポート
- 公開鍵・秘密鍵の管理

覚え方：

> gpg = 暗号化 + 署名 + 鍵管理

---

## 今日の重要ポイント

1. `PATH` = コマンドの検索先一覧
2. `source` / `.` = 現在のシェルで実行
3. `bash script.sh` / `./script.sh` = 基本的に別プロセス
4. `exec` = 今のプロセスを別のプログラムへ置き換える
5. 正規表現の `.` は1文字、`.*` は0文字以上
6. `SIGCONT` = continue = 再開
7. `K = Kill`、`S = Start`
8. `journalctl` = journald のログ表示
9. `cron` = 定期実行
10. `gpg` = 暗号・署名・鍵管理
