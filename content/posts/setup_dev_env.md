+++
title = "feat: 環境構築をやってみよう！"
date = 2026-04-24T00:00:00+00:00

[taxonomies]
tags = ["setup"]
+++

<!-- Flag 1/3 -->
<!-- 31 2f 33 20 46 49 54 7b e6 9c 80 e8 bf 91 ef bc 8c e8 a6 96 e5 8a 9b e3 81 ae e4 bd 8e e4 b8 8b e3 81 8c e5 87 84 e3 81 be e3 81 98 e3 81 84 e3 82 93 e3 81 a0 e3 81 91 e3 81 a9 2e 2e 2e 7d -->

## 1. はじめに

活動に使用する環境を構築します．<br>
既に **Linux** の環境を持っており，自分でツールをインストールできる知識がある場合はそれを使用してください．<br>
この記事は **Windows OS** を対象にしています．

初学者向けなため，`Kali・Parrot・BlackArch` 等のセキュリティ監査用のOSではなく，WSL2を介して `Ubuntu` を導入します．

<!--more-->

## 2. WSL2の有効化

### 2.1 導入要件

- **OS**: Windows 10 バージョン2004以降 / Windows 11
- **BIOS設定**: CPU仮想化機能 (`VT-x/AMD-V`) が有効であること

### 2.2 CPU仮想化機能の有効化・確認方法

CLIで確認する方法もありますが，今回はGUIを使用します．

1. `win + r` から `taskmgr` を実行するか，`Ctrl + Shift + Esc` でタスクマネージャーを開きます．
2. 左側のメニューから「**パフォーマンス**」タブを選択し，「**CPU**」をクリックします．
3. グラフの右下にある情報欄に「**仮想化: 有効**」と記載されているか確認します．

![setup_dev_env-01](/images/setup_dev_env-01.png)

これで，有効化されていない場合は，各コンピュータで固有の起動キーを調べ `UEFI/BIOS` を起動して，`Virtualization Technology` を有効化してください．

再起動後，仮想化が有効になっていれば次のセクションに進んでください．

### 2.3 Terminalのインストール

`Microsoft Store` から `Windows Terminal` を導入してください．

![setup_dev_env-02](/images/setup_dev_env-02.png)

### 2.4 WSL2の有効化 & Ubuntu のインストール

`PowerShell` を **管理者権限** で立ち上げ，以下のコマンドを実行してください．インストール後に，コンピュータを **再起動** してください．

```powershell
PS C:\WINDOWS\system32> wsl --install
```

このコマンドで，以下の機能が実行されます．

- WSL機能の有効化
- 仮想マシンプラットフォームの有効化
- Linuxカーネルのインストール
- WSL2をデフォルトに設定
- Ubuntuのインストール

### 2.5 インストール後の各種設定

`Windows Terminal` のタブから，`Ubuntu` を選択し起動するとパスワードとユーザネームを求められるので，設定してください．

```shell
# パッケージ更新
$ sudo apt update && sudo apt upgrade

# 日本語環境
$ sudo apt install language-pack-ja
$ sudo update-locale LANG=ja_JP.UTF-8

# タイムゾーン設定
$ sudo timedatectl set-timezone Asia/Tokyo

# 最低限必要なツール
$ sudo apt install build-essential curl wget git nano

# gitの設定 (Githubアカウントを所持している前提)
$ git config --global user.name "Your Name"
$ git config --global user.email "your.email@example.com"
$ git config --global init.defaultBranch main
```

## 4. WSL・Windowsホストの相互ファイル操作

### 4.1 Windows -> WSL

```powershell
PS C:\WINDOWS\system32> dir \\wsl$\Ubuntu
```

### 4.2 WSL -> Windows

```shell
$ ls -la /mnt/c
```