# TIL: Linuxのユーザー・所有権・アクセス権

## 今日の重要ポイント

今日一番意識したことは、

> **「誰の・何を・どうする？」**

を最初に考えること。

似たコマンドを暗記するのではなく、操作対象から判断する。

| やりたいこと         | コマンド       |
| -------------- | ---------- |
| ユーザーを作る        | `useradd`  |
| ユーザーを変更する      | `usermod`  |
| ユーザーを削除する      | `userdel`  |
| グループを作る        | `groupadd` |
| グループを削除する      | `groupdel` |
| ファイルの所有者を変更    | `chown`    |
| ファイルの所有グループを変更 | `chgrp`    |
| アクセス権を変更       | `chmod`    |

## 混同しやすい3つ

```bash
chown   # 所有者を変更
chgrp   # 所有グループを変更
usermod # ユーザーの設定を変更
```

### chown

```bash
chown yuko /home/ope
```

`/home/ope` の**所有者**を `yuko` に変更する。

### chgrp

```bash
chgrp opegrp /home/ope
```

`/home/ope` の**所有グループ**を `opegrp` に変更する。

### usermod

```bash
usermod -G opegrp openuser
```

`openuser` の補助グループを `opegrp` に設定する。

`-G` だけでは既存の補助グループが置き換わるため、既存グループを残して追加したい場合は `-aG` を使う。

```bash
usermod -aG opegrp openuser
```

## 「ユーザー」と「所有者」は違う

**ユーザー**
→ Linuxに登録されているアカウント。

**所有者**
→ ファイルやディレクトリを所有しているユーザー。

つまり、

> 所有者は「ファイルに対するユーザーの立場」

と考える。

## chmod の数字

```text
r = 4
w = 2
x = 1
```

権限は数字を足して表す。

```text
rwx = 7
rw- = 6
r-x = 5
r-- = 4
--x = 1
```

例えば、

```bash
chmod 751 /home/ope
```

なら、

```text
7 → rwx
5 → r-x
1 → --x
```

となる。

## `>` と `>>`

```text
>   上書き
>>  末尾に追加
```

### 上書き

```bash
date > today
```

`date` の結果を `today` に書き込む。

### 追加

```bash
cat today >> test_result.txt
```

`today` の内容を `test_result.txt` の末尾に追加する。

## userdel

```text
-f → force（強制）
-r → ホームディレクトリなども削除
```

特に `-f` と `-r` を混同しないようにする。

## tar

最低限、以下を覚える。

```bash
tar -czvf archive.tar.gz dir/
tar -xzvf archive.tar.gz
```

```text
c → create
x → extract
z → gzip
v → 処理内容を表示
f → アーカイブファイルを指定
```

## vi

```text
i    → 挿入モード
Esc  → ノーマルモードへ戻る
:wq  → 保存して終了
```

## 今日の気づき

コマンドそのものが分からないというより、

* `chown` と `chgrp`
* ユーザーと所有者
* `usermod` とグループ
* `userdel -f` と `-r`

のような、**似た概念の境界で迷いやすい**ことが分かった。

今後は問題を見たら、いきなりコマンドを思い出そうとせず、

1. **誰・何が対象か？**
2. **何をしたいのか？**
3. **どのコマンドが対応するか？**

の順番で考える。

今日のキーワードは、

> **「誰の・何を・どうする？」**

