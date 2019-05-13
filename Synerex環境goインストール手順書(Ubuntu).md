| Synerex Alpha環境 Go言語インストール手順書 |
| ------------------------------------------ |
| **（Ubuntu編）**                           |
| **R0.01版**                                |

| 作成者     | 榊原          |
| ---------- | ------------- |
| 初版作成日 | 2019年5月13日 |
| 最終更新日 | 2019年5月13日 |

 目次

[TOC]

# はじめに

 本書はGo言語インストールに関する手順書である。Synerex Alphaが実行できる環境の構築を目的としているため、gRPCなど関連モジュールの記述も含んでいる。

 モジュールインストール毎に正常確認を行うために、環境変数を取込むUbuntu再起動を都度行う手順としている。

# 前提条件

- ubuntu 16.04 LTS以降（説明ではWindows画面を一部流用している。）
- プロセッサ種別がIntel系でない場合、別途考察が必要
- 64bit版 OS（本書では64bit版OSを対象としているため、32bit版OSについては別途考察が必要）
- curlコマンドインストール済み
- Golangなどaptコマンドによるインストールでなく、最新モジュールを適用することを目的に手動によるインストール手順としている場合がある。


## セットアップ順序

 セットアップ順序は以下のとおりである。

1. Gitのインストール
2. Goのインストール
3. gRPCのインストール
4. Protocol Buffersのインストール
5. Node.jsとNPMのインストール
6. YARNのインストール
7. 関連パッケージのインストール

# 事前確認

 作業前に以下を確認する。

## OS情報の確認

 ubuntuのbitを確認するため、以下を実行する。

![](img/ubuntu/imgu01sys-01.png)

1. 「コンピュータを検索」選択
2. 「shi」入力
3. 「詳細」選択



![](img/ubuntu/imgu01sys-02.png)


「プロセッサ」と「OS種別」把握する。

※本書ではIntel系CPU、64bit版OSを対象としているため、その他の場合（特にARM系）については別途考察が必要となる

## curlコマンドの確認

curlコマンドは、HTTPアクセスをしてコンテンツを取得できるコマンドである。事前にcurlコマンドのインストール済み確認を行う。

ターミナルから以下のコマンドを実行し、バージョンが正しく表示されることを確認する。

```
$ curl --version
```

 バージョン表示で失敗する場合、curlのインストールを実施する。

```
$ sudo apt install curl
```


# Gitのインストール

 Git（ギット）は、プログラムのソースコードなどの変更履歴を記録・追跡するための分散型バージョン管理システムである。

 Gitでは、各ユーザのワーキングディレクトリに、全履歴を含んだリポジトリの完全な複製が作られる。したがって、ネットワークにアクセスできないなどの理由で中心リポジトリにアクセスできない環境でも、履歴の調査や変更の記録といったほとんどの作業を行うことができる。

## Gitインストール済み確認

ターミナルから以下のコマンドを実行し、バージョンが正しく表示されることを確認する。

```
$ git --version
```

 バージョン表示で失敗する、またはバージョンが古い場合、以下インストールを実施する。

## Gitインストール

 Git未インストール時、ターミナルから以下のコマンドを実行し、インストールを実施する。

```
$ sudo add-apt-repository ppa:git-core/ppa
$ sudo apt update
$ sudo apt install git
```

前項「Gitインストール済み確認」に従い、バージョンが正しく表示されることを確認する。

# Goのインストール

 Goはプログラミング言語の1つである。2009年、GoogleでRobert Griesemer、ロブ・パイク、ケン・トンプソンによって設計された。

 Goは、静的型付け、C言語の伝統に則ったコンパイル言語、メモリ安全性（英語版）、ガベージコレクション、構造的型付け（英語版）、CSPスタイルの並行性などの特徴を持つ。Goのコンパイラ、ツール、およびソースコードは、すべてフリーかつオープンソースである。

## Goインストール済み確認

 ターミナルから以下のコマンドを実行し、バージョンが正しく表示されることを確認する。

