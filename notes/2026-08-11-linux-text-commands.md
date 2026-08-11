# TIL: Linuxのテキスト操作コマンド

今日は会社のPC研修で、Linuxのファイル・テキスト操作コマンドを学習した。

## コマンド一覧

| コマンド    | 名前の由来・略      | 用途                 | 例                          |
| ------- | ------------ | ------------------ | -------------------------- |
| `cat`   | concatenate  | ファイルの内容を表示・連結する    | `cat file.txt`             |
| `nl`    | number lines | 行番号を付けて表示する        | `nl file.txt`              |
| `less`  | 略語ではない       | ファイルを1画面ずつ閲覧する     | `less file.txt`            |
| `head`  | 先頭           | ファイルの先頭部分を表示する     | `head file.txt`            |
| `tail`  | 末尾           | ファイルの末尾部分を表示する     | `tail file.txt`            |
| `cut`   | 切り出す         | 指定した列や文字を取り出す      | `cut -d ',' -f 1 file.csv` |
| `sort`  | 並べ替える        | 行を並べ替える            | `sort file.txt`            |
| `uniq`  | unique       | 重複している連続行をまとめる     | `uniq file.txt`            |
| `od`    | octal dump   | ファイルの内容を8進数などで表示する | `od file.txt`              |
| `wc`    | word count   | 行数・単語数・バイト数を数える    | `wc file.txt`              |
| `tr`    | translate    | 文字の置換・削除をする        | `tr 'a-z' 'A-Z'`           |
| `split` | 分割する         | ファイルを複数に分割する       | `split -l 100 file.txt`    |

## 覚えておきたいこと

Linuxコマンドは、`|`（パイプ）を使って組み合わせられる。

```bash
cat file.txt | sort | uniq
```

この例では、

**ファイルを表示 → 並べ替え → 重複行をまとめる**

という処理を行う。

今日学んだコマンドは、ファイルの内容を「見る・切り出す・並べる・数える・変換する・分割する」ときに使える。

