---
title: "初心者のためのペアリング 3"
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

# 第3章（Chapter 3）
# 因子（Divisors）

<details><summary>原文</summary>

In this chapter we introduce some basic language and definitions from algebraic geometry that are fundamental to the understanding of cryptographic pairing computations.
We continue with our example-driven approach and illustrate each concept and definition as it arises.
We will essentially just be expanding on the more concise section found in Galbraith’s chapter [Gal05, §IX.2].
However, we only focus on what we need to describe elliptic curve pairings, so we refer any reader seeking a more general and thorough treatment to Galbraith’s new book [Gal12, Ch.7-9].
Since our exposition targets the newcomer, we begin by assuring such a reader that their persistence through the definitions and examples will be amply rewarded.
On becoming comfortable with the language of divisors, one can immediately start to appreciate how pieces of the “pairings puzzle” fit together very naturally, and might even enjoy feeling intuition behind important theorems that would otherwise appear foreign.

The following statements apply to all curves $C$ over any perfect field $K$ and its closure $\overline{K}$ (see [Sil09, p. 17, p. 1] for the respective definitions).
However, for now we place the discussion in our context and specialise to the case where $C$ is an elliptic curve $E$ over a finite field $K=\mathbb{F}_q$ .
Later in this chapter we will expand to more general examples and statements in time to present the important theorems in their full generality.
A divisor $D$ on $E$ is a convenient way to denote a multi-set of points on $E$, written as the formal sum

$$D=\sum_{P\in E(\overline{\mathbb{F}}_q)}n_P(P),$$

where all but finitely many $n_P\in\mathbb{Z}$ are zero.
The standard parentheses $(\cdot )$ around the P’s and the absence of square parentheses $[\cdot ]$ around the $n_P$’s is what differentiates the formal sum in a divisor from an actual sum of points (i.e. using the group law) on $E$ .
The set of all divisors on $E$ is denoted by $\mathrm{Div}_{\overline{\mathbb{F}}_q}(E)$ and forms a group, where addition of divisors is natural, and the identity is the divisor with all $n_P=0$ , the zero divisor $0\in\mathrm{Div}_{\overline{\mathbb{F}}_q}(E)$ .
The degree of a divisor $D$ is $\mathrm{Deg}(D)=\sum_{P\in E(\overline{\mathbb{F}}_q)}n_P$ , and the support of $D$ , denoted $\mathrm{supp}(D)$ , is the set $\mathrm{supp}(D)=\{P\in E(\mathbb{F}_q):n_P\ne 0\}$ .

</details>

この章では、暗号ペアリング計算を理解するための基礎となる、代数幾何学からのいくつかの基本的な言語と定義を紹介します。
例題主導のアプローチを続け、それぞれの概念や定義が生じるたびに説明します。
基本的に、Galbraith の章 [Gal05、§IX.2] にあるより簡潔なセクションを拡張するだけです。
ただし、楕円曲線のペアリングを説明するために必要なことだけに焦点を当てているため、より一般的で完全な処理を求める読者には、Galbraith の新しい本 [Gal12、Ch.7-9] を参照してください。
この説明では、初めての人を対象にしているので、そのような読者には、定義と例題を粘り強く読んでもらうことで、十分に報われることを保証することから始めます。
因子の言語に慣れると、「ペアリング パズル」のピースがどのように非常に自然に組み合わされるかをすぐに理解し始めることができ、そうでなければ見慣れない重要な定理の背後にある直感を楽しむことさえできるかもしれません。

以下の記述は、任意の完全体 $K$ 上のすべての曲線 $C$ とその閉域 $\overline{K}$ に適用されます（それぞれの定義については[Sil09, p. 17, p. 1]を参照してください）。
しかし、ここでは、この議論を我々の文脈に置き、$C$ が有限体 $K=\mathbb{F}_q$ 上の楕円曲線 $E$ である場合に特化することにします。
この章の後半では、重要な定理を完全な一般性で提示するために、より一般的な例や記述を展開します。
$E$ 上の因子 $D$ は、$E$ 上の点の多重集合を表す便利な方法で、形式和として書かれます。

$$D=\sum_{P\in E(\overline{\mathbb{F}}_q)}n_P(P),$$
ここで、 $n_P\in\mathbb{Z}$ は有限個を除いてすべて零です。
$P$ の周りを標準の括弧 $(\cdot )$ で囲み、$n_P$ の周りを角括弧 $[\cdot ]$ で囲まないのは、因子での形式和と $E$ 上の実際の点の和（つまり群律を用いたもの）を区別するためのものです。
$E$ 上のすべての因子の集合を $\mathrm{Div}_{\overline{\mathbb{F}}_q}(E)$ と表し、因子の加算が自然で、すべての $n_P=0$ の約数を恒等式とする、零因子 $0\in\mathrm{Div}_{\overline{\mathbb{F}}_q}(E)$ を構成します。
因子 $D$ の次数は $\mathrm{Deg}(D)=\sum_{P\in E(\overline{\mathbb{F}}_q)}n_P$ であり、$D$ の台（_support_）は $\mathrm{supp}(D)$ で表され、集合 $\mathrm{supp}(D)=\{P\in E(\overline{\mathbb{F}}_q):n_P\ne 0\}$ となります。

<details><summary>原文</summary>

Example 3.0.1 (Magma script).
Let $P,Q,R,S\in E(\overline{\mathbb{F}}q)$ .
Let $D_1=2(P)−3(Q)$ , and $D_2=3(Q)+(R)−(S)$ , so that $\mathrm{Deg}(D_1)=2−3=−1$ , and $\mathrm{Deg}(D_2)=3+1−1=3$ . 
The sum $D_1+D_2=2(P)+(R)−(S)$ , and naturally $\mathrm{Deg}(D_1+D_2)=\mathrm{Deg}(D_1)+\mathrm{Deg}(D_2)=2$ .
The supports are $\mathrm{supp}(D_1)=\{P,Q\}$ , $\mathrm{supp}(D_2)=\{Q,R,S\}$ , and $\mathrm{supp}(D_1+D_2)=\{P,R,S\}$ .

Associating divisors with a function $f$ on $E$ is a convenient way to write down the intersection points (and their multiplicities) of $f$ and $E$ .
Let $\mathrm{ord}_P(f)$ count the multiplicity of $f$ at $P$ , which is positive if $f$ has a zero at $P$ , and negative if $f$ has a pole at $P$ 
.
We write the divisor of a function $f$ as $(f)$ , and it is defined as the divisor

$$(f)=\sum_{P\in E(\overline{\mathbb{F}}_q)}\mathrm{ord}_P(f)(P).$$

</details>

例 3.0.1 (Magma script)
$P,Q,R,S\in E(\overline{\mathbb{F}}q)$ とします。
$D_1=2(P)−3(Q)$ と $D_2=3(Q)+(R)−(S)$ とすると、$\mathrm{Deg}(D_1)=2−3=−1$ と $\mathrm{Deg}(D_2)=3+1−1=3$ となります。
合計 $D_1+D_2=2(P)+(R)−(S)$ は当然 $\mathrm{Deg}(D_1+D_2)=\mathrm{Deg}(D_1)+\mathrm{Deg}( D_2)=2$ となります。
台（_support_）は、$\mathrm{supp}(D_1)=\{P,Q\}$ 、$\mathrm{supp}(D_2)=\{Q,R,S\}$ 、$\mathrm{supp}(D_1+D_2)=\{P,R,S\}$ となります。

