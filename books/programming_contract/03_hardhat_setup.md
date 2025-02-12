---
title: "Hardhatのセットアップと基本操作"
---

# Hardhat のセットアップと基本操作

Hardhat は、Ethereum のスマートコントラクト開発用の柔軟で高度な開発環境およびフレームワークです。開発者が Ethereum のスマートコントラクトを簡単に作成、テスト、デプロイするためのツールを提供します。
本章では、Hardhat でプロジェクトを作成し

## 準備

作業フォルダを作成し、VSCode を起動して作成した作業フォルダを開きます。

## Hardhat のインストール

VSCode のメニューから **ターミナル &gt; 新しいターミナル** を選択してターミナルを開きます。
ターミナルから npm プロジェクトを初期化するために以下のコマンドを実行します。
`package.json` ファイルが自動的に作成されます。

```sh
npm init -y
```

以下のコマンドでプロジェクトに Hardhat をインストールします。

```sh
npm install --save-dev hardhat
```

## プロジェクトの初期化

Hardhat のプロジェクトを初期化するために以下のコマンドを実行します。

```sh
npx hardhat init
```

コマンドを実行すると以下のように表示されいずれかを選択する事となります。
通常は上の２つから JavaScript または TypeScript プロジェクトを作成します。
JavaScript か TypeScript については使い慣れている方を選択してください。

```shell
888    888                      888 888               888
888    888                      888 888               888
888    888                      888 888               888
8888888888  8888b.  888d888 .d88888 88888b.   8888b.  888888
888    888     "88b 888P"  d88" 888 888 "88b     "88b 888
888    888 .d888888 888    888  888 888  888 .d888888 888
888    888 888  888 888    Y88b 888 888  888 888  888 Y88b.
888    888 "Y888888 888     "Y88888 888  888 "Y888888  "Y888

Welcome to Hardhat v2.22.18

? What do you want to do? ...
> Create a JavaScript project
  Create a TypeScript project
  Create a TypeScript project (with Viem)
  Create an empty hardhat.config.js
  Quit
```

以下はコマンドの初期化完了例です。

```sh
888    888                      888 888               888
888    888                      888 888               888
888    888                      888 888               888
8888888888  8888b.  888d888 .d88888 88888b.   8888b.  888888
888    888     "88b 888P"  d88" 888 888 "88b     "88b 888
888    888 .d888888 888    888  888 888  888 .d888888 888
888    888 888  888 888    Y88b 888 888  888 888  888 Y88b.
888    888 "Y888888 888     "Y88888 888  888 "Y888888  "Y888

Welcome to Hardhat v2.22.18

√ What do you want to do? · Create a JavaScript project
√ Hardhat project root: · C:\work\hardhat_js
√ Do you want to add a .gitignore? (Y/n) · y
√ Help us improve Hardhat with anonymous crash reports & basic usage data? (Y/n) · y
√ Do you want to install this sample project's dependencies with npm (@nomicfoundation/hardhat-toolbox)? (Y/n) · y

npm install --save-dev "@nomicfoundation/hardhat-toolbox@^5.0.0"
```

- `√ What do you want to do? · Create a JavaScript project`
  JavaScript のプロジェクト作成を選択しています。
- `√ Hardhat project root: · C:\work\hardhat_js`
  プロジェクトのディレクトリパスです、デフォルトはコマンドを実行したフォルダが選択されます。
- `√ Do you want to add a .gitignore? (Y/n) · y`
  `.gitignore` ファイルを作成します、git に含めなくていいファイルやディレクトリが自動的に設定されています。
- `√ Help us improve Hardhat with anonymous crash reports & basic usage data? (Y/n) · y`
  クラッシュレポートの協力についての問い合わせです、これは `y` にすると次回からは出てきません。
- `√ Do you want to install this sample project's dependencies with npm (@nomicfoundation/hardhat-toolbox)? (Y/n) · y`
  サンプルプロジェクトの依存関係を解消する為に `hardhat-toolbox` をインストールします、テストなどで使用するのでインストールします。

プロジェクトの初期化が完了すると、いくつかのディレクトリとファイルが自動的に生成されます。基本的なディレクトリの構成は次の通りです。

- `contracts/`: スマートコントラクトのソースコードを格納するディレクトリ。
- `ignition/`: デプロイのモジュールや詳細データを格納するディレクトリ。Hardhat ignition スマート コントラクトをデプロイするための宣言型システムです。
- `test/`: テストファイルを格納するディレクトリ。スマートコントラクトの機能をテストするためのファイルを配置します。

プロジェクトが成長するにつれて、新しいディレクトリやファイルを追加することも可能です。

## コンパイル

Hardhat を使用してスマートコントラクトをコンパイルする手順は以下の通りです。

```sh
npx hardhat compile
```

コンパイルが正常に完了すると、コンパイル済みのファイルが `artifacts/` ディレクトリに生成されます。
これらのファイルには、スマートコントラクトのバイトコードや ABI（Application Binary Interface）が含まれています。

## デプロイ

スマートコントラクトをデプロイする方法について説明します。
デプロイ用のスクリプトを作成し、以下のコマンドを実行します。

```sh
npx hardhat ignition deploy <>
```

## テスト

最後に、スマートコントラクトのテスト方法について説明します。テストファイルを作成し、以下のコマンドを実行します。

```sh
npx hardhat test
```

このコマンドは、 `test/` ディレクトリ内のテストファイルを実行し、スマートコントラクトの機能が期待通りに動作するかを確認します。
