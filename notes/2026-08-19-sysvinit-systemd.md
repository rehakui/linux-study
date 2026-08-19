SysVinit と systemd の流れ

Linux の起動・サービス管理に使われる SysVinit と systemd の流れを対応させて整理する。

起動の流れ

SysVinit

Linuxカーネル
  ↓
init（PID 1）
  ↓
runlevel を決定
  ↓
/etc/inittab や rc スクリプトを確認
  ↓
/etc/init.d/ の init スクリプト
  ↓
各サービスを順番に起動

systemd

Linuxカーネル
  ↓
systemd（PID 1）
  ↓
target を決定
  ↓
unit ファイルを確認
  ↓
依存関係を判断
  ↓
各サービスを起動

対応関係

SysVinit

systemd

init

systemd

runlevel

target

init スクリプト

unit ファイル

service

systemctl

chkconfig

systemctl enable / disable

サービス操作の例

SysVinit

service httpd start
service httpd stop
service httpd status

systemd

systemctl start httpd
systemctl stop httpd
systemctl status httpd

自動起動の設定：

# SysVinit
chkconfig httpd on

# systemd
systemctl enable httpd

runlevel と target

代表的な対応：

SysVinit

systemd

runlevel 0

poweroff.target

runlevel 1

rescue.target

runlevel 3

multi-user.target

runlevel 5

graphical.target

runlevel 6

reboot.target

ポイント

どちらも PID 1 のプロセスがシステム起動を管理する。

SysVinit は runlevel + init スクリプト が中心。

systemd は target + unit ファイル が中心。

SysVinit は基本的にサービスを順番に起動する。

systemd は依存関係を見ながら、可能なものを並列に起動できる。

覚え方

runlevel     → target
init script  → unit
service      → systemctl
chkconfig    → systemctl enable / disable