因子を $E$ 上の関数 $f$ に関連付けることは、$f$ と $E$ の交点 (およびそれらの多重度) を書き留める便利な方法です。
$\mathrm{ord}_P(f)$ は $P$ における $f$ の多重度を数え、$f$ が $P$ で零を持つ場合は正、$f$ が $P$ で極を持つ場合は負とします。
関数 $f$ の因子を $(f)$ と書き、それを因子として定義します。

$$(f)=\sum_{P\in E(\overline{\mathbb{F}}_q)}\mathrm{ord}_P(f)(P).$$

<details><summary>原文</summary>

Example 3.0.2 (Magma script).
We have already seen examples of functions on $E$ in the previous section, namely the lines $\ell:y=\lambda x+\nu$ used in the chordand-tangent rule, and it is natural that we are really only interested in the points of intersection of $\ell$ and $E$ , which is exactly what the divisor $(\ell)$ tells us.
The chord $\ell$ in Figure 3.1 intersects $E$ in $P, Q$ and $−(P+Q)$ , all with multiplicity $1$, and (as we will discuss further in a moment) $\ell$ also intersects $E$ with multiplicity $−3$ at $\mathcal{O}$ , i.e. $\ell$ has a pole of order $3$ at $\mathcal{O}$ .
Thus, $\ell$ has divisor $(\ell)=(P)+(Q)+(−(P+Q))−3(\mathcal{O})$ .
The tangent $\ell$ in Figure 3.2 intersects $E$ with multiplicity $2$ at $P$ , with multiplicity $1$ at $−[2]P$, and again with multiplicity $−3$ at $\mathcal{O}$ , so in this case $(\ell)=2(P)+(−[2]P)−3(\mathcal{O})$ .
Notice that in both cases we have $\mathrm{Deg}((\ell))=0$ .

</details>

例 3.0.2 (Magma script)
前の章で $E$ 上の関数の例、つまり弦と接線規則で使用される線 ℓ : y = λx + ν を既に見ました。実際に ℓ と E、これはまさに除数 (ℓ) が教えてくれるものです。
当然ですが、実際には $\ell$ と $E$ の交点だけに関心があり、これはまさに因子 $(\ell)$ が教えてくれることです。
図 3.1 の弦 $\ell$ は、$P$、$Q$、および $−(P+Q)$ で $E$ と交差し、すべての多重度は $1$ であり、(後で詳しく説明します) $\ell$ は $E$ と $\mathcal{O}$ で多重度 $−3$ と交差します。つまり、$\ell$ は $\mathcal{O}$ で位数 $3$ の極を持ちます。
したがって、$\ell$ は因子 $(\ell)=(P)+(Q)+(−(P+Q))−3(\mathcal{O})$ となります。
図 3.2 の接線 $\ell$ は、$E$ と $P$ で多重度 $2$ と交差し、$-[2]P$ で多重度 $1$ と交差し、$\mathcal{O}$ で再び多重度 $-3$ と交差するので、この場合 $(\ell)=2(P)+(−[2]P)−3(\mathcal{O})$ となります。
どちらの場合も、$\mathrm{Deg}((\ell))=0$ であることに注意してください。

![](/images/pfb/figure3_1-3_2.png)

<details><summary>原文</summary>

The balance that occurred between the zeros and poles in Example 3.0.2 that led to $\mathrm{Deg}((\ell))=0$ is not a coincidence.
In fact, a fundamental result that lies at the heart of the discussion is that this always happens: namely, for any function $f$ on $E$ , we always have $\mathrm{Deg}((f))=0$ .
An instructive proof of this result is in Galbraith’s book [Gal12, Th. 7.7.1], but roughly speaking this property follows from observing that the degree of the affine equation that solves for the zeros $f$ f on $E$ matches the degree of the projective equation that determines the multiplicity of the pole of $f$ at $\mathcal{O}$, i.e. the projective version of $f$ is $g/h$ where $g$ and $h$ both have the same degree as $f$ .
We revisit Example 3.0.2 and illustrate in this special case.

</details>

例 3.0.2 で零と極の間で起こったバランスが $\mathrm{Deg}((\ell))=0$ になったのは、偶然ではありません。
実際、議論の中心にある基本的な結果は、これが常に起こるということです。つまり、$E$ 上の任意の関数 $f$ に対して、常に $\mathrm{Deg}((f))=0$ になります。
この結果の有益な証明は Galbraith の本[Gal12, Th. 7.7.1]にありますが、大雑把に言えば、$E$ 上の $f$ の零を解くアフィン方程式の次数と $\mathcal{O}$ での $f$ の極の多重度を決める射影方程式の次数が一致することに注目してこの性質は成り立っています、つまり $f$ の射影バージョンは $g/h$ であり、$g$ と $h$ が共に $f$ と同じ次数を持ちます。
例 3.0.2 に戻り、この特殊なケースを説明します。

<details><summary>原文</summary>

Example 3.0.3 (Magma script).
We already know that three zeros (counting multiplicities) will always arise from substituting $\ell :y=\lambda x+\nu$ into $E/\mathbb{F}_q:y^2=x^3+ax+b$ , but we have only considered $\ell$ on the affine curve $E\cup\mathbb{A}^2$ , where $\ell$ has no poles.
To consider $\ell$ on $E$ at $\mathcal{O}=(0:1:0)$ (in $\mathbb{P}^2(\mathbb{F}_q)$ ), we need to take $x=X/Z$ and $y=Y/Z$ which gives $(\frac{\lambda X+\nu Z}{Z})^2=(\frac{X}{Z})^3+a(\frac{X}{Z})+b$ , for which we clearly have a pole of order $3$ when $Z=0$ .

The algebra between functions naturally translates across to the algebra between their divisors, so $(fg)=(f)+(g)$ and $(f/g)=(f)−(g)$ , $(f)=0$ if and only if f is constant, and thus if $(f)=(g)$ , then $(f/g)=0$ so $f$ is a constant multiple of $g$ , which means that the divisor $(f)$ determines $f$ up to non-zero scalar multiples.

</details>

$\ell :y=\lambda x+\nu$ を $E/\mathbb{F}_q:y^2=x^3+ax+b$ に代入すると、常に 3 つの零 (多重度を数える) が生じることは既にわかっていますが、アフィン曲線 $E\cup\mathbb{A}^2$ 上の $\ell$ のみを考慮しました。ここで、$\ell$ には極がありません。
$\mathcal{O}=(0:1:0)$ ( $\mathbb{P}^2(\mathbb{F}_q)$ 上 ) における $E$ 上の $\ell$ を考えるには、 $x=X/Z$ , $y=Y/Z$ とすると $(\frac{\lambda X+\nu Z}{Z})^2=(\frac{X}{Z})^3+a(\frac{X}{Z})+b$ となり、$Z=0$ のとき明らかに $3$ 次の極を持つことになります。

関数間の代数は、自然にそれらの因子間の代数に変換されます、ということは、$(fg)=(f)+(g)$ と $(f/g)=(f)-(g)$ で、$f$ が定数のときだけ $(f)=0$ となり、$(f)=(g)$ の場合、$(f/g)=0$ なので、$f$ は $g$ の定数倍といえます、これは因子 $(f)$ によって $f$ が非ゼロのスカラー倍数まで決定されることを意味します。

