TIL: Linuxのワイルドカードとファイル操作

今日は、Linuxで複数のファイルをまとめて指定・操作する方法を学んだ。

*：0文字以上の任意の文字
ls *.txt → .txt ファイルを一覧表示

?：任意の1文字
ls data?.csv → data1.csv、dataA.csv など

[1] / [!1]：特定の文字に一致／除外
ls report[1]* → report1.txt
ls report[!1]* → report1.txt 以外の該当ファイル

{1,2}：複数パターンをまとめて指定
ls report{1,2}*

ファイル名にスペースを入れる場合は \ でエスケープする
touch file\ with\ spaces.txt

ワイルドカードは mv や rm にも使える
mv *.csv csv_files/
rm *.txt

ポイント

ワイルドカードを使うと、複数ファイルを効率よく一括操作できる。
