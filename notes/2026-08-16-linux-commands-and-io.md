# 2026-08-16 Linuxコマンドと標準入出力

## パイプ `|` とリダイレクト `>` の違い

### `|` パイプ

左側のコマンドの**標準出力**を、右側のコマンドの**標準入力**へ渡す。

```bash
ls | sort
```

イメージ：

```text
ls の標準出力
      ↓
      |
      ↓
sort の標準入力
```

パイプは「引数を渡す」のではなく、**データを標準入力として渡す**。

そのため、

```bash
ls | > directory_list.txt
```

では、パイプの右側に標準入力を処理するコマンドがないため、期待した動作にならない。

ファイルへ保存したい場合は、

```bash
ls > directory_list.txt
```

とする。

### `>` リダイレクト

コマンドの標準出力をファイルへ送る。

```bash
ls > directory_list.txt
```

```text
ls
 ↓ 標準出力
directory_list.txt
```

### `>>` 追記

既存の内容を消さずに、末尾へ追加する。

```bash
ls >> result.txt
```

`>` は上書き、`>>` は追記。

### `2>` 標準エラー出力

エラーメッセージだけをファイルへ保存する。

```bash
cat nonexistent.txt 2> error.txt
```

---

## `cat` と標準入力

通常、

```bash
cat sample.txt
```

とすると、`cat`自身が`sample.txt`を読み込んで標準出力へ表示する。

```text
sample.txt
    ↓
   cat
    ↓
標準出力
```

一方、

```bash
cat
```

のようにファイルを指定しない場合、`cat`は**標準入力**からデータを読み込む。

ターミナルから実行した場合、標準入力はキーボードにつながっているため、入力待ちになる。

```text
キーボード
    ↓
標準入力
    ↓
   cat
    ↓
標準出力
    ↓
   画面
```

`Ctrl + D`でEOF（入力終了）を伝えると`cat`が終了する。

### `cat > sample.txt`

```bash
cat > sample.txt
```

の場合、`cat`は標準入力からキーボード入力を受け取る。

`>`によって標準出力の行き先がファイルへ変更される。

```text
キーボード
    ↓
標準入力
    ↓
   cat
    ↓
標準出力
    ↓
sample.txt
```

そのため、キーボードで入力した内容をファイルへ保存できる。

---

## `sort -b`

`-b`は、**行頭の空白を無視してソートする**。

例：

```text
  banana
apple
    cherry
```

```bash
sort -b sample.txt
```

比較するときに先頭の空白を無視するため、

```text
apple
  banana
    cherry
```

のように文字列部分を基準に並べ替えられる。

```text
-b = leading blanksを無視
```

---

## `sort -f`

`-f`は、**大文字と小文字を区別せずにソートする**。

例：

```text
banana
Apple
cherry
apple
```

```bash
sort -f sample.txt
```

比較時には、

```text
Apple → apple
```

のように、大文字と小文字を同じものとして扱う。

```text
-f = fold case
```

---

## 圧縮とアーカイブの違い

Linuxでは、**圧縮**と**アーカイブ**は別の処理。

### 圧縮

```text
gzip   → .gz
bzip2  → .bz2
xz     → .xz
```

ファイルのデータ量を小さくする。

### アーカイブ

```text
tar
cpio
```

複数のファイルやディレクトリを1つにまとめる。

---

## `gzip`

ファイルを圧縮する。

```bash
gzip sample.txt
```

```text
sample.txt
    ↓
sample.txt.gz
```

展開：

```bash
gunzip sample.txt.gz
```

`gzip`はGNU zipと関連付けて覚える。

---

## `bzip2`

ファイルを圧縮する。

```bash
bzip2 sample.txt
```

```text
sample.txt
    ↓
sample.txt.bz2
```

展開：

```bash
bunzip2 sample.txt.bz2
```

---

## `xz`

ファイルを圧縮する。

```bash
xz sample.txt
```

```text
sample.txt
    ↓
sample.txt.xz
```

展開：

```bash
unxz sample.txt.xz
```

---

## `tar`

複数のファイルを1つのアーカイブにまとめる。

`tar`は**tape archive**が由来。

```bash
tar -cf archive.tar directory/
```

```text
c = create
f = file
```

展開：

```bash
tar -xf archive.tar
```

```text
x = extract
```

### `.tar.gz`

`.tar.gz`は、

```text
複数ファイル
    ↓
tarでまとめる
    ↓
.tar
    ↓
gzipで圧縮
    ↓
.tar.gz
```

という意味。

```bash
tar -czf archive.tar.gz directory/
```

```text
c = create
z = gzip
f = file
```

同様に、

```text
.tar.bz2 → tar + bzip2
.tar.xz  → tar + xz
```

となる。

---

## `cpio`

`cpio`もアーカイブを作成・展開するコマンド。

名前は、

```text
copy in / copy out
```

と関連付けて覚える。

アーカイブ作成：

```bash
ls | cpio -o > archive.cpio
```

```text
ls
 ↓
ファイル名一覧
 ↓
cpio -o
 ↓
archive.cpio
```

展開：

```bash
cpio -i < archive.cpio
```

```text
-o → copy out
-i → copy in
```

`cpio`は標準入力からファイル名の一覧を受け取るため、パイプやリダイレクトの理解にもつながる。

---

## `tail -f`

`tail`はファイルの末尾を表示する。

```bash
tail logfile
```

`-f`を付けると、ファイルへ新しく追加された内容を追跡し続ける。

```bash
tail -f app.log
```

```text
-f = follow
```

ログに新しい内容が追加されると、その内容がリアルタイムで表示される。

主にログ監視で使用する。

終了するときは、

```text
Ctrl + C
```

を使用する。

---

## 今日の重要ポイント

```text
|   → 標準出力を次のコマンドの標準入力へ渡す
>   → 標準出力をファイルへ上書き
>>  → 標準出力をファイルへ追記
2>  → 標準エラー出力をファイルへ保存

cat
→ 引数がなければ標準入力を読む

sort -b
→ 行頭の空白を無視

sort -f
→ 大文字・小文字を区別しない

gzip / bzip2 / xz
→ 圧縮

tar / cpio
→ アーカイブ

tail -f
→ ファイルへの追記を追跡する
```

特に、Linuxコマンドは「コマンド名を暗記する」だけでなく、

```text
標準入力 → コマンド → 標準出力
```

というデータの流れを意識すると、パイプやリダイレクトの動作を理解しやすい。