<details><summary>原文</summary>

Example 3.0.4 (Magma script).
Let $\ell :y=\lambda_1x+\nu_1$ be the chord (through $P$ and $Q$ ) with divisor $(\ell)=(P)+(Q)+(−(P+Q))−3(\mathcal{O})$ , and let $\ell':y=\lambda_2x+\nu_2$ be the tangent at $R$ with divisor $(\ell')=2(R)+(−[2]R)−3(\mathcal{O})$ .
The divisor of the function $\ell_{\mathrm{prod}}=\ell\ell'$ is $(\ell_{\mathrm{prod}})=(\ell)+(\ell')=(P)+(Q)+2(R)+(−(P+Q))+(−[2]R)−6(\mathcal{O})$ .
The divisor of $\ell_{\mathrm{quot}}=\ell/\ell'$ is $(\ell_{\mathrm{quot}})=(\ell)−(\ell')=(P)+(Q)+(−(P+Q))−2(R)−(−[2]R)$ .
Notice that $\ell_{\mathrm{quot}}$ does not intersect $E$ at $\mathcal{O}$; projectifying $\ell/\ell'=\frac{y-\lambda_1x-\nu_1}{y-\lambda_2x-\nu_2}$ gives $\frac{Y-\lambda_1X-\nu_1Z}{Y-\lambda_2X-\nu_2Z}$ , which does not give rise to any zeros or poles at $Z=0$ .
Suppose we wanted to depict the function $\ell\ell'$ on $E$ , and we multiplied out $(y−\lambda_1x−\nu_1)(y−\lambda_2x−\nu_2)$ , substituted the $y^2$ for $x^3+ax+b$ and wrote $y=\frac{x^3+ax+b+(\lambda_1x+\nu_1)(\lambda_2x+\nu_2)}{(\lambda_1+\lambda_2)x+\nu_1+\nu_2}$ .
It does not make sense to try and depict this function since all the pictures we have used for illustrative purposes also show how the functions (on $E$ ) behave at points that are not on $E$ , where the substitution $y^2=x^3+ax+b$ is not permitted.

</details>

例 3.0.4 (Magma script)
$\ell :y=\lambda_1x+\nu_1$ を ( $P$ と $Q$ を通る) 因子 $(\ell)=(P)+(Q)+(−(P+Q))−3(\mathcal{O})$ の弦とし、$\ell':y=\lambda_2x+\nu_2$ は $R$ での接線で、因子 $(\ell')=2(R)+(−[2]R)−3(\mathcal{O})$ です。
関数 $\ell_{\mathrm{prod}}=\ell\ell'$ の因子は $(\ell_{\mathrm{prod}})=(\ell)+(\ell')=(P)+(Q)+2(R)+(−(P+Q))+(−[2]R)−6(\mathcal{O})$ となります。
$\ell_{\mathrm{quot}}=\ell/\ell'$ の因子は、$(\ell_{\mathrm{quot}})=(\ell)−(\ell')=(P)+(Q)+(−(P+Q))−2(R)−(−[2]R)$ となります。
$\ell_{\mathrm{quot}}$ は $\mathcal{O}$ で $E$ と交差しないことに注意してください。
$\ell/\ell'=\frac{y-\lambda_1x-\nu_1}{y-\lambda_2x-\nu_2}$ を射影すると $\frac{Y-\lambda_1X-\nu_1Z}{Y-\lambda_2X-\nu_2Z}$ が得られますが、これは $Z=0$ で零や極を生じさせません。
関数 $\ell\ell'$ を $E$ 上に描きたいと仮定し、 $(y−\lambda_1x−\nu_1)(y−\lambda_2x−\nu_2)$ を掛け、$x^3+ax+b$ を $y^2$ に置き換え、$y=\frac{x^3+ax+b+(\lambda_1x+\nu_1)(\lambda_2x+\nu_2)}{(\lambda_1+\lambda_2)x+\nu_1+\nu_2}$ と書けます。
説明目的で使用したすべての図は、$E$ 上にない点で ($E$ 上の) 関数がどのように動作するかを示しているため、この関数を描写しようとしても意味がありません。ここで、置換 $y^2=x^3+ax+b$ は 許可されていません。

![](/images/pfb/figure3_3.png)

## 3.1 因子類群（The divisor class group）

<details><summary>原文</summary>

We can now start introducing important subgroups of the group of divisors $\mathrm{Div}_{\overline{\mathbb{F}}_q}(E)$ on $E$.
We temporarily drop the subscript, and write $\mathrm{Div}(E)$ as the group of all divisors on $E$ .
The set of degree zero divisors $\{D\in\mathrm{Div}(E):\mathrm{Deg}(D)=0\}$ forms a proper subgroup, which we write as $\mathrm{Div}^0(E)\subset\mathrm{Div}(E)$.
If a divisor $D$ on $E$ is equal to the divisor of a function, i.e. $D=(f)$ , then $D$ is called a principal divisor, and the set of principal divisors naturally form a group, written as $\mathrm{Prin}(E)$ .
We already know (from Example 3.0.3 and the preceding discussion) that principal divisors have degree zero, but there are also degree zero divisors that are not the divisors of a function, so the degree zero subgroup is strictly larger than the principal divisors, i.e. $\mathrm{Prin}(E)\subset\mathrm{Div}^0(E)$ .
There is, however, an extra condition on elements of $\mathrm{Div}^0(E)$ that does allow us to write an “if-and-only-if”: $D=\sum_Pn_P(P)\in\mathrm{Div}^0(E)$ is principal if and only if $\sum_P[n_P]P=\mathcal{O}$ on $E$ [Gal05, Th. IX.2].
We illustrate this statement, and the relationship between the three groups

$$\mathrm{Prin}(E)\subset\mathrm{Div}^0(E)\subset\mathrm{Div}(E)\tag{3.1}$$

in Example 3.1.1.

</details>

ここで、$E$ 上の因子群 $\mathrm{Div}_{\overline{\mathbb{F}}_q}(E)$ の重要な部分群の導入を開始できます。
ここでは一時的に添え字を外し、$E$ 上のすべての因子の群として $\mathrm{Div}(E)$ と書きます。
次数ゼロの因子の集合 $\{D\in\mathrm{Div}(E):\mathrm{Deg}(D)=0\}$ は適切な部分群を形成し、これを $\mathrm{Div}^0(E)\subset\mathrm{Div}(E)$ と書きます。
$E$ の因子 $D$ が関数の因子に等しい場合、つまり $D=(f)$ の場合、$D$ は主因子と呼ばれ、主因子の集合は自然に群を形成し、$\mathrm{Prin}(E)$ として記述されます。
(例 3.0.3 と前の議論から) 主因数にはゼロ次数があることは既にわかっていますが、関数の因子ではない次数ゼロの因子もあるため、ゼロ次数の部分群は主因数よりも厳密に大きくなります。 すなわち、 $\mathrm{Prin}(E)\subset\mathrm{Div}^0(E)$ です。
ただし、$\mathrm{Div}^0(E)$ の要素には、「if-and-only-if」を記述できる特別な条件があります。$D=\sum_Pn_P(P)\in\mathrm{Div}^0(E)$ は、$E$ 上で $\sum_P[n_P]P=\mathcal{O}$ の場合にのみ主となります。[Gal05, Th. IX.2]
この言明と、3 つの群間の関係

