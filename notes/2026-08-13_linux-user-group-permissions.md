今日のまとめ
① ユーザーを操作する
useradd
→ ユーザーを作る

usermod
→ ユーザーの設定を変更する

userdel
→ ユーザーを削除する例：

usermod -d /home/newhome userA→ userAのホームディレクトリを変更

② グループを操作する
groupadd
→ グループを作る

groupmod
→ グループの設定を変更する

groupdel
→ グループを削除する例：

groupmod -n groupB groupA→ groupAの名前をgroupBに変更

③ ファイル・ディレクトリを操作する
chown
→ ファイルの所有者・グループを変更

chgrp
→ ファイルのグループを変更

chmod
→ ファイルのアクセス権を変更今回の問題：

chown userA:groupA file1→ file1の所有者をuserA、グループをgroupAに変更

一番大事な考え方
迷ったら、
「何を変更するの？」を見る。
ユーザーを変更？
    ↓
  usermod

グループを変更？
    ↓
 groupmod

ファイルの所有者を変更？
    ↓
  chown

ファイルのグループを変更？
    ↓
  chgrp

ファイルの権限を変更？
    ↓
  chmod

chmod のアクセス権
r = read    = 4
w = write   = 2
x = execute = 1そして、
owner : group : otherの3者それぞれに権限があります。
例えば、

chmod 755 file1なら、
owner → rwx = 7
group → r-x = 5
other → r-x = 5

今日の最重要ポイント
usermod と chown を混同しない。
usermod
→ 「ユーザー自身の設定」を変更

chown
→ 「ファイルの持ち主」を変更chown userA:groupA file1 の userAは変更対象ではなく、file1の新しい所有者です。
これが分かれば、今日やったコマンドがかなり一本につながります。
