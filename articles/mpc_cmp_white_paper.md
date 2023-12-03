---
title: "『UC Non-Interactive, Proactive, Threshold ECDSA』の翻訳"
emoji: ""
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["crypto", "Threshold ECDSA"]
published: false
---

# はじめに

以下のWhitepaperを[Gooele翻訳](https://translate.google.com/)や[DeepL](https://www.deepl.com/translator)で翻訳してみます。

https://eprint.iacr.org/2020/492

# UC Non-Interactive, Proactive, Threshold ECDSA

Ran Canetti$^\ast$
Nikolaos Makriyannisy$^\dagger$
Udi Peledy
October 21, 2021

---

$^\ast$ Boston University. Email: canetti@bu.edu.
$^\dagger$ Fireblocks. Emails: nikos@fireblocks.com, udi@fireblocks.com

---

**要旨（Abstract）**

<details><summary>原文</summary>

Building on the Gennaro & Goldfeder and Lindell & Nof protocols (CCS ’18), we present a threshold ECDSA protocol, for any number of signatories and any threshold, that improves as follows over the state of the art:

- Signature generation takes only 4 rounds (down from the current 8 rounds), with a comparable computational cost. Furthermore, 3 of these rounds can take place in a preprocessing stage before the signed message is known, lending to a non-interactive threshold ECDSA protocol.

- The protocol withstands adaptive corruption of signatories. Furthermore, it includes a periodic refresh mechanism and offers full proactive security.

- The protocol realizes an ideal threshold signature functionality within the UC framework, in the global random oracle model, assuming Strong RSA, semantic security of the Paillier encryption, and a somewhat enhanced variant of existential unforgeability of ECDSA.

These properties (low latency, compatibility with cold-wallet architectures, proactive security, and composable security) make the protocol ideal for threshold wallets for ECDSA-based cryptocurrencies.

</details>

Gennaro & GoldfederおよびLindell & Nofプロトコル（CCS '18）を基に、任意の署名者数および任意の閾値に対して、現状よりも以下のように改善された閾値ECDSAプロトコルを提示します。

- 署名生成にかかる時間はわずか4ラウンド（現行の8ラウンドから削減）であり、計算コストも同程度です。さらに、これらのラウンドのうち3ラウンドは、署名されたメッセージが判明する前の前処理段階で行うことができ、非インタラクティブな閾値ECDSAプロトコルに貢献します。

- このプロトコルは、署名者の適応的な破損に耐えることができます。さらに、定期的なリフレッシュ機構を備え、完全なプロアクティブセキュリティを提供します。

- このプロトコルは、UCフレームワークのグローバルランダムオラクルモデルにおいて、Strong RSA、Paillier暗号のセマンティックセキュリティ、ECDSAのexistential unforgeabilityを多少強化した形で理想的な閾値署名機能を実現するものです。

これらの特性（低レイテンシー、コールドウォレットアーキテクチャとの互換性、プロアクティブセキュリティ、コンポーザブルセキュリティ）により、ECDSAベースの暗号通貨用の閾値ウォレットに理想的なプロトコルとなっています。

# 1 はじめに（Introduction）

<details><summary>原文</summary>

Introduced by Desmedt [25] and Desmedt and Frankel [26], threshold signatures allow a number of signatories to share the capability to digitally sign messages, so that a given message is signed if and only if a certain threshold of the signatories agree to sign it.
In more detail, a $t$-out-of-$n$ threshold signature scheme is a mechanism whereby a set of $n$ signatories, presented with a message $m$, jointly and interactively compute a _signature_ $\sigma$ such that (1) if at least $t$ of the signatories agree to sign $m$, then the pair $m,\sigma$ is accepted as a valid by a pre-determined public verification algorithm, and (2) no attacker that controls up to $t-1$ signatories can forge signatures – namely, it cannot come up with a pair $m',\sigma'$ such that the verification algorithm accepts $\sigma'$ as a valid signature on $m'$, if the latter was never signed before.

Threshold signatures are an instance of “threshold cryptography” which, in turn, is one of the main application areas of the more general paradigm of secure multi-party computation.
Threshold cryptography offers an additional layer of security to the entity performing a cryptographic task that involves using a private key, by distributing the capabilities that require using the secret key among multiple servers/devices.
Indeed, this way the system has no single point of failure. Examples include threshold El-Gamal, RSA, Schnorr, Cramer-Shoup, ECEIS and others [50, 23, 49, 51, 13].

With the advent of blockchain technologies and cryptocurencies in the past decade, there has been a strong renewed interest in threshold cryptography and threshold signatures in particular. 
Specifically, because transactions are made possible via digital signatures, many stakeholders are looking to perform signaturegeneration in a distributed way, and many companies are now offering solutions based on (or in combination with) threshold cryptography.<sup>1</sup>

</details>

Desmedt [25]とDesmedt and Frankel [26]によって紹介された閾値署名は、多数の署名者がメッセージを電子的に署名する能力を共有することを可能にし、署名者のある閾値が署名に同意する場合にのみ、与えられたメッセージが署名されるようにするものです。
より詳細には、$t$-out-of-$n$ 閾値署名方式は、$n$人の署名者のセットが、メッセージ$m$を提示され、共同で対話的に _署名（signature）_ $\sigma$を計算するメカニズムであり、(1) 署名者の少なくとも$t$が$m$に署名することに同意する場合、ペア $m,\sigma$ を計算し、(2) $t-1$ 人までの署名者を管理する攻撃者は、署名を偽造できません、 すなわち、$m'$が以前に署名されたことがない場合、$m',\sigma'$の組を有効な署名として検証アルゴリズムに 受け入れてもらえない、ようにします。

閾値署名は「閾値暗号」の一例であり、より一般的なパラダイムである安全なマルチパーティ計算の主要な応用分野の一つです。
閾値暗号は、秘密鍵の使用を必要とする機能を複数のサーバ／デバイスに分散させることで、秘密鍵の使用を伴う暗号タスクを実行するエンティティに追加のセキュリティ層を提供します。
実際、この方法では単一障害点を持たないシステムが実現できます。例としては、Threshold El-Gamal、RSA、Schnorr、Cramer-Shoup、ECEISなど[50, 23, 49, 51, 13]があります。

過去10年間のブロックチェーン技術や暗号技術の登場により、特に閾値暗号や閾値署名への関心が再び高まっています。
具体的には、電子署名によって取引が可能になることから、署名生成を分散型で行うことを希望する関係者が多く、現在、多くの企業が閾値暗号をベースとした（あるいは組み合わせた）ソリューションを提供しています。<sup>1</sup>

---

<sup>1</sup>See https://www.mpcalliance.org/ for companies in the threshold cryptography space.
<sup>1</sup>閾値暗号分野の企業については、https://www.mpcalliance.org/ をご参照ください。

---

## 1.1 成果（Our Results.）

<details><summary>原文</summary>

We present a new threshold ECDSA protocol.
The protocol builds on the techniques of Gennaro and Goldfeder [31] and Lindell et al. [45], but provides new functionality, and has improved efficiency and security guarantees.
We first discuss the new characteristics of the protocol at high level, and then provide more details on the construction and the analysis.
Figure 1 provides a rough comparison between the main cost and security guarantees of our scheme and those of Gennaro and Goldfeder [31], Lindell et al. [45], Doerner et al. [28], and Castagnos et al. [21].

![](/images/mpccmp/figure1.png)

**Figure 1:**
Comparison of our scheme with those of [31, 45, 28, 21] for signing.
Costs are displayed per party for an $n$-party protocol secure against $n-1$ corrupted parties, for computational security of 128 bits and statistical security of 80 bits.
Ring operations contain two types of operations (mod $N$ and $N^2$) that we do not distinguish in the table; operations modulo $N^2$ represent less than a third of the total number of ring operations for all protocols.
The communication column describes the number of group elements (encoded by $\kappa$ bits) and ring elements (encoded by $N$ bits) sent between each pair of parties; in parentheses we provide estimates, including the constant overhead, for concrete implementation for the curve size of Bitcoin and the standard security recommendation of Paillier, i.e. $\kappa=256$ and $N=2048$. 
($^\dagger$Estimates for [45] include optimizations that do not preserve UC – c.f. Section 1.3.
$^\ddagger$Reported numbers are different than [45] because of how ring operations are accounted for.
$^\ast$We note that [21] relies on somewhat incomparable hardness assumptions, and it involves operations in a different group than the underlying elliptic curve – c.f. Section 1.3.)

**Non-Interactive Signing.**
As seen in Figure 1, in all of these protocols the signing process is highly interactive, i.e. the parties exchange information in a sequence of rounds to compute a signature for a given message. 
However, in many real-life situations it is desirable to have non-interactive signature generation, namely have each signatory, having seen the message, generate is own “signature share” without having to interact with any other signatory, and then have a public algorithm for combining the signature shares into a single signature.
For instance, such a mechanism is mandatory if one wants to use a “cold wallet” mechanism for
some of the signatories — which is a common practice in the digital currency realm for securing non-threshold wallets.
Indeed, a number of popular signature schemes do admit threshold protocols with non-interactive
signing (e.g. RSA [39], BLS [1]).

In our protocol, the signing process can be split into two phases: A first, preprocessing, phase that takes 3 rounds and can be performed before the message is known, followed by a non-interactive step where each signatory generates its own signature share, after the message to be signed becomes known.
To the best of our knowledge, this is the first threshold ECDSA in the literature that has manageable performance and allows for non-interactive signing with preprocessing.
Furthermore, the non-interactive step is very efficient: it boils down to computing and sending a single field-element (i.e. 256 bits for the Bitcoin curve).
We mention that a similar pre-signing capability for ECDSA was noticed by Dalskov et al. [22] who employed generic (i.e. all-purpose) MPC to compute the ECDSA functionality in the context of securing DNSSEC Keys.

</details>

新しい閾値ECDSAプロトコルを発表します。
このプロトコルはGennaro and Goldfeder [31]とLindellら[45]の技術を基に構築されているが、新しい機能を提供し、効率と安全性保証を向上させたものです。
まず、プロトコルの新しい特徴を高いレベルで説明し、その後、構成と解析の詳細を説明します。
図1は、本方式とGennaro and Goldfeder [31], Lindell et al. [45], Doerner et al. [28], Castagnos et al.[21] の主要コストとセキュリティ保証の大まかな比較です。

![](/images/mpccmp/figure1.png)

**図1:**
我々の方式と[31, 45, 28, 21]の方式との署名に関する比較です。
計算安全性128ビット、統計安全性80ビットで、$n-1$破損した当事者に対して安全な$n$当事者プロトコルの当事者あたりのコストを表示しています。
リング演算には、表では区別していない2種類の演算(mod $N$と$N^2$)があり、mod $N^2$の演算は、全プロトコルのリング演算の1/3以下です。
通信欄には、各ペア間で送信されるグループ要素($\kappa$ビットでエンコード)とリング要素($N$ビットでエンコード)の数を記述する。括弧内には、BitcoinのカーブサイズとPailierの標準セキュリティ勧告、すなわち$\kappa=256$と$N=2048$に対する具体的実装に対する推定値を、一定のオーバーヘッドも含めて記述しています。
( $^\dagger$ [45]の推定値はUCを保存しない最適化を含んでいます。 - セクション 1.3 を参照
$^\ddagger$ リング演算を考慮したため、[45]とは異なる数値が報告されています。
$^\ddagger$ [21]は、比較にならない硬さの仮定に依存し、基礎となる楕円曲線とは異なる群の演算を含むことに注意します。セクション 1.3 を参照 )

**非対話式署名(Non-Interactive Signing)**
図1に見られるように、これらのプロトコルのすべてにおいて、署名プロセスは非常に対話型（インタラクティブ）です。
つまり、メッセージを見た各署名者は、他の署名者と対話することなく自分自身の「署名共有」を生成し、その後、署名共有を単一の署名に結合するための公開アルゴリズムを持つことが望ましいのです。
例えば、一部の署名者に「コールドウォレット」機構を使用したい場合、このような機構は必須です。
これは、デジタル通貨の分野では、閾値のないウォレットを確保するための一般的な方法である。
実際、多くの一般的な署名方式は、非対話的な署名で閾値プロトコルを認めています。(例:RS-232C)
実際、多くの一般的な署名方式は、非対話的な署名で閾値プロトコルを認めています。（例えば、RSA [39]、BLS [1]）

我々のプロトコルでは、署名プロセスは2つのフェーズに分けることができる。1つは3ラウンドを要する前処理で、メッセージが知られる前に実行され、もう1つは署名されるメッセージが知られた後に、各署名者が自身の署名シェアを生成する非対話的な段階です。
我々の知る限り、これは管理可能な性能を持ち、前処理を伴う非対話的な署名を可能にする、文献上初の閾値ECDSAです。
さらに、非対話的なステップは非常に効率的で、単一のフィールド要素（Bitcoinカーブでは256ビット）の計算と送信に集約されます。
Dalskovら[22]は、DNSSEC鍵のセキュリティの文脈でECDSAの機能を計算するために汎用（つまり、万能）MPCを採用したことで、ECDSAに対する同様の事前署名機能に注目したことを述べます。

<details><summary>原文</summary>

**Round-Minimal Interactive Signing.**
We stress that, even in its interactive variant, our protocol is the most round-efficient among the state-of-the-art protocols, and thus our protocol may notably improve the performance of many applications that require ECDSA (e.g. cryptocurrency custody).

**Proactive Key Refresh [48, 38, 14, 40].**
While threshold signatures do provide a significant security improvement over plain signature schemes, they may still be vulnerable to attacks that compromise all shareholders one by one, in an adaptive way, over time.
This vulnerability is particularly bothersome in schemes that need to function and remain secure over long periods of time.
Proactive security is designed to alleviate this concern: In a proactive threshold signature scheme, time is divided into epochs, such that at the end of each epoch the parties engage in a protocol for refreshing their keys and local states.
The security guarantee is that the scheme remains unforgeable as long as at most $t-1$ signatories are compromised within a single epoch, or more precisely in any time period that starts at the beginning of one key refreshment and ends at the end of the next key refreshment.
It is stressed that the public signature verification algorithm (and key) of the scheme remains the same throughout.

Our protocol offers a two-round key refresh phase.
The refreshment is the most expensive component of our protocol: for standard choice of security parameters, computation requires roughly $400+330n+n^2$ RSA ring operations (see Appendix C for more details).
However, this may be manageable, given that the refresh is done only periodically, and it can be scheduled at times of low use of the system.

We stress that none of the other protocols in Figure 1 support proactive key refreshing. 
In fact, these protocols are not even known to provide traditional threshold security against an adversary that corrupts parties adaptively as the system progresses.

**The communication model.**
For simplicity of exposition we assume that the signatories are connected via an authenticated (but lossy) broadcast mechanism.
That is, the communication is public, and every message sent can potentially be received by all parties.
Still, the adversary can drop and delay messages at will.
We note that the use of authenticated communication is in fact essential for obtaining proactive security.
Indeed, without already-established authenticated communication, an adversary that formally “left” a previously corrupted party and controls all the communication between the party and the rest of the network can continue impersonating that party indefinitely [15].

**Security & Composability.**
We provide security analysis of our protocols within the Universally Composable (UC) Security framework [10].
For this purpose, we first formulate an ideal threshold signature functionality which guarantees that legitimate signatures are verifiable by the standard ECDSA verification
algorithm, and, at the same time, guarantees ideal and unconditional unforgeability.
We show that our protocols UC-realize this ideal functionality even in the presence of an attacker that adaptively corrupts and controls parties under the sole restriction that at most $t$ parties are corrupted in between two consecutive refresh phases.
This way, we can use universal composability to assert that the protocol remains unforgeable even when put together with arbitrary other protocols.
Such a strong property is of particular importance in
decentralized, complext and highly security sensitive distributed systems such as cryptocurrencies.

Security of the interactive protocol is proven assuming the unforgeability of ECDSA, the semantic security of Paillier encryption and strong-RSA.
It might appear a bit unsatisfying to have the unforgeability of ECDSA as an underlying assumption, given that it is an interactive – and by no means “simple” – assumption.
We do this since this is the weakest assumption that one can hope for: indeed, recall that unforgeability of ECDSA is not known to follow from any standard hardness assumption on elliptic curve groups (we do however know that ECDSA is existentially unforgeable in the generic group model [6]).

Security of the non-interactive protocol is proven under the same assumptions, but with a somewhat stronger unforgeability property of ECDSA, that considers situations where the adversary obtains, ahead of time, some “leakage” information on the random string that the signer will be using for generating the upcoming several signatures.
Still, the adversary should not be able to forge signatures, even given this leakage.
We call this property enhanced unforgeability, and demonstrate that (a) ECDSA is enhanced unforgeable in the generic group model, and (b) in some cases, enhanced ungorgeability of ECDSA follows from standard unforgeability of ECDSA, in the random oracle model.

</details>

**ラウンド・ミニマム対話型署名(Round-Minimal Interactive Signing)**
対話型であっても、我々のプロトコルは最先端のプロトコルの中で最もラウンド効率が良いことを強調します。そのため、ECDSAを必要とする多くのアプリケーション（暗号通貨の保管など）の性能を著しく向上させる可能性があります。

**プロアクティブキーリフレッシュ[48, 38, 14, 40]**。
閾値署名は、平文署名方式と比較してセキュリティ面で大きな改善をもたらしますが、全ての株主を時間の経過とともに順応的に一つずつ危険にさらす攻撃に対して脆弱である可能性が残されています。
この脆弱性は、長期間にわたって機能し、安全性を維持する必要がある方式では特に厄介なものです。
プロアクティブな閾値署名方式では、時間をエポックに分割し、各エポックの終わりに当事者が鍵やローカル状態を更新するプロトコルを実行することで、この懸念を軽減するように設計されています。
安全性の保証は、1つのエポック内で最大$t-1$の署名者が妥協する限り、より正確には、ある鍵更新の始まりから次の鍵更新の終わりまでの任意の時間内に、この方式が偽造不可能であり続けることです。
この方式の公開署名検証アルゴリズム（および鍵）は終始同じであることが強調されます。

このプロトコルは、2ラウンドの鍵更新フェーズを提供します。
標準的なセキュリティパラメータを選択した場合、計算にはおよそ$400+330n+n^2$のRSAリング演算が必要です。（詳細は付録Cを参照）
しかし、これは、リフレッシュが定期的にしか行われず、システムの利用が少ない時間にスケジュールできることを考えると、管理可能であると思われます。

図1の他のプロトコルはどれもプロアクティブな鍵更新をサポートしていないことを強調します。
実際、これらのプロトコルは、システムの進行に伴い適応的に当事者を堕落させる敵対者に対して、従来の閾値のセキュリティを提供することさえ知られていません。

**コミュニケーションモデル**
説明を簡単にするために、署名者は認証された（ただしロスのある）ブロードキャストメカニズムで接続されていると仮定します。
つまり、通信は公開され、送信されたすべてのメッセージはすべての当事者によって潜在的に受信される可能性があります。
それでも、敵対者は自由にメッセージを削除したり遅延させたりできます。
我々は、認証された通信を使用することが、実はプロアクティブなセキュリティを得るために不可欠であることに注目します。
実際、すでに確立された認証付き通信がなければ、以前に破損した当事者を形式的に「残し」、その当事者とネットワークの残りの部分の間のすべての通信を制御する敵対者は、その当事者に無期限になりすまし続けることができます。[15]

**セキュリティ＆コンポジット性(Security & Composability)**
我々は、Universally Composable (UC) Security framework [10]を用いて、我々のプロトコルの安全性分析を行います。
この目的のために、まず、標準的なECDSA検証アルゴリズムによって正当な署名が検証可能であることを保証する理想的な閾値署名機能を定式化し、同時に理想的かつ無条件の偽造不能性を保証します。
同時に、理想的かつ無条件に偽造不可能であることを保証します。
我々のプロトコルは、連続する2つのリフレッシュフェーズの間に最大$t$個のパーティが破損するという唯一の制約の下、パーティを適応的に破損し制御する攻撃者の存在下でも、この理想的な機能をUC実現することを示します。
このように、普遍的結合可能性を用いて、任意の他のプロトコルと一緒にしても、このプロトコルは偽造不可能であることを主張することができます。
このような強い性質は、以下のような場合に特に重要です。
暗号通貨のような分散型、複合型、セキュリティ感度の高い分散システムにおいて、このような強力な特性は特に重要です。

対話型プロトコルの安全性は、ECDSAの偽造不能性、Paillier暗号のセマンティックセキュリティ、strong-RSAを前提として証明されています。
ECDSAが対話型であり、決して「単純」ではないことを考えると、ECDSAの偽造不可能性を仮定することは少し不満に見えるかもしれません。
実際、ECDSAの偽造不能性は楕円曲線群の標準的な硬度仮定から導かれることが知られていないことを思い出してください。（ただし、ECDSAは一般群モデルにおいて存在的に偽造不能であることが分かっています。[6]）

非対話型プロトコルの安全性は同じ仮定の下で証明されるが、ECDSAの偽造不可能性をいくらか強化し、敵対者が署名者が今後いくつかの署名を生成するために使用するランダム文字列に関する何らかの「漏洩」情報を事前に入手した場合を考慮したものです。
しかし、このような漏洩があったとしても、敵対者は署名を偽造できないはずです。
我々はこの性質をenhanced unforgeabilityと呼び、（a）ECDSAは一般的なグループモデルにおいてenhanced unforgeableであること、（b）ある場合には、ランダムオラクルモデルにおいてECDSAの拡張unforgeabilityはECDSAの標準unforgeabilityに従うことを実証します。

## 1.2 技術（Our Techniques）

<details><summary>原文</summary>

Hereafter, $\mathbb{F}_q$ denotes the finite field with $q$ elements and $\mathcal{H}:\mathbb{M}\rightarrow\mathbb{F}_q$ denotes a hash function used for embedding messages into the field with $q$ elements.
Furthermore, let $(\mathbb{G},q,g)$ denote the group-order-generator tuple associated with the ECDSA curve.
We use multiplicative notation for the group-operation.

</details>

以下、$\mathbb{F}_q$は $q$ 個の要素を持つ有限体、$\mathcal{H}:\mathbb{M}\rightarrow\mathbb{F}_q$ は $q$ 要素のフィールドにメッセージを埋め込むために用いるハッシュ関数を示します。
また、ECDSA曲線に関連する群-位数-生成元タプルを $(\mathbb{G},q,g)$ とします。
群演算には乗法的な表記を用います。

### 1.2.1 背景（Background）

**通常（非閾値）ECDSA（Plain (Non-Threshold) ECDSA）**

<details><summary>原文</summary>

Recall that an ECDSA signature for secret key $x\in\mathbb{F}_q$ and message $\mathrm{msg}$ has the form $(\rho,k\cdot(m+\rho x))\in\mathbb{F}^2_q$ , where $m=\mathcal{H}(\mathrm{msg})$ , $\rho$ is the $x$-projection $\pmod{q}$ of the point $g^{k^{-1}}\in\mathbb{G}$, and $k$ is a uniformly random element of $\mathbb{F}_q$ .
The verification algorithm accepts a signature $(\rho,\sigma)$ as valid for message $\mathrm{msg}\in M$ with respect to public key $X=g^x\in\mathbb{G}$ , if $\rho$ is the $x$-projection of $g^{m\sigma^{-1}}\cdot X^{\rho\sigma^{-1}}$ , where $m=\mathcal{H}(\mathrm{msg})$.

</details>

秘密鍵 $x\in\mathbb{F}_q$ とメッセージ $\mathrm{msg}$ に対する ECDSA 署名は $(\rho,k\cdot(m+\rho x))\in\mathbb{F}^2_q$ の形をとることを想起してください。ここで、$m=\mathcal{H}(\mathrm{msg})$ , $\rho$ は点 $g^{k^{-1}}\in\mathbb{G}$ の $x$投影$\pmod{q}$、$k$ は $\mathbb{F}_q$ の一様乱数要素です。
検証アルゴリズムは、公開鍵 $X=g^x\in\mathbb{G}$ に対するメッセージ $\mathrm{msg}\in M$ の署名 $(\rho,\sigma)$ を有効であると認めます。ここで、$\rho$ を $g^{m\sigma^{-1}}\cdot X^{\rho\sigma^{-1}}$ の$x$投影とし、$m=\mathcal{H}(\mathrm{msg})$ とします。

**GennaroとGoldfederによる閾値ECDSAの概要[31]（Overview of the threshold ECDSA of Gennaro and Goldfeder [31]）**

<details><summary>原文</summary>

We first describe the basic protocol for the honest-but-curious case with security threshold $t=n-1$ , i.e. the case where all signatories follow the protocol.
Each signatory (henceforth, party) $\mathcal{P}_i$ chooses a random $x_i\in\mathbb{F}_q$ and sends $X_i=g^{x_i}$ to all other parties.
The public key is defined as $X=X_1\cdot\ldots\cdot X_n\in\mathbb{G}$.<sup>2</sup>
The secret key then corresponds to the value $x=x_1+\ldots+x_n$ (it is stressed that no one knows $x$).
In addition, each party $\mathcal{P}_i$ is associated with parameters for an additively homomorphic public encryption scheme (specifically, Paillier encryption).
That is, all parties know $\mathcal{P}_i$’s public encryption key, and $\mathcal{P}_i$ knows its own decryption key.
We write $\mathsf{enc}_i$, $\mathsf{dec}_i$ for the encryption and decryption algorithm associated with $\mathcal{P}_i$. 
It is stressed that all parties can run the encryption algorithm.

---

<sup>2</sup> For presentation purposes we use additive $n$-out-of-$n$ secret-sharing of the private key, instead of $n$-out-of-$n$ Shamir secretsharing that is prescribed in [31].

---

To sign a message $\mathrm{msg}$, the parties $\mathcal{P}_1,\ldots,\mathcal{P}_n$ generate local shares $k_1,\ldots,k_n$ , respectively, of the random value $k=k_1+\ldots +k_n$, as well as local shares $\gamma_1,\ldots,\gamma_n$, respectively, of an ephemeral value $\gamma=\gamma_1+\ldots+\gamma_n$ which will be used to mask $k$.
Using their respective encryption schemes, each pair of parties $\mathcal{P}_i,\mathcal{P}_j$ computes additive shares $\alpha _{i,j}, \hat \alpha _{i,j}$ for $\mathcal{P}_i$ and $\beta _{j,i}, \hat \beta _{j,i}$ for $\mathcal{P}_j$ , such that $\alpha _{i,j}+\beta _{j,i}= \gamma _j k_i$ and $\hat\alpha _{i,j}+\hat\beta _{j,i}= x_j k_i$.
In more detail, the share computation phase between $\mathcal{P}_i$ and $\mathcal{P}_j$ for computing $\alpha _{i,j}$ and $\beta _{j,i}$ proceeds as follows ( $\hat \alpha _{i,j}$ and $\hat \beta _{j,i}$ are analogously constructed).
Party $\mathcal{P}_i$ sends $K_i=\mathsf{enc}_i(k_i)$ to $\mathcal{P}_j$ , i.e. $K_i$ an encryption of $k_i$ under his own public key.
Then, $\mathcal P_j$ samples a random $\beta _{j,i}$ from a suitable range, and, using the homomorphic properties of the encryption scheme, $ \mathcal P_j $ computes $ D_{i,j} = ( \gamma _j \odot K_i )\oplus \mathsf{enc}_i ( - \beta _{j,i}) $ ,<sup>3</sup> i.e. $D_{i,j}$ is an encryption of $\gamma _jk_i- \beta _{j,i}$ under $\mathcal{P}_i$ ’s public key.
Finally, $\mathcal P_j$ sends $D_{i,j}$ to $\mathcal{P}_i$ who sets $\alpha_{i,j} =\mathsf{dec}_i(D_{i,j})$ , and the share-computation phase terminates.
Upon completion, each party $\mathcal{P}_i$ can compute $\delta_i=\gamma_ik_i+\Sigma_{j\ne i}\alpha_{i,j}+\beta_{i,j}$ , where $\delta_1,\ldots,\delta_n$ is an additive sharing of $\gamma k$, ie. $\gamma k=\delta_1+\ldots +\delta_n$.

---

<sup>3</sup> We emphasize that $\oplus$ and $\odot$ denote homomorphic evaluation of addition and (scalar) multiplication, respectively, rather than standard addition and multiplication.

---

Next, each $\mathcal{P}_i$ sends $(g^{\gamma_i},\delta_i)$ to all, and the parties compute $g^{k^{-1}}=(\prod_i g^{\gamma_i})^{(\Sigma_j\delta_j)^{-1}}$ , and obtain their respective shares $\sigma_1,\ldots,\sigma_n$ of $\sigma=k(m+\rho x)$, by setting $\sigma_i=k_im+\rho(x_ik_i+\Sigma_{j\ne i}\hat\alpha_{i,j}+\hat\beta_{i,j})$, where $m=\mathcal{H}(\mathrm{msg})$ is the hash-value of $\mathrm{msg}$ and $\rho$ is the $x$-projection of $g^{k^{-1}}$ .
Finally, each $\mathcal{P}_i$ sends $\sigma_i$ to all, and the signature is set to $(\rho,\sigma )$.
To sum up, the protocol proceeds as follows from party $\mathcal{P}_i$’s perspective, where each item denotes a round:

1. Sample $k_i$, $\gamma_i$ and send $K_i=\mathsf{enc}_i(k_i)$ to all.
2. When obtaining $\{K_j\}_{j\ne i}$, set $\{D_{j,i},\hat D_{j,i}\}_{j\ne i}$ as prescribed, and send $(D_{j,i},\hat D_{j,i})$ to $\mathcal{P}_j$ , for each $j\ne i$.
3. When obtaining $\{(D_{i,j},\hat D_{i,j})\}_{j\ne i}$, set $\delta_i$ as prescribed, and send $(\Gamma_i = g^{\gamma_i},\delta_i)$ to all.
4. When obtaining $\{(\Gamma_j,\delta_j)\}_{j\ne i}$, set $\sigma_i$ as prescribed, and send it to all.

**Output.**
When obtaining $\{\sigma_j\}_{j\ne i}$, set $\sigma$ and $\rho$ as prescribed, and output $(\rho,\sigma)$.

The above protocol takes four rounds of communication.
For security, it can be seen that, if everything was computed correctly, then up to the point where the $\sigma_i$’s are released, no coalition of up to $n-1$ parties gains any information on the secret key $x$ .
Furthermore, releasing $\sigma_i$ is equivalent to releasing the signature $(\rho,\sigma)$.
However, if a corrupted party deviates from the specification of the protocol, then releasing an honest party’s (maliciously influenced) signature-share $\sigma_i$ may reveal information about the secret key share (potentially the entirety of it).
To mitigate this problem, Gennaro and Goldfeder [31] devise a special-purpose, clever technique that allows the parties to verify the validity of the signature-shares before releasing them. However, this alternative technique ends up adding five rounds of communication.

</details>

まず、セキュリティ閾値 $t=n-1$ で生真面目（honest-but-curious）な場合、すなわち、すべての署名者がプロトコルに従う場合の基本プロトコルを説明します。
各署名者（以後、当事者）$\mathcal{P}_i$はランダムな$x_i\in\mathbb{F}_q$を選び、$X_i=g^{x_i}$ を他の全ての当事者へ送信します。
公開鍵は $X=X_1\cdot\ldots\cdot X_n\in\mathbb{G}$ と定義されます。<sup>2</sup>
秘密鍵は $x=x_1+\ldots+x_n$ という値に対応します。（誰も$x$ を知らないことが強調されています。）
また、各当事者 $\mathcal{P}_i$ には、加法的準同型性公開暗号方式（具体的には Paillier 暗号）のパラメータが関連付けられています。
すなわち、すべての当事者は $\mathcal{P}_i$ の公開暗号鍵を知っており、$\mathcal{P}_i$ は自身の復号鍵を知っています。
また、$\mathcal{P}_i$ に関連する暗号化、復号化アルゴリズムを $\mathsf{enc}_i$, $\mathsf{dec}_i$ と記します。
すべての関係者が暗号化アルゴリズムを実行できることが強調されます。

---

<sup>2</sup> プレゼンテーションのため、[31]で規定されている$n$-of-$n$ Shamir秘密分散の代わりに、秘密鍵の加法的$n$-of-$n$秘密分散を使用します。

---

メッセージ $\mathrm{msg}$ に署名するために、当事者 $\mathcal{P}_1,\ldots,\mathcal{P}_n$ はランダム値 $k=k_1+\ldots +k_n$ のローカルシェア $k_1,\ldots,k_n$ をそれぞれ生成し、$k$ をマスクするために使用されるエフェメラル（短命な、一時的な）値 $\gamma=\gamma_1+\ldots+\gamma_n$ のローカルシェア $\gamma_1,\ldots,\gamma_n$ もそれぞれ生成します。
それぞれの暗号化方式を使用して、当事者の各ペア $\mathcal{P}_i,\mathcal{P}_j$ は、$\alpha _{i,j}+\beta _{j,i}= \gamma _j k_i$ と $\hat\alpha _{i,j}+\hat\beta _{j,i}= x_j k_i$ のように、$\mathcal{P}_i$ に対して $\alpha _{i,j}, \hat \alpha _{i,j}$ 、 $\mathcal{P}_j$ に対して $\beta _{j,i}, \hat \beta _{j,i}$ の加法分散を計算します。
具体的には、$\alpha _{i,j}$ と $\beta _{j,i}$ を計算するための $\mathcal{P}_i$ と $\mathcal{P}_j$ の共有計算段階は以下のように進みます。（$\hat \alpha _{i,j}$ と $\hat \beta _{j,i}$ は同様に構築されます。）
当事者 $\mathcal{P}_i$ は、$K_i=\mathsf{enc}_i(k_i)$ を $\mathcal{P}_j$ に送ります、つまり $k_i$ を自身の公開鍵で暗号化したものを送ります。
そして、$\mathcal P_j$ は適当な範囲からランダムな $\beta _{j,i}$ をサンプリングし、暗号化方式の同型性を利用して $ D_{i,j} = ( \gamma _j \odot K_i )\oplus \mathsf{enc}_i ( - \beta _{j,i}) $ <sup>3</sup> を計算します。 すなわち、$D_{i,j}$ は $\gamma _jk_i- \beta _{j,i}$ を $\mathcal{P}_i$ の公開鍵で暗号化したものです。
最後に、 $\mathcal P_j$ は $D_{i,j}$ を $\mathcal{P}_i$ に送り、$\alpha_{i,j} =\mathsf{dec}_i(D_{i,j})$ として、共有計算のフェーズを終了させます。
完了すると、各当事者 $\mathcal{P}_i$ は $\delta_i=\gamma_ik_i+\Sigma_{j\ne i}\alpha_{i,j}+\beta_{i,j}$ を計算できます。ここで $\delta_1,\ldots,\delta_n$ は $\gamma k$ の加法共有です、すなわち、$\gamma k=\delta_1+\ldots +\delta_n$ です。

---

<sup>3</sup> ここで、$\oplus$と$\odot$は、標準的な加算と乗算ではなく、それぞれ加算と（スカラー）乗算の同型評価を表すことを強調します。

---

次に、各 $\mathcal{P}_i$ は $(g^{\gamma_i},\delta_i)$ を全員に送信し、当事者は $g^{k^{-1}}=(\prod_i g^{\gamma_i})^{(\Sigma_j\delta_j)^{-1}}$ を計算し、 $m=\mathcal{H}(\mathrm{msg})$ を $\mathrm{msg}$ のハッシュ値、 $\rho$ を $g^{k^{-1}}$ の $x$ 投影とし $\sigma_i=k_im+\rho(x_ik_i+\Sigma_{j\ne i}\hat\alpha_{i,j}+\hat\beta_{i,j})$ を設定して、 $\sigma=k(m+\rho x)$ のそれぞれの分け前 $\sigma_1,\ldots,\sigma_n$ を求めます。
最後に、各 $\mathcal{P}_i$ は $\sigma_i$ を全員に送信し、署名は $( \rho,\sigma )$ に設定されます。
まとめると、プロトコルは当事者の $\mathcal{P}_i$ から見て以下のように進行し、各項目はラウンドを表します。

1. $k_i$、$\gamma_i$ をサンプリングし、 $K_i=\mathsf{enc}_i(k_i)$ を全員に送信する。
2. $\{K_j\}_{j\ne i}$ を取得した場合、 $\{D_{j,i},\hat D_{j,i}\}_{j\ne i}$ を規定通りに設定し、各  $j\ne i$ に対して $(D_{j,i},\hat D_{j,i})$ を $\mathcal{P}_j$ に送信します。
3. $\{(D_{i,j},\hat D_{i,j})\}_{j\ne i}$ を得るとき、$\delta_i$ を規定通りに設定し、 $(\Gamma_i = g^{\gamma_i},\delta_i)$ を全員に送信します。
4. $\{(\Gamma_j,\delta_j)\}_{j\ne i}$ を得るとき、$\sigma_i$ を規定通りに設定し、全員に送信します。

**出力（Output）**
$\{\sigma_j\}_{j\ne i}$ を得るとき、$\sigma$ と $\rho$ を規定通りに設定し、$(\rho,\sigma)$ を出力します。

上記のプロトコルは、4ラウンドの通信を必要とします。
安全性については、すべてが正しく計算されていれば、$\sigma_i$ が解放される時点まで、最大 $n-1$ 個の当事者連合が秘密鍵 $x$ に関する情報を得ることはないことがわかります。
さらに、$\sigma_i$ を解放することは、署名 $(\rho,\sigma)$ を解放することと等価です。
しかし、破損した当事者がプロトコルの仕様から逸脱した場合、正直な当事者の (悪意ある影響を受けた)署名共有 $\sigma_i$ を公開すると、秘密鍵共有に関する情報 (潜在的にその全体) が漏洩する可能性があります。
この問題を軽減するために、GennaroとGoldfeder [31]は、署名共有を解放する前に、当事者が署名共有の有効性を検証することを可能にする特別な目的の巧妙な技術を考案している。しかし、この代替技法は結局5ラウンドの通信を追加することになります。

### 1.2.2 我々のアプローチ（Our Approach）

<details><summary>原文</summary>

Using the above blueprint, we show how the parties can verify the validity of the signature shares without adding any rounds on top of the 4 rounds of the basic protocol, and at a comparable computational cost to that of [31].
Interestingly, we achieve this result by employing the “generic” (and often deemed prohibitively expensive) GMW-approach of proving in zero-knowledge the validity of every computation along the way, with optimizations owing to the nature of the signature functionality.
Furthermore, our approach preserves the natural property of the basic protocol, whereby the message is used only in the fourth and last round.
This, in turn, leads to our non-interactive variant. Proactive key-refresh phases are also built in a natural way, on top of the basic protocol, with appropriate zero-knowledge proofs.

For the analysis, we take a different approach than that taken by either [31] or [45].
Recall that Gennaro and Goldfeder [31] only demonstrates that an adversary which interacts with a stand-alone instance of their protocol and (non-adaptively) corrupts $t<n$ parties cannot forge ECDSA signatures under the public key chosen by the parties.
On the other hand, Lindell et al. [45] show that their protocol UC-realizes the ECDSA functionality, in the presence of an adversary that non-adaptively corrupts $t<n$ parties.
The latter is indeed a stronger property than stand-alone unforgeability, in two ways:
First , this result holds even when the threshold signature protocol is part of a larger system.
Second, secure evaluation of the ECDSA functionality is significantly stronger than mere unforgeability.
While the first strengthening is clearly needed, the second is perhaps overly strong (for instance, it implies that the distribution of the secret randomness $k$ is almost uniform for all signatures, regardless of the message).

We take a mid-way approach: We formulate a threshold variant of the ideal signature functionality $\mathcal{F}_{\mathsf{sign}}$ of [11] and show that our protocol UC-realizes this functionality.
This way, we obtain a result that holds even when our threshold signature protocol is part of a larger system. On the other hand, we avoid the need to show that our protocol UC-realizes the ECDSA functionality.
This seemingly small difference turns out to be crucial: For one, this is what allows us to prove security under adaptive (and even mobile [48]) corruption of parties.
It also allows for a number of significant simplifications in the protocol.

</details>

上記の設計図を用いて、当事者は基本プロトコルの4ラウンドの上にラウンドを追加することなく、[31]と同等の計算コストで署名共有の正当性を検証する方法を示します。
興味深いことに、署名機能の性質に起因する最適化によって、途中のすべての計算の妥当性をゼロ知識で証明する「一般的な」（そしてしばしば法外に高価とみなされる）GMW-アプローチを採用することによってこの結果を達成しました。
さらに、我々のアプローチは、メッセージが4ラウンド目と最終ラウンドでのみ使用されるという基本プロトコルの自然な性質を維持します。
このことは、我々の非対話型変種につながる。プロアクティブなキーリフレッシュフェーズも、基本プロトコルの上に、適切なゼロ知識証明を用いて自然な方法で構築されます。

解析については、[31]や[45]とは異なるアプローチをとっています。
GennaroとGoldfeder [31]は、彼らのプロトコルのスタンドアローンインスタンスと対話し、 (非適応的に) $t<n$ 当事者を破損する敵は、当事者が選んだ公開鍵でECDSA署名を偽造できないことを 証明するだけだったことを思い出してください。
一方、Lindellら[45]は、非適応的に $t<n$ 当事者を破損する敵対者の存在下で、彼らのプロトコルがECDSAの機能をUC実現することを示しました。
後者は、2つの意味で、単体の偽造不可能性（Unforgeability）よりも強い性質を持っています。
第一に、この結果は、閾値署名プロトコルがより大きなシステムの一部である場合にも成り立ちます。
第二に、ECDSAの機能を安全に評価することは、単なる偽造防止機能よりもはるかに強力です。
While the first strengthening is clearly needed, the second is perhaps overly strong (for instance, it implies that the distribution of the secret randomness $k$ is almost uniform for all signatures, regardless of the message).
最初の強化は明らかに必要であるが、2番目はおそらく強すぎます。
（例えば、秘密の乱数性 $k$ の分布がメッセージに関係なく、すべての署名に対してほぼ一様であることを意味します。）

我々は中間的なアプローチをとります。
[11]の理想的な署名機能 $\mathcal{F}_{\mathsf{sign}}$ の閾値変形を定式化し、我々のプロトコルがこの機能を UC実現 していることを示します。
このように、我々の閾値署名プロトコルがより大きなシステムの一部である場合にも成立する結果を得ることができます。
一方、我々のプロトコルがECDSAの機能をUC実現することを示す必要はありません。
This seemingly small difference turns out to be crucial: For one, this is what allows us to prove security under adaptive (and even mobile [48]) corruption of parties.
この一見小さな違いは、極めて重要であることが判明します。
一つは、この違いによって、当事者の適応的な（さらには移動可能な[48]）破損のもとでの安全性を証明することができます。
また、プロトコルの大幅な簡略化も可能です。

### 1.2.3 プロトコルの概要（Protocol overview）

<details><summary>原文</summary>

We proceed with an overview of our protocol. For simplicity, we have omitted many of the details, especially regarding the zero-knowledge proofs.
We refer the reader to the subsequent technical sections for further details.
Let $P = \{\mathcal{P}_1,\ldots,\mathcal{P}_n\}$ denote the set of parties.
Let $(\mathsf{enc}_i,\mathsf{dec}_i)$ denote the Paillier encryptiondecryption algorithms associated with party $\mathcal{P}_i$ the public key is specified below.
Throughout, when we say that some party broadcasts a message, we mean that the party simply sends the message to all other parties.

**Key-Generation.**
As in the basic protcol, Each $\mathcal{P}_i$ samples a local random secret share $x_i\leftarrow\mathbb{F}_q$ of the (master) secret key $x=\sum_ix_i$ and then reveals $X_i=g^{x_i}$ by committing and then decommitting to the group-element in a sequential fashion.
In addition, each party $\mathcal{P}_i$ broadcasts a Schnorr NIZK (non-interactive zero-knowledge proof of knowledge) of $x_i$.

**Auxiliary Info & Key-Refresh.**
Each $\mathcal{P}_i$ locally generates a Paillier key $N_i$ and sends it to the other parties together with a NIZK that $N_i$ is well constructed (i.e., that it is a product of suitable primes).
Next, each $\mathcal{P}_i$ chooses a random secret sharing of $0=\sum_jx^j_i$ and computes $X^j_i=g^{x^j_i}$ and $C^j_i=\mathsf{enc}_j(x^j_i)$ , for every $j$ , including himself.<sup>4</sup>
$\mathcal{P}_i$ then broadcasts $(X^j_i,C^j_i)_j$ , together with a NIZK that the plaintext value of $C^j_i$ modulo $q$ is equal to the exponent of $X^j_i$ .
The parties update their key shares by setting $x^{\star}_i=x_i+\sum_j\mathsf{dec}_i(C^i_j)$ mod $q$ if all the proofs are valid and $\prod_k X^k_j =\mathsf{id}_{\mathbb{G}}$ , for every $j$ .

---

<sup>4</sup> This instruction may appear rather superfluous, but it is important to our security analysis; it allows extraction of the adversary’s randomness.

---

**Pre-Signing.**
One technical innovation that differentiates our protocol from [31] is our use of the Paillier cryptosystem as a commitment scheme.
Namely, the process of encrypting values under the parties’ own public keys yields a commitment scheme that is perfectly binding and computationally hiding (as long as Paillier is semantically secure). 
Therefore, in the protocol we instruct each party to commit to $\gamma_i$ and $k_i$ by encrypting those values under their own keys and broadcast $G_i=\mathsf{enc}_i(\gamma_i)$ and $K_i=\mathsf{enc}_i(k_i)$. <sup>5</sup>
Concurrently, the parties initiate the share-computation phase (for $x_jk_i=\alpha_{i,j}+\beta_{j,i}$ and $\gamma_jk_i=\hat\alpha_{i,j}+\hat\beta_{j,i}$ ), while proving in zero-knowledge that the values used in the multiplication are the same as the values encrypted in $G_i$, $K_i$, as well as the exponent of the public key-share $X_i=g^{x_i}$ .
Finally, when the aforementioned share-computation phase terminates, the parties communicate an additional message to obtain information for computing the point $R=g^{k^{-1}}\in\mathbb{G}$ on the curve which corresponds to the nonce of the (future) signature, while proving in zero-knowledge that the relevant message is consistent with the committed values $K_i$, $G_i$ and $X_i$ .
At the end of the presigning phase, each party $\mathcal{P}_i$ stores in memory the tuple $(k_i,\chi_i,R)$ , i.e. the share $k_i$ of $k$ (i.e. $\sum_ik_i=k$ ),the share $\chi_i$ of $kx$ (i.e. $\sum_i\chi_i=kx$ ), and the nonce $R=g^{k^{-1}}\in\mathbb{G}$ .

---

<sup>5</sup> Notice that the ciphertexts are computationally hiding and thus the adversary cannot correlate his own $k$’s and $\gamma$’s with the honest parties’ values.

---

The advantage of using the Paillier cryptosystem as a commitment scheme is twofold.
On one hand, Paillier ciphertexts are amenable to Schnorr-type proofs for proving the correctness of a prescribed computation.
On the other hand, in the security analysis, it allows the simulator to extract the adversary’s secrets, because the corrupted parties’ Paillier keys are extracted during the preceding auxiliary information phase.
We expand on this point in the following subsection.

The main purpose of the ZK-proofs is to bypass the security pitfalls (also highlighted in [31] and [45]) that arise from using Paillier encryption (which resides in a ring of integers modulo an RSA modulus) to derive group elements on the elliptic curve associated with ECDSA.
In more detail, malicious choices of $k$’s and $\gamma$’s may allow the adversary to probe bits of the honest parties’ secrets which may have devastating effect.
To remedy this, similarly to [31; 45], we use ZK-range proofs with purpose of “forcing” the adversary to choose values from a suitable range, thus preventing the aforementioned attack.
For this purpose we devise new and more efficient range proofs, taking advantage of the use of Paillier encryption as a perfectly binding commitment.

In summary, by virtue of the “Paillier commitments” and the accompanying ZK-proofs, party $\mathcal{P}_i$ is confident that the tuple $(R,k_i,\chi_i)$ is well-formed at the end of pre-signing phase, and there is no need for additional communication rounds to verify the correctness of the tuple, as opposed to [31, 45, 28, 21].

**Signing.**
Once a message $\mathrm{msg}$ is known, to generate a signature for pre-signing data $(R,k_i,\chi_i)$ , each $\mathcal{P}_i$ sets $m=\mathcal{H}(\mathrm{msg})$ , computes $\rho=R|_{x\mathrm{-axis}}$ , and sends $\sigma_i=k_im+\rho\chi_i \mod q$ to all parties.
After receiving the other parties’ sigmas, the parties output the signature $(\rho,\sum_i\sigma_i)=(\rho,k(m+\rho x))$ .

</details>

プロトコルの概要を説明します。
簡略化のため、特にゼロ知識証明に関する多くの詳細は省略しました。
詳細については、この後の技術的なセクションを参照してください。
$P = \{\mathcal{P}_1,\ldots,\mathcal{P}_n\}$ を当事者の集合とします。
公開鍵を持つ当事者 $\mathcal{P}_i$ に関連するPaillier暗号化・復号化アルゴリズムを $(\mathsf{enc}_i,\mathsf{dec}_i)$ とすると、以下のようになります。
全体を通して、ある当事者がメッセージをブロードキャストすると言うとき、その当事者は単に他のすべての当事者にメッセージを送信することを意味します。

**鍵生成（Key-Generation）**
基本プロトコルと同様に、各 $\mathcal{P}_i$ は（マスター）秘密鍵 $x=\sum_ix_i$ のローカルランダム秘密共有 $x_i\leftarrow\mathbb{F}_q$ をサンプリングし、群要素へのコミット、デコミットを順次行うことで $X_i=g^{x_i}$ を明らかにします。
さらに、各当事者 $\mathcal{P}_i$ は $x_i$ の Schnorr NIZK (非対話型ゼロ知識証明) をブロードキャストします。

**補助情報＆キーリフレッシュ（Auxiliary Info & Key-Refresh）**
各 $\mathcal{P}_i$ はローカルに Paillier 鍵 $N_i$ を生成し、それを NIZK と共に他の当事者に送信して、$N_i$ が適切に構成されている (つまり、適切な素数の積である) ことを示します。
次に、各 $\mathcal{P}_i$ は $0=\sum_jx^j_i$ のランダムな秘密分散を選び、自身を含む全ての$j$に対して、 $X^j_i=g^{x^j_i}$ と $C^j_i=\mathsf{enc}_j(x^j_i)$ を計算します。<sup>4</sup>
そして、$\mathcal{P}_i$ は $(X^j_i,C^j_i)_j$ をブロードキャストし、 $q$ のモジュロで $C^j_i$ の平文値が $X^j_i$  の指数に等しいというNIZKも一緒にブロードキャストします。
各当事者は、すべての証明が有効であれば $x^{\star}_i=x_i+\sum_j\mathsf{dec}_i(C^i_j) \mod q$ 、すべての $j$ に対して $\prod_k X^k_j =\mathsf{id}_{\mathbb{G}}$ 、と設定することで鍵共有量を更新します。

---

<sup>4</sup> 
この命令は一見余分なように見えますが、セキュリティ分析には重要で、敵のランダム性を抽出することができます。

---

**事前署名（Pre-Signing）**
[31]と異なる技術的な工夫は、コミットメント方式にPaillier暗号を用いたことです。
すなわち、当事者自身の公開鍵で値を暗号化する処理により、（Paillierが意味的に安全である限り）完全に拘束力があり計算隠蔽可能なコミットメント方式を得ることができます。
そこで、プロトコルでは、各当事者に $\gamma_i$ と $k_i$ を自身の鍵で暗号化し、$G_i=\mathsf{enc}_i(\gamma_i)$ と$K_i=\mathsf{enc}_i(k_i)$ をブロードキャストすることでコミットを指示します。<sup>5</sup>
同時に、両当事者は（ $x_jk_i=\alpha_{i,j}+\beta_{j,i}$ と $\gamma_jk_i=\hat\alpha_{i,j}+\hat\beta_{j,i}$ の）共有計算フェーズを開始し、乗算に使用する値が $G_i$ 、 $K_i$ で暗号化された値と同じであること、および公開鍵共有 $X_i=g^{x_i}$ の指数をゼロ知識で証明します。
最後に、前述の共有計算フェーズが終了すると、当事者は、（将来の）署名の非点に対応する曲線上の点 $R=g^{k^{-1}}\in\mathbb{G}$ を計算するための情報を得るための追加メッセージを通信し、当該メッセージが約束値 $K_i$、$G_i$、$X_i$ と一致することをゼロ知識で証明します。
事前署名フェーズの終了後、各当事者 $\mathcal{P}_i$ は、タプル $(k_i,\chi_i,R)$  、すなわち $k$ 共有 $k_i$ （すなわち $\sum_ik_i=k$ ）、 $kx$ 共有 $\chi_i$ （すなわち $\sum_i\chi_i=kx$ ）、およびナンス $R=g^{k^{-1}}\in\mathbb{G}$ をメモリに格納します。

---

<sup>5</sup> 暗号文は計算的に隠れているため、敵は自身の $k$ と $\gamma$ を正直な当事者の価値と相関させることはできません。

---

Paillier暗号方式をコミットメント方式として用いる利点は2つあります。
一方では、Paillier暗号文は、所定の計算の正しさを証明するためのSchnorr型証明に従順です。
一方、安全性分析においては、破損した当事者のPaillier鍵が直前の補助情報段階で抽出されるため、シミュレータが敵の秘密を抽出することが可能となります。
この点について、以下の小節で展開します。

ZK-証明（ZK-proofs）の主な目的は、ECDSAに関連する楕円曲線上の群要素を導出するためにPaillier暗号（RSAモジュラスの整数の環に存在する）を使用することから生じるセキュリティの落とし穴（[31]と[45]でも強調されています）を回避することです。
より詳細には、$k$ と $\gamma$ の悪意ある選択により、敵対者は正直者の秘密のビットを探ることができ、壊滅的な影響を与える可能性があります。
これを改善するため、[31; 45]と同様に、敵に適切な範囲から値を選ぶよう「強制」する目的でZK-範囲証明を使用し、前述の攻撃を防ぐことができます。
この目的のために、我々は完全拘束力のあるコミットメントとしてPaillier暗号を用いることを利用し、より効率的な新しい範囲証明を考案しました。

要約すると、"Paillier commitments "とそれに付随するZK-証明により、当事者 $\mathcal{P}_i$ は事前署名フェーズの終了時にタプル $(R,k_i, \chi_i)$ が整形されていると確信し、 [31, 45, 28, 21] とは逆にタプルの正確さを検証するために追加の通信ラウンドを必要としません。

**署名（Signing）**
メッセージ $\mathrm{msg}$ が分かれば、事前署名データ $(R,k_i,\chi_i)$ に対する署名を生成するために、各 $\mathcal{P}_i$ は $m=\mathcal{H}(\mathrm{msg})$ を設定し、 $\rho=R|_{x\mathrm{-axis}}$ を計算し、$\sigma_i=k_im+\rho\chi_i \mod q$ を全関係者に送信します。
他の当事者のシグマを受け取った後、当事者は署名 $(\rho,\sum_i\sigma_i)=(\rho,k(m+\rho x))$ を出力します。

### 1.2.4 オンラインと非対話型署名（Online vs Non-Interactive Signing）

<details><summary>原文</summary

**Online Signing.**
For interactive (online signing), the parties simply run the pre-signing stage followed by the signing stage, for a total of 4 rounds.

**Non-interactive Signing.**
To be able to sign non-interactively, the parties need to prepare some number of pre-signatures in an offline stage.
That is, for some pre-signing parameter $L\in\mathbb{N}$ , the parties run the pre-signing phase L-times concurrently and obtain pre-signing data $\{(\ell,R_\ell,k^\ell_i,\chi^\ell_i)\}_{\ell=1,\ldots,L}$.

Later, for each signature request using pre-signing data $(\ell,R_\ell,k^\ell_i,\chi^\ell_i)$ and message $\mathrm{msg}$, the parties run the signing phase for the relevant input to generate a signature.
The parties then erase the pre-signing tuple $(\ell,\ldots)$.
It is important to make sure that, as part of the refresh stage, any unused pre-signatures are discarded.<sup>6</sup>

_Remark_ 1.1.
It is stressed that the security analysis of the non-iteractive protocol is different than the online protocol, because the signature nonces (the $R$’s) are known well in advance of the corresponding messages to be signed.
As mentioned earlier, to prove security we rely on a stronger assumption about the unforgeability of the underlying (non-threshold) scheme, and we present it in more detail in the next section.

---

<sup>6</sup> Alternatively, it is possible to keep the presigning data as long as it is appropriately refreshed, i.e. by re-randomizing the pair $(k_i,\chi_i)$ .

---

</details>

**オンライン署名（Online Signing）**
対話型（オンライン署名）の場合は、署名前のステージと署名のステージの合計4ラウンドを実行するだけです。

**非対話型署名（Non-interactive Signing）**
非対話的に署名するためには、オフラインの段階で、ある程度の数の事前署名を用意する必要があります。
すなわち、ある事前署名パラメータ $L\in\mathbb{N}$ に対して、当事者は事前署名フェーズをL回同時に実行し、事前署名データ $\{(\ell,R_\ell,k^\ell_i,\chi^\ell_i)\}_{\ell=1,\ldots,L}$ を取得します。

その後、事前署名データ $(\ell,R_\ell,k^\ell_i,\chi^\ell_i)$ とメッセージ $\mathrm{msg}$ を用いた署名要求ごとに、該当する入力に対して署名フェーズを実行し、署名を生成します。
その後、署名前のタプル $(\ell,\ldots)$ を消去します。
フレッシュの段階で、未使用の事前署名は破棄されることを確認することが重要です。<sup>6</sup>

_備考_ 1.1.
非反復プロトコルのセキュリティ解析は、署名ナンス（$R$'s）が対応する署名対象メッセージよりかなり前に分かっているため、オンラインプロトコルとは異なることが強調されます。
先に述べたように、安全性を証明するためには、基礎となる（非閾値）スキームの偽造不可能性に関するより強い仮定に依存し、次のセクションでそれをより詳細に示します。

---

<sup>6</sup> また，適切なリフレッシュを行う限り，ペア $(k_i,\chi_i)$ を再ランダム化することで，事前署名データを保持することも可能です。

---

### 1.2.5 セキュリティ（Security）

<details><summary>原文</summary>

The present section assumes some familiarity with the ideal-vs-real paradigm and the UC-framework.

**Real-vs-Ideal Paradigm & UC.**
We prove security via the real-vs-ideal paradigm and the Universal Composability framework.
Namely, we show that our protocol emulates an ideal process involving an idealized version of the task at hand, and we prove that for every adversary attacking the protocol, there is an ideal adversary (referred to as a simulator) that achieves the same goals.
In the non-UC (standalone) framework, this is done using the adversary’s code and by extracting the adversary’s secrets (typically via rewinding).

The UC-framework augments the above paradigm with an entity, called the environment, that interacts with the adversary (in the real world) or the simulator (in the ideal world), together with the parties in the computation.
The goal of the environment is to guess which process (real or ideal) is executed.
If no environment can tell the difference between the real and ideal processes, it follows that the protocol is secure even “in the wild”; i.e. even when it is composed arbitrarily with other (cryptographic or non-cryptographic) components of some larger system.

One major technical difference between standalone-secure and UC-secure protocols is that, in the security analysis of the latter, the simulator’s arsenal of extraction techniques lacks rewinding.
This typically makes the protocol more complicated because it requires tools that are amenable to so-called online extraction (see e.g. the non-rewinding version of Schnorr’s NIZK proof of knowledge in Fischlin [29]).
Disallowing the rewinding technique in the security analysis is also one of the major obstacles towards achieving security against adaptive party corruptions.

</details>

本節では、理想と現実のパラダイムとUCフレームワークについて、ある程度理解していることを前提とします。

**現実対理想のパラダイムと UC（Real-vs-Ideal Paradigm & UC）**
現実対理想のパラダイムとUniversal Composabilityのフレームワークにより、安全性を証明します。
すなわち、我々のプロトコルは、手元のタスクを理想化したプロセスを含む理想的なプロセスをエミュレートすることを示し、プロトコルを攻撃するすべての敵対者に対して、同じ目標を達成する理想的な敵対者（シミュレータと呼ばれる）が存在することを証明します。
非UC（スタンドアロン）フレームワークでは、敵のコードを使用し、敵の秘密を（一般的には巻き戻しによって）抽出することによって行われます。

UCフレームワークは、上記のパラダイムを、敵対者（現実世界）またはシミュレータ（理想世界）、および計算当事者と相互作用する環境と呼ばれるエンティティで補強しています。
この環境の目的は、どの処理（現実と理想）が実行されたかを推測することです。
現実プロセスと理想プロセスの違いを見分けることができない環境であれば、プロトコルは「野生の」状態でも安全であることになります。つまり、ある大きなシステムの他の（暗号または非暗号）コンポーネントと任意に構成されても安全です。

スタンドアロンセキュアとUCセキュアの技術的な大きな違いは、後者の安全性解析において、シミュレータの抽出技術の武器である巻き戻しがないことです。
これは、いわゆるオンライン抽出が可能なツールを必要とするため、一般的にプロトコルが複雑になります。（例えば、Fischlin [29]のSchnorrのNIZK知識証明の非rewindingバージョン参照）
また、セキュリティ分析において巻き戻し技術を認めないことは、適応当事者の破損に対するセキュリティの実現に向けた大きな障害の一つです。

<details><summary>原文</summary>

**UC-Secure Threshold ECDSA vs Threshold Signature.**
One important difference between our security proof and the simulation-based security proof of [45, 28] is that our protocol UC-realizes a “generic” ideal threshold signature functionality, rather than the ECDSA functionality per se.
We opted for the former for the following reasons.
First, it captures more accurately the purpose of our protocol; our goal is to compute unforgeable signatures that are verifiable with the ECDSA algorithm, rather than realizing the ECDSA functionality itself.
Second, and more importantly, it allows us to reintroduce the rewinding technique in the security analysis, which greatly simplifies both the protocol and the security analysis, as we explain next.

**Threshold Signature Ideal Functionality & UC-simulation.**
We define an ideal threshold signature functionality modeled after the (non-threshold) signature functionality of Canetti [11].
The definition of the functionality aims at capturing the essence of any threshold signature scheme. 

Namely (and very loosely):

1. Authorized sets of parties may generate valid signatures for any given message.
2. Unauthorized sets of parties cannot compute valid signatures for messages that were never signed before.

We stress that the ideal functionality is utterly oblivious to the format of the signature scheme (there are effectively no private/public keys).
Consequently, the UC simulator is straightforward: It runs the programs of the uncorrupted parties without modification, interacting with the environment in away that is distributed identically as in the real system — as long as the environment doesn’t manage to forge a signature.Indeed, as long as the environment does not forge a signature, the simulation is perfect.

To demonstrate the validity of the simulation, it remains to show that the environment cannot forge signatures for some message that was never signed before; this is the crux of our security proof.
Before we describe the proof, we stress that here we are only interested in demonstrating validity of the UC simulation, by way of reduction to the hardness of the underlying assumptions.
These reductions are allowed to “take the environment offline” and employ the entire arsenal of extraction techniques, including rewinding.
What’s more, this approach gives full power to the proof over the random oracle, so that any reduction may suitably program the environment’s queries to the random oracle, as long as these were never queried before.

**Unforgeability Proof.**
We show unforgeability via reduction to the unforgeability of non-threshold ECDSA.
In more detail, we consider the following experiments involving a simulator attempting to simulate the environment’s interaction with the honest parties.

1. In the first experiment, the simulator follows the specifications of the protocol except that:

    (a) The simulator samples an ECDSA key-pair $(x,X)$ and fixes the public key of the threshold protocol to $X$ (this is achieved by rewinding the environment).

    (b) The simulator extracts the corrupted parties’ Paillier keys (this is achieved by programming the random oracle).

    (c) The simulator never decrypts the ciphertexts encrypted under the honest parties’ Paillier keys.
    Rather, to carry on the simulation, the simulator extracts the relevant values from the corrupted parties’ messages, using the Paillier keys extracted in Item 1b.

    (d) To compute the honest parties’ ZK-proofs, the simulator invokes the zero-knowledge simulator to generate valid proofs, and programs the random oracle accordingly.

2. The second experiment is identical to the first one except that:

    (a) At the beginning of the experiment, the simulator picks a random honest party that is henceforth deemed as special (in fact, to handle adaptive corruptions, this random party is chosen afresh every time the key-refresh phase is executed.
    If the environment decides to corrupt the special party, then the experiment is reset to the last preceding key-refresh; by rewinding the environment).

    (b) Every time an honest party is instructed to encrypt a value under the special party’s Paillier key and send it to the corrupted parties, the simulator sends a random encryption of 0 instead.

3. The third experiment is similar to the second one, except that the simulation is carried out without knowledge of the special party’s secrets, using a standard/enhanced ECDSA oracle.

We show that our scheme is unforgeable by showing that if an environment forges signatures in an execution of our protocol, then the environment also forges signatures in all three experiments above, and from the third experiment we conclude that the environment forges signatures for the plain (non-threshold) ECDSA signature scheme, in contradiction with its presumed security.
The first two experiments are stepping stones towards proving that the environment forges in the third experiment.
In more detail, the real execution and the first experiment are statistically close as long as all the ZK-proofs are sound (and the simulator extracts the right values).
The first and the second experiment are computationally close as long as the Paillier cryptosystem is semantically secure.
Finally, the second and the third experiment are identical (in a perfect sense).

**Dealing w/ Adaptive Party Corruptions.**
To show that our protocol achieves security against adaptive party corruption, it is enough to argue that experiments 2 & 3 terminate.
Assuming CDR and strong-RSA, our analysis yields that both experiments terminate in time quasi-proportional to the number of parties, and the environment forges signatures in the third experiment, in contradiction with the presumed security of plain ECDSA.
Consequently, under suitable cryptographic assumptions, unless the environment corrupts all parties simultaneously in-between key-refresh phases, our scheme is unforgeable.

**Overall UC-Security of our Protocol.**
From the above, it follows that if the ECDSA signature scheme is existentially unforgeable, then the online variant of our protocol UC-realizes the ideal signature functionality.
Similarly, if ECDSA is enhanced existentially unforgeable, then the offline variant of our protocol UC-realizes the ideal signature functionality.

We remind the reader that existential unforgeability is defined via a game where a prospective forger is given access to a signing oracle allowing the attacker to sign (arbitrary) messages of his own choosing. 
The attacker wins the game if he manages to generate a valid signature for a previously unsigned message. 
We define an enhanced variant of the unforgeability game where the data of the signature that is independent of the message (i.e. $g^{k^{-1}}$ , henceforth referred to as the signature’s nonce) can be queried by the attacker before producing a message to be signed; that way the attacker can potentially choose messages for the signing oracle that are correlated with the random nonce, which may be useful towards generating a forgery.

**Evidence for Enhanced Unforgeability.**
To support our assumption that ECDSA is enhanced existentially unforgeable, we show that it holds in the following idealized model:

1. In the random oracle model, as long as not too-many nonces are queried in advance, and standard (non-enhanced) ECDSA is existentially unforgeable.
2. In the random oracle and generic group model, unconditionally.<sup>7</sup>

Both of the above are shown via reduction.
For Item 1, the reduction simulates the random oracle and attempts to guess the messages the adversary is going to request signatures for; this is why not too-many nonces may be queried in advance, since the guessing probability decreases (super) exponentially.
For Item 2, the reduction simulates the group as if it were a free-group generated by two base-points $G$ and $X$ (corresponding to the group-generator and ECDSA public key, respectively).
Since the simulated (free) group is indistinguishable from a generic group, it follows that any forgery exploits a weakness in the hash-function, which we rule out by assumption.

---

<sup>7</sup> To be more precise, we show that any generic forger finds $x$, $y$ such that $\mathcal{H}(x)/\mathcal{H}(y)=e$, for a random $e\leftarrow\mathbb{F}_q$ , where $\mathcal{H}$ denotes the hash-function.
We conjecture that the latter is hard also for the actual implementation of ECDSA involving SHA.

---

</details>

**UCセキュアの閾値ECDSAと閾値署名の比較（UC-Secure Threshold ECDSA vs Threshold Signature）**
我々の安全性証明と[45, 28]のシミュレーションに基づく安全性証明との重要な違いの一つは、我々のプロトコルがECDSA機能そのものではなく、「一般的な」理想の閾値署名機能をUC実現することです。
私たちが前者を選んだのは、次のような理由によります。
第一に、このプロトコルの目的をより正確に捉えている。我々の目的は、ECDSAの機能そのものを実現することではなく、ECDSAアルゴリズムで検証可能な偽造できない署名を計算することです。
第二に、より重要なこととして、安全性解析において巻き戻し技術を再導入することができ、次に説明するように、プロトコルと安全性解析の両方を大幅に簡略化することができます。

**閾値署名の理想的な機能とUC-シミュレーション（Threshold Signature Ideal Functionality & UC-simulation）**
Canetti [11]の（非）閾値署名機能をモデルとして、理想的な閾値署名機能を定義します。
この機能性の定義は、あらゆる閾値署名方式の本質を捉えることを目的としています。

すなわち（非常に大雑把ですが）

1. 権限のある当事者のセットは、任意のメッセージに対して有効な署名を生成することができます。
2. 認可されていない当事者のセットは、以前に署名されたことのないメッセージに対して 有効な署名を計算することができません。

理想的な機能は、署名スキームの形式を全く気にしない（事実上、秘密鍵/公開鍵は存在しない）ことを強調します。
その結果、UCシミュレータは簡単であす。UCシミュレータは、破壊されていない当事者のプログラムをそのまま実行し、実システムと同じように分散された環境と相互作用する--環境が署名を偽造しない限り、シミュレーションは完全です。

このシミュレーションの有効性を証明するためには、今まで署名されたことのないメッセージに対する署名を、環境が偽造できないことを示す必要があります。これが、我々のセキュリティ証明の核心です。
この証明を説明する前に、我々はUCシミュレーションの有効性を、基礎となる仮定の硬さへの還元によって証明することにのみ興味があることを強調します。
これらの削減は、「環境をオフラインにする」ことができ、巻き戻しを含む、抽出技術のすべてのアーセナル（武器庫）を使用することが可能です。
さらに、このアプローチは、ランダムオラクルに対する証明に完全な力を与えるので、どのような削減も、以前に問い合わせたことがない限り、ランダムオラクルに対する環境の問い合わせを適切にプログラムすることができます。

**偽造不能性証明（Unforgeability Proof）**
非閾値ECDSAの偽造不能性への漸近により、偽造不能性を示します。
より詳細には、環境と正直者の相互作用をシミュレートしようとするシミュレータを含む次のような実験を考えます。

1. 最初の実験では、シミュレータは以下を除き、プロトコルの仕様に従います。

    (a) ECDSA鍵ペア $(x,X)$ をサンプリングし、閾値プロトコルの公開鍵を $X$ に固定します。（これは環境を巻き戻すことで達成されます。）

    (b）シミュレータは、破壊された当事者のPaillier鍵を抽出します。（これはランダムオラクルをプログラミングすることによって達成されます。）

    (c）シミュレータは、正直者のPaillier鍵で暗号化された暗号文を復号化することはありません。
    むしろ、シミュレーションを行うために、1bで抽出したPaillier鍵を用いて、破壊された当事者のメッセージから関連する値を抽出します。

    (d）誠実な当事者のZK証明を計算するために、シミュレータはゼロ知識シミュレータを呼び出して有効な証明を生成し、それに応じてランダムオラクルをプログラムします。

2. 第二の実験は、以下を除いて第一の実験と同じです。

    (a) 実験の始めに、シミュレータはランダムな正直者を選び、以後は特別とみなします。(実際には、適応的破壊を扱うために、このランダムな者は鍵更新フェーズが実行されるたびに新たに選ばれます。
    もし、環境が特別な当事者を堕落させることに決めたら、環境を巻き戻すことによって、実験は直前のキーリフレッシュまでリセットされます。）

    (b）正直者が特別者のPaillier鍵で値を暗号化し、破損者に送るよう指示されるたびに、シミュレータは代わりに0をランダムに暗号化したものを送信します。

3. 3番目の実験は、特別な当事者の秘密を知らずに、標準/拡張ECDSAオラクルを用いてシミュレーションを行うことを除いて、2番目の実験と同様です。

もし、ある環境が我々のプロトコルの実行において署名を偽造するならば、上記の3つの実験すべてにおいて環境も署名を偽造することを示すことによって、我々の方式が偽造不可能であることを示します。また、3番目の実験から、環境は平文（非閾値）ECDSA署名方式の署名を偽造し、その推定安全性と相反する結論となりました。
最初の2つの実験は、3番目の実験で環境が偽造することを証明するための足がかりとなるものです。
より詳細には、すべてのZK証明が健全である限り（そしてシミュレータが正しい値を抽出する限り）、実際の実行と最初の実験は統計的に近いものです。
また、1回目と2回目の実験は、Paillier暗号が意味的に安全である限り、計算上も近いです。
最後に、2番目の実験と3番目の実験は（完全な意味で）同一です。

**適応的な当事者の腐敗に対処（Dealing w/ Adaptive Party Corruptions）**
我々のプロトコルが適応的当事者破損に対する安全性を達成することを示すには、実験2と3が終了することを論証すれば十分です。
CDRとstrong-RSAを仮定すると、我々の分析から、両実験は当事者数に準比例した時間で終了し、3番目の実験では、環境が署名を偽造し、平文ECDSAの安全性と相反することが判明しました。
その結果、適切な暗号学的仮定の下で、鍵更新の間に環境が全ての当事者を同時に破壊しない限り、我々の方式は偽造不可能であることがわかりました。

**プロトコルのUC-Security（安全性）の全体像（Overall UC-Security of our Protocol）**
上記から、ECDSA 署名スキームが存在的に偽造不可能である場合、プロトコル UC のオンラインバリアントは理想的な署名機能を実現することになります。
同様に、ECDSA が存在的に偽造不可能に強化されている場合、プロトコル UC のオフラインバリアントは理想的な署名機能を実現します。

実存的な偽造不可能性は、攻撃者が自分で選択した (任意の) メッセージに署名できるようにする署名オラクルへのアクセス権が偽造者候補に与えられるゲームを介して定義されることを読者に思い出してもらいます。
攻撃者は、以前に署名されていないメッセージに対して有効な署名を生成できた場合、ゲームに勝利します。
メッセージから独立した署名のデータ (つまり、 $g^{k^{-1}}$ 、以降、署名のナンスと呼ばれる) を攻撃者が照会できる、偽造不可能性ゲームの強化されたバリアントを定義します。 署名するメッセージを作成する前。 そうすれば、攻撃者はランダムナンスと相関する署名オラクルのメッセージを選択できる可能性があり、偽造の生成に役立つ可能性があります。

**強化された偽造不能性の証拠（Evidence for Enhanced Unforgeability）**
ECDSAが実存的に偽造不可能であるという我々の仮定を裏付けるために、以下の理想化されたモデルにおいて、それが成り立つことを示します。

1. ランダムオラクルモデルでは、あまり多くのナンスが事前に照会されない限り、標準的な（非強化）ECDSAは存在的に偽造不可能です。
2. ランダムオラクルモデルと一般グループモデルにおいて、無条件に。<sup>7</sup>

上記の両方は、還元によって示されています。
項目 1 の場合、削減はランダムなオラクルをシミュレートし、敵対者が署名を要求しようとしているメッセージを推測しようとします。 これが、推測確率が (超) 指数関数的に減少するため、あまり多くのノンスを事前に照会できない理由です。
項目 2 の場合、縮小は、2 つの基点 $G$ と $X$ (それぞれ群 ジェネレーターと ECDSA 公開キーに対応する) によって生成された自由グループであるかのように、群をシミュレートします。
シミュレートされた (無料の) 群は一般的な群と区別がつかないため、偽造はハッシュ関数の弱点を悪用することになり、仮定によって除外されます。

---

<sup>7</sup> より正確には、任意の一般的な偽造者が、ランダムな $e\leftarrow\mathbb{F}_q$ に対して $\mathcal{H}(x)/\mathcal{H}(y)=e$ 、ここで $\mathcal{H}$ はハッシュ関数を表すような$x$, $y$を見つけることを示します。
SHAを含むECDSAの実際の実装でも後者が困難であると推測されます。

---

### 1.2.6 Non-Interactive Zero-Knowledge

Our protocol makes extensive use of Non-Interactive Zero-Knowledge (NIZK) via the standard technique of compiling three-move zero-knowledge protocols (also known as $\Sigma$-protocols) with the Fiat-Shamir transform (FS), i.e. the Verifier’s messages are computed by the Prover himself by invoking a predetermined hash function.
In the random-oracle model, the Fiat-Shamir transform gives rise to NIZK proof-systems.
Furthermore, because we completely avoid the need for “online extraction” (c.f. Section 1.2.5), our use of the Fiat-Shamir transform does not interfere with universal composability, and our protocol is UC as described.
We conclude the overview of our techniques by presenting a vanilla version of the (interactive) zeroknowledge technique we employ.
The technique is somewhat standard [3, 7, 8, 30, 46]; we spell it out here for convenience.
However, the analysis is somewhat complicated, and it is not crucial for understanding our threshold signature protocol.
Thus the present section may be skipped, if so desired.

**Paillier & Strong-RSA.**
We recall that Paillier ciphertexts have the form $C=(1+N)^xr^N \mod N^2$ , where $N$ denotes the public key, $x\in\mathbb{Z}_N$ the plaintext, and $r$ is a random element of $\mathbb{Z}^*_N$ .
We further recall the strong-RSA assumption: for an RSA modulus $N$ of unknown factorization, for uniformly random $y\in\mathbb{Z}_N$ , it is infeasible to find $(x,e)$ such that $e>1$ and $x^e=y \mod N$ .
Finally, before we describe the zero-knowledge technique, we (informally) define ring-Pedersen commitments.<sup>8</sup>

---

<sup>8</sup> We use the prefix “ring” to distinguish between “group” Pedersen commitments which reside in groups of known order.

---

**Definition 1.2** (Ring-Pedersen – Informal).
Let $N$ be an RSA modulus and let $s,t\in\mathbb{Z}^*_N$ be non-trivial quadratic residues.
A ring-Pedersen commitment of $m\in\mathbb{Z}_N$ with public parameters $(N,s,t)$ is computed as $C=s^mt^\rho \mod N$ where $\rho\leftarrow\mathbb{Z}_N$ .

**Vanilla ZK Range-Proof.** Consider the following relation:

$$
R=\{(C_0,N_0,C_1,N_1,s,t;\alpha,\beta,r)|C_0=(1+N_0)^\alpha r^{N_0}\mod N^2_0 \land C_1=s^\alpha t^\beta \mod N_1 \land \alpha\in\pm 2^\ell \}.
$$

In words, the Prover must show that the Paillier plaintext of $C_0$ is equal to the hidden value in the ring-Pedersen commitment $C_1$ , and that it lies in the range $\pm 2^\ell=[-2^\ell ,+2^\ell ]$ where $2^\ell\ll N_0,N_1$ .
It is assumed that the Paillier modulus N0 was generated by the Prover and the ring-Pedersen parameters $(N_1,s,t)$ were generated by the Verifier.
We further assume that $N_0$ and $N_1$ were generated as products of suitable<sup>9</sup> primes and that $s$ and $t$ are non-trivial quadratic residues in $\mathbb{Z}^*_{N_1}$ .
This assumption does not incur loss of generality, since in the actual protocol we instruct the parties to prove in zero-knowledge that all the parameters were generated correctly.<sup>10</sup>

---

<sup>9</sup> $N_0$ and $N_1$ should be bi-primes obtained as products of safe primes.
<sup>10</sup> In reality, for efficiency reasons, we prove much weaker statements that are sufficient for our purposes.

---

We now turn to the description of the ZK-proof for the relation $R$ under its interactive variant (the actual proof is compiled to be non-interactive using the Fiat-Shamir transform).
We perform a Schnorr-type proof as follows: the Prover encrypts a random value $\gamma$ as $D_0=(1+N_0)^\gamma\rho^{N_0}\mod N^2_0$ for suitable random $\rho$ , computes a ring-Pedersen commitment $D_1=s^\gamma t^\delta \mod N_1$ to $\gamma$ for suitable random $\delta$ , and sends $(D_0,D_1)$ to the Verifier.
The Verifier then replies with a challenge $e\leftarrow\pm 2^\ell$ and the Prover solves the challenge by sending $z_1=\gamma+e\alpha$ .
The Verifier accepts only if $z_1$ is in a suitable range and passes two equality checks (one for the encryption and one for the commitment).
Intuitively, the Prover cannot fool the Verifier because “the only way” for the Prover to cheat is knowing the order of $\mathbb{Z}^*_{N_1}$ , which was secretly generated by the Verifier and therefore would violate the strong-RSA assumption.
In more detail:

1. The Prover computes $D_0=(1+N_0)^\delta\rho^{N_0}\mod N^2_0$ and $D_1=s^\gamma t^\delta\mod N_1$ , for random elements $\gamma\leftarrow\pm 2^{\ell+\epsilon}$ , $\delta\leftarrow\pm N_1\cdot 2^\epsilon$ and $\rho\leftarrow\mathbb{Z}^*_{N_0}$ , and sends $(D_0,D_1)$ to the Verifier.
2. The Verifier replies with $e\leftarrow\pm 2^\ell$ .
3. The Prover computes
$$
\begin{cases}
z_1 &=\gamma +e\alpha \\
z_2 &=\delta + e\beta \\
w &= \rho\cdot r^e \mod N_0
\end{cases}
$$
and sends $(z_1,z_2)$ to the Verifier.

- **Verification:** Accept if the $z_1\in\pm 2^{\ell+\epsilon}$ and $(1+N_0)^{z_1}w^N=C^e_0\cdot D_0 \mod N^2_0$ and $s^{z_1}t^{z_2}=C^e_1\cdot D_1\mod N_1$ .

We remark that there is a discrepancy between the range-check of $z_1$ and the desired range by a (multiplicative factor) of $2^\epsilon$ , referred to as the slackness-parameter; this is a feature of the proof since the range of $\alpha$ is only guaranteed within that slackness-parameter.
We now turn to the analysis of the ZK-proof (completeness, honest verifier zero-knowledge & soundness).

It is straightforward to show that the above protocol satisfies completeness and (honest-verifier) zeroknowledge with some statistical error.
The hard task is showing soundness [8, 30, 46].
Following the standard paradigm, we show special soundness by extracting the secrets from two accepting transcripts of the form $(D_0,D_1,e,z_1,z_2,w)$ and $(D_0,D_1,e',z'_1,z'_2,w')$ such that $e\ne e'$ .
Let $\Delta_e$ , $\Delta_{z_1}$ , $\Delta_{z_2}$ denote the relevant differences.
We observe that if $\Delta_e$ divides $\Delta_{z_1}$ and $\Delta_{z_2}$ (in the integers), then all the values can be extracted without issue as follows: \alpha and \beta are set to $\Delta_{z_1}/\Delta_e\in\pm 2^{\ell+\epsilon}$ and $\Delta_{z_2}/\Delta_e$ , respectively, and $\rho$ can be extracted from the equality $(w\cdot w'^{-1})^N = (C_0(1+N_0)^{-\alpha})^{\Delta_e} \mod N^2_0$ (which allows to compute a $\Delta_e$-th root of $w/w'$ modulo $N_1$ c.f. Fact D.2).
Thus, the soundness-proof boils down to showing that $\Delta_e$ divides both $\Delta_{z_1}$ and $\Delta_{z_2}$ , unless the strong-RSA problem is tractable.
Namely, there exists an algorithm $\mathcal{S}$ with black-box access to the Prover that can solve the strong-RSA challenge $t$ (the second ring-Pedersen parameter).<sup>11</sup>

---

<sup>11</sup> Parameter $t$ is not completely random in $\mathbb{Z}_{N_1}$ since it’s a quadratic residue, but this does not affect the analysis.

---

To elaborate further, it is assumed that $\mathcal{S}$ knows $\lambda$ such that $t^\lambda =s\mod N_1$ and that $\lambda$ is sampled from $[N^2_1]$ (and not just $[N_1]$).
Thus, without getting too deep into the details, if $\Delta_e\not| \Delta_z =\lambda \Delta_{z_1} + \Delta_{z_2}$ , then $\mathcal{S}$ can solve the strong-RSA challenge by computing Euclid’s extended algorithm on $\Delta_e$ and $\Delta_z$ .
On the other hand, if $\Delta_e\not| \Delta_{z_1}$ or $\Delta_{z_2}$ , we claim that $\Delta_e | \Delta_{z}$ with probability at most $1/2$ .
To see why, observe that there exists at least another $\lambda'\ne\lambda$ in $[N^2_1]$ such that $t^\lambda = t^{\lambda'} = s \mod N_1$ , because $t$ has order $\phi(N_1)/4=O(N_1)$ and $\lambda$ was sampled uniformly in $[N^2_1]$ .
Since the Prover cannot distinguish between the two $\lambda$’s (in a perfect information-theoretic sense), if $\Delta_e\not| \Delta_{z_1}$ or $\Delta_{z_2}$ , then the probability that $\Delta_e$ divides $\lambda\Delta_{z_1}+\Delta_{z_2}$ is at most $1/2$ (i.e. the Prover guessed correctly which of the $\lambda$’s the algorithm $\mathcal{S}$ knows).<sup>12</sup>
In conclusion, the probability that extraction fails is at most twice the probability of breaking strong-RSA, which is assumed to be negligible.

---

<sup>12</sup> The argument is more subtle because we need to show that $\Delta_e$ cannot divide both values simultaneously (see Section 4.1).

---

**Removing the Computational Assumption in the ZK-Proof.**
We point at that there is a somewhat standard way [3, 4, 7] to tweak the above ZK-proof to obtain an unconditional extractor (that does not rely on strong-RSA or any other hardness assumption), at the expense of higher communication costs.<sup>13</sup>
Consider the relation

$$
R=\{(C_0,N_0;\alpha,r)|C_0=(1+N_0)^\alpha r^{N_0}\mod N^2_0 \land \alpha\in\pm 2^\ell \}.
$$

Notice that it’s the same as the previous relation except that we got rid of the ring-Pedersen commitment.
Then, by removing $D_1$ and $z_2$ from the protocol above, and restricting $e\leftarrow\{0,1\}$ (instead of $\pm 2^\ell$ ), we obtain a zero-knowledge proof of knowledge with unconditional extraction and soundness error $1/2$ .
Using the same notation as before, notice that the new protocol guarantees that $\Delta_e$ divides $\Delta_{z_1}$ since $\Delta_e\in\{-1,1\}$ and thus divisibility is guaranteed without any hardness assumption.
On the downside, a malicious Prover may always cheat with probability $1/2$ and thus the protocol must be repeated to achieve satisfactory soundness.
Since the protocol involves Paillier operations, this would incur a rather expensive (super-logarithmic) blowup factor of the proof size.

---

<sup>13</sup> A similar trick in a different context appears in Lindell [42], from Boudot [3] and Brickell et al. [4]

---


本プロトコルでは、3手ゼロ知識プロトコル（別名 $\Sigma$-プロトコル）をFiat-Shamir変換（FS）でコンパイルする標準的な手法、すなわち検証者のメッセージを証明者自身が所定のハッシュ関数を呼び出して計算することにより、NIZK（Non-Interactive Zero-Knowledge）を多用しています。
ランダムオラクルモデルでは、Fiat-Shamir変換により、NIZK証明体系が実現されます。
さらに、「オンライン抽出」（セクション1.2.5参照）の必要性を完全に回避しているため、Fiat-Shamir変換の使用は普遍的な合成性を妨げず、プロトコルは説明したようにUCとなります。
最後に、我々が採用している（対話的な）ゼロ知識技術のバニラ版を紹介し、我々の技術の概観を終えることにします。
この手法はある程度標準的なものである[3, 7, 8, 30, 46]が、ここでは便宜上、その説明を省略します。
しかし、この解析はやや複雑であり、我々の閾値署名プロトコルを理解する上で重要ではありません。
したがって、本項目は必要に応じて省略することができます。

**Paillierと強いRSA（Paillier & Strong-RSA）**
ここで、Paillier暗号文は、$N$が公開鍵、 $x\in\mathbb{Z}_N$ が平文、$r$が $\mathbb{Z}^*_N$ のランダムな要素であり $C=(1+N)^xr^N \mod N^2$ という形式をとることを思い出してください。
さらに、強いRSAの仮定を思い出します：未知の因数分解のRSA係数 $N$ に対して、一様ランダムな $y\in\mathbb{Z}_N$ に対して、 $e>1$ と $x^e=y \mod N$ を満たすような $(x,e)$ を見つけることは実行不可能です。
最後に、ゼロ知識技術を説明する前に、（非公式に）リングペダーセンコミットメント（ring-Pedersen commitments）を定義しておきます。<sup>8</sup>

---

<sup>8</sup> 接頭辞「リング（ring）」を使用して、既知の順序の群に存在する「グループ（group）」ペダーセン コミットメントを区別します。

---

**定義 1.2** (リングペダーセン - 非公式)
$N$ をRSA剰余とし、$s,t\in\mathbb{Z}^*_N$ を非自明な二次剰余とします。
公開パラメータ $(N,s,t)$ を持つ $m\in\mathbb{Z}_N$ のリングペダーセンコミットメントは、$\rho\leftarrow\mathbb{Z}_N$ で $C=s^mt^\rho \mod N$ として計算されます。

**バニラ ZK 範囲証明（Vanilla ZK Range-Proof）**
次のような関係を考えてみます。

$$
R=\{(C_0,N_0,C_1,N_1,s,t;\alpha,\beta,r)|C_0=(1+N_0)^\alpha r^{N_0}\mod N^2_0 \land C_1=s^\alpha t^\beta \mod N_1 \land \alpha\in\pm 2^\ell \}.
$$

つまり、証明者は、$C_0$ の Paillier 平文がリング-ペダーセン コミットメント $C_1$ の隠された値と等しく、それが $2^\ell\ll N_0,N_1$ の範囲 $\pm 2^\ell=[-2^\ell ,+2^\ell ]$ にあることを示さなければなりません。
Paillier係数 $N_0$ は証明者によって生成され、リング-ペダーセン パラメータ $(N_1,s,t)$ は検証者によって生成されたと想定されます。
さらに、$N_0$ と $N_1$ は適切な <sup>9</sup> 素数の積として生成され、$s$ と $t$ は $\mathbb{Z}^*_{N_1}$ の自明でない二次剰余であると仮定します。
実際のプロトコルでは、すべてのパラメータが正しく生成されたことをゼロ知識で証明するよう当事者に指示するため、この仮定は一般性を損ないません。<sup>10</sup>

---

<sup>9</sup> $N_0$ と $N_1$ は、安全な素数の積として得られる双素数でなければなりません。
<sup>10</sup> 実際には、効率化のために、もっと弱いステートメントを証明するのですが、それで十分なのです。

---

次に、関係 $R$ のZK証明について、その対話型変形（実際の証明は、Fiat-Shamir変換を用いて非対話型にコンパイルされる）の下で説明します。
Schnorr タイプの証明を次のように実行します。証明者は、適切なランダム $\rho$ の $D_0=(1+N_0)^\gamma\rho^{N_0}\mod N^2_0$  としてランダム値 $\gamma$ を暗号化し、適切なランダム $\delta$ の $\gamma$ へのリング ペダーセン コミットメント $D_1=s^\gamma t^\delta \mod N_1$ を計算し、 $(D_0,D_1)$ を 検証者 に送信します。
次に、検証者はチャレンジ $e\leftarrow\pm 2^\ell$ で応答し、証明者は $z_1=\gamma+e\alpha$ を送信してチャレンジを解決します。
検証者は、$z_1$ が適切な範囲内にあり、2 つの等価性チェック (暗号化用とコミットメント用) に合格した場合にのみ受け入れます。
直感的には、証明者が検証者を騙す「唯一の方法」は、Verifierが秘密裏に生成した $\mathbb{Z}^*_{N_1}$ の順序を知ることであり、strong-RSA仮定に反するため、証明者は検証者を騙すことができません。
詳しくはこちら

1. 証明者は、ランダム要素 $\gamma\leftarrow\pm 2^{\ell+\epsilon}$  、 $\delta\leftarrow\pm N_1\cdot 2^\epsilon$ 、および $\rho\leftarrow\mathbb{Z}^*_{N_0}$ に対して $D_0=(1+N_0)^\delta\rho^{N_0}\mod N^2_0$ および $D_1=s^\gamma t^\delta\mod N_1$ を計算し、 $(D_0,D_1)$  を検証者に送信します。
2. 検証者は $e\leftarrow\pm 2^\ell$ で応答します。
3. 証明者は計算します。
$$
\begin{cases}
z_1 &=\gamma +e\alpha \\
z_2 &=\delta + e\beta \\
w &= \rho\cdot r^e \mod N_0
\end{cases}
$$
$(z_1,z_2)$ を検証者に送信します。

- **検証（Verification）**
$z_1\in\pm 2^{\ell+\epsilon}$ と $(1+N_0)^{z_1}w^N=C^e_0\cdot D_0 \mod N^2_0$ と $s^{z_1}t^{z_2}=C^e_1\cdot D_1\mod N_1$ の場合は受け入れます。

We remark that there is a discrepancy between the range-check of $z_1$ and the desired range by a (multiplicative factor) of $2^\epsilon$ , referred to as the slackness-parameter; this is a feature of the proof since the range of $\alpha$ is only guaranteed within that slackness-parameter.
We now turn to the analysis of the ZK-proof (completeness, honest verifier zero-knowledge & soundness).

It is straightforward to show that the above protocol satisfies completeness and (honest-verifier) zeroknowledge with some statistical error.
The hard task is showing soundness [8, 30, 46].
Following the standard paradigm, we show special soundness by extracting the secrets from two accepting transcripts of the form $(D_0,D_1,e,z_1,z_2,w)$ and $(D_0,D_1,e',z'_1,z'_2,w')$ such that $e\ne e'$ .
Let $\Delta_e$ , $\Delta_{z_1}$ , $\Delta_{z_2}$ denote the relevant differences.
We observe that if $\Delta_e$ divides $\Delta_{z_1}$ and $\Delta_{z_2}$ (in the integers), then all the values can be extracted without issue as follows: \alpha and \beta are set to $\Delta_{z_1}/\Delta_e\in\pm 2^{\ell+\epsilon}$ and $\Delta_{z_2}/\Delta_e$ , respectively, and $\rho$ can be extracted from the equality $(w\cdot w'^{-1})^N = (C_0(1+N_0)^{-\alpha})^{\Delta_e} \mod N^2_0$ (which allows to compute a $\Delta_e$-th root of $w/w'$ modulo $N_1$ c.f. Fact D.2).
Thus, the soundness-proof boils down to showing that $\Delta_e$ divides both $\Delta_{z_1}$ and $\Delta_{z_2}$ , unless the strong-RSA problem is tractable.
Namely, there exists an algorithm $\mathcal{S}$ with black-box access to the Prover that can solve the strong-RSA challenge $t$ (the second ring-Pedersen parameter).<sup>11</sup>

---

<sup>11</sup> Parameter $t$ is not completely random in $\mathbb{Z}_{N_1}$ since it’s a quadratic residue, but this does not affect the analysis.

---

To elaborate further, it is assumed that $\mathcal{S}$ knows $\lambda$ such that $t^\lambda =s\mod N_1$ and that $\lambda$ is sampled from $[N^2_1]$ (and not just $[N_1]$).
Thus, without getting too deep into the details, if $\Delta_e\not| \Delta_z =\lambda \Delta_{z_1} + \Delta_{z_2}$ , then $\mathcal{S}$ can solve the strong-RSA challenge by computing Euclid’s extended algorithm on $\Delta_e$ and $\Delta_z$ .
On the other hand, if $\Delta_e\not| \Delta_{z_1}$ or $\Delta_{z_2}$ , we claim that $\Delta_e | \Delta_{z}$ with probability at most $1/2$ .
To see why, observe that there exists at least another $\lambda'\ne\lambda$ in $[N^2_1]$ such that $t^\lambda = t^{\lambda'} = s \mod N_1$ , because $t$ has order $\phi(N_1)/4=O(N_1)$ and $\lambda$ was sampled uniformly in $[N^2_1]$ .
Since the Prover cannot distinguish between the two $\lambda$’s (in a perfect information-theoretic sense), if $\Delta_e\not| \Delta_{z_1}$ or $\Delta_{z_2}$ , then the probability that $\Delta_e$ divides $\lambda\Delta_{z_1}+\Delta_{z_2}$ is at most $1/2$ (i.e. the Prover guessed correctly which of the $\lambda$’s the algorithm $\mathcal{S}$ knows).<sup>12</sup>
In conclusion, the probability that extraction fails is at most twice the probability of breaking strong-RSA, which is assumed to be negligible.

---

<sup>12</sup> The argument is more subtle because we need to show that $\Delta_e$ cannot divide both values simultaneously (see Section 4.1).

---

**Removing the Computational Assumption in the ZK-Proof.**
We point at that there is a somewhat standard way [3, 4, 7] to tweak the above ZK-proof to obtain an unconditional extractor (that does not rely on strong-RSA or any other hardness assumption), at the expense of higher communication costs.<sup>13</sup>
Consider the relation

$$
R=\{(C_0,N_0;\alpha,r)|C_0=(1+N_0)^\alpha r^{N_0}\mod N^2_0 \land \alpha\in\pm 2^\ell \}.
$$

Notice that it’s the same as the previous relation except that we got rid of the ring-Pedersen commitment.
Then, by removing $D_1$ and $z_2$ from the protocol above, and restricting $e\leftarrow\{0,1\}$ (instead of $\pm 2^\ell$ ), we obtain a zero-knowledge proof of knowledge with unconditional extraction and soundness error $1/2$ .
Using the same notation as before, notice that the new protocol guarantees that $\Delta_e$ divides $\Delta_{z_1}$ since $\Delta_e\in\{-1,1\}$ and thus divisibility is guaranteed without any hardness assumption.
On the downside, a malicious Prover may always cheat with probability $1/2$ and thus the protocol must be repeated to achieve satisfactory soundness.
Since the protocol involves Paillier operations, this would incur a rather expensive (super-logarithmic) blowup factor of the proof size.

---

<sup>13</sup> A similar trick in a different context appears in Lindell [42], from Boudot [3] and Brickell et al. [4]

---