$$\mathrm{Prin}(E)\subset\mathrm{Div}^0(E)\subset\mathrm{Div}(E)\tag{3.1}$$

を、例 3.3.1 で説明します。

<details><summary>原文</summary>

Example 3.1.1 (Magma script).
Consider $E/\mathbb{F}_{103}:y^2=x^3+20x+20$ , with points $P=(26,20)$, $Q=(63,78)$, $R=(59,95)$, $S=(24,25)$, $T=(77,84)$, $U=(30,99)$ all on $E$.
The divisor $(S)+(T)−(P)\in\mathrm{Div}(E)$ is clearly not in the subgroup $\mathrm{Div}^0(E)$ , since it has degree $1$ ; there are also infinitely many other trivial examples.
The divisor $(P)+(Q)−(R)−(S)$ is in $\mathrm{Div}^0(E)$ , but is not principal since $P+Q−R−S=(18,49)\ne\mathcal{O}$ on $E$ .
Thus, a function $f$ with $(f)=(P)+(Q)−(R)−(S)$ does not exist.
On the other hand, the divisor $(P)+(Q)−(R)−(T)$ is principal, since it is degree $0$ and $P+Q−R−T=\mathcal{O}$ on $E$ .
Thus, there is some function f on E such that $(f)=(P)+(Q)−(R)−(T)$ ; it is $f=\frac{6y+71x^2+91x+91}{x^2+70x+11}$ .
The sum $R+T$ on $E$ is actually $U$ , thus $P+Q−U=\mathcal{O}$ on $E$ , but this time there is no function with divisor $(P)+(Q)−(U)$ because the degree of this divisor is not zero; however, we can keep the sum on $E$ as $O$ but manipulate the degree by instead taking the divisor $(P)+(Q)−(U)−(\mathcal{O})$ , which must be in $\mathrm{Prin}(C)$ , guaranteeing the existence of a function $g$ with $(g)=(P)+(Q)−(U)−(\mathcal{O})$ , namely $g=\frac{y+4x+82}{x+73}$ . 
Observe the difference between $f$ and $g$ in projective space, where $f=\frac{6YZ+71X^2+91XZ+91Z^2}{X^2+70XZ+11Z^2}$ and $g=\frac{Y+4X+82Z}{X+73Z}$.
For $f$ , the point at infinity $\mathcal{O}=(0:1:0)$ zeros both the numerator and denominator, giving a zero and a pole which cancels out its contribution to $(f)$, whilst for g, the point at infinity only zeros the denominator, which is why $\mathcal{O}\in\mathrm{supp}((g))$ , whereas $\mathcal{O}\notin\mathrm{supp}((f))$ .

</details>

例 3.1.1 (Magma script)
$E/\mathbb{F}_{103}:y^2=x^3+20x+20$ を考えます、$E$ 上の点は、$P=(26,20)$、$Q=(63,78)$、$R=(59,95)$、$S=(24,25)$、$T=(77,84)$、$U=(30,99)$ となります。
因子 $(S)+(T)−(P)\in\mathrm{Div}(E)$ は、次数が $1$ であるため、明らかに部分群 $\mathrm{Div}^0(E)$ にはありません。他にも無数の些細な例があります。
因子 $(P)+(Q)−(R)−(S)$ は $\mathrm{Div}^0(E)$ に含まれますが、 $E$ 上で $P+Q−R−S=(18,49)\ne\mathcal{O}$ となるため、主ではりません。
したがって、$(f)=(P)+(Q)−(R)−(S)$ となる関数 $f$ は存在しません。
一方、因子 $(P)+(Q)−(R)−(T)$ は次数 $0$ であり、$E$ では $P+Q−R−T=\mathcal{O}$ であるため、主です。
したがって、$(f)=(P)+(Q)−(R)−(T)$ となる $E$ 上の関数 $f$ が存在します。 $f=\frac{6y+71x^2+91x+91}{x^2+70x+11}$ です。
$E$ 上での加算 $R+T$ は実際には $U$ 、となるので、$E$ 上で $P+Q−U=\mathcal{O}$ 、ですが今回は、因子 $(P)+(Q)−(U)$ を持つ関数はありません。この因子の次数はゼロではないためです。ただし、$E$ 上の加算を $\mathcal{O}$ のままにしておくことはできますが、代わりに因子 $(P)+(Q)−(U)−(\mathcal{O})$ を取ることで次数を操作できます。
これは $\mathrm{Prin}(C)$ になければならず、 $(g)=(P)+(Q)−(U)−(\mathcal{O})$ の関数 $g$ の存在を保証します。つまり、 $g=\frac{y+4x+82}{x+73}$ です。
射影空間で $f$ と $g$ の差を観察します。ここで、 $f=\frac{6YZ+71X^2+91XZ+91Z^2}{X^2+70XZ+11Z^2}$ と $g=\frac{Y+4X+82Z}{X+73Z}$ です。
$f$ の場合、無限遠点 $\mathcal{O}=(0:1:0)$ は分子と分母の両方を例にし、$(f)$ への寄与を打ち消す零と極を与えます。
一方、$g$ の場合、無限遠点は分母を零にするだけです。これが、 $\mathcal{O}\in\mathrm{supp}((g))$ に対して $\mathcal{O}\notin\mathrm{supp}((f))$ である理由です。

<details><summary>原文</summary>

Returning to the subscript notation for a moment, the three subgroups (and other related groups) in Equation (3.1) are often accompanied by the field they apply to, e.g. for a general field $K$, they are written as $\mathrm{Prin}_K(E)$, $\mathrm{Div}^0_K(E)$, and  $\mathrm{Div}_K(E)$ .
Here $\mathrm{Div}_K(E)\subset\mathrm{Div}(E)$ is formally defined as the set of divisors invariant under the action of $\mathrm{Gal}(\overline{K}/K)$ , where $\sigma\in\mathrm{Gal}(\overline{K}/K)$ acts on $D=\sum_Pn_P(P)$ to give $D^{\sigma}=\sum_Pn_P(\sigma(P))$ , so that $D\in\mathrm{Div}_K(E)$ if $D=D^{\sigma}$ .
This is very natural in the contexts we consider, so we will continue on without subscripts.

Before we define the divisor class group of $E$ , we look at the important notion of divisor equivalence in $\mathrm{Div}(E)$.
We call the divisors $D_1$ and $D_2$ equivalent, written as $D_1∼D_2$, if $D_1=D_2+(f)$ for some function $f$.

</details>

少し添字表記に戻ると、式 (3.1) の 3 つの部分群 (およびその他の関連する群) には、それらが適用される体が付随することがよくあります。 一般的な体 K の場合、それらは $\mathrm{Prin}_K(E)$ 、 $\mathrm{Div}^0_K(E)$ 、および $\mathrm{Div}_K(E)$ として記述されます。
ここで、 $\mathrm{Div}_K(E)\subset\mathrm{Div}(E)$ は、 $\mathrm{Gal}(\overline{K}/K)$ の作用下で不変な因子の集合として正式に定義されます。
ここで、 $\sigma\in\mathrm{Gal}(\overline{K}/K)$ は $D=\sum_Pn_P(P)$ に作用して $D^{\sigma}=\sum_Pn_P(\sigma(P))$ を与えるため、 $D=D^{\sigma}$ の場合、 $D\in\mathrm{Div}_K(E)$ となります。
これは私たちが考える文脈ではごく自然なことなので、このまま添え字なしで話を進めることにします。

