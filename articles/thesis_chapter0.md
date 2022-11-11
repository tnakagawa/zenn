---
title: "ペアリングベース暗号システムの実装について 0"
emoji: ""
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["crypto", "bls"]
published: false
---

# はじめに（Introduction）

以下の論文を[DeepL](https://www.deepl.com/translator)で翻訳しました。（多少修正しています。）

https://crypto.stanford.edu/pbc/thesis.pdf


# ペアリングベース暗号システムの実装について
ON THE IMPLEMENTATION OF PAIRING-BASED CRYPTOSYSTEMS

<details>
<summary>原文</summary>
A DISSERTATION SUBMITTED TO THE DEPARTMENT OF COMPUTER SCIENCE AND THE COMMITTEE ON GRADUATE STUDIES OF STANFORD UNIVERSITY IN PARTIAL FULFILLMENT OF THE REQUIREMENTS FOR THE DEGREE OF DOCTOR OF PHILOSOPHY
</details>

スタンフォード大学コンピュータサイエンス学部および大学院研究科委員会に提出した、哲学博士号取得のための要件の一部を満たす学位論文

Ben Lynn
June 2007

---

&copy; Copyright by Ben Lynn 2007
All Rights Reserved

---

# 要旨（Abstract）

<details>
<summary>原文</summary>
Pairing-based cryptography has become a highly active research area. We define bilinear maps, or pairings, and show how they give rise to cryptosystems with new functionality.

There is only one known mathematical setting where desirable pairings exist: hyperelliptic curves.
We focus on elliptic curves, which are the simplest case, and also the only curves used in practice. 
All existing implementations of pairing-based cryptosystems are built with elliptic curves. 
Accordingly, we provide a brief overview of elliptic curves, and functions known as the Tate and Weil pairings from which cryptographic pairings are derived.

We describe several methods for obtaining curves that yield Tate and Weil pairings that are efficiently computable yet are still cryptographically secure.

We discuss many optimizations that greatly reduce the running time of a naive implementation of any pairing-based cryptosystem.
These techniques were used to reduce the cost of a pairing from several minutes to several milliseconds on a modern consumer-level machine.

Applications of pairings are largely beyond our scope, but we do show how pairings allow the construction of a digital signature scheme with the shortest known signature lengths at typical security levels.
</details>

ペアリングに基づく暗号は、非常に活発な研究分野となっている。我々は双線形写像、すなわちペアリングを定義し、それらがどのように新しい機能を持つ暗号システムを生み出すかを示す。

望ましいペアリングが存在する数学的環境は、超楕円曲線しか知られていない。
本発表では、最も単純であり、実際に使用されている唯一の曲線である楕円曲線に着目する。
既存のペアリング暗号の実装はすべて楕円曲線で構築されている。
そこで、楕円曲線と、暗号のペアリングの元となるTateペアリング、Weilペアリングと呼ばれる関数について簡単に説明する。

また、暗号の安全性を確保しつつ、効率的に計算可能なTateペアリングやWeilペアリングが得られる曲線を得るためのいくつかの方法について説明する。

また、ペアリングに基づく暗号システムの素朴な実装の実行時間を大幅に短縮する多くの最適化について説明します。
これらの技術は、最新のコンシューマーレベルのマシンにおいて、ペアリングのコストを数分から数ミリ秒に削減するために使用されました。

ペアリングの応用は我々の範囲外であるが、ペアリングによって、一般的なセキュリティレベルで既知の最短署名長を持つデジタル署名方式を構築する方法を示す。

# 序文（Preface）

<details>
<summary>原文</summary>
Pairing-based cryptography is a relatively young area of cryptography that revolves around a particular function with interesting propreties.
It allows the construction of novel cryptosystems that are otherwise difficult or impossible to assemble using standard primitives.

We first define a cryptographic pairing abstractly and show how it can be used to build a signature scheme that is simple yet has many desirable properties.
Next we examine how a pairing can be implemented in practice. The only known mathematical setting where　suitable pairings exist are groups on certain families of curves.
We focus on the simplest and most well-understood case: elliptic curves.

We discuss methods for finding curves that yield cryptographic pairings, and outline various optimizations that can be applied.
We shall see that pairing-based cryptosystems are quite practical and compare well against traditional cryptosystems.

It is hoped that this work will be a useful guide to implementing pairing-based cryptography to a programmer who has experience with conventional cryptosystems.
While not comprehensive, the information contained herein should be enough to allow one to build practical pairing-based applications from scratch.
Notably absent are in-depth discussion of the characteristic 3 case, pairings where the subgroup size is necessarily significantly smaller than the field size, and hyperelliptic curves.

This text is intended to be self-contained.
Aside from arithmetic at the lowest level, algorithms not likely to be found in a basic course in number theory or cryptography are quoted here, enabling the reader to implement pairings without referring to any other sources.

Some background is required by the reader as we do not review basic abstract algebra.
On the other hand, we introduce enough elliptic curve theory for cryptographic purposes.
The following publications form the foundation of this thesis:
</details>

ペアリング暗号は、比較的新しい暗号分野であり、興味深い特性を持つ特定の関数を中心に展開されている。
この暗号は、標準的なプリミティブを用いなければ組み立てが難しい、あるいは不可能な新しい暗号システムを構築することを可能にします。

本稿では、まず暗号のペアリングを抽象的に定義し、それを用いて、単純でありながら多くの望ましい特性を持つ署名方式を構築する方法を示す。
次に、ペアリングが実際にどのように実装されるかを検討する。適切なペアリングが存在する唯一の数学的設定は、ある種の曲線の族上の群であることが知られている。
ここでは、最も単純でよく理解されている楕円曲線に注目する。

暗号のペアリングを実現する曲線の探索方法と、適用可能な様々な最適化の概要を説明する。
ペアリングに基づく暗号システムは非常に実用的であり、従来の暗号システムと比較して優れていることが分かるだろう。

従来の暗号方式を経験したプログラマにとって、ペアリング暗号を実装するための有用なガイドとなることが期待される。
また、本書の内容は包括的なものではないが、ゼロから実用的なペアリングアプリケーションを構築するのに十分なものであると思われる。
なお、特性3の場合、サブグループサイズがフィールドサイズより著しく小さい場合のペアリング、超楕円曲線については、深く掘り下げていません。

このテキストは自己完結することを意図している。
最下層の演算は別として、整数論や暗号学の基礎講座では出てこないようなアルゴリズムが引用されているので、読者は他の文献を参照することなくペアリングを実装することが可能である。

基本的な抽象代数の復習はしないので、読者にはある程度のバックグラウンドが必要である。
一方、暗号に必要な楕円曲線理論も十分に紹介している。
本論文の基礎となるのは以下の論文である:

- D.Boneh, B. Lynn, and H. Shacham. Short signatures from the Weil pairing. Journal of Cryptology, 17(1):297–319, 2004.

- P. S. L. M. Barreto, H. Y. Kim, B. Lynn and M. Scott. Efficient algorithms for pairing-based cryptosystems. In CRYPTO 2002, pages 354–368.

- P. S. L. M. Barreto, B. Lynn and M. Scott. Constructing elliptic curves with prescribed embedding degree. In Third Conference on Security in Communication Networks 2002.

- P. S. L. M. Barreto, B. Lynn and M. Scott. On the selection of pairing-friendly groups. In Selected Areas of Cryptography 2003

<details>
<summary>原文</summary>
More polished versions of the above works can be found in the Journal of Cryptology [18, 7].
Most of the described algorithms and optimizations feature in the PBC (Pairing-Based Cryptography) library, maintained by the author, and available under the GNU Public License at http://crypto.stanford.edu/pbc/.
</details>

上記の作品のより洗練されたバージョンはJournal of Cryptology [18, 7]で見ることができる。
説明したアルゴリズムと最適化のほとんどは、著者が保守しているPBC (Pairing-Based Cryptography) ライブラリに含まれており、GNU Public Licenseの下、http://crypto.stanford.edu/pbc/ で利用可能です。

