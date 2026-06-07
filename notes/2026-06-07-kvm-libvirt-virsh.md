# 2026-06-07 KVM・libvirt・virshの関係

## 学習内容

仮想マシンの管理に関する仕組みについて学習した。

当初は KVM、libvirt、virsh がそれぞれ何をするものなのか整理できていなかったが、それぞれの役割を理解することで全体像が見えてきた。

---

## 今までの学習とのつながり

現在の学習環境

```text
Mac
↓
VirtualBox
↓
Ubuntu
```

* Mac上でVirtualBoxを利用してUbuntuを動かしている
* UbuntuはLinuxディストリビューションの一つ

---

企業でよく利用される仮想化環境

```text
Linux
↓
KVM
↓
仮想マシン
```

* KVM（Kernel-based Virtual Machine）はLinuxカーネルに組み込まれた仮想化機能
* Linux上で仮想マシンを動作させることができる

---

仮想マシンの管理

```text
KVM
↓
libvirt
↓
virsh
```

### KVM

* Linuxの仮想化機能
* 仮想マシンを実際に動かす役割

### libvirt

* 仮想マシンを管理するための共通インターフェース
* KVMなどの仮想化環境を操作しやすくする

### virsh

* Virtual Shell の略
* libvirtをコマンドラインから操作するためのツール
* 仮想マシンの起動・停止・状態確認などを行う

---

## 関連ツール

### virt-manager

* libvirtをGUIで操作するツール

```text
virt-manager
↓
libvirt
↓
KVM
```

### virsh

* libvirtをCUIで操作するツール

```text
virsh
↓
libvirt
↓
KVM
```

---

## 学んだこと

* KVMはLinuxに組み込まれた仮想化機能である
* libvirtは仮想マシン管理を容易にする仕組みである
* virshはlibvirtを操作するためのコマンドラインツールである
* virt-managerはlibvirtをGUIで操作するツールである
* MacでVirtualBoxとUbuntuを学習している環境は、企業で利用されるKVMベースの仮想化技術を理解するための土台になっている