$E$ の因子類群を定義する前に、$\mathrm{Div}(E)$ における因子の等価性の重要な概念を見ていきます。
因子 $D_1$ と $D_2$ を同等と呼び、ある関数 $f$ に対して $D_1=D_2+(f)$ の場合、$D_1\sim D_2$ と書きます。

<details><summary>原文</summary>

Example 3.1.2 (Magma script).
Consider $P=(57,24)$, $Q=(25,37)$, $R=(17,32)$ and $S=(42,35)$ on $E/\mathbb{F}_{61}:y^2=x^3+8x+1$ .
The divisors $D_1=(P)+(Q)+(R)$ and $D_2=4(\mathcal{O})−(S)$ are equivalent as follows.
The function $f:y=33x^2+10x+24$ , which intersects $E$ at $P$, $Q$, $R$ and $S$ with multiplicity $1$ , and therefore has a pole of order $4$ at infinity, has divisor $(f)=(P)+(Q)+(R)+(S)−4(\mathcal{O})$ , meaning $D_1=D_2+(f)$ , so $D_1\sim D_2$ .
Alternatively, if we did not want to find $f$ , we could have used $D_1−D_2=(P)+(Q)+(R)+(S)−4(\mathcal{O})$ , which has degree zero, and computed that $P+Q+R+S−[4]\mathcal{O}=\mathcal{O}$ on $E$ , which means $D_1−D_2\in\mathrm{Prin}(E)$ , so that $D_1−D_2=(f)$ for some function $f$ .

The divisor class group, or Picard group, of $E$ is defined as the quotient group

$$\mathrm{Pic}^0(E)=\mathrm{Div}^0(E)/\mathrm{Prin}(E),\tag{3.2}$$

i.e. the divisor class group is the group of all degree zero divisors modulo the principal divisors on $E$ .
At first read, this notion of equivalence (modulo divisors of functions) may seem a little abstract, but once we see it in action (particularly in more general scenarios than elliptic curves), it becomes very natural.
We will first use this notion to describe the elliptic curve group law in terms of divisors, following along the lines of Galbraith [Gal05, §IX.2].

</details>

例 3.1.2 (Magma script)
$E/\mathbb{F}_{61}:y^2=x^3+8x+1$ 上の $P=(57,24)$、$Q=(25,37)$、$R=(17,32)$、$S=(42,35)$ を考えます。
因子 $D_1=(P)+(Q)+(R)$ と $D_2=4(\mathcal{O})−(S)$ は、次と同等です。
関数 $f:y=33x^2+10x+24$ は、多重度 $1$ で $P$、$Q$、$R$、および $S$ で $E$ と交差し、したがって無限遠点で次数 $4$ の極を持ち、因子 $(f)=(P)+(Q)+(R)+(S)−4(\mathcal{O})$ 、つまり $D_1=D_2+(f)$ なので、 $D_1\sim D_2$ です。
または、 $f$ を見つけたくない場合は、 $D_1−D_2=(P)+(Q)+(R)+(S)−4(\mathcal{O})$ を使用できます。 $E$ で $P+Q+R+S−[4]\mathcal{O}=\mathcal{O}$ を計算し、これは $D_1−D_2\in\mathrm{Prin}(E)$ であるため、関数 $f$ に対して $D_1−D_2=(f)$ となります。

$E$ の因子類群（divisor class group）、またはピカール群（Picard group）は、商群（quotient group）として定義されます。

$$\mathrm{Pic}^0(E)=\mathrm{Div}^0(E)/\mathrm{Prin}(E),\tag{3.2}$$

つまり、因子類群（divisor class group）は、$E$ の主因子を法とするすべての次数ゼロ因子の群です。
最初に読んだとき、この等価性 (関数のモジュロ因子) の概念は少し抽象的なように見えるかもしれませんが、実際の動作 (特に楕円曲線よりも一般的なシナリオ) を見ると、非常に自然になります。
まず、この概念を使用して、Galbraith [Gal05, §IX.2] の方針に従って、除数の観点から楕円曲線群律を説明します。

<details><summary>原文</summary>

Example 3.1.3 (Magma script).
Referring back to Figure 2.5 (or Figure 2.6 in the case that $Q=P$ ), the line $\ell$ joining $P$ and $Q$ has divisor $(\ell)=(P)+(Q)+(−R)−3(\mathcal{O})$ , whilst the vertical line $v=x−x_R$ has divisor $(v)=(−R)+(R)−2(\mathcal{O})$ .
The quotient $\frac{\ell}{v}$ has divisor $(\frac{\ell}{v})=(P)+(Q)−(R)−(\mathcal{O})$ .
Thus, the equation $R=P+Q$ on $E$ is the same as the divisor equality $(R)−(\mathcal{O})=(P)−(\mathcal{O})+(Q)−(\mathcal{O})−(\frac{\ell}{v})$ , and the map of points to divisor classes $P\mapsto(P)−(\mathcal{O})$ is a group homomorphism.
To concretely connect this back to Equation (3.2), both $(R)−(\mathcal{O})$ and $(P)+(Q)−2(\mathcal{O})$ are clearly in $\mathrm{Div}^0(E)$ , but they represent the same class in $\mathrm{Pic}^0(E)$ , because the divisor $(\frac{l}{v})=(P)+(Q)−(R)−(\mathcal{O})$ (which is their difference) is principal, and therefore zero in $\mathrm{Pic}^0(E)$ .

</details>

例 3.1.3 (Magma script)
図 2.5 (または $Q=P$ の場合は図 2.6) を参照すると、$P$ と $Q$ を結ぶ線 $\ell$ は因子 $(\ell)=(P)+(Q)+(−R)−3(\mathcal{O})$ を持ちますが、 垂直線 $v=x−x_R$ の因子は $(v)=(−R)+(R)−2(\mathcal{O})$ です。
商 $\frac{\ell}{v}$ の因子は  $(\frac{\ell}{v})=(P)+(Q)−(R)−(\mathcal{O})$ です。
したがって、$E$ 上の方程式 $R=P+Q$ は、因子等式 $(R)−(\mathcal{O})=(P)−(\mathcal{O})+(Q)−(\mathcal{O})−(\frac{\ell}{v})$ と同じであり、点の因子類への写像 $P\mapsto(P)−(\mathcal{O})$ は群準同型になります。
これを式 (3.2) に具体的に結び付けると、 $(R)−(\mathcal{O})$ と $(P)+(Q)−2(\mathcal{O})$ の両方が明らかに $\mathrm{Div}^0(E)$ に含まれますが、因子 $(\frac{l}{v})=(P)+(Q)−(R)−(\mathcal{O})$ (それらの差) が主であり、$\mathrm{Pic}^0(E)$ ではゼロなので、$\mathrm{Pic}^0(E)$ では同じ類を表します。

## 3.2 リーマン・ロッホの定理の帰結（A consequence of the Riemann-Roch Theorem）

<details><summary>原文</summary>

