---
title: "BLS12-381 For The Rest Of Us"
emoji: ""
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["crypto", "bls"]
published: false
---

# はじめに（Introduction）

以下の内容を[DeepL](https://www.deepl.com/translator)で翻訳しました。（多少修正しています。）

https://hackmd.io/@benjaminion/bls12-381

# BLS12-381 For The Rest Of Us

<details>
<summary>原文</summary>

_Everything I wish I’d known before I started fiddling with this thing._

The elliptic curve BLS12-381 has become something of a celebrity in recent years.
Many protocols are putting it to use for digital signatures and zero-knowledge proofs: Zcash, Ethereum 2.0, Skale, Algorand, Dfinity, Chia, and more.

Unfortunately, existing material about BLS12-381 is full of obscure incantations like “instantiating its sextic twist”, and “optimal extension field towers”.
I’m here to fix that :smile:[1]

I won’t be giving a general introduction to elliptic curves and their exciting group properties.
There are already some great primers out there and I will be assuming basic knowledge of these things.
Equally, there’s much in here that is not specific to BLS12-381, and applies to other curves.

</details>

__これをいじり始める前に知っておきたかったこと全て。__

[楕円曲線BLS12-381](https://electriccoin.co/blog/new-snark-curve/)は、近年、ある種の有名人になっています。
多くのプロトコルで、デジタル署名やゼロ知識証明に利用されている。Zcash、Ethereum 2.0、Skale、Algorand、Dfinity、Chiaなどなどです。

残念ながら、BLS12-381に関する既存の資料は、「６次ツイストのインスタンス化」や「最適拡張場の塔」のような不明瞭な呪文でいっぱいです。
それを修正するためのものです :smile:[1].

楕円曲線とそのエキサイティングな群の特性について一般的な紹介をするつもりはありません。
すでに[素晴らしい入門書](https://andrea.corbellini.name/2015/05/17/elliptic-curve-cryptography-a-gentle-introduction/)がいくつかありますし、これらの基本的な知識を前提とするつもりです。
同様に、ここにはBLS12-381に特化したものではなく、他の曲線にも適用できることがたくさんあります。

## モチベーション（Motivation）

<details>
<summary>原文</summary>

BLS12-381 is a pairing-friendly elliptic curve.

Pairing-based cryptography has been developed over the last couple of decades, enabling useful new applications such as short digital signatures that are efficiently aggregatable, identity-based cryptography, single-round multi-party key exchange, and efficient polynomial commitment schemes such as KZG commitments.

Pairing-friendly elliptic curves are curves with both a favourable embedding degree (to be explained below!), and a large prime-order subgroup (also see below).
These are rare.
If you create an elliptic curve at random, it has a miniscule chance of being pairing-friendly.
However, they can be constructed, and BLS curves are explicitly constructed to be pairing-friendly. Several other families of pairing-friendly curves are also known.

Some good reading if you want to learn more about pairing-based cryptography:

- A short (but technical) explainer, and another one.
- Vitalik with a great general introduction to elliptic curve pairings.
- This NIST report is quite readable. I recommend Section 2 and the Appendix.
- Also good background is the draft IETF standard for pairing-friendly curves.

If you really want to understand this stuff then Pairings for Beginners is awesome.
It turns out to be a lot less scary than it looks if you work through it carefully, studying the examples as you go.
I really recommend this (I’m perpetually half way through it…).

</details>

BLS12-381はペアリングに適した楕円曲線である。

[ペアリングに基づく暗号](https://en.wikipedia.org/wiki/Pairing-based_cryptography)はここ数十年の間に開発され、[効率的に集約可能](https://crypto.stanford.edu/~dabo/pubs/papers/aggreg.pdf)な[短いデジタル署名](https://www.iacr.org/archive/asiacrypt2001/22480516.pdf)、[IDベース暗号](https://en.wikipedia.org/wiki/Boneh-Franklin_scheme)、単一ラウンドの[マルチパーティ鍵交換](http://cgi.di.uoa.gr/~aggelos/crypto/page4/assets/joux-tripartite.pdf)、[KZGコミットメント](https://dankradfeist.de/ethereum/2020/06/16/kate-polynomial-commitments.html)などの効率的な多項式コミットメント方式などの有用な新しいアプリケーションを可能にしています。

ペアリングに適した楕円曲線とは、好ましい埋め込み度（以下で説明します！）と、大きな素数次部分群（以下も参照）の両方を持つ曲線です。
このような楕円曲線は稀です。
楕円曲線をランダムに作成しても、ペアリングに適した楕円曲線になる確率はごくわずかです。
しかし、楕円曲線は作ることができ、BLS曲線はペアリングに適した曲線として明示的に構成されています。その他にも、ペアリング・フレンドリーな曲線のファミリーがいくつか[知られています](https://dankradfeist.de/ethereum/2020/06/16/kate-polynomial-commitments.html)。


ペアリングベース暗号についてもっと知りたい方は、ぜひご一読ください。

- 短い（しかし技術的な）[説明](https://courses.csail.mit.edu/6.897/spring04/L25.pdf)と、[もう1つ](https://www.math.uwaterloo.ca/~ajmeneze/publications/pairings.pdf)。
- Vitalikによる[楕円曲線ペアリング](https://vitalik.ca/general/2017/01/14/exploring_ecp.html)の素晴らしい一般的な紹介です。
- この [NIST のレポート](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC4730686/)は非常に読みやすいものです。第2節と付録がお勧めです。
- また、ペアリングに適した曲線のための[IETF標準のドラフト](https://tools.ietf.org/id/draft-yonezawa-pairing-friendly-curves-02.html)も良い背景となります。

本当に理解したいのなら、「[Pairings for Beginners](https://www.craigcostello.com.au/s/PairingsForBeginners.pdf)」はすごいですよ。
例題を勉強しながら丁寧に読み進めると、見た目よりもずっと怖くないことがわかります。
本当におすすめです（私はずっと途中までしか読んでいませんが・・・）。

## BLS12-381について（About curve BLS12-381）

### 由来（History）

<details>
<summary>原文</summary>

Curve BLS12-381 was designed by Sean Bowe in early 2017 as the foundation for an upgrade to the Zcash protocol.
It is both pairing-friendly (making it efficient for digital signatures) and effective for constructing zkSnarks.

A proliferation of “next-generation”, scalable blockchain protocols has put a premium on generating short digital signatures that can be efficiently aggregated or easily thresholded.
The properties of BLS12-381 frequently make it the curve of choice for these protocols.

Several cryptographic libraries—established ones such as Apache’s Milagro, and emerging ones such as Herumi and Blst—support BLS12-381.
And there are already moves to include BLS12-381 in IETF standards, such as Pairing-Friendly Curves, Hashing to Elliptic Curves, and BLS signatures.
This is good news for protocol interoperability!

</details>

Curve BLS12-381は、Zcashプロトコルのアップグレードの基礎として、2017年初めに[Sean Bowe](https://twitter.com/ebfull)によって[設計](https://electriccoin.co/blog/new-snark-curve/)されました。
ペアリングに優れ（デジタル署名に効率的）、かつzkSnarkを構築するのに有効です。

「次世代」のスケーラブルなブロックチェーンプロトコルの普及により、効率的に集約したり、簡単に閾値を設定できる短いデジタル署名の生成が重視されるようになりました。
BLS12-381の特性は、これらのプロトコルで頻繁に選択される曲線となっています。

Apacheの[Milagro](https://milagro.apache.org/)のような確立されたものから、[Herumi](https://github.com/herumi/mcl)や[Blst](https://github.com/supranational/blst)のような新しいものまで、いくつかの暗号ライブラリはBLS12-381をサポートしています。
また、[Pairing-Friendly Curves](https://tools.ietf.org/id/draft-yonezawa-pairing-friendly-curves-02.html#rfc.section.4.2.2)、[Hashing to Elliptic Curves](https://tools.ietf.org/html/draft-irtf-cfrg-hash-to-curve-05#section-8.9)、[BLS signature](https://tools.ietf.org/html/draft-irtf-cfrg-bls-signature-00#section-4.2)などのIETF標準にBLS12-381を含めようという動きもすでにある。
これはプロトコルの相互運用性にとって良いニュースです!

### ネーミング（Naming）

<details>
<summary>原文</summary>

BLS12-381 is part of a family of curves described by Barreto, Lynn, and Scott (they are the B, L, and S in view here - a different BLS trio will appear later).

The 12 is the embedding degree of the curve: neither too low, nor too high.
We’ll discuss embedding degrees in a little while.

The 381 is the number of bits needed to represent coordinates on the curve: the field modulus, $q$.
The coordinates of a point come from a finite field that has a prime order, and that prime number, $q$, is 381 bits wide.
381 is a fairly handy number as we can use 48 bytes per field element, with 3 bits left over for useful flags or arithmetic optimisations.
This size of this number is guided both by security requirements and implementation efficiency.

</details>

BLS12-381は、[Barreto、Lynn、Scott](https://eprint.iacr.org/2002/088.pdf)によって記述された曲線群の一部です（ここに表示されているのはB、L、Sで、別のBLSトリオは後ほど登場します）。

12は、曲線の埋め込み次数（embedding degrees）です。低すぎず、高すぎず、です。
埋め込み次数（embedding degrees）については、もう少し詳しく説明します。

381は曲線上の座標を表すのに必要なビット数：場のモジュラス、$q$ である。
点の座標は素数位数を持つ有限体から得られ、その素数 $q$ の幅は381ビットである。
381ビットというのはかなり便利な数字で、フィールドの要素ごとに48バイトを使い、残りの3ビットはフラグや演算の最適化に使うことができる。
この数値の大きさは、セキュリティ要件と実装効率の両方から導かれるものです。

## 曲線方程式とパラメータ（Curve equation and parameters）

<details>
<summary>原文</summary>

The basic equation of the BLS12-381 curve is $y^2=x^3+4$ .

The key parameters for a BLS curve are set using a single parameter $\mathbf{x}$ (different from the $x$ in the curve equation!) that can be selected to give the curve nice properties for implementation.
BLS12-381 is derived from the $k\equiv 0 \pmod{6}$ case of Construction 6.6 in the taxonomy.

Specific design goals for BLS12-381 are:

- $\mathbf{x}$ has “low hamming weight”, meaning that it has very few bits set to $1$.
This is particularly important for the efficiency of the algorithm that calculates pairings (the Miller loop).
- The field modulus $q$ mentioned above is prime and has 383 bits or fewer, which makes 64-bit or 32-bit arithmetic on it more efficient.
- The order $r$ of the subgroups we use is prime and has 255 bits or fewer, which is good for the same reason as above.
- The security target is 128 bits - see below.
- To support zkSnark schemes, we want to have a large power of two root of unity in the field $F_r$.
This means we want $2^n$ to be a factor of $r−1$, for some biggish $n$.
(Making $\mathbf{x}$ a multiple of $2^{\frac{n}{2}}$ will achieve this.)
This property is key to being able to use fast Fourier transforms for interesting things like polynomial multiplication.

The value $\mathbf{x} = \mathrm{-0xd201000000010000}$ (hexadecimal, note that it is negative) gives the largest $q$ and the lowest Hamming weight meeting these criteria.
With this $\mathbf{x}$ value we have,

|Parameter||Equation|Value (hexadecimal)|Comments|
|:--|:--|:--|:--|:--|
|Field modulus|$q$|$\frac{1}{3}(\mathrm{x}-1)^2(\mathrm{x}^4-\mathrm{x}^2+1)+\mathrm{x}$|0x1a0111ea397fe69a4b1ba7b6434bacd764774b84f38512bf6730d2a0f6b0f6241eabfffeb153ffffb9feffffffffaaab	|381 bits, prime|
|Subgroup size|$r$|$(\mathrm{x}^4-\mathrm{x}^2+1)$|0x73eda753299d7d483339d80809a1d80553bda402fffe5bfeffffffff00000001|255 bits, prime|

Reference for much of this section. Lots of curve data is also in the IETF specification.

</details>

BLS12-381曲線の基本方程式は $y^2=x^3+4$ である。

BLS曲線のキーパラメータは、$\mathbf{x}$ (曲線方程式の$x$とは異なる!)という単一のパラメータで設定され、このパラメータを選択することで曲線が実装上良い特性を持つようになります。
BLS12-381は、[taxonomy](https://eprint.iacr.org/2006/372.pdf)(タクソノミー？タキソノミー？）の Construction 6.6 の $k \equiv 0 \pmod{6}$ の場合から派生しています。

BLS12-381の具体的な設計目標は以下の通りである。

- hamming weightが小さいこと（low hamming weight）、つまり、$1$ にセットされるビットが非常に少ないこと。
これは、ペアリングを計算するアルゴリズム（ミラーループ）の効率を上げるために特に重要である。
- 上記のフィールドモジュラス $q$ は素数で383ビット以下であり、これに対する64ビットまたは32ビット演算が効率的である。
- 使用する部分群の位数 $r$ は素数で255ビット以下であり、上記と同様の理由で良好である。
- セキュリティの目標は128ビットである-後述。
- zkSnark方式をサポートするために、フィールド $F_r$ に大きな2のべき乗の単一根を持たせたい。
つまり、$2^n$ が $r-1$ の倍数になるように、$n$ を大きくする。
($\mathbf{x}$ を $2^{\frac{n}{2}}$ の倍数にすると、これが実現します）。
この性質は、高速フーリエ変換を多項式乗算のような面白いことに使えるようにするための鍵です。

この条件を満たす最大の$q$と最小のハミング重量を与えるのは、$\mathbf{x} = \mathrm{-0xd201000000010000}$ (16進数、負であることに注意)という値である。
この $\mathbf{x}$ の値で、次のようになる。

|Parameter||Equation|Value (hexadecimal)|Comments|
|:--|:--|:--|:--|:--|
|Field modulus|$q$|$\frac{1}{3}(\mathrm{x}-1)^2(\mathrm{x}^4-\mathrm{x}^2+1)+\mathrm{x}$|0x1a0111ea397fe69a4b1ba7b6434bacd764774b84f38512bf6730d2a0f6b0f6241eabfffeb153ffffb9feffffffffaaab	|381 bits, prime|
|Subgroup size|$r$|$(\mathrm{x}^4-\mathrm{x}^2+1)$|0x73eda753299d7d483339d80809a1d80553bda402fffe5bfeffffffff00000001|255 bits, prime|

このセクションの多くの部分を[参照](https://github.com/zkcrypto/pairing/tree/master/src/bls12_381)。また、多くの曲線データが[IETF仕様](https://datatracker.ietf.org/doc/id/draft-yonezawa-pairing-friendly-curves-02.html#rfc.section.4.2.2)にあります。

## 拡大体（Field extensions）

<details>
<summary>原文</summary>

Field extensions are fundamental to elliptic curve pairings.
The “12” is BLS12-381 is not only the embedding degree, it is also (relatedly) the degree of field extension that we will need to use.

The field $F_q$ can be thought of as just the integers modulo $q: 0,1,\ldots,q−1$.
But what kind of beast is $F_{q^{12}}$, the twelfth extension of $F_q$ ?

I totally failed to find any straightforward explainers of field extensions out there, so here’s my attempt after wrestling with this for a while.

Let’s construct an $F_{q^2}$ , the quadratic extension of $F_q$.
In $F_{q^2}$ we will represent field elements as first-degree polynomials like $a_0+a_1x$ , which we can write more concisely as $(a_0,a_1)$ if we wish.

Adding two elements is easy: $(a,b)+(c,d)=a+bx+c+dx=(a+c)+(b+d)x=(a+c,b+d)$ .
We just need to be sure to reduce $a+c$ and $b+d$ modulo $q$ .

What about multiplying? 
$(a,b)\times(c,d)=(a+bx)(c+dx)=ac+(ad+bc)x+bdx^2=???$ .
 Oops - what are we supposed to do with that $x^2$ coefficient?

We need a rule for reducing polynomials so that they have a degree less than two.
In this example we’re going to take $x^2+1=0$ as our rule, but we could make other choices.
There are only two rules about our rule[2]:

1.it must be a degree $k$ polynomial, where $k$ is our extension degree, $2$ in this case; and
2.it must be irreducible in the field we are extending.
That means it must not be possible to factor it into two or more lower degree polynomials.

Applying our rule, by substituting $x^2=−1$ , gives us the final result $(a,b)\times(c,d)=ac+(ad+bc)x+bdx^2=(ac−bd)+(ad+bc)x=(ac−bd,ad+bc)$ .
This might look a little familiar from complex arithmetic: $(a+ib)\times(c+id)=(ac−bd)+(ad+bc)i$.
This is not a coincidence! The complex numbers are a quadratic extension of the real numbers.

Complex numbers can’t be extended any further because there are no irreducible polynomials over the complex numbers.
But for finite fields, if we can find an irreducible $k$-degree polynomial in our field $F_q$ , and we often can, then we are able to extend the field to $F_{q^k}$ , and represent the elements of the extended field as degree $k−1$ polynomials, $a_0+a_1x+\ldots+a_{k−1}x^{k−1}$ .
We can represent this compactly as $(a_0,\ldots,a_{k−1})$ , as long as we remember that there may be some very funky arithmetic going on.

Also worth noting is that modular reductions like this (our reduction rule) can be chosen so that they play nicely with the twisting operation.

In practice, large extension fields like $F_{q^{12}}$ are implemented as towers of smaller extensions.
That’s an implementation aspect, so I’ve put it in the more practical section below.

</details>

楕円曲線ペアリングでは、拡大体が基本です。
BLS12-381の "12 "は埋め込み次数（embedding degree）だけでなく、（関連して）我々が必要とする拡大体の次数でもあります。

$F_q$ 体は $q: 0,1,\dots,q-1$ のモジュロ整数と考えることができる。
しかし、$F_q$ の12次拡大である $F_{q^{12}}$ はどのような獣なのでしょうか？

拡大体のわかりやすい解説を見つけることができなかったので、しばらく格闘した後、ここに書いてみます。

$F_q$ の2次拡大である $F_{q^2}$ を作ろう。
$F_{q^2}$ では、場の要素を $a_0+a_1x$ のような1次多項式で表しますが、もっと簡潔に $(a_0,a_1)$ と書いてもよいでしょう。

2つの要素を足すのは簡単で、 $(a,b)+(c,d)=a+bx+c+dx=(a+c)+(b+d)x=(a+c,b+d)$ となります。
あとは、$a+c$ と $b+d$ を $q$ のモジュロで必ず減らせばよい。

掛け算は？
$(a,b)\times(c,d)=(a+bx)(c+dx)=ac+(ad+bc)x+bdx^2=???$ .
 おっと、この $x^2$ の係数はどうすればいいんだろう？

多項式の次数が2より小さくなるように、多項式を削減するルールが必要です。
この例題では、 $x^2+1=0$ をルールとしますが、他の選択も可能です。
ルール[2]については、2つだけルールがあります。

1.次数 $k$ の多項式であること。$k$ は拡大次数で、この場合 $2$ である。
2.拡大体において[既約多項式](https://en.wikipedia.org/wiki/Irreducible_polynomial)であること。
つまり、2以上の低次の多項式に分解することができないものでなければならない。

この規則を適用して $x^2=-1$ を代入すると、最終的に $(a,b)\times(c,d)=ac+(ad+bc)x+bdx^2=(ac-bd)+(ad+bc)x=(ac-bd,ad+bc)$ になります。
これは、複素数計算でおなじみの $(a+ib)\times(c+id)=(ac-bd)+(ad+bc)i$ ですね。
これは偶然の一致ではありません。これは偶然ではありません！複素数は実数の2次拡大です。

複素数には[既約多項式がない](https://en.wikipedia.org/wiki/Fundamental_theorem_of_algebra)ので、それ以上拡張することはできません。
しかし、有限体では、もし私たちの場 $F_q$ で既約 $k$ 次の多項式を見つけることができれば、その体を $F_{q^k}$ に拡張することができます。そして、その体の要素を $a_0+a_1x+\ldots +a_{k-1}x^{k-1}$ という $k-1$ 次の多項式として表します。
これをコンパクトに表すと $(a_0,\ldots,a_{k-1})$ となり、非常におかしな演算が行われている可能性があることを覚えておけばOKです。

また、注目すべきは、このようなモジュラー・リダクション（我々のリダクション・ルール）は、ねじれ操作とうまく調和するように選ぶことができることである。

実際には、$F_{q^{12}}$のような大きな拡張場は、小さな拡張場の塔として実装されます。
それは実装の側面なので、以下のより実用的なセクションに入れた。

## 曲線（The curves）

<details>
<summary>原文</summary>

One of the initially non-obvious things about BLS12-381 is that we’re really dealing with two curves, not one.
Both curves share more-or-less the same curve equation, but are defined over different fields.

The simpler one is over the finite field $F_q$ , which is just the integers mod $q$ .
So the curve has points only where the equation $y^2=x^3+4$ has solutions with $x$ and $y$ both integers less than $q$ .
Such a point is $(0,2)$ , for example[3]. We shall call this curve $E(F_q)$ .

The other curve is defined over an extension of $F_q$ to $F_{q^2}$ (think complex numbers). In this case, the curve equation is slightly modified to be $y^2=x^3+4(1+i)$[4], and we’ll call the curve $E'(F_{q^2})$[5].
We’ll explain where this comes from under Twists, below.

As an aside, the curve order of $E'(F_{q^2})$ is vastly bigger than that of $E(F_q)$:
The curve equation has many more solutions when the domain is extended to the complex numbers.
In fact, the order of $E$ is close to $q$ , and the order of $E'$ is close to $q^2$ .
This is no accident, but a result of the Hasse bound.

</details>

BLS12-381について、当初は明らかでなかったことのひとつに、私たちが実際に扱っているのは1つではなく2つの曲線であるということがあります。
両方の曲線はほぼ同じ曲線方程式を共有していますが、異なる体上で定義されています。

もっと簡単なのは有限体 $F_q$ 上のもので、これは単に整数の mod $q$ である。
したがって、この曲線は、方程式 $y^2=x^3+4$ が、 $x$ と $y$ がともに $q$ より小さい整数である解を持つ点のみを持つことになる。
このような点は、例えば $(0,2)$ である[3]。この曲線を $E(F_q)$ と呼ぶことにする。

もう一つの曲線は、$F_q$ の $F_{q^2}$ への[拡張](https://en.wikipedia.org/wiki/Field_extension)上で定義される(複素数を考える)。この場合、曲線の方程式は $y^2=x^3+4(1+i)$[4] と少し修正され、この曲線を $E'(F_{q^2})$[5] と呼ぶことにします。
これがどこから来たかは、後述の「ねじれ」のところで説明する。

余談ですが、$E'(F_{q^2})$ の曲線次数は $E(F_q)$ のそれよりも圧倒的に大きな値になっています。
この曲線方程式は、領域を複素数まで広げると、より多くの解を持つようになります。
実際、$E$ の次数は $q$ に近く、$E'$ の次数は $q^2$ に近い。
これは偶然ではなく、[ハッセの束縛](https://en.wikipedia.org/wiki/Hasse%27s_theorem_on_elliptic_curves)の結果である。

## 部分群（The Subgroups）

<details>
<summary>原文</summary>

In this section and the next I’ll explain how BLS12-381 ended up having two curve equations rather than one.

A pairing is a bilinear map. This means that it takes as input two points, each from a group of the same order, $r$ . 
$r$ must be prime, and for security needs to be large.
Also, for rather technical reasons, these two groups need to be distinct.
Let’s call them $G_1$ and $G_2$ .

Unfortunately, our simple curve $E(F_q)$ has only a single large subgroup of order $r$ , so we can’t define a pairing based solely on $E(F_q)$ .

However, if we keep extending the field over which $E$ is defined, it can be proved that we eventually find a curve that has more than one subgroup of order $r$ (in fact, $r+1$ of them).
That is, for some $k, E(F_{q^k})$[6] contains other subgroups of order $r$ that we can use.
One of these subgroups contains only points having a trace of zero[7], and we choose that subgroup to be $G_2$ .

This number $k$, the amount that we need to extend the base field by to find the new group, is called the embedding degree of the curve, which in our case is the “12” in BLS12-381.
We’ll discuss embedding degree more in a moment.

For completeness, note that each of $G_1$ and $G_2$ shares with its containing curve the “point at infinity”.
This is the identity element of the elliptic curve arithmetic group, often denoted $\mathcal{O}$ .
For any point $P,P+\mathcal{O}=\mathcal{O}+P=P$ .

So, at this point, we have a group $G_1$ of order $r$ in $E(F_q)$ , and we have a distinct group $G_2$ of order $r$ in $E(F_{q^{12}})$ .
Yay - we can do pairings!

</details>

この章と次の章では、BLS12-381の曲線方程式が1つではなく2つになってしまった理由を説明します。

ペアリングはバイリニア写像である。つまり、2つの点を入力とし、それぞれを同じ次数 $r$ の群とする。
$r$ は素数でなければならないし、安全性のために大きくする必要がある。
また、技術的な理由から、この2つの群は別個のものである必要がある。
これらを $G_1$ と $G_2$ と呼ぶことにする。

残念ながら、この単純な曲線 $E(F_q)$ は次数 $r$ の大きな部分群しか持たないので、 $E(F_q)$ だけからペアリングを定義することはできないのですが、 $E(F_q)$ を使ってペアリングを定義することができます。

[しかし](https://web.archive.org/web/20131203082655/https://www.computing.dcu.ie/~mike/tate.html)、$E$ が定義されている体をどんどん拡大していけば、やがて次数 $r$ の部分群を複数（実際には $r+1$ 個）持つ曲線が見つかることが証明される。
つまり、ある $k$ に対して、$E(F_{q^k})$[6] には、使える他の位数 $r$ の部分群が含まれている。
そのうちの1つは、トレースが0である点のみを含む部分群[7]であり、その部分群 を$G_2$ と呼ぶことにする。

この数 $k$ は、新しい群を見つけるために基底場をどれだけ拡張すればよいかということで、曲線の埋め込み次数と呼ばれ、我々の場合は BLS12-381 の "12" にあたります。
埋め込み次数については、もう少し詳しく説明します。

$G_1$ と $G_2$ のそれぞれは、その含む曲線と「無限遠の点」を共有していることに注意してください。
これは、楕円曲線算術群の恒等式要素であり、しばしば $\mathcal{O}$ と表記される。
任意の点に対して $P,P+\mathcal{O}=\mathcal{O}+P=P$ 

つまり、この時点で、$E(F_q)$ に位数 $r$ の群 $G_1$ があり、$E(F_{q^{12}})$ に位数 $r$ の異なる群 $G_2$ があることがわかります。
やったー、ペアリングができるぞー。

## ねじれ（Twists）

<details>
<summary>原文</summary>

But there’s another challenge.
As discussed earlier, doing arithmetic in $F_{q^{12}}$ is horribly complicated and inefficient.
And curve operations need a lot of arithmetic.
But it looks like that’s what we are stuck with.

Or are we? Well, there’s a twist in the tale…[8]

A twist is something like a coordinate transformation. Rather wonderfully, this can be used to transform our $E(F_{q^{12}})$ curve into a curve defined over a lower degree field that still has an order $r$ subgroup.
Moreover, this subgroup has a simple mapping to and from our $G_2$ group[9].

BLS12-381 uses a “sextic twist”.
This means that it reduces the degree of the extension field by a factor of six.
So $G_2$ on the twisted curve can be defined over $F_{q^2}$ instead of $F_{q^{12}}$, which is a huge saving in complexity.

I haven’t seen this written down anywhere—but attempting to decode section 3 of this — if we find a $u$ such that $u^6=(1+i)^{−1}$ , then we can define our twisting transformation as $(x,y)\rightarrow (x/u^2,y/u^3)$ .
This transforms our original curve $E:y^2=x^3+4$ into the curve $E':y^2=x^3+4/u^6=x^3+4(1+i)$ . 
$E$ and $E'$ look different, but are actually the same object presented with respect to coefficients in different base fields[10].

When the twist is done correctly, the resulting $E'$ has a subgroup of order $r$ that maps to our $G_2$ group and vice-versa.
So, it turns out that we can work in $E'$ over $F_{q^2}$ for most purposes, and map $G_2$ back to $E(F_{q^{12}})$ only when required (i.e. while actually computing pairings).

So these are the two groups we will be using:

- $G_1\subset E(F_q)$ where $E:y^2=x^3+4$
- $G_2\subset E'(F_{q^2})$ where $E':y^2=x^3+4(1+i)$

And that’s the story of why BLS12-381 looks like two curves, not one.

Note that coordinates of points in the $G_1$ group are pairs of integers, and coordinates of points in the $G_2$ group are pairs of complex integers, so $G_2$ points take twice the amount of storage, and are more expensive to work with.
This leads to interesting implementation tradeoffs.

</details>

しかし、もう一つ課題があります。
先に述べたように、$F_{q^{12}}$で演算をすることは恐ろしく複雑で非効率です。
そして、曲線演算は多くの演算を必要とします。
しかし、それで行き詰っているように見えます。

それとも私たち？さて、物語にはどんでん返しがある...[8]。

[ねじれ（ツイスト）](http://indigo.ie/~mscott/twists.pdf)とは、座標変換のようなものです。これを使うと、$E(F_{q^{12}})$ 曲線を、低次の体上で定義された曲線に変換することができるのですが、その際、やはり位数 $r$ の部分群を持つことになります。
さらに、この部分群は、我々の $G_2$ 群との間で簡単な写像を持つ[9]。

BLS12-381は、「6次ツイスト（sextic twist）」を採用しています。
これは、拡大体の次数を6分の1にすることを意味します。
つまり、ねじれた曲線上の $G_2$ は $F_{q^{12}}$ ではなく $F_{q^2}$ 上で定義できるため、複雑さが大幅に軽減されるのです。

このことはどこにも書いていないのですが、[この](https://eprint.iacr.org/2005/133.pdf)第3節を解読してみると、 $u^6=(1+i)^{-1}$ となるような $u$ があれば、ねじれ変換を $(x,y)\rightarrow (x/u^2,y/u^3)$ として定義することができるのですね。
これによって、元の曲線 $E:y^2=x^3+4$ は、曲線 $E':y^2=x^3+4/u^6=x^3+4(1+i)$ に変換される。
$E$ と $E'$ は異なるように見えるが、実は同じ物体を異なる基底場の係数に関して表現したものである[10]。

ねじれが[正しく行われる](http://indigo.ie/~mscott/twists.pdf)と、結果として得られる $E'$ は、我々の $G_2$ 群に写像する次数 $r$ の部分群を持ち、その逆もまた真なりとなる。
つまり、ほとんどの場合、$F_{q^2}$ 上の $E'$ で作業し、$G_2$ を必要なときだけ（つまり、実際にペアリングを計算しているとき）$E(F_{q^{12}})$ にマップすることができることがわかった。）

ということで、今回使用するのはこの2グループです。

- $G_1\subset E(F_q)$ ここで $E:y^2=x^3+4$
- $G_2\subset E'(F_{q^2})$ ここで $E':y^2=x^3+4(1+i)$

そして、BLS12-381が1本ではなく、2本の曲線に見えるのは、そういうことなのだ。

$G_1$ 群の点の座標は整数のペア、$G_2$ 群の点の座標は複素数のペアなので、$G_2$ の点は2倍の記憶容量を必要とし、処理にコストがかかることに注意してください。
このことは、実装上の興味深いトレードオフにつながる。

## ペアリング（Pairings）

<details>
<summary>原文</summary>

So, what’s this pairing thing all about?

As far as BLS12-381 is concerned, a pairing simply takes a point $P\in G_1 \subset E(F_q)$ , and a point $Q\in G_2\subset E'(F_{q^2})$ and outputs a point from a group $G_T\subset F_{q^{12}}$ .
That is, for a paring $e, e:G_1\times G_2\rightarrow G_T$ .

Pairings are usually denoted $e(P,Q)$ and have very special properties.
I’m not going to go into all the details—we can pretty much treat them as a black box—but a great introduction is Vitalik’s article, and for all the glorious details let me pitch again Pairings for Beginners.

What we are interested in is that:

- $e(P,Q+R)=e(P,Q)\cdot e(P,R)$, and
- $e(P+S,R)=e(P,R)\cdot e(S,R)$

From this, we can deduce that all of the following identities hold:

- $e([a]P,[b]Q)=e(P,[b]Q)^a=e(P,Q)^{ab}=e(P,[a]Q)^b=e([b]P,[a]Q)$ [11].

This is just what we need when verifying digital signatures.

</details>

では、このペアリングというのはどういうものなのでしょうか？

BLS12-381でいうペアリングは、点 $P\in G_1 \subset E(F_q)$ , 点 $Q\in G_2\subset E'(F_{q^2})$ を取って群 $G_T\subset F_{q^{12}}$ から点を出力するだけで、ペアリングの意味は以下の通り。
つまり、 ペアリング $e$ の場合 $e:G_1\times G_2\rightarrow G_T$

ペアリングは通常 $e(P,Q)$ と表記され、非常に特殊な性質を持っています。
しかし、Vitalikの記事は素晴らしい入門書です。また、すべての素晴らしい詳細については、「初心者のためのペアリング」を参照ください。
詳細を説明するつもりはありません-ブラックボックスとして扱うことができます-しかし、[Vitalikの記事](https://vitalik.ca/general/2017/01/14/exploring_ecp.html)は素晴らしい入門書であり、すべての輝かしい詳細については、再び[Pairings for Beginners](https://www.craigcostello.com.au/s/PairingsForBeginners.pdf)を紹介させてください。

関心を持っているのは

- $e(P,Q+R)=e(P,Q)\cdot e(P,R)$, と
- $e(P+S,R)=e(P,R)\cdot e(S,R)$

これより、以下の恒等式がすべて成り立つことが推論される。

- $e([a]P,[b]Q)=e(P,[b]Q)^a=e(P,Q)^{ab}=e(P,[a]Q)^b=e([b]P,[a]Q)$ [11].

これはまさに、デジタル署名の検証を行う際に必要なものです。

## 埋め込み次数（Embedding degree）

<details>
<summary>原文</summary>

We’ve mentioned the embedding degree several times, and it is significant enough to appear in the name of the curve.

The embedding degree, $k$, is calculated as the smallest positive integer such that $r$ divides $(q^k−1)$ .
So, in the case of BLS12-381, $r$ is a factor of $(q^{12}−1)$ [12], but not of any lower power.

It turns out that this number gives the smallest field extension $F_q^k$ that satisfies the two equivalent conditions:

- $F_{q^k}$ contains more than one subgroup of order $r$ (used for constructing $G_2$ , see above);
- $F_{q^k}$ contains all the $r$th roots of unity (used for constructing $G_T$ , see below)

These are the conditions we need to satisfy for pairings to be possible.

The choice of an embedding degree is a balance between security and efficiency (as ever).
On the security front, the embedding degree is also known as the security multiplier: a higher embedding degree makes the discrete logarithm problem harder to solve in $G_T$ .
However, a high embedding degree means we have to do field operations in high-degree extensions, such as $F_{q^{12}}$ , which is clunky and inefficient.
(This is true even when using twists: the maximum available twist is degree six, so the best we can do is to reduce the field extension degree by six. And in any case pairing must be done in the large extension field.)

Embedding degrees of 12 or 24 seem to be a current sweet-spot for many applications.
Once again, the embedding degree of BLS12-381 is 12 - it’s in the name.

</details>

埋め込み次数については何度か触れていますが、曲線の名前に登場するほど重要なものです。

埋め込み次数 $k$ は、$r$ が $(q^k-1)$ を割り切る（約数）ような最小の正の整数として計算される。
つまり、BLS12-381の場合、$r$ は $(q^{12}-1)$ の因数であり[12]、それより小さい累乗ではありません。

この数は、2つの等価条件を満たす最小の拡大体 $F_q^k$ を与えることが判明した。

- $F_{q^k}$ は、位数 $r$ の部分群を複数含む（ $G_2$ を構成するために使用、前述参照）。
- $F_{q^k}$ はすべての $r$ 番目の統一根を含む（ $G_T$ の構築に使用、下記参照）。

これが、ペアリングが可能になるために満たすべき条件です。

埋め込み次数の選択は、安全性と効率性のバランスをとるものです（これまでと同様）。
安全性の面では、埋め込み次数は安全性乗数とも呼ばれ、埋め込み度が高いほど、 $G_T$ における離散対数問題が解きづらくなります。
しかし、埋め込み次数が高いと、$F_{q^{12}}$ のような高次の拡張で場の演算をしなければならず、不便で非効率的です。
(これは、ねじれを利用する場合でも同じです。利用できる最大ねじれは次数6なので、拡大体の次数を6下げるのが精一杯です。また、どのような場合でも、ペアリングは大きな拡大体で行わなければなりません)。

12や24の埋め込み次数は、現在のところ、多くのアプリケーションでスイートスポットになっているようだ。
繰り返しになるが、BLS12-381の埋め込み度は12であり、それは名前に現れている。

## セキュリティレベル（Security level）

<details>
<summary>原文</summary>

Security of cryptographic systems is measured in bits.
Informally, I take $n$-bit security to mean something like, “would need about $2^n$ operations to break it”.

For elliptic curve cryptography, security is all about making the discrete logarithm problem hard.
That is, given a point $g$ and a point $g^k$ (in multiplicative group notation), finding $k$ must be infeasible without prior knowledge.
That is, it must take at least $2^n$ operations to achieve this, for $n>100$ or so in today’s terms.

For pairing-friendly curves, the discrete logarithm problem must be hard in each of the three of the groups we are using.
Thus, to target $n$-bit security,

- The prime group order $r$ must be at least $2^n$ bits long as there are algorithms such as Pollard’s rho algorithm that have cost $O(\sqrt{r})$ .
- Our extended field $F_{q^k}$ must be large enough not to be vulnerable to methods like the number field sieve.

BLS12-381 was intended to offer around a 128 bit security level, based on these criteria, and this was supported by initial analyses.
See Table 1.1 in the Taxonomy, for example.

However, on closer examination it seems that “the finite extension field of size 3072 = 12 × 256 bits is not big enough” (quoting section 2 here), in view of the second criterion above.

According to a report by NCC Group, citing other sources, the actual security level is probably between 117 and 120 bits (see page 8).
They regard this as a perfectly adequate level of security: “The value of reaching ‘128-bit’ [being] mostly psychological”.
Sean Bowe has also commented on the security level in the light of the original design goals.

</details>

暗号システムの安全性はビットで測られる。
非公式には、$n$ ビットの安全性は「これを破るには約 $2^n$ 回の演算が必要」というような意味と捉えています。

楕円曲線暗号の安全性は、離散対数問題を難しくすることにある。
すなわち、ある点 $g$ と点 $g^k$（乗法群表記）が与えられたとき、$k$ を見つけることは予備知識なしでは実行不可能でなければならない。
つまり、$n>100$ 程度の今風に言えば、少なくとも $2^n$ の演算が必要でなければならない。

ペアリングに適した曲線では、離散対数問題は、我々が使っている3つのグループのそれぞれでハードでなければならない。
したがって、$n$ ビットの安全性を狙うには

- 素数群の位数 $r$ は、Pollard’s rho アルゴリズムのようにコスト $O(\sqrt{r})$ を持つアルゴリズムが存在する以上、少なくとも $2^n$ ビットでなければならない。
- 拡大体の $F_{q^k}$ は、数場のふるいのような手法に対して脆弱にならない程度の大きさである必要があります。

BLS12-381は、これらの基準に基づき、128ビット程度のセキュリティレベルを提供することを意図しており、これは初期の分析でも支持された。
例えば、[タクソノミ（Taxonomy）](https://eprint.iacr.org/2006/372.pdf)の表 1.1 を参照。

しかし、よくよく考えてみると、上記の2番目の基準から見て、「サイズ3072＝12×256ビットの有限拡張フィールドは十分な大きさではない」（[ここ](https://eprint.iacr.org/2019/077.pdf)で2項を引用）ようです。

[NCCグループの報告書](https://www.nccgroup.trust/globalassets/our-research/us/public-reports/2019/ncc_group_zcash2018_public_report_2019-01-30_v1.3.pdf)（8ページ参照）によると、実際のセキュリティレベルは117ビットから120ビットの間であろうとのことだ。
彼らは、これは完全に十分なセキュリティレベルであるとみなしている。「128ビットに到達することの価値は、（ほとんど）心理的なものである」と述べている。
Sean Bowe氏も、[当初の設計目標](https://github.com/zcash/zcash/issues/4065#issuecomment-572202467)に照らして、このセキュリティレベルについてコメントしている。

## 余因子（Cofactor）

<details>
<summary>原文</summary>

A subgroup’s cofactor is the ratio of the size of the whole group to the size of the subgroup.
Normal elliptic curve cryptography requires the cofactor to be very small, usually one, in order to avoid small subgroup attacks on discrete logarithms.
In pairing-based cryptography, however, this is not the case, and the cofactors of the $G_1$ and $G_2$ groups can be truly enormous.

It turns out that, with care, we can have large cofactors and still be secure.
Namely, when the cofactors of $G_1$, $G_2$ and $G_T$ contain no prime factors smaller than $r$ .
Section 3.2 of this paper discusses this in detail. This is not the case for BLS12-381, however, and the $G_1$ and $G_2$ cofactors both have several small factors.
Thus, we have to be mindful of small subgroup attacks in our implementations.

For reference, the cofactors of $G_1$ and $G_2$ are as follows.

|Group|Cofactor|Equation|Value (hexadecimal)|
|:--|:--|:--|:--|
|$G_1$|$h_1$|$(\mathbf{x}−1)^2/3$|0x396c8c005555e1568c00aaab0000aaab|
|$G_2$|$h_2$|$(\mathbf{x}^8−4\mathbf{x}^7+5\mathbf{x}^6−4\mathbf{x}^4+6\mathbf{x}^3−4\mathbf{x}^2−4\mathbf{x}+13)/9$|0x5d543a95414e7f1091d50792876a202cd91de4547085abaa68a205b2e5a7ddfa628f1cb4d9e82ef21537e293a6691ae1616ec6e786f0c70cf1c38e31c7238e5|

What’s the point of all this?
Well, multiplying by the cofactor turns out to be a straightforward way to map any arbitrary point on the elliptic curve into the respective subgroup $G_1$ or $G_2$ [13].
This is important when doing “hash to curve” operations and the like: we first make a point on the curve, and then we map it into the appropriate group by multiplying by the cofactor, so-called cofactor clearing.

</details>

部分群の余因子とは、群全体の大きさと部分群の大きさの比のことである。
通常の楕円曲線暗号では、離散対数に対するsmall subgroup attacks（小さな部分群攻撃）を避けるために、余因子は非常に小さく、[通常は1](https://crypto.stackexchange.com/questions/2881/why-would-anyone-use-an-elliptic-curve-with-a-cofactor-1)であることが要求されます。
しかし、ペアリング暗号ではそうはいかず、$G_1$ 群や $G_2$ 群の余因子は実に巨大になり得る。

その結果、注意すれば、大きな余因子を持っていても安全であることがわかった。
すなわち、$G_1$、$G_2$、$G_T$ の余因子には $r$ より小さい素因数が含まれない場合である。
[この論文](https://eprint.iacr.org/2015/247.pdf)の3.2節で詳しく説明している。しかし、BLS12-381の場合はそうではなく、$G_1$ と $G_2$ の余因子はともにいくつかの小さな因子を含んでいる。
そのため、実装ではsmall subgroup attacks（小さな部分群攻撃）に注意する必要がある。

参考までに、$G_1$ と $G_2$ の余因子は以下の通りである。

|Group|Cofactor|Equation|Value (hexadecimal)|
|:--|:--|:--|:--|
|$G_1$|$h_1$|$(\mathbf{x}−1)^2/3$|0x396c8c005555e1568c00aaab0000aaab|
|$G_2$|$h_2$|$(\mathbf{x}^8−4\mathbf{x}^7+5\mathbf{x}^6−4\mathbf{x}^4+6\mathbf{x}^3−4\mathbf{x}^2−4\mathbf{x}+13)/9$|0x5d543a95414e7f1091d50792876a202cd91de4547085abaa68a205b2e5a7ddfa628f1cb4d9e82ef21537e293a6691ae1616ec6e786f0c70cf1c38e31c7238e5|

何が言いたいのか？
楕円曲線上の任意の点を、それぞれの部分群 $G_1$ または $G_2$ に写すには、余因子をかけるのが簡単であることがわかった[13]。
これは、「hash to curve」操作などをするときに重要で、まず曲線上の点を作り、それに余因子を掛けて適当な群に写す、いわゆる余因子クリアリングが行われるのである。

## 1の累乗根（Roots of unity）

<details>
<summary>原文</summary>

Just a note on roots of unity, because they come up in two completely different and unrelated contexts, which can be confusing.

First, we said that to support zkSnark schemes with this curve, for some biggish $n$ we want to have a $2^n$th root of unity in the field $F_r$ (not $F_q$, note).
This is to facilitate efficient fast Fourier transforms for manipulating very large polynomials over the scalar field $F_r$ .
From the hexadecimal representation of $r−1$ , it’s clearly a multiple of $2^{32}$ , so there is a $2^32$th root of unity ($2^{32}$ of them, in fact).
Job done, :thumbsup:

Second, and completely unrelated, the effect of the pairing is to map the two points from $G_1$ and $G_2$ onto an $r$th root of unity in $F_{q^{12}}$ .
These $r$th roots of unity actually form a subgroup in $F_{q^{12}}$ of order $r$[14], which is the group we call $G_T$ .

Let’s briefly revisit our discussion of extending the base field for $E$ to $F_{q^{12}}$ , which we did in order to find another subgroup of order $r$ .
It also turns out $F_{q^{12}}$ treated as a multiplicative group is the smallest field extension that contains the $r$th roots of unity in the field, the 12 coming from the embedding degree once again.
This is why $G_T$ is defined over $F_{q^{12}}$ .

</details>

1の累乗根については、2つの全く異なる無関係な文脈で出てくるので、混乱する可能性があるので、一応メモしておきます。

まず、この曲線でzkSnark法をサポートするためには、ある大きな $n$ に対して、$F_r$ 体（ $F_q$ ではない、注意）に $2^n$ 番目の1の累乗根を持たせたいと述べました。
これは、スカラー体 $F_r$ 上の非常に大きな多項式を操作するための高速フーリエ変換を効率的に行うためである。
$r-1$ の16進数表現から、 $2^{32}$ の倍数であることは明らかなので、 $2^{32}$ 個目の単根が存在します（実際には $2^{32}$ 個の単根が存在します）。
これで仕事完了。 :thumbsup:

次に、全く関係ないが、ペアリングの効果は、$G_1$ と $G_2$ の2点を $F_{q^{12}}$ の $r$ 番目の1の累乗根に写すことである。
この $r$ 番目の1の累乗根は、実際には $F_{q^{12}}$ の位数 $r$[14] の部分群となり、これを $G_T$ と呼んでいる。

ここで、$E$ の基礎体を $F_{q^{12}} $に拡大したときの話を簡単に復習しておきましょう、これは、もう1つの位数 $r$ の部分群を見つけるために行ったことでした。
$F_{q^{12}}$ を乗法群として扱うと、 $r$ 番目の1の累乗根を含む最小の拡大体であり、12 は再び埋め込み次数に由来することがわかりました。
このため、$G_T$ は $F_{q^{12}}$ 上で定義されます。

# 曲線BLS12-381の使用（Using curve BLS12-381）

<details>
<summary>原文</summary>

This section is a miscellany of things relevant to using BLS12-381 in practice.

</details>

ここでは、BLS12-381を実際に使用する際に関連する事柄を雑多に紹介します。

## BLDデジタル署名（BLS digital signatures）

<details>
<summary>原文</summary>

Now it’s time to introduce the other BLS: Boneh, Lynn and Shacham.
(The L is the same L as in BLS12-381; the B and the S are different.)

BLS signatures were introduced back in 2001, a little before the BLS curve family was published in 2002.
But, pleasingly, they go hand-in-hand.
(BLS signatures can use other curves; BLS curves have uses other than signatures. But it’s nice when they come together.)

There’s a pretty concise but lucid description of the BLS signature scheme in the draft IETF standard.
See also the GitHub repo.

</details>

それでは、もう一つのBLSを紹介します。Boneh、Lynn、Shachamです。
(LはBLS12-381と同じLで、BとSは異なります)。

BLSの署名は[2001年に導入](https://www.iacr.org/archive/asiacrypt2001/22480516.pdf)され、[BLS曲線族](https://eprint.iacr.org/2002/088.pdf)が2002年に発表されるより少し前のことです。
しかし、嬉しいことに、両者は相性が良いのです。
(BLS署名は他の曲線を使うことができ、BLS曲線は署名以外の用途があります。しかし、両者が一緒になるのはいいことです)。

[IETF標準のドラフト](https://tools.ietf.org/html/draft-boneh-bls-signature-00)には、BLS署名スキームに関する簡潔で分かりやすい説明があります。
また、[GitHubのリポジトリ](https://github.com/kwantam/draft-irtf-cfrg-bls-signature)も参照してください。


## 秘密鍵と公開鍵（Private and public keys）

<details>
<summary>原文</summary>

The private/secret key (to be used for signing) is just a randomly chosen number between $1$ and $r−1$ inclusive.
We’ll call it $sk$ .

The corresponding public key (if we’re using $G_1$ for public keys) is $pk=[sk]g_1$ , where $g_1$ is the chosen generator of $G_1$ .
That is, $g_1$ multiplied by $sk$ , which is $g_1$ added to itself $sk$ times.

The discrete logarithm problem means that it is unfeasible to recover $sk$ given the public key $pk$ .

</details>

署名に使う秘密鍵は、$1$ から $r-1$ の間でランダムに選んだ数字である。
これを $sk$ と呼ぶことにする。

公開鍵は $pk=[sk]g_1$ であり、$g_1$ は $G_1$ の生成元である。
つまり、$g_1$ に $sk$ を掛けたもので、$g_1$ をそれ自身に $sk$ 回加算したものである。

離散対数問題とは、公開鍵 $pk$ があれば $sk$ を復元することは不可能であることを意味する。

## 署名（Signing）

<details>
<summary>原文</summary>

To sign a message $m$ we first need to map $m$ onto a point in group $G_2$ (if we’re using $G_2$ for signatures).
See hashing to the curve, below, for a discussion on ways to do this.
Anyway, let’s assume we can do this, and call the resulting $G_2$ point $H(m)$ .

We sign the message by calculating the signature $\sigma =[sk]H(m)$ .
That is, by multiplying the hash point by our secret key.

</details>

メッセージ $m$ に署名するためには、まず $m$ を群 $G_2$ の点に写す必要がある（署名に $G_2$ を使っている場合）。
これを行う方法については、後述の曲線へのハッシュ化を参照。
とにかく、これができたと仮定して、できた $G_2$ 点を $H(m)$ と呼ぶことにする。

署名は、$\sigma =[sk]H(m)$ という計算をすることでメッセージを署名する。
つまり、ハッシュ点に秘密鍵を乗算することで、署名する。

## 検証（Verification）

<details>
<summary>原文</summary>

Given a message $m$ , a signature $\sigma$ , and a public key $pk$ , we want to verify that it was signed with the $sk$ corresponding to $pk$ .

This is where pairing comes in. 
The signature is valid if, and only if, $e(g_1,\sigma )=e(pk,H(m))$ .

We can confirm this using the properties of pairings: 
$e(pk,H(m))=e([sk]g_1,H(m))=e(g_1,H(m))^{(sk)}=e(g_1,[sk]H(m))=e(g_1,\sigma )$ .

</details>

メッセージ $m$、署名 $\sigma$、公開鍵 $pk$ が与えられたとき、$pk$ に対応する $sk$ で署名されていることを検証したい。

ここでペアリングの出番です。
$e(g_1,\sigma )=e(pk,H(m))$ のとき、署名は有効である。

このことは、ペアリングの性質を利用して確認することができます。
$e(pk,H(m))=e([sk]g_1,H(m))=e(g_1,H(m))^{(sk)}=e(g_1,[sk]H(m))=e(g_1,\sigma )$ 

## 集約（Aggregation）

<details>
<summary>原文</summary>

A really neat property of BLS signatures is that they can be aggregated (see also the original paper), so that we need only two pairings to verify a single message signed by $n$ parties, or $n+1$ pairings to verify $n$ different messages signed by $n$ parties, rather than $2n$ pairings you might naively expect to need.
Pairings are expensive to compute, so this is very attractive.

It’s possible to aggregate signatures over different messages, or signatures over the same message.
In the case of Ethereum 2.0 we aggregate over the same message, so for brevity I’m just going to consider that.

To aggregate signatures we just have to add up the $G_2$ points they correspond to: $\sigma_{agg}=\sigma_1+\sigma_2+\ldots +\sigma_n$ .
We also aggregate the corresponding $G_1$ public key points $pk_{agg}=pk_1+pk_2+\sigma +pk_n$ .

Now the magic of pairings means that we can just verify that $e(g_1,\sigma_{agg})=e(pk_{agg},H(m))$ to verify all the signatures together with just two pairings.

</details>

BLS署名の非常に優れた特性は、署名の[集約](https://eprint.iacr.org/2018/483.pdf)が可能なことです（[原著論文](https://crypto.stanford.edu/~dabo/pubs/papers/aggreg.pdf)も参照）。そのため、$n$ 人の当事者によって署名された一つのメッセージを検証するのに必要なペアリングは2つだけで、$n$ 人が署名した $n$ 種類のメッセージを検証するには $n+1$ ペアリングと、単純に期待する $n$ ペアリングよりも少なくなります。
ペアリングは計算コストが高いので、これは非常に魅力的です。

異なるメッセージ上の署名を集約することも、同じメッセージ上の署名を集約することも可能である。
Ethereum 2.0の場合、同じメッセージに対して集約するので、簡潔にするために、それだけを考えることにする。

署名を集約するには、それらが対応する $G_2$ 点を加算すればよい。$\sigma_{agg}=\sigma_1+\sigma_2+\ldots +\sigma_n$ .
また、対応する $G_1$ 公開鍵 $pk_{agg}=pk_1+pk_2+\ldots +pk_n$ も合算する。

さて、ペアリングのマジックですが、$e(g_1,\sigma_{agg})=e(pk_{agg},H(m))$ を検証すれば、たった二つのペアリングで全ての署名をまとめて検証できることになります。

## 不正鍵攻撃（Rogue key attacks）

<details>
<summary>原文</summary>

As noted in section 1.1 here, when aggregating signatures over the same message, we need to take care of a possible “rogue public key attack”.

Say your public key is $pk_1$ , and I have a secret key, $sk_2$ .
But instead of publishing my true public key, I publish $pk'_2=[sk_2]g_1−pk_1$ (that is, my real public key plus the inverse of yours).
I can sign a message $H(m)$ with my secret key to make $\sigma =[sk_2]H(m)$ .
I then publish this claiming that it is an aggregate signature that both you and I have signed, and that the aggregate public key is $pk_{agg}=pk_1+pk'_2$ .

Now, when verifying, my claim checks out: it looks like you signed the message when you didn’t: 
$e(g_1,\sigma )=e(g_1,[sk_2]H(m))=e([sk_2]g_1,H(m))=e(pk_1+pk'_2,H(m))$ .

One relatively simple defence against this—the one we are using in Ethereum 2.0—is to force validators to register a “proof of possession” of the private key corresponding to their claimed public key.
You see, the attacker doesn’t have the $sk'_2$ corresponding to $pk'_2$ .
This can be done simply by getting the validator to sign its public key on registration: if the signature validates with that public key then all is well.

More complex schemes, not requiring proof of knowledge of the secret key (KOSK), are available

</details>

[ここ](https://eprint.iacr.org/2018/483.pdf)の1.1節で記されているように、同一メッセージに対する署名を集約する場合、「不正公開鍵攻撃（rogue public key attack）」の可能性に注意する必要がある。

あなたの公開鍵が $pk_1$ で、私が秘密鍵 $sk_2$ を持っているとする。
しかし、私は自分の本当の公開鍵を公開する代わりに、 $pk'_2=[sk_2]g_1-pk_1$ (つまり、私の本当の公開鍵とあなたの逆数を加えたもの)を公開します。
私の秘密鍵でメッセージ $H(m)$ に署名して、$\sigma =[sk_2]H(m)$ を作ることができる。
これを、あなたと私の両方が署名した集合署名であり、集合公開鍵は $pk_{agg}=pk_1+pk'_2$ であると主張して、公開する。

さて、検証してみると、私の主張は、署名していないのに署名したように見える、ということです。
$e(g_1,\sigma )=e(g_1,[sk_2]H(m))=e([sk_2]g_1,H(m))=e(pk_1+pk'_2,H(m))$ .

これに対する比較的簡単な防御方法（Ethereum 2.0で採用している方法）は、バリデータに、主張した公開鍵に対応する秘密鍵の「所有証明」の登録を強制することである。
つまり、攻撃者は $pk'_2$ に対応する $sk'_2$ を持っていないのである。
これは、バリデータの登録時に、その公開鍵に署名させることで簡単に実現できる。

秘密鍵の知識の証明（KOSK）を必要としない、より複雑な方式が[利用可能](https://crypto.stanford.edu/~dabo/pubs/papers/BLSmultisig.html)です


## G1、G2の入れ替え（Swapping G1 and G2）

<details>
<summary>原文</summary>

For the purposes of digital signatures, the $G_1$ and $G_2$ groups are interchangeable.
We can choose our public keys to be members of $G_1$ and our signatures to be members of $G_2$ , or vice versa.

The trade-offs are execution speed and storage size. 
$G_1$ has small points and is fast; $G_2$ has large points and is slow.
BLS12-381 was initially designed to implement Zcash, and for performance reasons they chose to use $G_1$ to represent signatures and $G_2$ to represent public keys.

With respect to Zcash, most other implementations are “reversed”.
In Ethereum 2.0 we use $G_1$ for public keys: for one thing, aggregation of public keys happens much more often than aggregation of signatures; for another, public keys of validators need to be stored in state, so keeping the representation small is important. Signatures, then, are $G_2$ points.

</details>

電子署名の方針では、$G_1$ 群と $G_2$ 群は交換可能である。
公開鍵を $G_1$ の元に、署名を $G_2$ の元に選ぶこともできるし、その逆も可能である。

トレードオフは実行速度とストレージサイズである。
$G_1 $はポイントが小さいので高速、$G_2$ はポイントが大きいので低速である。
BLS12-381は当初Zcashを実装するために設計され、パフォーマンス上の理由から、署名を表すのに $G_1$ を、公開鍵を表すのに $G_2$ を使うことを選択した。

Zcashに関して、他のほとんどの実装は「逆」である。
一つは、公開鍵の集約は署名の集約よりはるかに頻繁に起こること、もう一つは、バリデータの公開鍵はステートで保存する必要があるため、表現を小さく保つことが重要であることである。署名は $G_2$ 点である。

## 点圧縮（Point compression）

<details>
<summary>原文</summary>

(Note that sometimes, the twisting operation is referred to as point compression - that’s something completely different to what we’re discussing here.)

For storing and transmitting elliptic curve points, it is common to drop the $y$-coordinate.
This halves the amount of data. For BLS12-381, $G_1$ points are reduced from 96 bytes (2 × 381 bits-rounded-to-bytes) to 48 bytes, and $G_2$ points are reduced from 192 bytes to 96 bytes.

Any elliptic curve point can be regenerated from the $x$ coordinate by using the relevant curve equation, $E$ or $E'$ .
For any valid $x$ coordinate on the curve, $y$ is either zero or has two possible values that are the negative of each other: $y=\pm\sqrt{x^3+4}$ for $G_1$, and analogously for $G_2$ .

Since field elements are 381 bits, and 48 bytes is 384 bits, we have a few bits to spare for flags.
The most important is a flag to show which of the $y$ values the point corresponds to (positive or negative).
Another bit is used to signal whether this is the point at infinity (which has many possible representations).
A third is simply to indicate whether this is a compressed or uncompressed representation, though context should handle this in practice.

For both $G_1$ and $G_2$ , about half of $x$ values are not on the curve.
In this case, the point is conventionally decoded to the point at infinity.
But unless the infinity flag is set—in which case we would not have attempted to decode the point—this is an error condition.

The specific details of how the flag bits and $x$ values are encoded is here.

</details>

(なお、ねじり操作のことを点圧縮と呼ぶことがありますが、ここで説明するのとは全く別のものです）。

楕円曲線点の保存と送信では、$y$ 座標を落とすのが一般的である。
これにより、データ量は半分になる。BLS12-381の場合、$G_1$ 点は96バイト(2×381bit-rounded to-bytes) から48バイトに、$G_2$ 点は192バイトから96バイトに削減される。

楕円曲線上の任意の点は、関連する曲線方程式 $E$ または $E'$ を使用して $x$ 座標から再生成することができる。
$G_1$ では $y=\pm\sqrt{x^3+4}$ であり、$G_2$ では同様に $x$ 座標が 0 であるか、またはその負値であるかのどちらかである。

曲線上の任意の有効な $x$ 座標では、$y$ はゼロか、$y=\pm\sqrt{x^3+4}$　の2つの値のいずれかです、 $G_2$ についても同様です。

フィールド要素は 381 ビットで、48 バイトは 384 ビットであるため、フラグ用に数ビットを空けておく必要があります。
最も重要なのは、ポイントが対応する $y$ 値 (正または負) を示すフラグです。
別のビットを使用して、これが無限遠点であるかどうかを通知します (多くの可能な表現があります)。
3 つ目は、これが圧縮表現か非圧縮表現かを示すだけですが、実際にはコンテキストでこれを処理する必要があります。

$G_1$ と $G_2$ の両方で、$x$ 値の約半分が曲線上にありません。
この場合、点は通常、無限遠点にデコードされます。
しかし、無限フラグが設定されていない限り (この場合、ポイントをデコードしようとはしなかったでしょう)、これはエラー状態です。

フラグ ビットと $x$ 値がどのようにエンコードされるかについての具体的な詳細は、[こちら](https://github.com/zcash/librustzcash/blob/6e0364cd42a2b3d2b958a54771ef51a8db79dd29/pairing/src/bls12_381/README.md#serialization)にあります。


## 部分群元判定（Subgroup membership checks）

<details>
<summary>原文</summary>

When dealing with any point with an unknown origin, whether it comes to us compressed or uncompressed, it’s important that we check that it lies in the correct subgroup.
The point decompression described above only results in a point on the curve; we don’t know whether it lies in the appropriate $G_1$ or $G_2$ .

The main issue seems to be that both $E(F_q)$ and $E'(F_{q^2})$ contain small subgroups (you can see this by factoring the cofactors, eg. with this tool).
Inadvertantly working with points in these small subgroups could lead to vulnerabilities, as discussed in this paper.

Subgroup checks are easy in principle: simply multiply our point by $r$ .
For points in $G_1$ or $G_2$ this will result in the respective points at infinity; for points outside the groups, it won’t.

Unfortunately, this is slow in practice, especially for $G_2$ , since $r$ is so large.
As an alternative, there are new techniques making use of endomorphisms for performing faster subgroup checks.

</details>

起源が不明の点を扱う場合、それが圧縮されているかどうかに関わらず、正しい部分群にあるかどうかをチェックすることが重要である。
先ほどの点の展開では、曲線上の点が得られるだけで、それが適切な $G_1$ や $G_2$ にあるかはわからない。

主な問題は、$E(F_q)$ と $E'(F_{q^2})$ の両方が小さな部分群を含むことだと思われます（例えば[この](https://www.alpertron.com.ar/ECM.HTM)ツールを使って因数分解することでわかります）。
このような小さな部分群にある点を不用意に扱うと、この[論文](https://eprint.iacr.org/2015/247.pdf)で議論されているような脆弱性が発生する可能性があります。

部分群のチェックは原理的に簡単で、点に $r$ をかけるだけである。
$G_1$ や $G_2$ に含まれる点では、それぞれの点が無限遠になるようになるが、群から外れた点ではそうならない。

特に $G_2$ の場合は $r$ が大きいので、残念ながら実用上時間がかかる。
これに対して、内同型を利用して、より高速に部分群判定を行う[新しい手法](https://eprint.iacr.org/2019/814.pdf)がある。

## ジェネレーター（Generators）

<details>
<summary>原文</summary>

$G_1$ and $G_2$ are cyclic groups of prime order, so any point (except the identity/point at infinity) is a generator. 
Thus, picking generators is just a matter of convention.

Generator points for $G_1$ and $G_2$ are specified in decimal here and the same points in hexadecimal here.

These were chosen as follows:

> The generators of $G_1$ and $G_2$ are computed by finding the lexicographically smallest valid x-coordinate, and its lexicographically smallest y-coordinate and scaling it by the cofactor such that the result is not the point at infinity.

By my calculations, with $h_1$ and $h_2$ the respective group cofactors, this makes the $G_1$ generator $g_1=[h_1]p_1$ , with $p_1$ as follows,

<table><tr><td>
p1 = (0x04, 0x0a989badd40d6212b33cffc3f3763e9bc760f988c9926b26da9dd85e928483446346b8ed00e1de5d5ea93e354abe706c)
</td><tr></table>

and the $G_2$ generator $g_2=[h_2]p_2$ , with $p_2$ as follows,

<table><tr><td>
p2 = ([0x02, 0x00],[0x013a59858b6809fca4d9a3b6539246a70051a3c88899964a42bc9a69cf9acdd9dd387cfa9086b894185b9a46a402be73,0x02d27e0ec3356299a346a09ad7dc4ef68a483c3aed53f9139d2f929a3eecebf72082e5e58c6da24ee32e03040c406d4f])
</td><tr></table>

(I think “lexicographically smallest” means treating all numbers in the base field as non-negative, and just taking the smaller one, prioritising real parts over imaginary parts.)

</details>

$G_1$ と $G_2$ は素数位数の巡回群なので、どんな点（恒等点/無限遠の点を除く）も生成元になる。
したがって、生成元を選ぶのは単なる慣習の問題である。

$G_1$ と $G_2$ のジェネレータポイントを10進数で指定し、同じポイントを16進数で指定する。

これらは、以下のように選ばれました。

> $G_1$ と $G_2$ の生成元は、辞書的に最小の有効なx座標と、その辞書的に最小のy座標を求め、結果が無限遠の点にならないような係数でスケーリングして計算されます。

私の計算では、$h_1$ と $h_2$ をそれぞれの群余因子とすると、$G_1$ ジェネレーターは $g_1=[h_1]p_1$ となり、$p_1$ は次のようになります。

<table><tr><td>
p1 = (0x04, 0x0a989badd40d6212b33cffc3f3763e9bc760f988c9926b26da9dd85e928483446346b8ed00e1de5d5ea93e354abe706c)
</td><tr></table>

とし、$G_2$ ジェネレーターは $g_2=[h_2]p_2$ となり、$p_2$を次のように表す。

<table><tr><td>
p2 = ([0x02, 0x00],[0x013a59858b6809fca4d9a3b6539246a70051a3c88899964a42bc9a69cf9acdd9dd387cfa9086b894185b9a46a402be73,0x02d27e0ec3356299a346a09ad7dc4ef68a483c3aed53f9139d2f929a3eecebf72082e5e58c6da24ee32e03040c406d4f])
</td><tr></table>

(「辞書的に最小」というのは、基本場のすべての数を非負として扱い、虚数部より実数部を優先して、小さい方を取るということだと思います)。

##　最終的な指数計算（Final exponentiation）

<details>
<summary>原文</summary>

Calculation of a pairing has two parts: the Miller loop and the final exponentiation.
Both are quite expensive, but there’s a nice hack you can do to reduce the impact of the final exponentiation.

Normally, we calculate two full pairings in order to perform signature verification, to check whether $e(g_1,\sigma )=e(pk,H(m))$ .

If we denote as $e'(\cdot ,\cdot )$ the pairing without the final exponentiation, then for, some $x$ , we are checking whether $e'(g_1,\sigma )^x=e'(pk,H(m))^x$ .
 ( $x$ happens to be $(q^k−1)/r$ , which is huge.)

We know how to multiply in group $G_T$ , so we can reorganise this as a check whether $(e'(−g_1,\sigma )e'(pk,H(m)))^x=1$ . 
(We can negate any one of the points: the magic of pairings makes this equivalent to taking the inverse in $G_T$ .)

So, to verify a signature, we do the two Miller loops, one with a negated input value, multiply the results and then do a single final exponentiation.
If the result is unity in $G_T$ then our pairings match.
This ought to give a worthwhile speedup.

</details>

ペアリングの計算には、ミラーループ（Miller loop）と最後の指数計算の2つの部分があります。
どちらもかなり高価ですが、最後の指数計算の影響を減らすためにできる素敵なハックがあります。

通常、署名検証を行うためには、2つの完全ペアリングを計算し、$e(g_1, \sigma )=e(pk,H(m))$ かどうか確認します。

最後の指数を除いたペアリングを $e'(\cdot ,\cdot )$ とすると、ある $x$ に対して、 $e'(g_1,\sigma )^x=e'(pk,H(m))^x$ かどうか調べていることになる。
 ($x$ は $(q^k-1)/r$ であり、巨大である)。

群 $G_T$ での掛け算は知っているので、 $(e'(-g_1,\sigma )e'(pk,H(m)))^x=1$ かどうかのチェックとして整理し直せばいいのです。
(どれか1点を否定することができます。ペアリングのマジックで、$G_T$で逆数をとるのと同じです)。


そこで、署名を検証するために、2つのミラーループ（Miller loop）を行い、1つは入力値を否定（negated）し、結果を乗算して、最後に1回指数計算を行うのです。
もし結果が $G_T$ で単一なら、我々の組合せは一致する。
これによって、かなりのスピードアップが期待できる。

## 曲線へのハッシュ化（Hashing to the curve）

<details>
<summary>原文</summary>

To calculate a digital signature over a message, we first need to transform an arbitrary message (byte string) to a point on the $G_2$ curve (if we are using $G_2$ for signatures).
There are many ways to do this, with varying degrees of efficiency and security.

</details>

メッセージに対する電子署名を計算するためには、まず、任意のメッセージ（バイト列）を$G_2$ 曲線上の点に変換する必要がある（署名に $G_2$ を使用する場合）。
これを行うには多くの方法があり、効率や安全性の程度は様々である。

###　ハッシュ＆チェック（Hash and check）

<details>
<summary>原文</summary>

The initial implementation in Eth2 was “hash-and-check”.
This is very simple.

1.Hash your message to an integer modulo $q$
2.Check if there is a point on the curve with this $x$-coordinate (real part $x$ , imaginary part $0$ ).
If not, add one and repeat[15].
3.We have a point on the curve! Multiply by the $G_2$ cofactor to convert it into a point in $G_2$ .

About half the points that we try will result in a point on the curve, so this is not constant time—we don’t know how many iterations it will take to find one.
In one sense it doesn’t matter: all the information is public, so we’re not leaking anything. However, it does open up a griefing attack.
An attacker could pre-calculate messages that take a very long time to find a point (1 in one million messages will take 20 tries, for example) and slow us down considerably.

</details>

Eth2での最初の実装は、「ハッシュ＆チェック」でした。
これは非常にシンプルです。

1.メッセージを $q$ 単位の整数にハッシュ化する。
2.この $x$ 座標(実部 $x$ , 虚部 $0$ )を持つ点が曲線上に存在するかどうかを調べる。
ない場合は1つ追加して[15]を繰り返す。
3.曲線上に点がある! $G_2$ 余因子をかけて、$G_2$ の点に変換する。

試した点の約半分は曲線上の点になるので、これは定数時間ではなく、何回繰り返せば点が見つかるかわからない。
ある意味では、すべての情報は公開されているので、何も漏れていない。しかし、グリーフ攻撃は可能です。
攻撃者は、点を見つけるのに非常に長い時間を要するメッセージを事前に計算し（例えば、100万通のうち1通は20回かかる）、我々の速度を大幅に低下させることができるのです。

### SWUマップの簡略化（Simplified SWU map）

<details>
<summary>原文</summary>

We are now adopting a better approach which is described in this paper and used in the new (draft) IETF standard for hashing to curves.
As before (but a bit differently to ensure a uniform distribution of output points) we first create a field point by hashing the message mod $q$ .

Now we use a special map (the SWU map) that is guaranteed to translate that field point into a valid point on an elliptic curve.
For technical reasons, this is not the curve $E'(F_{q^2}))$ , but a curve isogenous to it (i.e. having the same number of points).
We then use another map (3-isogeny) to transfer this to a point on $E'(F_{q^2})$ .
Finally we use cofactor clearing to end up with a point in $G_2$ .

You can take a look at my implementation of this in Java , based on the reference code in Python.
The idea is for this approach to be generally adopted to enhance the interoperability of blockchains.

</details>

現在では、[この論文](https://eprint.iacr.org/2019/403.pdf)で説明され、曲線へのハッシュのための新しい[IETF標準](https://tools.ietf.org/html/draft-irtf-cfrg-hash-to-curve-05) (ドラフト)で使用されている、より良いアプローチを採用している。
以前と同様に(ただし、出力点の均一な分布を確保するために少し異なる)、まずハッシュ化したメッセージを mod $q$ することによって体となる点を作成する。

ここで、その体の点を楕円曲線上の有効点に変換することが保証されている特別な写像(SWU写像)を使用する。
技術的な理由から、これは曲線 $E'(F_{q^2}))$ ではなく、それと[同質](https://www.johndcook.com/blog/2019/04/21/what-is-an-isogeny/)の曲線（つまり、同じ数の点を持つ）である。
そこで、別の写像（3-isogeny）を使って、これを $E'(F_{q^2})$ 上の点に移します。
最後に余因子クリアーを使って、$G_2$ の点に行き着く。

[Pythonの参照コード](https://github.com/algorand/bls_sigs_ref/tree/master/python-impl)に基づいて、私が[Javaで実装](https://github.com/PegaSysEng/artemis/pull/898)したものをご覧ください。
このアプローチは、ブロックチェーンの相互運用性を高めるために一般的に採用されることを想定しています。

## 余因子クリア（Cofactor clearing）

<details>
<summary>原文</summary>

We discussed multiplying by the cofactor as a way to make an arbitrary point on $E$ or $E'$ into a point in $G_1$ or $G_2$ respectively.
This is useful when hashing to the curve, for example.

The $G_2$ co-factor is huge, so multiplying by it is slow.
However, there are faster ways to map curve points into $G_2$ using an endomorphism (a map of a group to itself).
This features in the new standard (see section 7).

The endomorphism we want to use was subject to a patent, but this has now expired everywhere.

As a workaround to the patent, instead of multiplying by the $G_2$ cofactor, the standard suggests multiplying by an effective cofactor (see section 8.9.2 for the value) which gives the same result as the endomorphism.
The effective cofactor is even larger than the $G_2$ cofactor, but the multiplication can be implemented using an addition chain as an optimisation.

The idea is that, now that the patent has expired, the endomorphism can be just dropped in as a replacement for the effective cofactor multiplication.

</details>

$E$ または $E'$ 上の任意の点をそれぞれ $G_1$ または $G_2$ の点にする方法として、係数の乗算について説明した。
これは曲線にハッシュをかけるときなどに有効である。

$G_2$ の余因子は巨大なので、それを掛けると遅くなる。
しかし、曲線点を $G_2$ に写すには、内同型（ある群のそれ自身への写像）を使ってより速く写す方法がある。
これが新標準の特徴である(7節参照)。

私たちが使いたい内型は特許の対象になっていましたが、現在はどこも期限切れになっています。

この特許の回避策として、$G_2$ 係数を乗じる代わりに、規格では内相と同じ結果を与える実効係数を乗じることを提案している(値は8.9.2節参照)。
有効係数は $G_2$ 係数よりさらに大きいが、最適化として加算鎖を用いて乗算を実装することができる。

特許が切れた今、エンドモルフィズムは有効な補因子乗算の代わりとして、そのまま落とし込めばいいという発想だ。

## 拡大タワー（Extension towers）

<details>
<summary>原文</summary>

Recall our discussion of field extensions?
In practice, rather than implementing a massive 12th-degree extension directly, it is more efficient to build it up from smaller extensions: a tower of extensions.

For BLS12-381, the $F_{q^{12}}$ field is implemented as a quadratic (degree two) extension, on top of a cubic (degree three) extension, on top of a quadratic extension of $F_q$ .

As long as the modular reduction polynomial (our reduction rule) is irreducible (can’t be factored) in the field being extended at each stage, then this all works out fine.

Specifically:

1.$F_{q^2}$ is constructed as $F_q(u)/(u^2−\beta )$ where $\beta =−1$ .
2.$F_{q^6}$ is constructed as $F_{q^2}(v)/(v^3−\xi )$ where $\xi =u+1$ .
3.$F_{q^{12}}$ is constructed as $F_{q^6}(w)/(w^2-\gamma )$ where $\gamma =v$

Interpreting these in terms of our previous explanation:

1.We write elements of the $F_{q^2}$ field as first degree polynomials in $u$ , with coefficients from $F_{q}$ , and apply the reduction rule $u^2+1=0$ , which is irreducible in $F_{q}$ .

- an element of $F_{q^2}$ looks like $a_0+a_1u$ where $a_j\in F_q$ .

2.We write elements of the $F_{q^6}$ field as second degree polynomials in $v$ , with coefficients from the $F_{q^2}$ field we just constructed, and apply the reduction rule $v^3−(u+1)=0$ , which is irreducible in $F_{q^2}$ .

- an element of $F_{q^6}$ looks like $b_0+b_1v+b_2v^2$ where $b_j\in F_{q^2}$ .

3.We write elements of the $F_{q^{12}}$ field as first degree polynomials in $w$, with coefficients from the $F_{q^6}$ field we just constructed, and apply the reduction rule $w^2−v=0$, which is irreducible in $F_{q^6}$ .

- an element of $F_{q^{12}}$ looks like $c_0+c_1w$ where $c_j\in F_{q^6}$ .

This towered extension can replace the direct extension as a basis for pairings, and when well-implemented can save a huge amount of arithmetic when multiplying $F_{q^{12}}$ points.
See Pairings for Beginners section 7.3 for a full discussion of the advantages.

</details>

拡大体の話を思い出してほしい。
実際には、12次の巨大な拡大を直接実装するよりも、小さな拡大を積み上げていく方が効率的である。[拡大体タワー](https://eprint.iacr.org/2009/556.pdf)

BLS12-381では、$F_{q^{12}}$ 体は、$F_q$ の2次拡大の上に3次拡大を重ねた、その2次拡大として実装されています。

剰余還元多項式 (私たちの還元規則) が各段階で拡大体で既約 (因数分解できない) である限り、これはすべてうまくいきます。

[仕様](https://github.com/zcash/librustzcash/blob/6e0364cd42a2b3d2b958a54771ef51a8db79dd29/pairing/src/bls12_381/README.md)：

1.$F_{q^2}$ は、$F_q(u)/(u^2−\beta )$ として構築されます。ここで、$\beta =−1$ です。
2.$F_{q^6}$ は、$F_{q^2}(v)/(v^3−\xi )$ として構築されます。ここで、$\xi =u+1$ です。
3.$F_{q^{12}}$ は、$F_{q^6}(w)/(w^2-\gamma )$ として構築されます。ここで、 $\gamma =v$ です。

これらを前の説明の観点から解釈すると、次のようになります。

1.$F_{q^2}$ 体の要素を $F_{q}$ からの係数を使用して $u$ の一次多項式として書き込み、縮小規則 $u^2+1=0$ を適用します。 これは $F_{q}$ で既約です。

- $F_{q^2}$ の要素は $a_0+a_1u$ のように見えますが、 $a_j\in F_q$ です。

2.$F_{q^6}$ 体の要素を、作成したばかりの $F_{q^2}$ 体の係数を使用して $v$ の 2 次多項式として書き込み、簡約規則 $v^3−(u+1)=0$ を適用します。これは $F_{q^2}$ で既約です。

- $F_{q^6}$ の要素は $b_0+b_1v+b_2v^2$ のように見えますが、ここで $b_j\in F_{q^2}$ .

3.$F_{q^{12}}$ 体の要素を、作成したばかりの $F_{q^6}$ 体の係数を使用して $w$ の一次多項式として書き込み、簡約規則 $w^2−v=0$ を適用します。これは $F_{q^6}$ で既約です。

- $F_{q^{12}}$ の要素は $c_0+c_1w$ のように見えますが、ここで $c_j\in F_{q^6}$ .

この塔状の拡張は、ペアリングの基礎として直接拡張を置き換えることができ、うまく実装されていれば、$F_{q^{12}}$ 点を乗算するときに膨大な量の計算を節約できます。
利点の完全な説明については、[Pairings for Beginners](https://www.craigcostello.com.au/s/PairingsForBeginners.pdf) セクション 7.3 を参照してください。

## 座標系（Coordinate systems）

<details>
<summary>原文</summary>

Finding the inverse of a field element (i.e. division) is an expensive operation, so implementations of elliptic curve arithmetic try to avoid it as much as possible.
It helps if we choose the right coordinate system for representing our points.

</details>

体要素の逆数を求める（つまり除算する）ことは高価な操作なので、楕円曲線演算の実装ではできるだけ避けようとする。
そのため、楕円曲線演算の実装では、できるだけそれを避けるようにしています。そのためには、点を表現するための正しい座標系を選択する必要があります。

### アフィン座標（Affine coordinates）

<details>
<summary>原文</summary>

Affine coordinates are the traditional representation of points with just an $(x,y)$ pair of coordinates, where $x$ and $y$ satisfy the curve equation.
This is what we normally use when storing and transmitting points.

However, it is not always the most efficient form to use when actually working with points, and there are two other schemes I’m aware of that are used for BLS12-381.

The basic idea is to represent the coordinate using notional fractions, reducing the number of actual division operations needed.
To do this, a third coordinate is introduced and we use $(X,Y,Z)$ for the internal representation of a point.
Like our familiar fractions, there are many representations of the same value, all corresponding to a single actual value ($\frac{1}{2}, \frac{3}{6}, \frac{197}{394}$ are all the same number).

The two systems I know of in use for BLS12-381 are Standard Projective coordinates and Jacobian coordinates.

</details>

アフィン座標は、$(x,y)$ という座標の組だけで点を表現する伝統的なもので、$x$ と $y$ が曲線方程式を満たすものである。
点を保存したり転送したりするときは、通常これを使う。

しかし、実際に点を扱う際には、必ずしも最も効率的な形式とは言えず、私が知る限り、BLS12-381では他に2つの方式が採用されています。

基本的な考え方は、座標を観念的な分数で表し、実際に必要な除算の回数を減らすことである。
そのために、第3の座標を導入し、点の内部表現に $(X,Y,Z)$ を使う。
身近な分数のように、同じ値でも多くの表現があり、すべて1つの実際の値に対応します（$\frac{1}{2}, \frac{3}{6}, \frac{197}{394}$ はすべて同じ数です）。

BLS12-381で使用されているシステムとして、私が知っているのは、標準射影座標とヤコビアン座標の2つです。

### 標準射影座標（Standard Projective coordinates）

<details>
<summary>原文</summary>

The Standard Projective coordinate point $(X,Y,Z)$ represents the Affine coordinate point $(X/Z,Y/Z)$ .
These are also called homogeneous projective coordinates because the curve equation takes on the homogeneous form $Y^2Z=X^3+4Z^3$ .
Points become straight lines through the origin in $(X,Y,Z)$ space, with the Affine point being the intersection of the line with the plane $Z=1$ .
Figure 2.10 in PfB gives a nice illustration.

Standard Projective coordinates are used by the Apache Milagro BLS12-381 library under the hood.

</details>

[標準射影座標](https://en.wikibooks.org/wiki/Cryptography/Prime_Curve/Standard_Projective_Coordinates)点 $(X,Y,Z)$ はアフィン座標点 $(X/Z,Y/Z)$ を表します。
曲線の方程式が $Y^2Z=X^3+4Z^3$ という同次の形をとるので、同次射影座標とも呼ばれる。
点は、$(X,Y,Z)$ 空間の原点を通る直線となり、その直線と平面 $Z=1$ との交点がアフィン点となる。
[PfB](https://www.craigcostello.com.au/s/PairingsForBeginners.pdf)の図2.10を見ると、よくわかります。

標準射影座標は、[Apache Milagro](https://milagro.apache.org/) BLS12-381ライブラリで使用されています。

### ヤコビアン座標（Jacobian coordinates）

<details>
<summary>原文</summary>

A different kind of projective coordinates are Jacobian coordinates.
In this scheme, the Jacobian point $(X,Y,Z)$ represents the Affine point $(X/Z^2,Y/Z^3)$ .
The curve equation becomes $Y^2=X^3+4Z^6$ .

The sample code for the constant-time hash-to-curve uses Jacobian coordinates under the hood.

Note that, in both schemes, the easiest way to import the Affine point $(x,y)$ is to map it to $(x,y,1)$ .

</details>

射影座標の別の種類として、[ヤコビアン座標](https://en.wikibooks.org/wiki/Cryptography/Prime_Curve/Jacobian_Coordinates)があります。
この方式では、ヤコビ点 $(X,Y,Z)$ はアフィン点 $(X/Z^2,Y/Z^3)$ を表している。
曲線方程式は $Y^2=X^3+4Z^6$ となる。

constant-time hash-to-curveの[サンプルコード](https://github.com/algorand/bls_sigs_ref/tree/master/python-impl)では、ヤコビアン座標が使われている。

どちらの方式でも、アフィン点 $(x,y)$ をインポートする最も簡単な方法は、$(x,y,1)$ にマップすることであることに注意してください。

## リソースとその他の資料（Resources and further reading）

<details>
<summary>原文</summary>

There are lots of references linked in the above, and I’m not going to repeat many here.
I’ll just pick out a few particularly useful or interesting things.

Useful reference material:

- The original BLS12-381 announcement
- Concise description of the parameters and serialisation
- Draft IETF standard
- A tl;dr version of the above, BLS for Busy People

In general, implementations of pairing libraries tend to be highly optimised and/or very generic (supporting many curves) which makes them quite hard to learn from.
The Noble BLS12-381 library in JavaScript/TypeScript by Paul Miller is definitely among the easier to follow.

Finally, a couple of fun and interesting reads:

- This brand new whitepaper on Curve9769 is not directly relevant to BLS12-381, but is a well-written and wonderful exploration of the joys and pains of designing and implementing an elliptic curve (not a pairing-friendly one in this case).
- Pairings are not dead, just resting. A great overview presention. Some BLS12-381 things.

That’s all, folks!

</details>

上記にはたくさんの参考文献がリンクされており、ここで多くを繰り返すつもりはない。
特に有用なもの、興味深いものをいくつかピックアップしておきます。

役立つ参考資料です。

- BLS12-381の[オリジナル](https://electriccoin.co/blog/new-snark-curve/)発表資料
- パラメータとシリアライゼーションの[簡潔な](https://github.com/zcash/librustzcash/blob/6e0364cd42a2b3d2b958a54771ef51a8db79dd29/pairing/src/bls12_381/README.md)説明
- [IETF標準](https://tools.ietf.org/id/draft-yonezawa-pairing-friendly-curves-02.html#rfc.section.4.2.2)のドラフト
- 上記のTl;dr版、[忙しい人のためのBLS](https://gist.github.com/hermanjunge/3308fbd3627033fc8d8ca0dd50809844)

一般に、ペアリングライブラリの実装は、高度に最適化されていたり、非常に汎用的（多くのカーブをサポート）であることが多く、そこから学ぶのは非常に困難です。
Paul MillerによるJavaScript/TypeScriptの[Noble BLS12-381](https://github.com/paulmillr/noble-bls12-381)ライブラリは、間違いなく最も簡単なものの1つです。

- この[Curve9769](https://github.com/pornin/curve9767/raw/master/doc/curve9767.pdf)に関する全く新しいホワイトペーパーは、BLS12-381とは直接関係ありませんが、楕円曲線（この場合はペアリングに適した曲線ではありません）の設計と実装の喜びと苦しみについて、よく書かれた素晴らしい探求書です。
- [ペアリングは死んでいない、ただ休んでいるだけだ。](https://ecc2017.cs.ru.nl/slides/ecc2017-aranha.pdf)素晴らしい概観のプレゼンです。いくつかのBLS12-381のこと。

以上、皆さん、お疲れ様でした。

<details>
<summary>原文</summary>

Many thanks to my esteemed colleagues Olivier Bégassat, Thomas Piellard, Herman Junge, Błażej Kolad, and Gus Gutoski for review, sanity check and comments. (I made some changes since you last saw it, guys - any screw-ups are all my own.)

My avatar Ben Edgington, ben@benjaminion.xyz
PegaSys, ConsenSys

</details>

私の尊敬する同僚であるOlivier Bégassat、Thomas Piellard、Herman Junge、Błażej Kolad、そしてGus Gutoskiにレビュー、健全性チェック、コメントをいただき、大変感謝しています。(前回見ていただいてから、いくつか変更を加えました。おかしなところがあれば、すべて私の責任です。)

私のアバター  [Ben Edgington](https://twitter.com/benjaminion_xyz), ben@benjaminion.xyz
[PegaSys](https://pegasys.tech/), [ConsenSys](https://consensys.net/)


---

<details>
<summary>原文</summary>
</details>

1.I studied mathematics many years ago, but diligently shirked anything to do with pure maths, including group theory.
I regret that now. Anyway, this will not be too technical, but I’m also not an expert, so might get some things wrong and will be a bit hand-wavy in general.
In case it’s not obvious, I am not a cryptographer.

2.Our rule is “an extension field modular reduction” (terminology from here).

3.Another point on $E(F_q)$ is (0x04,0x0a989badd40d6212b33cffc3f3763e9bc760f988c9926b26da9dd85e928483446346b8ed00e1de5d5ea93e354abe706c).
On average about half of $x$ values result in a point on the curve, and for most of those both $(x,y)$ and $(x,−y)$ are on the curve (for some, $y=0$).
You soon get used to these ridiculously big numbers. 

4.Sometimes $u$ is used rather than $i$ here, with $u^2+1=0$ . I’m using $i$.

5.Here’s a point on the $E'$ curve:
(1+i, 0x17faa6201231304f270b858dad9462089f2a5b83388e4b10773abc1eef6d193b9fce4e8ea2d9d28e3c3a315aa7de14ca + i * 0xcc12449be6ac4e7f367e7242250427c4fb4c39325d3164ad397c1837a90f0ea1a534757df374dd6569345eb41ed76e) 

6.Note that we lost the $'$ on $E$ here - this is the original curve $y^2=x^3+4$ , but now defined over $F_{q^k}$ .

7.The “trace zero subgroup” qualifies as an obscure incantation. Basically, the trace of a point is $\sum^{k−1}_{i=0}(x^{q^i},y^{q^i})$ , where k=12 in our case.
Understanding this involves stuff like the Frobenius endomorphism, and that rabbit hole goes deep.

8.:joy: Please forgive me.

9.…because we previously selected the trace zero subgroup.
Pairings for Beginners dives into the details on this.

10.Thank you to my reviewers for this insight

11.$[a]P$ is multiplication of the point $P$ by the scalar $a$ .
That is, adding $P$ , $a$ times.
Traditionally, the group operation in $G_1$ and $G_2$ is represented additively, and in $G_T$ multiplicatively.

12.Numbers in this world are truly enormous.
The number of times $r$ divides $(q^{12}−1)$ is 1299 digits long in decimal.
This number is actually used in the final exponentiation when computing pairings.

13.This is easy to see.
The subgroup $G$ has order $r$ , and its cofactor is $h$ , such that $hr=n$ , the order of the whole elliptic curve group.
Consider an arbitrary element $P$ of the elliptic curve group.
We have $O=[n]P=[r]([h]P)$ .
Thus, $[h]P\in G$ .
While not specific to BLS12-381, here is an excellent article about cofactor clearing.

14.This is a general property of roots of unity in multiplicative groups, not special to elliptic curves or pairings

15.This should have been “increment the message and go back to one” on failure, which is more secure.

1.私は昔、数学を勉強していましたが、群論を含む純粋数学に関係することは真面目に敬遠していました。
今になって後悔している。とにかく、これはあまり専門的ではありませんが、私も専門家ではないので、いくつかのことを間違っているかもしれませんし、一般的に少し手探り状態になるでしょう。
因みに、私は暗号解読者ではありません。

2.私たちのルールは「拡大体のモジュラーリダクション」（用語は[ここ](https://informatik.rub.de/emsec/)から）です。

3.$E(F_q)$上のもう一つの点は(0x04,0x0a989badd40d6212b33cffc3f3763e9bc760f988c9926b26da9ddd85e928483446346b8e00e1de5d5ea93e354abe706c)。
平均して約半数の $x$ の値が曲線上の点となり、そのほとんどで $(x,y)$ と $(x,-y)$ の両方が曲線上になります（中には $y=0$ となるものもあります）。
このようなバカでかい数字にはすぐに慣れる。

4.ここでは $i$ ではなく $u$ を使うことがあり、$u^2+1=0$ . 私は $i$ を使っています。

5.ここに $E'$ 曲線上の点がある。
(1+i, 0x17faa6201231304f270b858dad9462089f2a5b83388e4b10773abc1eef6d193b9fce4e8ea2d9d28e3c3a315aa7de14ca + i * 0xcc12449be6ac4e7f367e7242250427c4fb4c39325d3164ad397c1837a90f0ea1a534757df374dd6569345eb41ed76e) 

6.ここで、$E$上の $'$ がなくなったことに注意。これは、元の曲線 $y^2=x^3+4$ であるが、今は $F_{q^k}$ 上で定義されている。

7.トレースゼロ部分群(trace zero subgroup)は、不明瞭な呪文のようなものです。
基本的に、点のトレースは $\sum^{k-1}_{i=0}(x^{q^i},y^{q^i})$ で、この場合 k=12 です。
これを理解するにはフロベニウス内同型のようなものが必要で、このウサギの穴は奥が深い。

8.:joy: 許してください

9...以前、トレースゼロの部分群を選択したからです。
[Pairings for Beginners](https://www.craigcostello.com.au/s/PairingsForBeginners.pdf)では、この点について詳しく説明しています。

10.レビュアーの皆様、このような見識をお持ちいただきありがとうございます。

11.$[a]P$ は点 $P$ とスカラー $a$ の掛け算である。
つまり、$P$ の $a$ 回の加算である。
従来、$G_1$ と $G_2$ における群演算は加法的に、$G_T$ では乗法的に表現されていた。

13.これは簡単にわかる。
部分群 $G$ の位数は $r$ であり、その補数は $h$ で、楕円曲線群全体の位数 $hr=n$ となるようにする。
楕円曲線群の任意の要素$P$を考える。
このとき、$\mathcal{O}=[n]P=[r]([h]P)$ が成り立つ。
したがって、$[h]P$ は $G$ の中にある。
BLS12-381に限ったことではありませんが、余因子クリアーに関する[優れた記事](http://loup-vaillant.fr/tutorials/cofactor)を紹介します。

14.これは乗法群における単一根の一般的な性質であり、楕円曲線やペアリングに特別なものではありません。

15.これは、失敗時に「メッセージをインクリメントして1に戻る（increment the message and go back to one）」であるべきで、その方がより安全です。
