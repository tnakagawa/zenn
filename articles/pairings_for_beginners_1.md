---
title: "初心者のためのペアリング 1"
emoji: ""
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["crypto", "pairing"]
published: false
---

# はじめに（Introduction）

以下の論文を[DeepL](https://www.deepl.com/translator)や[Google翻訳](https://translate.google.co.jp/)で翻訳しました。（多少修正しています。）

Pairings for beginners
by
Craig Costello

https://static1.squarespace.com/static/5fdbb09f31d71c1227082339/t/5ff394720493bd28278889c6/1609798774687/PairingsForBeginners.pdf


# 記号・略語（Symbols and abbreviations）

|||
|:--|:--|
|$(f)$|関数 $f$ の因子<br>divisor of the function $f$|
|$[n]P$|$P$ の $n\in Z$ スカラー倍算 (指数) <br>scalar multiplication (exponentiation) of $P$ by $n\in Z$|
|$\#E$|$E$ 上の点の数<br>number of points on $E$|
|$A^n(K)$|$K$ 体のアフィン $n$ 空間<br>affine $n$-space over the field $K$|
|$\epsilon(D)$|因子 $D$ の有効部<br>effective part of the divisor $D$|
|$\eta T$|eta ($T$) ペアリング<br>eta ($T$) pairing|
|$\mathbb{F}_q$|$q$ 個の要素を持つ有限体<br>finite field with $q$ elements|
|$\mathbb{F}_{q^k}$|完全拡大体<br>full extension field|
|$\mathbb{G}_1$|基礎体部分群： $E[r]\cap \mathrm{ker}(\pi −[1])$ (タイプ 3 ペアリング)<br>base field subgroup: $E[r]\cap \mathrm{ker}(\pi −[1])$ (in Type 3 pairing)|
|$\mathbb{G}_2$|トレースゼロ部分群： $E[r]\cap \mathrm{ker}(\pi −[q])$ (タイプ 3 ペアリング)<br>trace-zero subgroup: $E[r]\cap \mathrm{ker}(\pi −[q])$ (in Type 3 pairing)|
|$\mathbb{G}_T$| $F^∗_{q^k}$ の位数 $r$ 部分群 (一般に $r$ 番目の1の冪根 $\mu_r$)<br>order $r$ subgroup of $F^∗_{q^k}$ (commonly the $r$-th roots of unity $\mu_r$)|
|$\mathcal{O}$|楕円曲線 $E$ 上の無限遠点<br>point at infinity on an elliptic curve $E$|
|$\overline{K}$|$K$ 体の代数的閉包<br>algebraic closure of the field $K$|
|$\mathbb{P}^n(K)$|$K$ 体の射影 $n$ 空間<br>projective $n$-space over the field $K$|
|$\phi$|超特異曲線の歪曲写像として、またGLV自己準同型として存在<br>occurs as the distortion map on supersingular curves and as the GLV endomorphism|
|$\Phi_i$|$i$ 次の円分多項式<br>$i$-th cyclotomic polynomial|
|$\pi$|$q$ 乗フロベニウス自己準同型: $(x,y)\rightarrow (x^q,y^q)$ <br> $q$-power Frobenius endomorphism: $(x,y)\rightarrow (x^q,y^q)$|
|$\Psi$|ねじれ同型写像<br>the (un)twisting isomorphism|
|$\psi$|$\mathbb{G}_2$ から $\mathbb{G}_1$ への同型写像とGLS同型写像の両方として存在<br>occurs as both the isomorphism from $\mathbb{G}_2$ to $\mathbb{G}_1$ and as the GLS isomorphism|
|$\psi_\ell(x)$|$E$ の $\ell$ 次除算多項式（奇数 $\ell$ ）<br>$\ell$-th division polynomial on $E$ (for odd $\ell$)|
|$\rho$|ペアリングに適した曲線の基礎体サイズと部分群サイズとの比率<br>ratio between base field size and subgroup size for a pairingfriendly curve|
|$a_T$|ate ペアリング<br>ate pairing|
|$C$|任意の曲線<br>an arbitrary curve|
|$C_g$|$g$ を元とする（虚二次）超楕円曲線<br>(imaginary quadratic) hyperelliptic curve of genus $g$|
|$D$|$E$ 上の因子として、また $E$ のCM判別式として存在<br>occurs as both a divisor on $E$ and the CM discriminant of $E$|
|$d$|ねじれ度<br>degree of twist|
|$D_P$|因子 $(P)−(O)$<br>divisor $(P)−(O)$|
|$D_Q$|因子 $(Q)−(O)$<br>divisor $(Q)−(O)$|
|$E$|楕円曲線<br>an elliptic curve|
|$e$|一般的なペアリング<br>a general pairing|
|$E'$|ねじれ曲線（$\mathbb{F}_{q^{k/d}}$ 上で定義）<br>twisted curve (defined over $\mathbb{F}_{q^{k/d}}$)|
|$E(K)$|$E$ 上の $K$ 有理点集合<br>set of $K$-rational points on $E$|
|$e(P,Q)$|$P$ と $Q$ のペアリング (ペアとなる値)<br> pairing of $P$ and $Q$ (the paired value)|
|$E/K$|$K$ 上で定義された楕円曲線<br>elliptic curve defined over $K$|
|$E[r]$|（全） $r$ ねじれ<br>the (entire) $r$-torsion|
|$f_{m,P}$|因子 $(f_m,P)=m(P)−([m]P)−(m−1)(\mathcal{O})$ の関数<br>function with divisor $(f_m,P)=m(P)−([m]P)−(m−1)(\mathcal{O})$|
|$g$|曲線の元<br>genus of a curve|
|$K$|任意の体<br>arbitrary field|
|$k$|$E$ の埋め込み次数 ($q$ と $r$ に関して)<br>embedding degree of $E$ (with respect to $q$ and $r$)|
|$nP$|同伴因子上の点 $P$ の多重度<br>multiplicity of point $P$ in associated divisor|
|$P$|$\mathbb{G}_1$ の生成元<br>generator of $\mathbb{G}_1$|
|$Q$|$\mathbb{G}_2$ の生成元<br>generator of $\mathbb{G}_2$|
|$r$|$E(\mathbb{F}_q)$ の大きい素数部分群の位数<br>order of the large prime subgroup in $E(\mathbb{F}_q)$|
|$T$|ateペアリングループパラメータ$(T=t−1)$ <br>ate pairing loop parameter $(T=t−1)$|
|$t$|フロベニウスのトレース<br>trace of Frobenius|
|$T_r(P,Q)$|位数 $r$ の被約環（reduced）Tateペアリング<br>order $r$ reduced Tate pairing|
|$t_r(P, Q)$|位数 $r$ のTateペアリング<br>order $r$ Tate pairing|
|$w_r(P, Q)$|位数 $r$ のWeilペアリング<br>order $r$ Weil pairing|
|$a\mathrm{Tr}$|anti-trace写像<br>anti-trace map|
|BKLS−GHS|Barreto-Kim-Lynn-Scott/Galbraith-Harrison-Soldera アルゴリズム<br>Barreto-Kim-Lynn-Scott/Galbraith-Harrison-Soldera algorithm|
|BLS|Barreto-Lynn-Scott 族<br>Barreto-Lynn-Scott families|
|BN|$k=12$ のBarreto-Naehrig 族<br>Barreto-Naehrig family with $k=12$|
|CM|複素数乗算<br>complex multiplication|
|$\mathrm{Deg}(D)$|degree of the divisor $D$|
|$\mathrm{Div}^0(E)$|因子 $D$ の次数<br>group of degree zero divisors on $E$|
|$\mathrm{Div}_{\mathbb{F}_q}(E)$|$E/F_q$ 上の因子群<br>group of divisors on $E/F_q$|
|DLP|離散対数問題<br>discrete logarithm problem|
|ECC|楕円曲線暗号<br>elliptic curve cryptography|
|ECDLP|楕円曲線離散対数問題<br>elliptic curve discrete logarithm problem|
|$\mathrm{End}(E)$|$E$ の自己準同型環<br>endomorphism ring of $E$|
|$\mathrm{Gal}(L/K)$|$K$ 上の $L$ のガロア群<br>Galois group of $L$ over $K$|
|GLS|Galbraith-Lin-Scott メソッド<br>Galbraith-Lin-Scott method|
|GLV|Gallant-Lambert-Vanstone メソッド<br>Gallant-Lambert-Vanstone method|
|HECC|超楕円曲線暗号<br>hyperelliptic curve cryptography|
|KSS|Kachisa-Schaefer-Scott 族<br>Kachisa-Schaefer-Scott families|
|MNT|Miyaji-Nakabayashi-Takano（施工／基準値）<br>Miyaji-Nakabayashi-Takano (construction/criteria)|
|NIST|国立標準技術研究所<br>National Institute of Standards and Technology|
|NSS|非特異曲線<br>not supersingular curves|
|$\mathrm{ord}_P(f)$|$E$ 上の $P$ における $f$ の多重度<br>the multiplicity of $f$ at $P$ on $E$|
|PBC|ペアリング暗号<br>pairing-based cryptography|
|$\mathrm{Pic}^0(E)$|$E$のピカール群<br>Picard group of $E$|
|$\mathrm{Prin}(E)$|$E$ 上の主因子の群<br>group of principal divisors on $E$|
|$\mathrm{QR}(q)$|$q$ を方とした平方剰余の集合<br>set of quadratic residues modulo $q$|
|$\mathrm{supp}(D)$|因子 $D$ のサポート<br>support of the divisor $D$|
|$\mathrm{Tr}$|トレース写像<br>trace map|

# 第1章（Chapter 1）
# 序論（Introduction）

<details><summary>原文</summary>

Aficionados of cryptographic pairing computation are often asked by interested newcomers to point towards literature that is a good starting point.
My answer usually differs depending on the mathematical background volunteered from the “pairing beginner”, but almost always involves accordingly picking a subset of the following excellent references.

- Galbraith’s chapter [Gal05] is a stand-out survey of the field (up until 2005).
It provides several theorems and proofs fundamental to pairingbased cryptography and gives some useful toy examples that illustrate key concepts.
- Lynn’s thesis [Lyn07] is also a great survey of the entire arena of pairing computation (up until 2007), and gives all the details surrounding the pioneering papers he co-authored [BKLS02,BLS02,BLS03,BLS04], which are themselves good starting points.
- The first chapter of Naehrig’s thesis [Nae09, Ch. 1] conveniently presents the necessary algebro-geometric results required to be able to read most of the literature concerning pairing computation.
- Scott’s webpage [Sco04] gives a short and very friendly introduction to the basics of the groups involved in pairing computations by means of an illustrative toy example.
- In his new chapter entitled Algorithmic Aspects of Elliptic Curves,  ilverman’s second edition [Sil09, Ch. XI.7] includes a concise introduction to pairing-based cryptography that also points to foundational results found elsewhere in his book.

In addition, digging up talks from some of the big players in the field is usually (but not always!) a good way to avoid getting bogged down by minor technical details that slow one’s progress in grasping the main ideas.
In particular, we refer to the nice talks by Scott [Sco07a, Sco07b] and Vercauteren [Ver06b, Ver06a].

</details>

暗号のペアリング計算の愛好家が、興味を持った初心者に、良い出発点となるような文献を教えて欲しいとよく言われる。
答えは、「ペアリング初心者」が志願する数学的背景によって異なるが、ほとんどの場合、以下の優れた文献のサブセットを適宜選んでいる。

- Galbraithの章[Gal05]は、この分野の傑出したサーベイである（2005年まで）。
この章では、ペアリングベース暗号の基礎となるいくつかの定理と証明を提供し、主要な概念を説明するいくつかの有用なおもちゃの例を与えています。
- Lynnの論文[Lyn07]は、ペアリング計算の分野全体（2007年まで）の素晴らしいサーベイであり、彼が共著した先駆的論文[BKLS02,BLS02,BLS03,BLS04]に関するすべての詳細を提供しており、これらはそれ自体良い出発点となっています。
- Naehrigの論文[Nae09, Ch. 1]の第1章は、ペアリング計算に関するほとんどの文献を読むのに必要な代数幾何学的結果を都合よく提示している。
- Scott の Web ページ [Sco04] では、ペアリング計算に関係するグループの基本を、例示的なおもちゃの例を使って、短く、非常に親切に紹介しています。
- ilvermanの第2版[Sil09, Ch.XI.7] では、「Algorithmic Aspect of Elliptic Curves」という新しい章で、ペアリングに基づく暗号を簡潔に紹介していますが、これは彼の本の他の部分で見られる基礎的な結果も指摘しています。

さらに、この分野の大物研究者の講演を聴くことは、通常（常にではありませんが！）、主要なアイデアを理解する上で、技術的な細部に埋もれてしまうことを避けるための良い方法です。
特に、Scott [Sco07a, Sco07b]とVercauteren [Ver06b, Ver06a]による素晴らしい講演を参考にすることができます。

<details><summary>原文</summary>

In any case, correctly prescribing the best reading route for a beginner naturally requires individual diagnosis that depends on their prior knowledge and technical preparation.
A student who is interested in learning pairings, but who has never seen or played with an elliptic curve, may quickly become overwhelmed if directed to dive straight into the chapters of Silverman’s book or Naehrig’s thesis.
This is not due to lack of clarity, or to lack of illuminating examples (both chapters are ample in both), but perhaps more because of the vast amount of technical jargon that is necessary for one to write a complete and self-contained description of cryptographic pairings.
On the other hand, an informal, exampledriven approach to learning the broad field of pairing computation may ease the beginner’s digestion in the initial stages.
For instance, a novice would be likely to find it more beneficial to first see the simple toy example of the quadratic twisting isomorphism in action on Scott’s webpage [Sco04], before heading to Silverman’s book [Sil09, Ch. X.5.4] to see all possible twisting isomorphisms formally defined, and then later returning to his earlier chapters (specifically Ch. II.2) to read about maps between curves in full generality.

</details>

いずれにせよ、初心者に最適な読書法を正しく処方するには、当然ながら、その人の予備知識や技術的準備に応じた個別の診断が必要である。
ペアリングに興味はあるが、楕円曲線を見たことも遊んだこともない学生が、いきなりSilvermanの本やNaehrigの論文の章に飛び込むように指示されたら、すぐに圧倒されてしまうかもしれません。
これは、わかりにくかったり、示唆に富む例がなかったりするためではなく（どちらの章も充実している）、おそらく、暗号の組合せについて完全かつ自己完結した記述をするために必要な膨大な技術用語のためであろう。
一方、ペアリング計算の広い分野を学ぶための非公式な、試験的なアプローチは、初心者の初期段階での消化を容易にするかもしれない。
例えば、初心者はまずScottのWebページ[Sco04]で2次ねじれ同型の簡単なおもちゃの例を見てから、Silvermanの本[Sil09, Ch.X.5.4] に向かい、すべての可能なねじれ同型の公式定義を見、後で彼の初期の章（特にCh. II.2 ）に戻って曲線間の写像について完全に一般的に読めば、より有益だと分かるはずである。

<details><summary>原文</summary>

In this light we discuss the major aim of this text.
We intend to let illustrative examples drive the discussion and present the key concepts of pairing computation with as little machinery as possible.
For those that are fresh to pairing-based cryptography, it is our hope that this chapter might be particularly useful as a first read and prelude to more complete or advanced expositions (e.g. the related chapters in [Gal12]).

On the other hand, we also hope our beginner-friendly intentions do not leave any sophisticated readers dissatisfied by a lack of formality or generality, so in cases where our discussion does sacrifice completeness, we will at least endeavour to point to where a more thorough exposition can be found.

</details>

このような観点から、この文章の主な目的について説明する。
本章では、例題を中心に議論を進め、ペアリング計算の重要な概念をできるだけ機械的に説明しないことを意図している。
ペアリング暗号の初心者にとって、この章が最初に読む本として、またより完全で高度な説明（例えば[Gal12]の関連章）の前段階として、特に有用であることが期待されます。

一方、初心者にやさしいことを意図しているので、洗練された読者には形式や一般性の不足で不満に思われることがないように、我々の議論が完全性を犠牲にするような場合には、少なくとも、より完全な説明が得られる場所を示すように努力するつもりだ。

<details><summary>原文</summary>

One advantage of writing a survey on pairing computation in 2012 is that, after more than a decade of intense and fast-paced research by mathematicians and cryptographers around the globe, the field is now racing towards full maturity. 
Therefore, an understanding of this text will equip the reader with most of what they need to know in order to tackle any of the vast literature in this remarkable field, at least for a while yet.
Anyone who understands our examples will also comfortably absorb the basic language of algebraic geometry in the context of curve-based cryptography.
Since we are aiming the discussion at active readers, we have matched every example with a corresponding snippet of (hyperlinked) Magma [BCP97] code1 , where we take inspiration from the helpful Magma pairing tutorial by Dominguez Perez et al. [DKS09].
In the later chapters we build towards a full working pairing code that encompasses most of the high-level optimisations; this culminates to finish the chapter in Example 7.5.1.

</details>

2012 年にペアリング計算に関する調査を作成した利点の 1 つは、世界中の数学者と暗号学者による 10 年以上にわたる熱心でペースの速い研究の後、この分野が現在、完全な成熟に向かって競争しているということです。
したがって、このテキストを理解することで、読者は、少なくともしばらくの間、この注目すべき分野の膨大な文献に取り組むために知っておくべきことのほとんどを身につけることができます。
私たちの例を理解している人なら誰でも、曲線ベースの暗号化のコンテキストで代数幾何学の基本的な言語を快適に吸収できます。
私たちはアクティブな読者を対象に議論を行っているため、すべての例を (ハイパーリンクされた) Magma [BCP97] コード<sup>1</sup> の対応するスニペットと一致させました。このコードでは、Dominguez Perez らによる便利な Magma ペアリング チュートリアルからインスピレーションを得ています。 [DKS09]。
後の章では、高レベルの最適化のほとんどを含む、完全に機能するペアリング コードに向けて構築します。これで、例 7.5.1 の章が終了します。

<details><summary>原文</summary>

The text is organised as follows.
We start in Chapter 2 by giving an overview of elliptic curve cryptography (ECC).
Indeed, elliptic curves are the main object on which cryptographic pairings take place, so this first chapter forms a basis for the entire text.
In Chapter 3 we introduce the important concept of divisors, as well as other essential theory from algebraic geometry that is needed to properly understand cryptographic pairings.
In Chapter 4 we detail the specific elliptic curve groups that are employed in a cryptographic pairing, before presenting Miller’s algorithm to compute the Weil and Tate pairings in Chapter 5.
In Chapter 6 we introduce the notion of pairing-friendly curves and give a brief
survey of the most successful methods of constructing them.
In Chapter 7 we bring the reader up to speed with the landmark achievements and improvements that have boosted pairing computation to the point it is today.

</details>

本文の構成は以下の通りである。
まず第2章では、楕円曲線暗号（ECC）の概要を説明する。
実際、楕円曲線は暗号のペアリングが行われる主要な対象であり、この最初の章はテキスト全体の基礎を形成している。
第3章では、暗号のペアリングを正しく理解するために重要な因子（divisors）の概念をはじめ、代数幾何学の重要な理論を紹介する。
第4章では、暗号のペアリングに採用される特定の楕円曲線群について詳述し、第5章ではWeilペアリングとTateペアリングを計算するMillerのアルゴリズムについて紹介します。
第6章では、ペアリングに適した曲線の概念を紹介し、ペアリングを構成する最も成功した手法の簡単なサーベイを行う。
第6章では、ペアリングに適した曲線の概念を紹介し、最も成功した曲線の構成方法について簡単なサーベイを行う。
第7章では、ペアリング計算を今日まで発展させた画期的な業績と改良について紹介する。

---
<sup>1</sup> If one does not have access to Magma, the scripts we provide can be run at the online Magma calculator: http://magma.maths.usyd.edu.au/calc/
<sup>1</sup> Magmaにアクセスできない場合は、私たちが提供するスクリプトをオンラインのMagma計算機で実行することができます: http://magma.maths.usyd.edu.au/calc/


# まとめ（Conclusion）

「記号・略語」は知らないものが多い・・・なんども参照する必要がありそうです。

とりあえず、自分のペースで翻訳していきます。
間違いなどがありましたら、お知らせください。
（Deeplは勝手にまとめたり、加筆したりする・・・・）


