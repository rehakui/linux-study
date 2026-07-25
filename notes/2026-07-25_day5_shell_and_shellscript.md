# TIL - 2026-07-25

## 学習教材

### 『1週間でLPICの基礎が学べる本』

**Day5**

### 1. シェル
- 1-1 シェルの役割と基本を知ろう
- 1-2 シェルの便利な機能
- 1-3 シェル変数と環境変数

### 2. シェルスクリプト
- 2-1 シェルスクリプトの基本
- 2-2 引数と終了ステータス

---

# 学習内容

## シェルとは

- シェルはユーザーから入力されたコマンドを解釈し、Linuxカーネルへ処理を依頼するプログラム。
- ターミナルで入力したコマンドはシェルによって実行される。

---

## シェルスクリプト

シェルで実行するコマンドをファイルにまとめたもの。

例

```bash
#!/bin/bash

echo "Hello $1."
```

実行

```bash
bash test/diskmem.sh haruka
```

結果

```text
Hello haruka.
```

### 引数

```bash
$1
```

は1番目の引数を表す。

例えば

```bash
bash test/diskmem.sh haruka
```

では

```text
$1 = haruka
```

となる。

---

# 今日理解できたこと

## 相対パスと絶対パス

Linuxでは同じファイルでも書き方が2種類ある。

### 相対パス

```bash
test/diskmem.sh
```

現在いるディレクトリを基準に探す。

例えば現在地が

```text
/home/haruka
```

なら

```text
/home/haruka/test/diskmem.sh
```

を指す。

---

### 絶対パス

```bash
/test/diskmem.sh
```

先頭に「/」が付くと**ルートディレクトリ**から探す。

つまり

```text
/
└── test
```

を探す。

今回

```bash
cat /test/diskmem.sh
```

がエラーになった理由は、

```text
/test
```

というディレクトリが存在しなかったため。

実際に存在していたのは

```text
/home/haruka/test
```

だった。

---

## lsで確認したこと

```bash
ls
```

現在のディレクトリを見る。

```bash
ls test
```

現在のディレクトリ内の`test`を見る。

```bash
ls /test
```

ルートディレクトリ直下の`test`を見る。

今回の結果

```text
/home/haruka
├── diskmem.sh
└── test
    ├── diskmem.sh
    ├── sample.py
    └── ...
```

この結果から、`/`が付くかどうかで探す場所が変わることを理解できた。

---

## vim・cat・bash の違い

どれも**パスの解釈は同じ**。

違うのは処理内容だけ。

| コマンド | 役割 |
|----------|------|
| `vim test/diskmem.sh` | ファイルを編集する |
| `cat test/diskmem.sh` | ファイルの内容を表示する |
| `bash test/diskmem.sh` | シェルスクリプトとして実行する |

---

## リダイレクト（>>）

スクリプト

```bash
#!/bin/bash

echo "Hello $1."

date >> diskmem.sh
echo "-----------------------------------" >> diskmem.sh
```

実行すると

```bash
bash test/diskmem.sh haruka
```

画面には

```text
Hello haruka.
```

だけ表示される。

これは

```bash
date >> diskmem.sh
```

が

> `date` の結果を画面ではなくファイルへ追記する

という意味だから。

確認するには

```bash
cat diskmem.sh
```

を実行すると、日時が追加されていることが分かった。

---

## 気付き

今回は

```text
/home/haruka
├── diskmem.sh
└── test
    └── diskmem.sh
```

という構成になっていたため、

どちらの`diskmem.sh`なのか混乱した。

ログファイルは

```text
diskmem.log
```

のように別名にした方が分かりやすい。

---

# 今日の学び

- シェルはLinuxへ命令を伝える役割を持つ。
- シェルスクリプトで処理を自動化できる。
- `$1`は1番目の引数を表す。
- 相対パスは現在地基準、絶対パスはルート(`/`)基準。
- `/`が付くかどうかで探す場所が大きく変わる。
- `vim`・`cat`・`bash`は同じパス指定ルールで動作し、役割だけが異なる。
- `>>`は画面表示ではなくファイルへ追記するリダイレクト。
- スクリプトとログは別ファイル名にすると管理しやすい。