The notion of equivalence allows us to reduce divisors of any size $D\in\mathrm{Pic}^0(E)$ into much smaller divisors. 
We will make this statement precise after an example, but we must first define what we mean by “size”.
A divisor $D=\sum_Pn_P(P)$ is called effective if $n_P\geq 0$ for all $P\in E$ .
The only divisor in $\mathrm{Div}^0(E)$ that is effective is the zero divisor.
Thus, we define the effective part of a divisor $D$ as $\epsilon(D)=\sum_Pn_P(P)$ , where $n_P\geq 0$ .
For example, the divisor $D=(P)+(Q)-2(\mathcal{O})$ is not effective, but the effective part is $\epsilon(D)=(P)+(Q)$ .
By the size of $D$ , we mean the degree of the effective part, so in our example, although $\mathrm{Deg}(D)=0$ , it is size $2$, since $\mathrm{Deg}(\epsilon(D))=2$ .

</details>

同値の概念により、任意のサイズ $D\in\mathrm{Pic}^0(E)$ の因子をより小さな因子に減らすことができます。
例の後でこのステートメントを正確に説明しますが、最初に「サイズ」の意味を定義する必要があります。
因子 $D=\sum_Pn_P(P)$ は、すべての $P\in E$ に対して $n_P\geq 0$ の場合に有効であるといいます。
$\mathrm{Div}^0(E)$ で有効な因子は零因子だけです。
したがって、因子 $D$ の効果的部分を $\epsilon(D)=\sum_Pn_P(P)$ と定義します。ここで、$n_P\geq 0$ です。
たとえば、因子 $D=(P)+(Q)-2(\mathcal{O})$ は無効ですが、効果的部分は $\epsilon(D)=(P)+(Q)$ です。
$D$ の大きさは効果的部分の次数を意味するので、この例では $\mathrm{Deg}(D)=0$ ですが、$\mathrm{Deg}(\epsilon(D))=2$ であるため、サイズは $2$ です。

<details><summary>原文</summary>

