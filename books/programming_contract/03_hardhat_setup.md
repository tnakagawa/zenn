---
title: "Hardhatのセットアップと基本操作"
---

# Hardhat のセットアップと基本操作（Setup and Basic Operation）

Hardhat は、Ethereum のスマートコントラクト開発用の柔軟で高度な開発環境およびフレームワークです。開発者が Ethereum のスマートコントラクトを簡単に作成、テスト、デプロイするためのツールを提供します。
本章では、Hardhat でプロジェクトを作成します、作成したプロジェクトにはサンプルコードがあります、それらを利用してコンパイル、テスト、デプロイの操作を実施します。

## 準備（Setting）

作業ディレクトリを作成し、VSCode を起動して作成した作業ディレクトリを開きます。

## Hardhat のインストール（Install）

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

## プロジェクトの初期化（Init Project）

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
  プロジェクトのディレクトリパスです、デフォルトはコマンドを実行したディレクトリが選択されます。
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

## コンパイル（Compile）

Hardhat プロジェクトを作成すると、サンプルコントラクト `contracts/Lock.sol` があります。
以下のコマンドでスマートコントラクトをコンパイルします。

```sh
npx hardhat compile
```

コンパイルが正常に完了すると、コンパイル済みのファイルが `artifacts/` ディレクトリに生成されます。
`artifacts/contracts/<コントラクト名（拡張子あり）>` ディレクトリにある `<コントラクト名（拡張子なし）>.json` というファイルです。
このファイルには、スマートコントラクトのバイトコードや ABI（Application Binary Interface）が含まれています。

以下がコンパイル後の `artifacts` ディレクトリ例です。
サンプルコントラクトの場合、 `artifacts/contracts/Lock.sol/Lock.json` ファイルが作成されています。

```sh
artifacts
├─build-info
│      ad47b6fd82fea4f651540333f2a0887a.json
└─contracts
    └─Lock.sol
            Lock.dbg.json
            Lock.json
```

## テスト（Test）

スマートコントラクトをテストする方法について説明します。
`test/` ディレクトリにテストファイルがあります。
サンプルとして、JavaScriptでは `Lock.js` 、TypeScriptでは `Lock.ts` ファイルがあります。
テストは以下のコマンドで実行できます。

```sh
npx hardhat test
```

このコマンドは、 `test/` ディレクトリ内のテストファイルを実行し、スマートコントラクトの機能が期待通りに動作するかを確認します。

以下が、実行結果例です。

```sh
> hardhat_js@1.0.0 npx
> hardhat test



  Lock
    Deployment
      ✔ Should set the right unlockTime (671ms)
      ✔ Should set the right owner
      ✔ Should receive and store the funds to lock
      ✔ Should fail if the unlockTime is not in the future
    Withdrawals
      Validations
        ✔ Should revert with the right error if called too soon
        ✔ Should revert with the right error if called from another account
        ✔ Shouldn't fail if the unlockTime has arrived and the owner calls it
      Events
        ✔ Should emit an event on withdrawals
      Transfers
        ✔ Should transfer the funds to the owner


  9 passing (759ms)
```

## デプロイ（Deploy）

スマートコントラクトをデプロイする方法について説明します。
`ignition/modules` ディレクトリに、デプロイ用のスクリプトがあります。
サンプルとして、JavaScriptでは `Lock.js` 、TypeScriptでは `Lock.ts` ファイルがあります。
デプロイは以下のコマンドはで実行できます。
`npx hardhat ignition deploy <デプロイ用のスクリプトファイル>`

JavaScriptのコマンド
```sh
npx hardhat ignition deploy .\ignition\modules\Lock.js
```

TypeScriptのコマンド
```sh
npx hardhat ignition deploy .\ignition\modules\Lock.ts
```

TypeScriptのコマンド実行結果例です。

```sh

> hardhat_ts@1.0.0 npx
> hardhat ignition deploy .\ignition\modules\Lock.ts

You are running Hardhat Ignition against an in-process instance of Hardhat Network.
This will execute the deployment, but the results will be lost.
You can use --network <network-name> to deploy to a different network.

Hardhat Ignition 🚀

Deploying [ LockModule ]

Batch #1
  Executed LockModule#Lock

[ LockModule ] successfully deployed 🚀

Deployed Addresses

LockModule#Lock - 0x5FbDB2315678afecb367f032d93F642f64180aa3
```

## まとめ（Conclusion）

これで、プロジェクトの作成と基本的なコマンド（コンパイル、テスト、デプロイ）が実行可能となりました。
