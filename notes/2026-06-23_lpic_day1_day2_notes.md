# LPIC学習メモ Day1〜Day2

## 学習書籍

書籍名：1週間でLPICの基礎が学べる本

進捗状況：

* Day1 完了
* Day2 完了（1-4 オンラインマニュアルまで）
* 現在：Day2 1-4まで完了

---

## Day1

### Linuxとは

* LinuxはUNIXの改良版ではない
* UNIXの設計思想を参考にして作られた「UNIXライクOS」
* LinuxカーネルはLinus Torvalds氏によってゼロから開発された

### GNU GPL

Linuxカーネルが採用しているオープンソースライセンス。

**GNU GPL（General Public License）**

特徴

* ソースコードの閲覧が可能
* ソースコードの改変が可能
* 再配布が可能
* 改変して再配布する場合もGPLを継承する必要がある

---

## Day2

### オンラインマニュアル（man）

Linuxでは `man` コマンドを使ってオンラインマニュアルを参照できる。

例

```bash
man ls
```

### オンラインマニュアルのセクション

同じ名前でも用途が異なる場合があるため、マニュアルは種類ごとに分類されている。

主なセクション

| セクション | 内容           |
| ----- | ------------ |
| 1     | 一般ユーザー向けコマンド |
| 5     | 設定ファイル形式     |
| 8     | システム管理コマンド   |

例

```bash
man 1 passwd
```

→ passwdコマンドの説明

```bash
man 5 passwd
```

→ `/etc/passwd` ファイルの説明

### whatisコマンド

コマンドの概要を1行で表示する。

例

```bash
whatis ls
```

出力例

```text
ls (1) - list directory contents
```

### passwdの例

```text
passwd(1) - modify a user's password
passwd(5) - format of the password file
```

#### passwd(1)

* パスワード変更コマンド
* セクション1（一般コマンド）

#### passwd(5)

* `/etc/passwd` ファイルの形式説明
* セクション5（設定ファイル）

同じ名前でもセクション番号によって意味が異なる。

### 関連コマンド

```bash
man ls
```

詳細なマニュアルを表示

```bash
whatis ls
```

コマンド概要を1行表示

```bash
apropos password
```

キーワードに関連するコマンドを検索

---

## 学んだこと

* LinuxはUNIXライクOSであり、UNIXそのものではない
* LinuxカーネルはGNU GPLライセンスを採用している
* manコマンドでオンラインマニュアルを参照できる
* manにはセクション番号が存在する
* whatisでコマンド概要を素早く確認できる
* 同じ名前の項目でもセクション番号によって意味が異なる