Example 3.2.1 (Magma script).
Consider the divisor $D=(P_1)+\ldots +(P_{11})−11(\mathcal{O})$ (with $\mathrm{Deg}(\epsilon(D))=11$ ) as an element of $\mathrm{Pic}^0(E)$ on $E/\mathbb{F}_q:y^2=x^3+ax+b$ , where the $P_i$ are not necessarily distinct.
To find a divisor that is equivalent to $D$, we can construct function $\ell_{10}:y=a_{10}x^{10}+\ldots +a_1x+a_0$ to interpolate the distinct points in $\mathrm{supp}(D)$ with appropriate multiplicities.
Substituting $\ell_{10}$ into $E$ gives a degree $20$ polynomial in $x$, the roots of which reveals the $20$ affine points of intersection (counting multiplicities) between $\ell_{10}$ and $E$ .
We already know 11 of these points (the $P_i$’s), so let $P'_1,\ldots,P'_9$ be the other $9$ .
An important point to note is that these points are not necessarily defined over $\mathbb{F}_q$ .
Since $(\ell_{10})=\sum^{11}_{i=1}(P_i)+\sum^9_{i=1}(P'_i)−20(\mathcal{O})\in\mathrm{Prin}(E)$ , $D'=−(\sum^9_{i=1}(P'_i)−9(\mathcal{O}))$ is a divisor equivalent to $D$ in $\mathrm{Pic}^0(E)$ , i.e. $D'\sim D$ .
We can repeat this process, interpolating the points in $\mathrm{supp}(D')$ with a degree 8 polynomial $\ell_8:y=a'_8x^8+\ldots +a'_1x+a'_0$ , which will intersect $E$ (in the affine sense) $16$ times, giving $7$ new intersection points, thereby finding a divisor $D''=\sum^7_{i=1}(P''_i)−7(\mathcal{O})$ equivalent to $D'$ , meaning $D''\sim D$ .
It is easy to infer that the number of new roots (maximum number of divisors in the consecutive supports) decreases each time by two, so that in two more steps we will arrive at $\tilde{D}=(\tilde{P}_1)+(\tilde{P}_2)+(\tilde{P}_3)−3(\mathcal{O})$.
We can interpolate the three points in $\mathrm{supp}(\tilde{D})$ with a quadratic function $\tilde{\ell}:y=\tilde{a}_2x^2+\tilde{a}_1x+\tilde{a}_0$ that clearly intersects $E$ at one more affine point, say $Q$ .
That is, $(\tilde{\ell})=(\tilde{P}_1)+(\tilde{P}_2)+(\tilde{P}_3)+(Q)−4(\mathcal{O})$ , and since $(\tilde{\ell})\in\mathrm{Prin}(E)$ , then $(\tilde{D})\sim(\mathcal{O})−(Q)$ .
Lastly, the vertical line $\tilde{v}$ has divisor $(\tilde{v})=(Q)+(R)−2(\mathcal{O})$ , meaning $(\mathcal{O})−(Q)\sim(R)−(\mathcal{O})$ , which gives $(\tilde{D})\sim(R)−(\mathcal{O})$ .
To summarise, we started with a divisor $D=(P_1)+\ldots (P_{11})−11(\mathcal{O})$ which had size $11$ , and reduced to the equivalent divisor $(R)−(\mathcal{O})\sim D$ in $\mathrm{Pic}^0(E)$ which has size $1$ .

</details>

例 3.2.1 (Magma script)
因子 $D=(P_1)+\ldots +(P_{11})−11(\mathcal{O})$ ( $\mathrm{Deg}(\epsilon(D))=11$ ) を $E/\mathbb{F}_q:y^2=x^3+ax+b$ 上の $\mathrm{Pic}^0(E)$ の要素と見なします。ここで $P_i$ は必ずしも明確ではありません。
$D$ に等しい因子を見つけるには、関数 $\ell_{10}:y=a_{10}x^{10}+\ldots +a_1x+a_0$ を作成して、適切な多重度で $\mathrm{supp}(D)$ の個別の点を補間します。
$\ell_{10}$ を $E$ に代入すると、$x$ の次数 $20$ の多項式が得られ、その根は、$\ell{10}$ と $E$ の間の $20$ 個のアフィン交点 (多重度をカウント) を明らかにします。
これらの点 ($P_i$) のうち $11$ 点は既にわかっているので、 $P'_1,\ldots,P'_9$ を残りの $9$ 点とします。
注意すべき重要な点は、これらの点が必ずしも $\mathbb{F}_q$ で定義されているわけではないということです。
$(\ell_{10})=\sum^{11}_{i=1}(P_i)+\sum^9_{i=1}(P'_i)−20(\mathcal{O})\in\mathrm{Prin}(E)$ なので,$D'=−(\sum^9_{i=1}(P'_i)−9(\mathcal{O}))$ は $\mathrm{Pic}^0(E)$ において $D$ と等価な因子、すなわち $D'\sim D$ となります。
このプロセスを繰り返すことができ、 $\mathrm{supp}(D')$ の点を次数 $8$ の多項式 $\ell_8:y=a'_8x^8+\ldots +a'_1x+a'_0$ で補間します、これは (アフィンの意味で) $E$ と $16$ 回交差します、$7$ つの新しい交点を与えることにより、$D'$  に相当する除数 $D''=\sum^7_{i=1}(P''_i)−7(\mathcal{O})$ を見つけます。これは、$D''\sim D$ を意味します。
このとき，新しい根の数（連続するサポートの中の最大の因子の数）は，毎回2ずつ減っていくことが容易に推測できます。そのため、さらに 2 つのステップで $\tilde{D}=(\tilde{P}_1)+(\tilde{P}_2)+(\tilde{P}_3)−3(\mathcal{O})$ に到達します。
$\mathrm{supp}(\tilde{D})$ の 3 つの点を二次関数 $\tilde{\ell}:y=\tilde{a}_2x^2+\tilde{a}_1x+\tilde{a}_0$ で補間できます。これは $E$ ともう 1 つのアフィン点、たとえば $Q$ で明確に交差します。
つまり、$(\tilde{\ell})=(\tilde{P}_1)+(\tilde{P}_2)+(\tilde{P}_3)+(Q)−4(\mathcal{O})$ であり、$(\tilde{\ell})\in\mathrm{Prin}(E)$ であるため、$(\tilde{D})\sim(\mathcal{O})−(Q)$ となります。
最後に、垂直線 $\tilde{v}$ は、因子 $(\tilde{v})=(Q)+(R)−2(\mathcal{O})$ 、つまり $(\mathcal{O})−(Q)\sim(R)−(\mathcal{O})$ を持つので、 $(\tilde{D})\sim(R)−(\mathcal{O})$ が得られます。
要約すると、サイズ $11$ の因子 $D=(P_1)+\ldots (P_{11})−11(\mathcal{O})$ から始めて、サイズ $1$ の $\mathrm{Pic}^0(E)$ の同値の因子 $(R)−(\mathcal{O})\sim D$ に減らしました。

![](/images/pfb/figure3_4.png)

<details><summary>原文</summary>

The above example illustrates a key consequence of one of the most central theorems in our study: the Riemann-Roch theorem.
To present the theorem in its generality requires a few more definitions than we need for our exposition, so for the full story we refer the reader to any of [Ful08, §8], [Sil09, §II.5], [Gal12, §8.7].
The important corollary we use is the following: for any curve $C$, there is a unique minimal integer $g$, called the genus of $C$, such that any divisor $D\in\mathrm{Pic}^0(C)$ is equivalent to a divisor $D'$ with $\mathrm{Deg}(\epsilon(D'))\leq g$.
Elliptic curves $E$ are curves of genus $g=1$ , meaning that every $D\in\mathrm{Pic}^0(E)$ can be written as $(P_1)−(Q_1)$ ; this is why we were able to reduce the divisor in Example 3.2.1 to $(R)−(\mathcal{O})$ .

We will only be dealing with elliptic curves in this text, since they have proved most successful in the context of pairings, but for now it aids one’s understanding to see where elliptic curves fit in a slightly broader context.
Assuming an odd characteristic field, a general (“imaginary quadratic”) hyperelliptic curve of genus $g$ is a generalisation of an elliptic curve, which can be written as

$$C_g:y^2=x^{2g+1}+f_{2g}x^{2g}+\ldots +f_1x+f_0.\tag{3.3}$$

Each divisor $D\in\mathrm{Pic}^0(C_g)$ has a unique reduced representative of the form

$$(P_1)+(P_2)+\ldots +(P_n)−n(\mathcal{O}),$$

where $n\leq g$, $P_i\ne −P_j$ for all $i\ne j$, and no $P_i$ satisfying $P_i=−P_i$ appears more than once [BBC+09, §2.3].
The following examples illustrate this in the case of genus 2 and genus 3 respectively.

</details>

上記の例は、私たちの研究で最も中心的な定理の 1 つであるリーマン・ロッホの定理（Riemann-Roch theorem）の重要な結果を示しています。
定理を一般化して提示するには、説明に必要な定義よりもいくつか多くの定義が必要になるため、完全なストーリーについては、[Ful08, §8]、[Sil09, §II.5]、[Gal12, §8.7] のいずれかを参照してください。
使用する重要な系は次のとおりです。任意の曲線 $C$ に対して、$C$ の種数と呼ばれる固有の最小整数 $g$ が存在します。任意の因子 $D\in\mathrm{Pic}^0(C)$ は、$\mathrm{Deg}(\epsilon(D'))\leq g$ の因子 $D'$ と等価です。
楕円曲線 $E$ は種数 $g=1$ の曲線です。つまり、すべての $D\in\mathrm{Pic}^0(E)$ は $(P_1)−(Q_1)$ として記述できます。 これが、例 3.2.1 の因子を $(R)−(\mathcal{O})$ に減らすことができた理由です。

楕円曲線はペアリングの文脈で最も成功しているので、この文章では楕円曲線だけを扱うことにする。しかし、とりあえず、楕円曲線がもう少し広い文脈の中でどのように位置づけられるかを知ることは、理解の助けになります。
奇標数体を仮定すると、種数 $g$ の一般的な (「虚二次（imaginary quadratic）」) 超楕円曲線は楕円曲線の一般化であり、次のように記述できます。

$$C_g:y^2=x^{2g+1}+f_{2g}x^{2g}+\ldots +f_1x+f_0.\tag{3.3}$$

各因子 $D\in\mathrm{Pic}^0(C_g)$ には、次の形式の固有の簡約表現があります。

$$(P_1)+(P_2)+\ldots +(P_n)−n(\mathcal{O}),$$

ここで、$n\leq g$ 、すべての $i\ne j$ に対して $P_i=−P_i$ であり、Pi = −Pi を満たす $P_i$ は複数回出現しません [BBC+09, §2.3]。
次の例は、それぞれ種数 2 と種数 3 の場合について説明しています。

<details><summary>原文</summary>

Example 3.2.2 (Magma script).
A general (odd characteristic field) hyperelliptic curve of genus $g=2$ is given (via Equation (3.3)) as $C_2:y^2=x^5+f_4x^4+\ldots +f_0$ ; we give a typical depiction in Figure 3.5.
Suppose we have a divisor $D=(P_1)+(P_2)+(P_3)+(P_4)−4(\mathcal{O})\in\mathrm{Pic}^0(C_2)$ , the affine support of which is depicted in red.

The Riemann-Roch theorem guarantees a (unique) equivalent divisor of the form $(P'_1)+(P'_2)−2(\mathcal{O})$ .
We find it by constructing the cubic function $\ell:y=a_3x^3+\ldots+a_0$ that has $4$ zeros corresponding to the effective part of $D$ , and therefore $4$ poles at $\mathcal{O}$ .
Substitution of $\ell$ into $E$ reveals two more points of intersection, $\overline{P}_1$ and $\overline{P}_2$ , meaning $(\ell)=(P_1)+(P_2)+(P_3)+(P_4)+(\overline{P}_1)+(\overline{P}_2)−6(\mathcal{O})$ .
Since $(\ell)\in\mathrm{Prin}(C_2)$ , then $D=D−(\ell)$ in $\mathrm{Pic}^0(C_2)$ meaning $D\sim 2(\mathcal{O})−(\overline{P}_1)−(\overline{P}_2)$ .
As usual, we reverse the ordering (so the effective part is affine) by making use of the vertical lines $v_1$ and $v_2$ with divisors $(v_1)=(\overline{P}_1)+(P'_1)−2(\mathcal{O})$ and $(v_2)=(\overline{P}_2)+(P'_2)−2(\mathcal{O})$ , to write $2(\mathcal{O})−(\overline{P}_1)−(\overline{P}_2)=2(\mathcal{O})−(\overline{P}_1)−(\overline{P}_2)+(v_1)+(v_2)=(P'_1)+(P'_2)−2(\mathcal{O})=D'$ , meaning $D\sim D'$ .
We have reduced a divisor $D$ with $\mathrm{Deg}(\epsilon(D))=4$ to a divisor $D'$ with $\mathrm{Deg}(\epsilon(D'))=2\leq g$ .
Note that the points in the support of $D'$ are not necessarily defined over $\mathbb{F}_q$ .
Also note that trying to reduce $D'$ any further, say by running a line $\ell':y=\lambda x+\nu$ through $P'_1$ and $P'_2$ , will not work in general, since this line will intersect $E$ in $3$ more places, creating an unreduced divisor $D''$ with $\mathrm{Deg}(\epsilon(D''))=3>g$ .

</details>

例 3.2.2 (Magma script)
種数 $g=2$ の一般的な (奇標数体) 超楕円曲線は (式 (3.3) により)  $C_2:y^2=x^5+f_4x^4+\ldots +f_0$ として与えられます。 図 3.5 に典型的な描写を示します。
赤で描かれたアフィン台を持つ因子 $D=(P_1)+(P_2)+(P_3)+(P_4)−4(\mathcal{O})\in\mathrm{Pic}^0(C_2)$ があるとする。

リーマン・ロッホ（Riemann-Roch）の定理により、 $(P'_1)+(P'_2)−2(\mathcal{O})$ の形の（唯一）等価な因子が保証されています。
$D$ の有効部分に対応する $4$ つのゼロを持つ３次関数 $\ell:y=a_3x^3+\ldots+a_0$ を構成し、 $\mathcal{O}$ に $4$ つの極を作ることで求められます。
$E$ に $\ell$ を代入すると、さらに2つの交点 $\overline{P}_1$ と $\overline{P}_2$ が現れ、 $(\ell)=(P_1)+(P_2)+(P_3)+(P_4)+(\overline{P}_1)+(\overline{P}_2)−6(\mathcal{O})$ となります。
$(\ell)\in\mathrm{Prin}(C_2)$ なので、$\mathrm{Pic}^0(C_2)$ の $D=D−(\ell)$ は $D\sim 2(\mathcal{O})−(\overline{P}_1)−(\overline{P}_2)$ を意味します。
いつものように、垂直線 $v_1$ と $v_2$ を因子 $(v_1)=(\overline{P}_1)+(P'_1)−2(\mathcal{O})$ と $(v_2)=(\overline{P}_2)+(P'_2)−2(\mathcal{O})$ で使用して順序を逆にし (有効部分はアフィン)、 $2(\mathcal{O})−(\overline{P}_1)−(\overline{P}_2)=2(\mathcal{O})−(\overline{P}_1)−(\overline{P}_2)+(v_1)+(v_2)=(P'_1)+(P'_2)−2(\mathcal{O})=D'$  と書きます。これは $D\sim D'$ を意味します。
$\mathrm{Deg}(\epsilon(D))=4$ の因子 $D$ を $\mathrm{Deg}(\epsilon(D'))=2\leq g$ の因子 $D'$ に減らしました。
なお、$D'$ の台（support）に含まれる点は、必ずしも $\mathbb{F}_q$ 上で定義されているわけではありません。
また、$P'_1$ と $P'_2$ を通る線 $\ell':y=\lambda x+\nu$ を実行するなどして、$D'$ をさらに削減しようとすると、この線がさらに $3$ つの場所で $E$ と交差するため、一般に機能しないことに注意してください。$\mathrm{Deg}(\epsilon(D''))=3>g$ で非簡約因子 $D''$ を作成します。

![](/images/pfb/figure3_5.png)

<details><summary>原文</summary>

Example 3.2.3 (Magma script).
Consider a general genus $3$ hyperelliptic curve $C_3:y^2=x^7+f_6x^6+\ldots +f_0;$ a typical depiction is given in Figure 3.6, with a vertically magnified Figure version in 3.7.
Consider the divisor $D=\sum^6_{i=1}((P_i)−(\mathcal{O}))\in\mathrm{Pic}^0(C_3)$ , the affine support of which is the red points in Figure 3.6.

We reduce $D$ by determining the other points of intersection between the quintic interpolator $\ell:y=a_5x^5+\ldots +a_0$ and $C_3$ , of which there are $4$: $\overline{P}_1,\ldots ,\overline{P}_4$ depicted in green on $C_3$ .
$(\ell)=0$ in the divisor class group so $\sum^6_{i=1}((P_i)−(\mathcal{O}))+\sum^4_{i=1}((\overline{P}_i)−(\mathcal{O}))=0$ , but the degree of the effective part of $\sum^4_{i=1}((\overline{P}_i)−(\mathcal{O}))$ is still larger than $g$, so obtaining the unique reduced divisor requires further reduction.
Namely, the cubic function $\overline{\ell}:y=\overline{a}_3x^3+\ldots +\overline{a}_0$ (depicted in green) interpolates the four green points and (when substituted into $C_3$ ) clearly intersects $C_3$ in another $3$ affine points, depicted in blue.
Thus, $\sum^4_{i=1}((\overline{P}_i)−(\mathcal{O}))+\sum^3_{i=1}((P'_i)−(\mathcal{O}))=0$ , which means that $D\sim D'=\sum^3_{i=1}((P'_i)−(\mathcal{O}))$ in the divisor class group, and $D'$ is the unique representative of $D$ since $\mathrm{Deg}(\epsilon(D'))=3\leq g$ .

</details>

例 3.2.3 (Magma script)
一般的な種数 $3$ の超楕円曲線 $C_3:y^2=x^7+f_6x^6+\ldots +f_0$ を考えてみましょう。 典型的な図を図 3.6 に、垂直方向に拡大した図を 図3.7 に示します。
因子 $D=\sum^6_{i=1}((P_i)−(\mathcal{O}))\in\mathrm{Pic}^0(C_3)$ を考えてみましょう。そのアフィン台は図 3.6 の赤い点です。

5 次補間 $\ell:y=a_5x^5+\ldots +a_0$ と $C_3$ の間の他の交点を決定することにより、$D$ を減らします。そのうちの $4$ つは、$C_3$ に緑で示されている $\overline{P}_1,\ldots ,\overline{P}_4$ です。
$(\ell)=0$ で因子類群では $\sum^6_{i=1}((P_i)−(\mathcal{O}))+\sum^4_{i=1}((\overline{P}_i)−(\mathcal{O}))=0$ であるが、$\sum^4_{i=1}((\overline{P}_i)−(\mathcal{O}))$ の有効部の次数はまだ $g$ よりも大きいため、一意の簡約因子を取得するには、さらに簡約する必要があります。
つまり、3 次関数  $\overline{\ell}:y=\overline{a}_3x^3+\ldots +\overline{a}_0$ (緑色で示されている) は 4 つの緑色の点を補間し、($C_3$ に代入されると) 青で示されている別の $3$ つのアフィン点で $C_3$ と明確に交差します。
したがって、 $\sum^4_{i=1}((\overline{P}_i)−(\mathcal{O}))+\sum^3_{i=1}((P'_i)−(\mathcal{O}))=0$ 、つまり、 $D\sim D'=\sum^3_{i=1}((P'_i)−(\mathcal{O}))$ であり、 $\mathrm{Deg}(\epsilon(D'))=3\leq g$ であるため、$D'$ は $D$ の一意の代表です。

![](/images/pfb/figure3_6-3_7.png)

# まとめ（Conclusion）

<details><summary>原文</summary>
</details>



