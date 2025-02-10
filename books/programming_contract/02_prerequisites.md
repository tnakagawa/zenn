---
title: "準備"
---

# 準備（Setting）

本書を進めるにあたり、以下のソフトウェアとツールをインストールする必要があります。
まずは以下の準備を行ってください。

## 必要なソフトウェアとツール（Software and Tools）

実行環境と開発環境に必要なソフトウェアをインストールします。

### Node.js と npm

Node.js は、JavaScript の実行環境です。
npm は、Node.js のパッケージ管理ツールです。
[Node.js の公式サイト](https://nodejs.org/ja)から最新の LTS（Long Term Support）バージョンをダウンロードしてインストールしてください。

インストールが完了したら、以下のコマンドで確認します。

```sh
node -v
npm -v
```

筆者の環境での結果例です。

```sh
> node -v
v22.13.1
> npm -v
10.6.0
```

### Visual Studio Code (VSCode)

VSCode は、軽量で強力なソースコードエディタです。プログラミングに最適な環境を提供します。
[VSCode の公式サイト](https://code.visualstudio.com/)からダウンロードしてインストールしてください。

#### 拡張機能（VSCode Extensions）

VSCode の拡張機能として、次をインストールしておくと非常に便利です。
拡張機能のインストールは左サイドバーの一番下にあるアイコン（Windows のショートカットは`Ctrl+Shift+X`）からインストールできます。
上部にある入力欄からそれぞれの拡張機能名を入力してインストールをします。


:::message alert
Solidity は同じ名前で２つ出てきます、黄色でヘルメットが書いてある方をインストールしてください。
:::

##### Japanese Language Pack for Visual Studio Code

VSCode のユーザーインターフェースを日本語にローカライズし、より親しみやすい操作環境を提供するための拡張機能です。
マーケットサイト：[Japanese Language Pack for Visual Studio Code - Visual Studio Marketplace](https://marketplace.visualstudio.com/items?itemName=MS-CEINTL.vscode-language-pack-ja)

##### Solidity

Solidity 拡張機能は、Solidity と Hardhat のサポートを追加し、コード補完、定義への移動などの機能を提供します。
マーケットサイト：[Solidity - Visual Studio Marketplace](https://marketplace.visualstudio.com/items?itemName=NomicFoundation.hardhat-solidity)

:::message
VSCode おすすめの設定
**保存時に自動的にフォーマット**
`ファイル(F) > ユーザー設定 > 設定` （Windows のショートカットは `Ctrl + ,`）で設定がでます。
上部の設定の検索で`format on save` と入力し、`Editor: Format On Save`にチェックします。
これで、ファイルを保存した時にファイルが自動的にフォーマットされソースコードが見やすくなります。
:::

## まとめ（Conclusion）

これで、開発環境が整いました。