```
$ go version
```

 バージョン表示で失敗する、またはバージョンが古い場合、以下インストールを実施する。

## Goインストール

 Go未インストール時、ターミナルから以下のコマンドを実行し、インストールを実施する。

 Go未インストール時に以下を実施する。

### インストールの準備

 公式サイト（URL: [https://golang.org/dl/](https://golang.org/dl/)）より該当する実行ファイルをダウンロードする。

 ![](/img/ubuntu/imgu03go-01.png)

### 実行ファイルの展開

 ターミナルから以下のコマンドを実行し、実行ファイルを展開する。

```
$ cd [ダウンロード先]									 ← 例：~/Download
$ sudo tar -C /usr/local/ -xzf [ダウンロードファイル名]  ← 例：go 1.12.4.linux-amd64.tar.gz
```



$HOME/.profile ファイルを開き、以下のコマンドを追記する。

```
export PATH=$PATH:/usr/local/go/bin
```

【追記例】

```
$ gedit ~/.profile    ← エディタ起動
```

![](/img/ubuntu/imgu03go-02.png)

 編集後、保存してエディタを閉じる。その後、環境変数の取込みを行うため、ターミナルから以下のコマンドを実行、またはUbuntuを再起動する。

```
$ source ~/.profile
```

 前項「Goインストール済み確認」に従い、バージョンが正しく表示されることを確認する。

## 環境変数の設定

 $HOME/.bashrc ファイルを開き、以下のコマンドを追記する。

```
export GOPATH=$HOME/go
export PATH=$PATH:$GOPATH/bin
```

【追記例】

```
$ gedit ~/.bashrc    ← エディタ起動
```

![](img/ubuntu/imgu03go-03.png)

 編集後、保存してエディタを閉じる。その後、ターミナルから以下のコマンドを実行し、環境変数の取込み、およびフォルダ作成を行う。

```
$ source ~/.bashrc
$ mkdir $GOPATH
```

ターミナルから以下のコマンドを実行し、goフォルダが存在していることを確認する。

```
$ ls $HOME
```

# gRPCのインストール

 gRPCは、RPC (Remote Procedure Call)を実現するためにGoogleが開発したプロトコルの1つである。
 Protocol Buffersを使ってデータをシリアライズし、高速な通信を実現できる点が特長である。
 gRPCでは、IDL（インターフェース定義言語）を使ってあらかじめAPI仕様を.protoファイルとして定義し、そこからサーバー側＆クライアント側に必要なソースコードのひな形を生成する。
 言語に依存しないIDLで先にインタフェースを定義することで、様々なプログラミング言語の実装を生成できるというメリットがある。

## gRPCインストール済み確認

 以下フォルダ内に「genproto」「grpc」フォルダがあればOK

 $GOPATH/src/google.golang.org

 【確認例】

```
$ ls $GOPATH/src/google.golang.org
```

 フォルダが存在しない場合、以下インストールを実施する。

## gRPCインストール

 ターミナルから以下のコマンドを実行する。

```
$ go get -u google.golang.org/grpc
```

 ※処理に数分程度要することがあるので気長に待つ。

 処理終了後、前項「gRPCインストール済み確認」に従い、フォルダがあることを確認する。

# Protocol Buffersのインストール

 Protocol Buffers（プロトコルバッファー）はインタフェース定義言語 (IDL) で構造を定義する通信や永続化での利用を目的としたシリアライズフォーマットであり、Googleにより開発されている。

 オリジナルのGoogle実装はC++、Java、Pythonによるものであり、フリーソフトウェアとしてオープンソースライセンスで公開されている。

 また、ActionScript・C言語・C#・Clojure・Common Lisp・D言語・Erlang・Go・Haskell・JavaScript・Lua・MATLAB・Mercury・Objective-C・OCaml・Perl・PHP・R言語・Ruby・Scala・.NET Frameworkなどの実装が利用可能である。

## Protocol Buffersインストール済み確認

 ターミナルから以下のコマンドを実行し、バージョンが正しく表示されることを確認する。

```
$ protoc --version
```

 バージョン表示で失敗する場合、以下インストールを実施する。

## Protocol Buffersインストール

 Protocol Buffers未インストール時に以下を実施する。

### モジュールのダウンロード

 公式サイト（URL: [https://github.com/protocolbuffers/protobuf/releases](https://github.com/protocolbuffers/protobuf/releases)）より該当するUbuntu用モジュールをダウンロードする。

 ![](/img/ubuntu/imgu04prtc-01.png)

 ![](/img/ubuntu/imgu04prtc-02.png) 

 ダウンロード後、ターミナルから以下のコマンドを実行し、インストールを実施する。

※以下ダウンロードディレクトリ名、ダウンロードファイル名は一例、「#」はコメントなので読み跳ばすこと

```
# カレント変更 (ディレクトリ名は一例)
cd ~/Download

# zip解凍（ファイル名は一例）
unzip protoc-3.7.1-linux-x86_64.zip -d protoc3

# protocバイナリ移動 （移動先 /usr/local/bin/）
sudo mv protoc3/bin/* /usr/local/bin/

# protocインクルード移動 （移動先 /usr/local/include/）
sudo mv protoc3/include/* /usr/local/include/

# change owner
sudo chown $USER /usr/local/bin/protoc
sudo chown -R $USER /usr/local/include/google
```

前項「Protocol Buffersインストール済み確認」に従い、バージョンが正しく表示されることを確認する。

## Protocol BuffersのGo用プラグインの確認とインストール

 ターミナルから以下のコマンドを実行し、Protocol BuffersのGo用プラグインが存在することを確認する。

```
$ which protoc-gen-go
```

 上記コマンドにてファイルが存在しない場合、ターミナルから以下のコマンドを実行し、Go用プラグインをインストールする。

```
$ go get -u github.com/golang/protobuf/protoc-gen-go
```

 ※処理に数分程度要することがあるので気長に待つ。

 処理終了後、上記whichコマンドを実行し、ファイルが存在することを確認する。

# Node.jsとNPMのインストール

 JavaScriptは本来クライアントサイドで動く言語で、HTMLで書かれたページに動きをつけたりするものである。Node.jsはサーバサイドで動くJavaScriptである。

 NPM(Node Package Manager)とは、Node.jsのパッケージを管理するものである。

## Node.jsとNPMインストール済み確認

 ターミナルから以下のコマンドを実行し、バージョンが正しく表示されることを確認する。

```
$ node --version    ← Node.jsの確認
$ npm --version     ← NPMの確認
```

 バージョン表示で失敗する場合、以下インストールを実施する。

## Node.jsとNPMインストール

 Node.jsとNPM未インストール時、ターミナルから以下のコマンドを実行し、インストールを実施する。

```
$ sudo curl -sL https://deb.nodesource.com/setup_10.x | sudo bash -
$ sudo apt install -y nodejs		←NPMも同時にインストールされる
```

前項「Node.jsとNPMインストール済み確認」に従い、バージョンが正しく表示されることを確認する。

# YARNのインストール

 YARN（Yet Another Resource Negotiator）とは、NPMの高速版パッケージのようなもの

## YARNインストール済み確認

 ターミナルから以下のコマンドを実行し、バージョンが正しく表示されることを確認する。

```
$ yarn --version
```

 バージョン表示で失敗する場合、以下インストールを実施する。

## YARNインストール
YARN未インストール時、ターミナルから以下のコマンドを実行し、インストールを実施する。

・リポジトリのGPGキーをインポート

```
$ curl -sS https://dl.yarnpkg.com/debian/pubkey.gpg | sudo apt-key add -
```

・Yarn APTリポジトリをシステムのソフトウェアリポジトリリストに追加

(※以下は一行のコマンド)

```
$ echo "deb https://dl.yarnpkg.com/debian/ stable main" | sudo tee /etc/apt/sources.list.d/yarn.list
```

・パッケージリストを更新してYarnをインストール

```
$ sudo apt update
$ sudo apt install yarn
```

前項「YARNインストール済み確認」に従い、バージョンが正しく表示されることを確認する。

# 関連パッケージのインストール

 Synerex Alphaのインストール例と関連パッケージのインストール方法を記述する。インストール要否を判断し作業を行うこと。

## Synerex Alphaインストール

 ターミナルから以下のコマンドを実行し、Synerex Alphaをインストールする。

 （※以下はインストール例であるため、適宜調整する）

```
$ go get -u [Synerex Alphaリポジトリ]
```

| 【 Synerexリポジトリ例】         |
| -------------------------------- |
| github.com/synerex/synerex_alpha |
| 上記フォーク先                   |



## 関連パッケージのインストール（参考）

 Synerex Alphaを実行するためには、関連パッケージのインストールが必要となる。

 通常は、go buildで関連パッケージを自動でダウンロードするが、go version 1.12以降では、go modulesパッケージ仕様が変更され、デフォルトでは、ビルド対象パッケージが $GOPATH/src 配下にある場合、関連パッケージのダウンロードがされない。そのため、モジュールが見つからない旨のエラーが発生する場合は、go build前に以下コマンド実行するか、手動にて関連パッケージをインストールする必要がある。

### ダウンロード先

| ダウンロード種別 | ダウンロード先                                               |
| ---------------- | ------------------------------------------------------------ |
| 自動             | $GOPATH/pkg/mod（$GOPATH/srcにないモジュールを自動ダウンロード） |
| 手動             | $GOPATH/src                                                  |



### 自動ダウンロードする場合

 Synerex Alphaが動作するために必要な関連パッケージを自動インストールする。

 go buildコマンド発行前に、コマンドプロンプトから以下のコマンドを実行するか、環境変数「GO111MODULE」を設定する。

```
$ export GO111MODULE=on
```

 以下buildサンプル

```
【サンプル1】
$ cd $GOPATH/src/github.com/synerex/synerex_alpha/cli/daemon
$ export GO111MODULE=on
$ go build               ← 正常終了すると「se-daemon」が生成される 
$ se-daemon build

【サンプル2】
$ cd $GOPATH/src/github.com/synerex/synerex_alpha/cli/daemon
$ export GO111MODULE=on
$ go mod init se-daemon  ← 正常終了すると「go.mod」が生成される
$ go mod tidy            ← 正常終了すると「go.sum」が生成される
$ go build               ← 正常終了すると「se-daemon」が生成される 
```

 上記でビルドエラーになる場合は次項「手動ダウンロードする場合」を実行することで解決する場合がある。
 ※環境変数にて「GO111MODULE」を設定する方法については省略する。

### 手動ダウンロードする場合

 Synerex Alphaが動作するために必要な全関連パッケージを手動インストールする。

 ターミナルから以下のコマンドを実行する。

```
 【Synerex Alphaディレクトリ例】：$GOPATH/src/github.com/synerex/synerex_alpha

$ cd [Synerex Alphaディレクトリ]
$ go get ./...
```

※1 ダウンロードに時間がかかるため気長に待つ。
※2 GO111MODULE=autoに伴うエラーが発生する場合は、前項「自動ダウンロードする場合」同様にexportコマンドにて対応する。

# その他

## 実行エラー時の対応

 「go run」「go build」等、実行時にパッケージなしエラーになる場合は、前項「関連パッケージのインストール」の他に、個別でパッケージをインストールすることも可能。

 【エラー例】※Windows画面（Macでは類似画面となる）

![](img/ubuntu/imgu07msc-01.png)

 内容：「github.com/spf13/viper」パッケージが見つからない

 【対策コマンド】

```
$ go get -u github.com/spf13/viper
```

## go generateエラー

 go generateでエラーになる要因として、goパッケージの未インストール、パス不正の場合があるので、パスが正常に通っているか確認する。

 Protocのパス定義でinclude部分を参照する部品があるが、パス不正によるエラーとなる例があった。（Protocパスの設定見直しにて解決）。



― 以上 ―