---
title: "初心者のためのペアリング 2"
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

# 第2章（Chapter 2）
# 暗号群としての楕円曲線（Elliptic curves as cryptographic groups）

<details><summary>原文</summary>

The purpose of this chapter is to introduce elliptic curves as they are used in cryptography.
Put simply, an elliptic curve is an abstract type of group.
Perhaps a newcomer will find this abstractness apparent immediately when we insist that to understand elliptic curve groups in cryptography, the reader should be familiar with the basics of finite fields $\mathbb{F}_q$ .
This is because, more generally, elliptic curves are groups which are defined on top of (over) fields.
Even though elliptic curve groups permit only one binary operation (the so called group law), the operation itself is computed within the underlying field, which by definition permits two operations (and their inverses).
For a general field $K$, the group elements of an elliptic curve $E$ are points whose $(x,y)$ coordinates come from $K$ (the algebraic closure of $K$ ), and which satisfy the (affine) curve equation for $E$ , given as

$$
E : y^2 + a_1xy + a_3y = x^3 + a_2x^2 + a_4x + a_6, \tag{2.1}
$$ 

where $a_1,\ldots ,a_6\in K$.
Equation (2.1) is called the general Weierstrass equationbfor elliptic curves.
Aside from all the $(x,y)\in K$ solutions to the equation above, there is one extra point which can not be defined using the affine equation, but which must be included to complete the group definition.
This point is called the point at infinity, which we denote by $\mathcal{O}$, and we will define it properly in a moment.

</details>

本章の目的は、暗号技術に使用される楕円曲線を紹介することです。
楕円曲線は、簡単に言えば抽象的な群の一種です。
暗号における楕円曲線群を理解するためには、有限体 $\mathbb{F}_q$ の基礎に精通している必要があると主張すると、おそらく初心者はすぐにこの抽象性を理解することができるだろう。
これは、楕円曲線が、より一般的には、体の上に定義される群であるためである。
楕円曲線群は 1 つの二項演算 (いわゆる群法則) のみを許可しますが、演算自体は基本となる体で計算され、定義により 2 つの演算 (およびその逆) が許可されます。
一般の体 $K$ に対して、楕円曲線 $E$ の群要素は、$(x,y)$ 座標が $K$（ $K$ の代数的閉包）に由来し、$E$ の（アフィン）曲線方程式を満たす点で、次のように与えられる。

$$
E : y^2 + a_1xy + a_3y = x^3 + a_2x^2 + a_4x + a_6, \tag{2.1}
$$ 

ここで $a_1,\ldots ,a_6\in K$
式（2.1）は楕円曲線に対する一般的なWeierstrass方程式と呼ばれます。
上の方程式のすべての解 $(x,y)\in K$ とは別に、アフィン方程式では定義できないが、群の定義を完成させるために含まれなければならない点が1つある。
この点を無限遠点と呼び、$\mathcal{O}$ と表記し、これから正しく定義します。

<details><summary>原文</summary>

If $a_1,\ldots ,a_6\in K$ , then we say $E$ is defined over $K$ , and write this as $E/K$ (the same goes for any extension field $L$ of $K$ ).
Before we go any further, we make a convenient simplification of the general Weierstrass equation.
If the field characteristic is not 2 or 3, then divisions by 2 and 3 in $K$ permit the substitutions $y\mapsto (y−a_1x−a_3)/2$ to give $E:y^2=4x^3+b_2x^2+2b_4x+b_6$ , and then $(x,y)\mapsto (\frac{x−3b_2}{36},\frac{y}{108})$ , which (upon appropriate rescaling) yields the following simplified equation.

$$
E:y^2=x^3+ax+b. \tag{2.2}
$$

Equation (2.2) is called the short Weierstrass equation for elliptic curves, and will be used all the way through this text. 
Namely, we will always be working over large prime fields, where the short Weierstrass equation covers all possible isomorphism classes of elliptic curves, so the curves we use will always be an instance of (2.2).

</details>

$a_1,\ldots ,a_6\in K$  ならば $E$ は $K$ 上で定義されているといい、これを $E/K$ と書く（$K$ の任意の拡大体 $L$ についても同様）。
その前に、一般的なWeierstrass方程式を簡単に簡略化しておく。
体の特性が 2 または 3 でない場合、$K$ を 2 および 3 で割ると、$y\mapsto (y-a_1x-a_3)/2$ の代入で $Ey^2=4x^3+b_2x^2+2b_4x+b_6$ が得られる。そして $(x,y)\mapsto (\frac{x-3b_2}{36},\frac{y}{108})$ （適切にリスケールすると）以下の簡便な式が得られます.

$$
E:y^2=x^3+ax+b. \tag{2.2}
$$

式(2.2)は楕円曲線に対する短いWeierstrass方程式と呼ばれ、このテキストではずっとこの方程式を用います。
すなわち、我々は常に大きな素数体上で活動し、短いWeierstrass方程式は楕円曲線のすべての可能な同型性クラスをカバーするので、我々が使う曲線は常に（2.2）のインスタンスとなる。

<details><summary>原文</summary>

Example 2.0.1 (Magma script).
$E/\mathbb{Q}:y^2=x^3−2$ is an elliptic curve.
Along with the point at infinity $\mathcal{O}$ (which we are still yet to define), the set of points over $\mathbb{Q}$ is written as $E(\mathbb{Q})$ , and is defined as $E(\mathbb{Q})=\{(x,y)\in \mathbb{A}^2(Q):y^2=x^3−2\}\cup \{\mathcal{O}\}$ .
The point $P=(x_P,y_P)=(3,5)$ lies in $E(Q)$ , as do $Q=(x_Q, y_Q)=(\frac{129}{100},\frac{−383}{1000})$ and $R=(x_R,y_R)=(\frac{164323}{29241}),(\frac{−66234835}{5000211})$ , so we can write $P,Q,R\in E(\mathbb{Q})$ .
We usually write $E$ to represent the group of points over the full algebraic closure, so for example, the point $S=(x_S,y_S)=(0,\sqrt{−2})\in E=E(\overline{\mathbb{Q}})$, but $S\notin E(\mathbb{Q})$ .
Soon we will be defining the binary group operation $\oplus$ on $E$ using rational formulas in the underlying field, so an active reader can return to this example with these formulas to verify that $R=P\oplus Q$ , where $x_R,y_R$ are computed from $x_P,y_P,x_Q,y_Q$ using additions and multiplications (also subtractions and inversions) in $\mathbb{Q}$ .
Furthermore, it can also be verified that $Q=P\oplus P$, so that $R=P\oplus P\oplus P$ ; we usually write these as $Q=[2]P$ and $R=[3]P$ , where $\underbrace{P\oplus P\cdots\oplus P}_{n}=[n]P$ in general.
To finish this example, we remark that if $(x',y')\in E$ , then $(x',−y')\in E$ (but is not distinct if $y'=0$ ), which is true for any elliptic curve in short Weierstrass form.

</details>

例 2.0.1 (Magma script)
$E/\mathbb{Q}:y^2=x^3-2$は楕円曲線である。
無限遠点 $\mathcal{O}$ と共に $\mathbb{Q}$ 上の点の集合を $E(\mathbb{Q})$ と書き、 $E(\mathbb{Q})=\{(x,y)\in \mathbb{A}^2(Q):y^2=x^3-2\}\cup \{\mathcal{O}\}$ と定義される。
点 $P=(x_P,y_P)=(3,5)$ は $E(\mathbb{Q})$ にあり、 $Q=(x_Q, y_Q)=(\frac{129}{100},\frac{-383}{1000})$ と同様に $R=(x_R, y_R)=(\frac{164323}{29241}),(\frac{-66234835}{5000211})$ , ですから、 $P,Q,R\in E(\mathbb{Q})$ と書くことができます。
通常、完全な代数的閉包上の点群を表すために $E$ と書くので、例えば点 $S=(x_S,y_S)=(0,\sqrt{−2})\in E=E(\overline{\mathbb{Q}})$ しかし $S\notin E(\mathbb{Q})$
すぐに、$E$ 上のバイナリ群演算 $\oplus$ を基本となる体の有理式を使って定義する予定なので、積極的な読者はこの例題に戻って、$R=P\oplus Q$ 、ここで $x_R,y_R$ は $x_P,y_P,x_Q,y_Q$ で加算と乗算（減算と逆数も）を使って計算されているかを確認できるだろう。
さらに、$Q=P\oplus P$ であることから $R=P\oplus P$ であることも検証できる。通常、これらを $Q=[2]P$, $R=[3]P$ と書き、 $\underbrace{P\oplus P\cdots\oplus P}_{n}=[n]P$ が一般的である。
最後に、もし $(x',y')\in E$ ならば $(x',-y')\in E$ (ただし $y'=0$ ならば区別できない) であり、これは短いWeierstrass 形式の楕円曲線に対して正しいことを述べておく。

<details><summary>原文</summary>

Example 2.0.2 (Magma script). 
$E/\mathbb{F}_{11}:y^2=x^3+4x+3$ is an elliptic curve.
$E(\mathbb{F}_{11})$ has 14 points: $(0,5),(0,6),(3,3),(3,8),(5,4),(5,7),(6,1),(6,10),(7,0),(9,3),(9,8),(10,3),(10,8),$ not forgetting the point at infinity $\mathcal{O}$ .
Notice that all but two points come in pairs $(x',y')$ and $(x',−y')$ , the exceptions being $(x',y')=(7,0)$ (since $y'=−y'=0$ ) and $\mathcal{O}$ .
If we form the quadratic extension $\mathbb{F}_{q^2}=\mathbb{F}_q(i)$ with $i^2+1=0$ , then considering $E$ over $\mathbb{F}_{q^2}$ will allow many more solutions, and give many more points: namely, $\#E(\mathbb{F}_{q^2})=140$ .
In addition to the points in $E(\mathbb{F}_q),E(\mathbb{F}_{q^2})$ will also contain those points with $x$-coordinates in $\mathbb{F}_q$ that did not give $x^3+4x+3$ as a quadratic residue in $\mathbb{F}_q$ (but necessarily do in $\mathbb{F}_{q^2}$ ), and many more with both coordinates in $\mathbb{F}_{q^2}\backslash\mathbb{F}_q$ .
Examples of both such points are $(2,5i)$ and $(2i+10,7i+2)$ respectively.
It is not a coincidence that $\#E(\mathbb{F}_q)|\#E(\mathbb{F}_{q^2})$ , since $E(\mathbb{F}_q)$ is a subgroup of $E(\mathbb{F}_{q^2})$ .

</details>

例 2.0.2（Magma script）
$E/\mathbb{F}_{11}:y^2=x^3+4x+3$ は楕円曲線である。
$E(\mathbb{F}_{11})$ は、14点: $(0,5),(0,6),(3,3),(3,8),(5,4),(5,7),(6,1),(6,10),(7,0),(9,3),(9,8),(10,3),(10,8),$ そして無限遠点 $\mathcal{O}$ を忘れてはいけない。
例外は $(x',y')=(7,0)$ ( $y'=-y'=0$ なので) $\mathcal{O}$ であることに注意してください。
$i^2+1=0$ で二次拡大  $\mathbb{F}_{q^2}=\mathbb{F}_q(i)$ を形成すると、$\mathbb{F}_{q^2}$ 上の $E$ でさらに多くの解を考えられ、さらに多くの点が得られる。 $\#E(\mathbb{F}_{q^2})=140$
$E(\mathbb{F}_q),E(\mathbb{F}_{q^2})$ の点以外にも、$x$ 座標が $\mathbb{F}_q$ で、$x^3+4x+3$ が二次剰余にならない点（ただし $\mathbb{F}_{q^2}$ では必ずそうなる）が含まれることになり、両方の座標が $\mathbb{F}_{q^2}\backslash\mathbb{F}_q$ になるものがたくさんあります。
このような点の例として、それぞれ $(2,5i)$ と $(2i+10,7i+2)$ がある。
$E(\mathbb{F}_q)$ は $E(\mathbb{F}_{q^2})$ の部分群なので、 $\#E(\mathbb{F}_q)|\#E(\mathbb{F}_{q^2})$ は偶然の一致ではありません。

<details><summary>原文</summary>

Not every tuple $(a,b)\in K\times K$ gives rise to the curve given by $f(x,y)=y^2−(x^3+ax+b)=0$ being an elliptic curve.
If there exists $P=(x_P,y_P)$ on $f$ such that both partial derivatives $\frac{\partial f}{\partial x}$ and $\frac{\partial f}{\partial y}$ vanish simultaneously at $P$ , then $P$ is called a singular point and $f$ is also deemed singular.
Conversely, if no such point exists, $f$ is called non-singular, or smooth, and is then an elliptic curve.
It is easy enough to show that a singularity occurs if and only if $4a^3+27b^2=0$ (see [Sil09, Ch. III.1, Prop. 1.4]), so as long as $4a^3 + 27b^2\ne 0$ in $K$ , then $E/K:y^2=x^3+ax+b$ is an elliptic curve.

In cryptography we only ever instantiate elliptic curves defined over finite fields, but it is often conceptually helpful to view graphs of elliptic curves over $\mathbb{R}$ .
We illustrate the difference between singular and non-singular (smooth) elliptic curves in Figures 2.1-2.4.

</details>

すべてのタプル $(a,b)\in K\times K$ が $f(x,y)=y^2-(x^3+ax+b)=0$ で与えられる曲線を作るわけではありません．
$f$ 上の $P=(x_P,y_P)$ が存在し、偏導関数 $\frac{\partial f}{\partial x}$ と $\frac{\partial f}{\partial y}$ が共に $P$ で同時に消える場合、$P$ は特異点と呼ばれ、$f$ も特異と見なされます。
逆に、そのような点が存在しない場合、$f$は非特異、または平滑と呼ばれ、楕円曲線となる。
$4a^3+27b^2=0$ (see [Sil09, Ch. III.1, Prop. 1.4]) のとき、特異が発生することを示すのは簡単です、$K$ 上で $4a^3 + 27b^2\ne 0$ である限り、$E/K:y^2=x^3+ax+b$ は楕円曲線です。

暗号技術では、有限体上で定義された楕円曲線しかインスタンス化しないが、$\mathbb{R}$ 上の楕円曲線のグラフを見ることは概念的に有用なことが多い。
図2.1-2.4は、特異楕円曲線と非特異楕円曲線（滑らかな楕円曲線）の違いを示している。

![](/images/pfb/figure2_1-2_4.png)


## 2.1 群律：弦と接線の法則（The group law: the chord-and-tangent rule）

<details><summary>原文</summary>

We now turn to describing the elliptic curve group law, and it is here that viewing pictures of elliptic curves over $\mathbb{R}$  is especially instructive.
We start with a less formal description until we define the role of the point at infinity $\mathcal{O}$ .
The group law exploits the fact that, over any field, a line (a degree one equation in $x$ and $y$ ) intersects a cubic curve (a degree three equation in $x$ and $y$ ) in three places (this is a special case of a more general theorem due to Bezout [Har77, I.7.8]).
Namely, if we run a line $\ell:y=\lambda x+\nu$ between two points $P=(x_P,y_P)$ and $Q=(x_Q,y_Q)$ on $E$ , then substituting this line into $E:y^2=x^3+ax+b$ will give a cubic polynomial in $x$ , the roots of which are the $x$-coordinates of the three points of intersection between $\ell$ and $E$ .
Knowing the two roots ( $x_P$ and $x_Q$ ) allows us to determine a unique third root that corresponds to the third and only other point in the affine intersection $\ell\cap E$ , which we denote by $\ominus R$ (the reason will become clear in a moment).
The point $\ominus R$ is then “flipped” over the $x$-axis to the point $R$ .
In general, the elliptic curve composition law $\oplus$ is defined by this process, namely  $R=P\oplus Q$ .
When computing $R=P\oplus Q$ , the line $\ell$ is computed as the tangent to $E$ at $P$ .
That is, the derivatives of $\ell$ and $E$ are matched at $P$ , so (counting multiplicities) $\ell$ intersects $E$ “twice” at $P$ .
Figures 2.5 and 2.6 illustrate why this process is aptly named the chord-and-tangent rule.

</details>

ここで、楕円曲線群律を説明することになるが、$\mathbb{R}$ 上の楕円曲線の絵を見ることが特に有益である。
無限遠の点 $\mathcal{O}$ の役割を定義するまでは、あまり形式的でない記述から始めます。
群律は、任意の場において、直線（ $x$ と $y$ の1次方程式）が3次曲線（ $x$ と$y$ の3次方程式）と3箇所で交差するという事実を利用する（これはBezout [Har77, I.7.8]によるより一般的な定理の特殊な例である）。
すなわち、$E$ 上の2点 $P=(x_P,y_P),Q=(x_Q,y_Q)$ の間に直線 $\ell:y=\lambda x+\nu$ を走らせ、この直線を $E:y^2=x^3+ax+b$ に代入すると、$x$ の3次多項式が得られ、その根は $\ell$ と $E$ の交点3点のx座標である。
2つの根 ( $x_P$ と $x_Q$ ) がわかれば、アフィン交点 $\ell\cap E$ の3番目の、そして唯一の他の点に対応する唯一の第3の根を決定することができ、これを $\ominus R$ と呼ぶ（理由はすぐに明らかになる）。
点 $\ominus R$ は $x$ 軸で点 $R$ に "反転 "となる。
一般に、楕円曲線合成則 $\oplus$ は、この過程、すなわち $R=P\oplus Q$ によって定義される。
$R=P\oplus Q$ を計算するとき、線分 $\ell$ は $P$ における $E$ の接線として計算される。
つまり、$\ell$ と $E$ の導関数が $P$ で一致するので、（乗数を数えて） $\ell$ は $P$ で $E$ と「2回」交差している。
図2.5と図2.6は、このプロセスが弦と接線の法則と名づけられた理由を示している。

![](/images/pfb/figure2_5-2_6.png)

<details><summary>原文</summary>

Having loosely defined the general group operation, we can now (also loosely) define the role of the point at infinity  $\mathcal{O}$ .
To try and place it somewhere in the above diagrams, one can think of $\mathcal{O}$ as being a point that simultaneously sits infinitely high and infinitely low in the $y$ direction.
This allows us to informally conceptualise two properties of elliptic curve groups: firstly, that the point at infinity $\mathcal{O}$ plays the role of the identity of the group; and secondly, that the unique inverse of a point is its reflected image over the $x$-axis (e.g. the $\ominus R$’s in Figures 2.5 and 2.6 are the respective inverses of the $R$’s, and vice versa).
If we apply the process in the previous paragraph to compute R ⊕ (⊖R), we start by finding the vertical line that connects them (the dashed lines in Figures 2.5 and 2.6).
This line also intersects $E$ (twice) at the point at infinity  $\mathcal{O}$ , which is then reflected back onto itself, giving $R\oplus(\ominus R)=\mathcal{O}$ .
Thus, if we define the identity of the group to be $\mathcal{O}$ , then the inverse of any element $R=(x_R,y_R)$ is taken as $\ominus R=(x_R,-y_R)$ .

</details>

一般的な群演算を緩やかに定義したところで、今度は無限遠の点 $\mathcal{O}$ の役割を（これも緩やかに）定義することができる。
上の図のどこかに配置しようとすると、$\mathcal{O}$ は $y$ 方向に限りなく高く、限りなく低く位置する点であると考えることができる。
このことは、楕円曲線群の二つの性質を非公式に概念化することを可能にする。第一に、無限遠の点 $\mathcal{O}$ が群の恒等式の役割を果たすこと、第二に、点の唯一の逆数が $x$ 軸上のその反射像であること（例えば、図2.5と2.6の $\ominus R$ はそれぞれの逆数、逆は $R$ の逆数）。
また、この線は無限遠の点 $\mathcal{O}$ で $E$ と（2回）交差し、この点を自分自身に反射して $R\oplus(\ominus R)=\mathcal{O}$ とする。
したがって、群の恒等式を $\mathcal{O}$ と定義すると、任意の要素 $R=(x_R,y_R)$ の逆元は、$\ominus R=(x_R,-y_R)$ と取られることになる。

<details><summary>原文</summary>

Example 2.1.1 (Magma script). 
$E/R:y^2=x^3−2x$ is an elliptic curve.
The points $(−1,−1),(0,0)$ and $(2,2)$ are all on $E$ , and are also on the line $\ell :y=x$ .
Applying the technique described above to compute some example group law operations via the line $\ell$ , we have $(−1,−1)\oplus (0,0)=(2,−2), (2,2)\oplus (0,0)=(−1,1)$, and $(−1,−1)\oplus (2,2)=(0,0)$ .
All but four points come in pairs with their inverse (i.e. $(x',y')$ and $(x',−y')$ ); the exceptions being $(0,0),(\sqrt{2}, 0), (−\sqrt{2}, 0)$ (notice the vertical tangents when $y=0$ in these cases), and $\mathcal{O}$ , which are all their own inverse, e.g. $(0,0)=\ominus (0,0)$, so $(0,0)\oplus (0,0)=\mathcal{O}$ on $E$.
The tangent line $\ell'$ to $E$ at $(−1,−1)$ is $\ell' :y=−\frac{1}{2}x−\frac{3}{2}$ , and it intersects $E$ once more at $(\frac{9}{4},−\frac{21}{8})$ , which gives $(−1,−1)\oplus (−1,−1)=[2](−1,−1)=(\frac{9}{4},\frac{21}{8})$ .

</details>

例 2.1.1 (Magma script)
$E/R:y^2=x^3−2x$ は楕円曲線である。
点 $(−1,−1),(0,0)$ と $(2,2)$ はすべて $E$ 上にあり、直線 $\ell$ 上にもある。
上記の手法を応用して、直線 $\ell$ による群律演算の例をいくつか計算する。
$(−1,−1)\oplus (0,0)=(2,−2), (2,2)\oplus (0,0)=(−1,1)$ と$(−1,−1)\oplus (2,2)=(0,0)$
4 つの点のほとんどの点は、それらの逆数 (つまり、 $(x',y')$ と $(x',−y')$ ) とペアになります。 例外は $(0,0),(\sqrt{2}, 0), (−\sqrt{2}, 0)$ (これらの場合は $y=0$ のときの垂直接線に注意してください)、および $\mathcal{O}$ 、これらはすべて自身の逆です。 $(0,0)=\ominus (0,0)$ なので、$E$ 上で $(0,0)\oplus (0,0)=\mathcal{O}$ です。
$(−1,−1)$ における $\ell'$ から $E$ への接線 $\ell' :y=−\frac{1}{2}x−\frac{3}{2}$ であり、$(\frac{9}{4},−\frac{21}{8})$ でもう一度 $E$ と交差し、$(−1,−1)\oplus (−1,−1)=[2](−1,−1)=(\frac{9}{4},\frac{21}{8})$ が得られます。

<details><summary>原文</summary>

Example 2.1.2 (Magma script).
In this example we consider the same curve equation as the last example, but this time over a small finite field, namely $E/\mathbb{F}_{11}:y^2=x^3−2x$ .
Rational points are injected naturally across to the finite field case (as long as there is no conflict with the characteristic), so we can immediately find the points $(0, 0), (2, 2)$ and $(−1, −1) = (10, 10)$ (and their inverses) in Figure 2.9.
In this case, consider performing the group law operation between the (blue) points $(5, 7)$ and $(8, 10)$ .
The line $\ell$ that joins them is $y=x+2$ , which intersects $E$ once more at $(10, 1)$.
Negating the $y$-coordinate finds the other point on the dashed line, and gives $(5,7)\oplus (8, 10)=(10,10)$ .

Example 2.1.2 is also intended to justify why, although (in cryptography) we only ever use elliptic curves over finite fields, we often opt to illustrate the group law by drawing the continuous pictures of curves over $\mathbb{R}$ .

</details>

例 2.1.2 (Magma script).
この例では、前回の例と同じ曲線方程式を考えますが、今回は小さな有限体上、つまり $E/\mathbb{F}_{11}:y^2=x^3−2x$ を対象とします。
有理点は（特性との衝突がない限り）有限体の場合にも自然に渡って注入されるので、図2.9の点 $(0,0),(2,2),(-1,-1)=(10,10)$ (とその逆) を直ちに見つけることができます。
ここで、（青）点 $(5,7)$ と $(8,10)$ の間で群法演算を行うことを考える。
これらを結ぶ直線 $\ell$ は $y=x+2$ であり、$(10, 1)$ で $E$ ともう一回交差している。
$y$ 座標を否定すると、破線上のもう一つの点が見つかり、 $(5,7)\oplus (8, 10)=(10,10)$ が得られる。

また、例題2.1.2は、（暗号技術では）有限体上の楕円曲線しか使わないのに、$\mathbb{R}$ 上の曲線の連続画を描いて群律を説明することが多い理由を説明するためのものです。

![](/images/pfb/figure2_7-2_9.png)


2.1.1 射影空間における無限遠点（The point at infinity in projective space）

<details><summary>原文</summary>

We now focus our attention on giving a more formal definition for the point at infinity.
So far we have been describing elliptic curves in affine space as a set of affine points together with the point at infinity: $E=\{(x,y)\in \mathbb{A}^2(\overline{K}):y^2=x^3+ax+b\} \cup \{\mathcal{O}\}$ .
In general, a more precise way to unify (or include) points at infinity with the affine points is to work in projective space: essentially, instead of working with points in $n$-space, we work with lines that pass through the origin in $(n+1)$-space.
For our purposes, this means our affine points in 2-space become lines in 3-space, namely that $(x,y)\in \mathbb{A}^2(\overline{K})$ corresponds to the line defined by all points of the form $(\lambda x,\lambda y,\lambda)\in \mathbb{P}^2(K)$ , where $\lambda\in \overline{K}^∗$.
That is, $\mathbb{P}^2$ is $\mathbb{A}^3\backslash \{(0,0,0)\}$ modulo the following congruence condition: $(x_1,y_1,z_1)\sim (x_2,y_2,z_2)$ if there exists $\lambda\in \overline{K}^∗$ such that $(x_1,y_1,z_1)=(\lambda x_2,\lambda y_2,\lambda z_2)$ .
Figure 2.10 illustrates the relationship between points in $\mathbb{A}^2$ with their congruence classes (lines) in $\mathbb{P}^2$ ; the lines in 3-space should also extend “downwards” into the region where $Z<0$ but we omitted this to give more simple pictures.
We reiterate that these lines do not include the point $(0,0,0)$.

</details>

ここで、無限遠点のより正式な定義を与えることに注目する。
これまで、アフィン空間における楕円曲線を、無限遠点とともにアフィン点の集合として記述してきました。$E=\{(x,y)\in \mathbb{A}^2(\overline{K}):y^2=x^3+ax+b\} \cup \{\mathcal{O}\}$
一般に、無限遠点をアフィン点に統一する（あるいは含める）より正確な方法は、射影空間での作業である。基本的に、$n$-空間での点の作業の代わりに、$(n+1)$-空間での原点を通過する線での作業を行う。
私たちの目的のために、これは 2 空間のアフィン点が 3 空間の線になることを意味します。つまり、$(x,y)\in \mathbb{A}^2(\overline{K})$ は $(\lambda x,\lambda y,\lambda)\in \mathbb{P}^2(\overline{K})$ の形式のすべての点によって定義される線に対応する、ここで $\lambda\in \overline{K}^∗$ 。
は、(x1, y1, z1) = (λx2, λy2, λz2)となるλ∈K＊が存在すれば
つまり、$\mathbb{P}^2$ は次の合同条件を法とする $\mathbb{A}^3\backslash \{(0,0,0)\}$ です: $(x_1,y_1,z_1)\sim (x_2,y_2,z_2)$ は$\lambda\in \overline{K}^∗$ が存在する場合 $(x_1,y_1,z_1)=(\lambda x_2,\lambda y_2,\lambda z_2)$ となる。
図 2.10 は、 $\mathbb{A}^2$ の点と $\mathbb{P}^2$ の合同クラス (線) との関係を示しています。 3 空間の線も $Z<0$ の領域まで「下向き」に延長する必要がありますが、より単純な図を示すためにこれを省略しました。
これらの線は点 $(0,0,0)$ を含まないことを再度確認する。

![](/images/pfb/figure2_10.png)

<details><summary>原文</summary>

We usually use capital letters and colons to denote a (representative of a) congruence class in projective coordinates, so that in general $(X:Y:Z)$ represents the set of all points on the “line” in $\mathbb{P}^2$ that correspond to $(x,y)\in\mathbb{A}^2$ .
There are many copies of $\mathbb{A}^2$ in $\mathbb{P}^2$ , but we traditionally map the affine point $(x,y)\in\mathbb{A}^2$ to projective space via the trivial inclusion $(x,y)\mapsto (x:y:1)$ , and for any $(X:Y:Z)\ne\mathcal{O}\in\mathbb{P}^2$ , we map back to $\mathbb{A}^2$ via $(X:Y:Z)\mapsto (X/Z,Y/Z)$ .
The point at infinity $\mathcal{O}$ is represented by $(0:1:0)$ in projective space (see the last diagram in Figure 2.10), for which we immediately note that the map back to A2 is ill-defined.

</details>

通常、射影座標における（代表的な）合同クラスを大文字とコロンを使って表すので、一般に $(X:Y:Z)$ は、 $\mathbb{P}^2$ の「線」上のすべての点のうち、$(x,y)\in\mathbb{A}^2$ に対応する点の集合を表していることになる。
$\mathbb{P}^2$ には $(x,y)\in\mathbb{A}^2$ のコピーが多数存在するが、伝統的にアフィン点  $(x,y)\in\mathbb{A}^2$ を自明な包含 $(x,y)\mapsto (x:y:1)$ を介して射影空間に写像し、任意の $(X:Y:Z)\ne\mathcal{O}\in\mathbb{P}^2$ に対して、$(X:Y:Z)\mapsto (X/Z,Y/Z)$ を介して $\mathbb{A}^2$ に写像する。
無限遠点 $\mathcal{O}$ の点は射影空間において $(0:1:0)$ で表され（図2.10の最後の図参照）、これについては $\mathbb{A}^2$ への写像が不定であることにすぐ気がつく。

<details><summary>原文</summary>

Example 2.1.3 (Magma script).
$E/\mathbb{R} :y^2=x^3+3x$ is an elliptic curve. 
$P=(3,6)\in\mathbb{A}^2(\overline{\mathbb{R}})$ is a point on $E$ .
In projective space, $P$ becomes $P=(3:6:1)\in\mathbb{P}^2(\overline{\mathbb{R}})$ , which represents all points in $(3\lambda , 6\lambda , \lambda )$ for $λ\in\overline{\mathbb{R}}\backslash\{0\}$ .
For example, the points $(12,24,4)$ , $(−3\sqrt{−1},−6\sqrt{−1},−1\sqrt{−1})$, $(3\sqrt{2},6\sqrt{2},\sqrt{2})$ in $\mathbb{A}^3(\overline{\mathbb{R}})$ are all equivalent (modulo the congruence condition) in $\mathbb{P}^2(\overline{\mathbb{R}})$ , where they are represented by $P$.
As usual, the point at infinity on $E$ is $\mathcal{O}=(0:1:0)$ .

</details>

例 2.1.3 (Magma script).
$E/\mathbb{R} :y^2=x^3+3x$ は楕円曲線である。
$P=(3,6)\in\mathbb{A}^2(\overline{\mathbb{R}})$ は $E$ 上の点である。
射影空間では、$P$ は $P=(3:6:1)\in\mathbb{P}^2(\overline{\mathbb{R}})$ となり、 $λ\in\overline{\mathbb{R}}\backslash\{0\}$ に対して $(3\lambda , 6\lambda , \lambda )$ のすべての点を表します。
例えば、 $\mathbb{A}^3(\overline{\mathbb{R}})$ の点 $(12,24,4)$ 、 $(−3\sqrt{−1}、−6\sqrt{−1}、−1\sqrt{−1})$、$(3\sqrt{2},6\sqrt{2},\sqrt{2})$ は、 $\mathbb{P}^2(\overline{\mathbb{R}})$ においてすべて等価（合同条件による）であり、それらは $P$ で表される。
通常、$E$ 上の無限遠点は $\mathcal{O}=(0:1:0)$ です。

<details><summary>原文</summary>

The way we define the collection of points in projective space is to homogenise $E:y^2=x^3+ax+b$ by making the substitution $x=X/Z$ and $y=Y/Z$ , and multiplying by $Z^3$ to clear the denominators, which gives

$$E_{\mathbb{P}}:Y^2Z=X^3+aXZ^2+bZ^3.\tag{2.3}$$

The set of points $(X, Y, Z)$ with coordinates in $\overline{K}$ that satisfies (2.3) is called the projective closure of $E$.
Notice that $(0,\lambda ,0)$ is in the projective closure for all $\lambda\in\overline{K^∗}$ , and that all such points cannot be mapped into $\mathbb{A}^2$ , justifying the representative of point at infinity being $\mathcal{O}=(0:1:0)$ .

</details>

射影空間の点の集まりを定義する方法は、$E:y^2=x^3+ax+b$ を $x=X/Z$ と $y=Y/Z$ の代入で均質化し、$Z^3$ を掛けて分母を消去すると、次のようになる。

$$E_{\mathbb{P}}:Y^2Z=X^3+aXZ^2+bZ^3.\tag{2.3}$$

$\overline{K}$ の座標が $(2.3)$ を満たす点 $(X, Y, Z)$ の集合は、$E$ の射影閉包と呼ぶ。
$(0,\lambda ,0)$ はすべての $\lambda\in\overline{K^∗}$ に対して射影閉包にあり、そのような点はすべて $\mathbb{A}^2$ に写像できないので、無限遠の点の代表は $\mathcal{O}=(0:1:0)$ となることに注意されたい。

<details><summary>原文</summary>

Example 2.1.4 (Magma script).
Consider $E/\mathbb{F}_{13}:y^2=x^3+5$ .
There are 15 affine points $(x,y)\in\mathbb{A}^2(\mathbb{F}_{13})$ on $E$ , which (with the point at infinity $\mathcal{O}$ ) gives $\#E(\mathbb{F}_{13})=16$ .
On the other hand, if we homogenise (or projectify) $E$ to give $P/\mathbb{F}_{13}:Y^2Z=X^3+5Z^3$ , then there are 16 classes $(X:Y:Z)\in\mathbb{P}^2(\mathbb{F}_{13}): (0 : 1 : 0), (2 : 0 : 1), (4 : 2 : 1), (4 : 11 : 1), (5 : 0 : 1), (6 : 0 : 1), (7 : 6 : 1), (7 : 7 : 1), (8 : 6 : 1), (8 : 7 : 1), (10 : 2 : 1), (10 : 11 : 1), (11 : 6 : 1), (11 : 7 : 1), (12 : 2 : 1), (12 : 11 : 1)$.
Each of these classes represents several points $(X,Y,Z)\in\mathbb{A}^3(\mathbb{F}_{13})$ whose coordinates satisfy $Y^2Z=X^3+5Z^3$ (there are actually 195 such points, but this is not important).
In fact, each class represents infinitely many points on $E_\mathbb{P}(\mathbb{F}_{13})$ .
Any reader that is familiar with Magma, or has been working through our examples with the accompanying Magma scripts, will recognise the representation of points as representatives in $\mathbb{P}^2$ .

</details>

例 2.1.4 (Magma script)
$E/\mathbb{F}_{13}:y^2=x^3+5$ を考える。
$E$ 上には15個のアフィン点 $(x,y)\in\mathbb{A}^2(\mathbb{F}_{13})$ があり、（無限遠の点を $\mathcal{O}$ として）$\#E(\mathbb{F}_{13})=16$ が得られる。
一方、$E$ を均質化（射影）して $P/\mathbb{F}_{13}:Y^2Z=X^3+5Z^3$ とすると、16クラス $(X:Y:Z)\in\mathbb{P}^2(\mathbb{F}_{13})$ が存在することになる。$(0 : 1 : 0), (2 : 0 : 1), (4 : 2 : 1), (4 : 11 : 1), (5 : 0 : 1), (6 : 0 : 1), (7 : 6 : 1), (7 : 7 : 1), (8 : 6 : 1), (8 : 7 : 1), (10 : 2 : 1), (10 : 11 : 1), (11 : 6 : 1), (11 : 7 : 1), (12 : 2 : 1), (12 : 11 : 1)$
これらのクラスのそれぞれは、座標が $Y^2Z=X^3+5Z^3$ を満たすいくつかの点 $(X,Y,Z)\in\mathbb{A}^3(\mathbb{F}_{13})$ を表します (実際には 195 のそのような点がありますが、これは重要ではありません)。
実際、各クラスは $E_\mathbb{P}(\mathbb{F}_{13})$ 上の無数の点を表します。
Magmaに詳しい読者や、Magmaのスクリプトを使った例題に取り組んだことのある読者なら、 $\mathbb{P}^2$ における点の表現が代表的であることはおわかりいただけると思います。

<details><summary>原文</summary>

The projective coordinates $(X,Y,Z)$ used to replace the affine coordinates $(x,y)$ above are called homogenous projective coordinates, because the projective version of the curve equation in $(2.3)$ is homogeneous.
These substitutions ( $x=X/Z,y=Y/Z$ ) are the most simple (and standard) way to obtain projective coordinates, but we are not restricted to this choice of substitution.
For example, many papers in ECC have explored more general substitutions of the form $x=X/Z^i$ and $y=Y/Z^j$ on various elliptic curves [BL07a].

</details>

上記のアフィン座標 $(x,y)$ を置換するのに用いた射影座標 $(X,Y,Z)$ は、 $(2.3)$ の曲線方程式の射影版が均質であることから、同次射影座標（homogenous projective coordinates）と呼ばれる。
これらの置き換え（ $x=X/Z,y=Y/Z$ ）は射影座標を得るための最も簡単な（そして標準的な）方法ですが、私たちはこの置き換えの選択に制限されるわけではありません。
例えば、ECCの多くの論文では、様々な楕円曲線上の $x=X/Z^i$ と $y=Y/Z^j$ という形式の、より一般的な置換が研究されています[BL07a]。

<details><summary>原文</summary>

Example 2.1.5 (Magma script).
Consider $E/\mathbb{F}_{41}:y^2=x^3+4x−1$ .
Using homogeneous coordinates gives rise to the projective equation $Y^2Z=X^3+4XZ^2−Z^3$ , with the point at infinity being $\mathcal{O}=(0:1:0)$ .
An alternative projection we can use is $x=X/Z$ and $y=Y/Z^2$ , which in this instance give the projective equation $Y^2=X^3Z+4XZ^3−Z^4$ , from which the point at infinity is seen (from putting $Z=0$ ) to be $\mathcal{O}=(1:0:0)$ .
Another commonly used coordinate system is Jacobian coordinates, which use the substitutions $x=X/Z^2$ and $y=Y/Z^3$ to give the projective equation $Y^2=X^3+4XZ^4−Z^6$ .
In this case, we substitute $Z=0$ to see that the point at infinity is defined by the line $\mathcal{O}=(\lambda^2:\lambda^3:0)\in\mathbb{P}_2(\mathbb{F}_{41})$ .

</details>

例 2.1.5 (Magma script)
$E/\mathbb{F}_{41}:y^2=x^3+4x−1$ を考える。
同次座標を用いると、射影方程式  $Y^2Z=X^3+4XZ^2−Z^3$ となり、無限遠点は $\mathcal{O}=(0:1:0)$ となる。
もう一つの射影は、$x=X/Z$ と $y=Y/Z^2$ で、この場合、射影方程式 $Y^2=X^3Z+4XZ^3−Z^4$ が与えられ、そこから無限遠の点は $\mathcal{O}=(1:0:0)$ （ $Z=0$ を置くことから ）であることがわかります。
もう 1 つの一般的に使用される座標系は、射影方程式 $Y^2=X^3+4XZ^4−Z^6$ を与えるために代入 $x=X/Z^2$ および $y=Y/Z^3$ を使用するヤコビアン座標です。
このとき、$Z=0$ を代入すると、無限遠の点が直線 $\mathcal{O}=(\lambda^2:\lambda^3:0)\in\mathbb{P}_2(\mathbb{F}_{41})$ となる。


### 2.1.2 群律演算のための明示公式の導出（Deriving explicit formulas for group law computations）

<details><summary>原文</summary>

We are now in a position to give explicit formulas for computing the elliptic curve group law.
The chord-and-tangent process that is summarised in Figures 2.5 and 2.6 allows a simple derivation of these formulas.
We derive the formulas in affine space, but will soon transfer them into projective space as well.
The derivation of the formulas for point additions $R=P\oplus Q$ and for point doublings $R=P\oplus P$ follow the same recipe, the main difference being in the calculation of the gradient $\lambda$ of the line $\ell :y=\lambda x+\nu$ that is used.
We will first derive the formulas for the addition $R=P\oplus Q$ in the general case, and will then make appropriate changes for the general doubling formulas.
By “general case”, we mean group law operations between points where neither point is $\mathcal{O}$ , and the points that are being added are not each inverses of one another; we will handle these special cases immediately after the general cases.
Referring back to Figure 2.5, the line $\ell :y=\lambda x+\nu$ that intersects $P=(x_P,y_P)$ and $Q= (x_Q,y_Q)$ has gradient $\lambda =(y_Q−y_P)/(x_Q−x_P)$ .
From here, $\nu$ can simply be calculated as either $\nu=y_P−\lambda x_P$ or $\nu =y_Q−\lambda x_Q$ , but in the literature we will often see an unbiased average of the two as $\nu =(y_Qx_P−y_Px_Q)/(x_P− x_Q)$ .
From here we substitute $\ell :y=\lambda x+\nu$ into $E:y^2=x^3+ax+b$ to find the third affine point of intersection, $\ominus R$, in $\ell\cap E$ .
Finding the coordinates of $\ominus R$ trivially reveals the coordinates of $R=(x_R,y_R)$ , since $\ominus R=(x_R,−y_R)$ ; the roots of the cubic that result will be $x_P$  ,$x_Q$ and $x_R$.
Namely,

$$
\begin{align*}
(x−x_P)(x−x_Q)(x−x_R)&=(x^3+ax+b)−(\lambda x+\nu)^2 \\
&=x^3−\lambda^2x^2+(a−2\lambda\nu)x+b−\nu^2.
\end{align*}
$$

</details>

これで、楕円曲線群律を計算するための明示的な公式を与えることができる状態になった。
図2.5 と 図2.6 に要約されている弦と接線の処理により、これらの式を簡単に導き出すことができます。
アフィン空間での公式を導出するが、射影空間にもすぐに移植する予定である。
点の加算 $R=P\oplus Q$ と点の倍算 $R=P\oplus P$ の公式の導出は同じ方法で行い、主な違いは使用する直線 $\ell :y=\lambda x+\nu$ の傾き  $\lambda$ の計算方法である。
まず、一般的な場合の加算 $R=P\oplus Q$ の公式を導出し、その後、一般的な倍算の公式を適当に変更することにする。
「一般的な場合」とは、どちらの点も $\mathcal{O}$ ではない点間の群法則演算を意味し、追加される点はそれぞれ互いに逆ではありません。 これらの特殊なケースは、一般的なケースの直後に処理します。
図2.5を参照して、 $P=(x_P,y_P)$ と $Q= (x_Q,y_Q)$ に交わる直線 $\ell :y=\lambda x+\nu$ は傾き $\lambda =(y_Q−y_P)/(x_Q−x_P)$ となります。
ここから $\nu$ は単純に $\nu=y_P−\lambda x_P$ または  $\nu =y_Q−\lambda x_Q$ として計算できるが、文献では $\nu =(y_Qx_P−y_Px_Q)/(x_P− x_Q)$ として２つの不偏平均を見ることが多い。
ここから、$\ell :y=\lambda x+\nu$ を $E:y^2=x^3+ax+b$ に代入すると、 $\ell\cap E$ における3番目のアフィン交点が求まる。
 $\ominus R$ の座標を求めると、$\ominus R=(x_R,−y_R)$ であるから、$R=(x_R,y_R)$ の座標は自明であり、結果として得られる三乗の根は $x_P$、$x_Q$、$x_R$ となる。
すなわち

$$
\begin{align*}
(x−x_P)(x−x_Q)(x−x_R)&=(x^3+ax+b)−(\lambda x+\nu)^2 \\
&=x^3−\lambda^2x^2+(a−2\lambda\nu)x+b−\nu^2.
\end{align*}
$$

<details><summary>原文</summary>

We only need to look at the coefficient of $x^2$  to determine xR, since the coefficient on the left hand side is $-(x_P+x_Q+x_R)$ .
From here, recovering the $y$-coordinate is simple, since $−y_R$ lies on $\ell$ , so

$$
x_R=\lambda^2−x_P−x_Q;\hspace{50pt}y_R=−(\lambda x_R+\nu).
$$

This finishes the description of addition in the general case.
When adding $P$ to itself (i.e. doubling $P$ – refer back to Figure 2.6), the line $\ell :y=\lambda x+\nu$ is the tangent to $E$ at $P$ .
Thus, its gradient $\lambda$ is the derivative function $dy/dx$ of $E$ , evaluated at $P$ .
To obtain $dy/dx$ , we differentiate the curve equation implicitly, as

$$
\begin{align*}
\frac{d}{dx}(y^2)&=\frac{d}{dx}(x^3+ax+b)\\
\frac{d}{dy}(y^2)\frac{dy}{dx}&=3x^2+a\\
\frac{dy}{dx}&=\frac{3x^2+a}{2y}.
\end{align*}
$$

Thus, $\lambda =\frac{dy}{dx}(P)=(3x^2_P+a)/(2y_P)$, and $\nu =y_P−\lambda x_P$ .
Again, we substitute $\ell$ into $E$ , but this time two of the roots of the resulting cubic are $x_P$ , so we obtain $x_R$ and $y_R$ as

$$
x_R=\lambda^2−2x_P;\hspace{50pt}y_R=−(\lambda x_R+\nu).
$$

This finishes the derivation of doubling formulas in the general case.
We now complete the group law description by looking at the special cases.
The point at infinity $\mathcal{O}$ is the identity, or neutral element, so any operation involving it is trivial. 
Otherwise, any operation between elements $P$ and $Q$ with different $x$-coordinates employs the general addition.
This leaves the remaining cases of $x_P=x_Q$ : (i) if $y_P=−y_Q$ , then $P$ and $Q$ are inverses of each other and $P\oplus Q=\mathcal{O}$ (note that this includes $y_P=y_Q=0$ ), and (ii) if $y_P=y_Q\ne 0$ , then $P=Q$ and we use the point doubling formulas.

</details>

左辺の係数は $-(x_P+x_Q+x_R)$ なので、$x^2$ の係数だけを見て $x_R$ を決定すればいいのです。
ここから $y$ 座標を求めるのは簡単で、$-y_R$ は $\ell$ 上に存在するので

$$
x_R=\lambda^2−x_P−x_Q;\hspace{50pt}y_R=−(\lambda x_R+\nu).
$$

以上で、一般的な場合の足し算の説明を終わります。
$P$ を自身に加算する（ $P$ の倍算 - 図2.6参照）とき、直線 $\ell :y=\lambda x+\nu$ は $P$ で $E$ の接線となる。
したがって、その傾き $\lambda$ は、$P$ で $E$ の微分関数 $dy/dx$ の値を求める。
$dy/dx$ を取得するには、次のように曲線方程式を暗黙的に微分します。

$$
\begin{align*}
\frac{d}{dx}(y^2)&=\frac{d}{dx}(x^3+ax+b)\\
\frac{d}{dy}(y^2)\frac{dy}{dx}&=3x^2+a\\
\frac{dy}{dx}&=\frac{3x^2+a}{2y}.
\end{align*}
$$

したがって、$\lambda =\frac{dy}{dx}(P)=(3x^2_P+a)/(2y_P)$ であり、$\nu =y_P−\lambda x_P$ である。
ここでも $E$ に $\ell$ を代入するが、今度はできた3乗根のうち2つが $x_P$ となるので、$x_R$ と $y_R$ が次のように求められる。

$$
x_R=\lambda^2−2x_P;\hspace{50pt}y_R=−(\lambda x_R+\nu).
$$

これで、一般的な場合の倍算の導出は終了です。
ここで、特殊なケースを見て、群律の説明を完成させます。
無限遠点 $\mathcal{O}$ は恒等要素、つまり中立要素であるため、それを含む操作は簡単です。
それ以外の場合、異なる $x$ 座標を持つ要素 $P$ と $Q$ の間の演算では、一般的な加算が使用されます。
これにより、$x_P=x_Q$ の残りのケースが残る。
(i) $y_P=-y_Q$ なら、$P$ と $Q$ は互いに逆であり、$P\oplus Q=\mathcal{O}$ ( $y_P=y_Q=0$ を含むことに注意) である。
(ii)  $y_P=y_Q\ne 0$ ならば $P=Q$ であり、点倍算の公式を用いる。

<details><summary>原文</summary>

Much of the literature concerning the elliptic curve group law tends to present the complete description in the previous paragraph using an “if-then-else” style algorithm, where the “if” statements distinguish which of the above scenarios we are in.
In optimised cryptographic implementations however, this is not the way that the group law operation is coded.
This is because the groups we use are so large that the chances of running into a special case (that is not general doubling or general addition) randomly is negligible.
Moreover, the parameters are usually chosen so that we are guaranteed not to run into these cases.
In this light then, it will soon become clear that the major operations we are concerned with are point additions $R=P\oplus Q$ and point doublings $R=P\oplus P$ , the formulas for which are summarised in $(2.4)$ and $(2.5)$ respectively.

(Affine addition)
$$
\begin{align*}
\lambda =\frac{y_Q−y_P}{x_Q−x_P};\hspace{50pt}\nu =y_P−\lambda x_P; \\
(x_P,y_P)\oplus (x_Q,y_Q)=(x_R,y_R)=(\lambda^2−x_P−x_Q,−(\lambda x_R+\nu)).\tag{2.4}\\
\end{align*}
$$

(Affine doubling)
$$
\begin{align*}
\lambda =\frac{3x^2_P+a}{2y_P};\hspace{50pt} \nu =y_P−\lambda x_P;\\
[2](x_P,y_P)=(x_P,y_P)\oplus (x_P,y_P)=(x_R,y_R)=(\lambda^2−2x_P,−(\lambda x_R+\nu)).\tag{2.5}
\end{align*}
$$

</details>

楕円曲線群律に関する多くの文献は、前項の完全な記述を「if-then-else」形式のアルゴリズムで表現する傾向があり、「if」文で上記のシナリオのどれに入るかを区別しています。
しかし、最適化された暗号実装では、群律演算はこのようにコード化されない。
これは、使用する群が非常に大きいため、特殊なケース（一般的な倍算でも一般的な加算でもない）にランダムに遭遇する可能性が無視できるためです。
また、パラメータは通常、このようなケースに遭遇しないよう に選択されている。
このように考えると、私たちが扱う主要な操作は、点の加算 $R=P\oplus Q$ と点の倍算 $R=P\oplus P$ であり、その公式はそれぞれ $(2.4)$ と $(2.5)$ でまとめられることがすぐにわかる。

(アフィン加算)
$$
\begin{align*}
\lambda =\frac{y_Q−y_P}{x_Q−x_P};\hspace{50pt}\nu =y_P−\lambda x_P; \\
(x_P,y_P)\oplus (x_Q,y_Q)=(x_R,y_R)=(\lambda^2−x_P−x_Q,−(\lambda x_R+\nu)).\tag{2.4}\\
\end{align*}
$$

(アフィン倍算)
$$
\begin{align*}
\lambda =\frac{3x^2_P+a}{2y_P};\hspace{50pt} \nu =y_P−\lambda x_P;\\
[2](x_P,y_P)=(x_P,y_P)\oplus (x_P,y_P)=(x_R,y_R)=(\lambda^2−2x_P,−(\lambda x_R+\nu)).\tag{2.5}
\end{align*}
$$

<details><summary>原文</summary>

Example 2.1.6 (Magma script).
We revisit the curve $E/\mathbb{Q} :y^2=x^3−2$ from Example 2.0.1 to verify the group law calculations that were stated.
We start with the point doubling of $P=(x_P,y_P)=(3,5)$ , to compute $Q=[2]P=P\oplus P$ using $(2.5)$ .
Here, $\lambda =\frac{3x^2_P+a}{2y_P}=\frac{3\cdot 3^2+0}{2\cdot 5}=\frac{27}{10}$ , from which $\nu$ follows as $\nu =y_P−\lambda x_P=5−\frac{27}{10}\cdot 3=−\frac{31}{10}$ .
Thus, $x_Q=\lambda^2−2x_P=(\frac{27}{10})^2−2\cdot 3=\frac{129}{100}$ , and $y_Q=−(\lambda x_Q+\nu)=−(\frac{27}{10}\cdot\frac{129}{100}−\frac{31}{10})=−\frac{383}{1000}$ , giving $(x_Q,y_Q)=[2](x_P,y_P)=(\frac{129}{100},−\frac{383}{1000})$ .
For the addition $R=P\oplus Q$ , we use the formulas in $(2.4)$, so $\lambda =\frac{y_Q−y_P}{x_Q−x_P}=(−\frac{383}{1000}−5)/(\frac{129}{100}−3)=\frac{5383}{1710}$ , and $\nu =y_P−\lambda x_P=5−\frac{5383}{1710}\cdot 3=−\frac{2533}{570}$ .
Thus, $x_R=\lambda^2−x_P−x_Q=(\frac{5383}{1710})^2−3−\frac{129}{100}=\frac{164323}{29241}$ , and $y_R=\lambda x_R+\nu =\frac{5383}{1710}\cdot\frac{164323}{29241}−\frac{2533}{570}=−\frac{66234835}{5000211}$ , so $(x_R,y_R)=(\frac{164323}{29241},−\frac{66234835}{5000211})$ .
Since $Q=[2]P=P\oplus P$, then $R=P\oplus Q=[3]P$ . 
We finish this example with a remark that further justifies the use of finite fields as the underlying fields in cryptography.
It is not too painful to show that $P=(3,5)$ and $\ominus P=(3,−5)$ are the only integral points on $E$ [Sil09, Ch. IX, Prop. 7.1(b)], or that $E(\mathbb{Q})$ is actually infinite cyclic [Sil09, Ch. IX, Remark 7.1.1], meaning that among infinitely many rational points, only two have integer coordinates.
Besides the infinite nature of $E(\mathbb{Q})$ (the lack of any finite subgroups is not useful in the context of discrete logarithm based cryptographic groups), observing the growing size of the numerators and denominators in $[n]P$, even for very small values of $n$ , shows why using $E(\mathbb{Q})$ would be impractical.
Using Magma, we can see that the denominator of the $y$-coordinate of $[10]P$ is 290 bits, whilst the denominator in $[100]P$ is 29201 bits, which agrees with the group law formulas in $(2.4)$ and $(2.5)$ that suggest that denominators of successive scalar multiples of $P$ would grow quadratically; even Magma takes its time computing $[1000]P$, whose denominator is 2920540 bits, and Magma could not handle the computation of $[10000]P$.
In Figure 2.11 we plot multiples of $P=(3,5)$ that fall within the domain $x<6$ .

</details>

例 2.1.6 (Magma script)
例 2.0.1 の曲線 $E/\mathbb{Q} :y^2=x^3−2$ を再確認し、記載された群律計算を確認します。
まず、$P=(x_P,y_P)=(3,5)$ の点倍算から、$(2.5)$ を用いて $Q=[2]P=P\oplus P$ を計算する。
ここで、$\lambda =\frac{3x^2_P+a}{2y_P}=\frac{3\cdot 3^2+0}{2\cdot 5}=\frac{27}{10}$ であり、そこから $\nu$ は $\nu =y_P−\lambda x_P=5−\frac{27}{10}\cdot 3=−\frac{31}{10}$ となる。
したがって、$x_Q=\lambda^2−2x_P=(\frac{27}{10})^2−2\cdot 3=\frac{129}{100}$ と $y_Q=−(\lambda x_Q+\nu)=−(\frac{27}{10}\cdot\frac{129}{100}−\frac{31}{10})=−\frac{383}{1000}$ から $(x_Q,y_Q)=[2](x_P,y_P)=(\frac{129}{100},−\frac{383}{1000})$ が得られる。
加算 $R=P\oplus Q$ については、$(2.4)$ の式を使うので、$\lambda =\frac{y_Q−y_P}{x_Q−x_P}=(−\frac{383}{1000}−5)/(\frac{129}{100}−3)=\frac{5383}{1710}$ 、$\nu =y_P−\lambda x_P=5−\frac{5383}{1710}\cdot 3=−\frac{2533}{570}$ となります。
したがって、$x_R=\lambda^2−x_P−x_Q=(\frac{5383}{1710})^2−3−\frac{129}{100}=\frac{164323}{29241}$ と $y_R=\lambda x_R+\nu =\frac{5383}{1710}\cdot\frac{164323}{29241}−\frac{2533}{570}=−\frac{66234835}{5000211}$ から $(x_R,y_R)=(\frac{164323}{29241},−\frac{66234835}{5000211})$ が得られる。
$Q = [2]P = P \oplus P$ なので、$R = P \oplus Q = [3]P$ となります。
この例は、暗号化の基礎となる体として有限体を使用することをさらに正当化する発言で締めくくります。
$P=(3,5)$ と $\ominus P=(3,−5)$ が $E$ 上の唯一の積分点 [Sil09, Ch. IX, Prop 7.1(b)] であること、あるいは $E(\mathbb{Q})$ が実は無限循環 [Sil09, Ch. IX, Remark 7.1.1] つまり無限個の有理点のうち整数座標を持つものは2つだけだということを示すのはそれほど苦痛ではないでしょう。
$E(\mathbb{Q})$ の無限性（有限の部分群がないことは離散対数ベースの暗号群の文脈では役に立たない）に加え、$n$ が非常に小さな値でも $[n]P$ の分子と分母が大きくなることを観察すると、$E(\mathbb{Q})$ を使うことがなぜ非実用的であるかがわかる。
Magma を使用すると、$[10]P$ の $y$ 座標の分母が 290 ビットであるのに対し、$[100]P$ の分母は 29201 ビットであることがわかります。これは、$(2.4)$ および $(2.5)$ の群律式と一致します。 これは、$P$ の連続するスカラー倍数の分母が 2 次的に増加することを示唆しています。 Magmaでさえ、分母が 2920540 ビットである $[1000]P$ の計算に時間がかかり、Magmaは $[10000]P$ の計算を処理できませんでした。
図2.11では、領域 $x<6$ に収まる $P=(3,5)$ の倍数をプロットしています。

![](/images/pfb/figure2_11.png)

<details><summary>原文</summary>

From now on we will only be working with elliptic curves over finite fields.
We start with a simple example of basic group law computations on $E(\mathbb{F}_q)$ to summarise the discussion up until this point.

</details>

これからは、有限体上の楕円曲線だけを扱うことにします。
まず、$E(\mathbb{F}_q)$ 上の基本的な群律演算の簡単な例で、ここまでの議論をまとめる。

<details><summary>原文</summary>

Example 2.1.7 (Magma script).
$E/\mathbb{F}_{23}:y^2=x^3+5x+7$ is an elliptic curve, and both $P=(x_P,y_P)=(2,5)$ and $Q=(x_Q,y_Q)=(12,1)$ are on $E$ .
Using the affine point addition formulas in $(2.4)$, we find $R=P\oplus Q$ by first computing $\lambda =\frac{y_Q−y_P}{x_Q−x_P}=\frac{1−5}{12−2}=−4\cdot 10^{−1}=−28=18$ , from which $\nu$ follows as $\nu =y_P−\lambda x_P=5−18\cdot 2=−31=15$ , so $\ell :y=18x+15$ is the line running through $P$ and $Q$ .
We then compute $(x_R,y_R)=(\lambda^2−x_P−x_Q,−(\lambda x_R+\nu ))$ , so $x_R=18^2−2−12=11$ and $y_R=−(18\cdot 11+15)=17$ , meaning $R=(11,17)$ .
Applying $(2.5)$ to compute $S=[2]P$ gives $\lambda'=\frac{3x^2_P+a}{2y_P}=\frac{3\cdot 2^2+5}{2\cdot 5}=17\cdot 10^{−1}=17\cdot 7=4$ , and $\nu'$ follows as $\nu'=y_P−\lambda' x_P=5−4\cdot 2=20$ , so $\ell':y=4x+20$ is the tangent line that intersects $E$ with multiplicity two at $P$.
We then compute $(x_S,y_S)=(\lambda'^2−2x_P,−(\lambda'x_S+\nu'))$ , so $x_S=4^2−2\cdot 2=12$ and $y_S= −(4\cdot 12+20)=−68=1$ , meaning $S=(12,1)$ .

</details>

例 2.1.7 (Magma script)
$E/\mathbb{F}_{23}:y^2=x^3+5x+7$ は楕円曲線であり、$P=(x_P,y_P)=(2,5)$ と$Q=(x_Q,y_Q)=(12,1)$ はともに $E$ 上にある。
アフィン点加算の公式 $(2.4)$ を使用すると、$R=P\oplus Q$ はまず $\lambda =\frac{y_Q−y_P}{x_Q−x_P}=\frac{1−5}{12−2}=−4\cdot 10^{−1}=−28=18$ を計算する、そこから $\nu$ は次となる $\nu =y_P−\lambda x_P=5−18\cdot 2=−31=15$ したがって、$P$ と $Q$ を通る直線は、$\ell :y=18x+15$ となる。
$(x_R,y_R)=(\lambda^2−x_P−x_Q,−(\lambda x_R+\nu ))$ を計算すると、 $x_R=18^2−2−12=11$ と $y_R=−(18\cdot 11+15)=17$ となり、 $R=(11,17)$ となる。
$(2.5)$ を適用して $S=[2]P$ を計算する、$\lambda'=\frac{3x^2_P+a}{2y_P}=\frac{3\cdot 2^2+5}{2\cdot 5}=17\cdot 10^{−1}=17\cdot 7=4$ を得られ、 $\nu'$ は次となる $\nu'=y_P−\lambda' x_P=5−4\cdot 2=20$ したがって、$E$ と多重度2で交わる接線は $\ell':y=4x+20$ である。
$(x_S,y_S)=(\lambda'^2−2x_P,−(\lambda'x_S+\nu'))$ を計算すると、$x_S=4^2−2\cdot 2=12$ と $y_S= −(4\cdot 12+20)=−68=1$ となり、$S=(12,1)$ となる。

<details><summary>原文</summary>

We now give an example of the multiplication-by-$m$ map on $E$ , defined as 

$$[m]:E\rightarrow E,\hspace{50pt}P \mapsto [m]P,$$

and illustrate the straightforward way to compute it in practice.
This operation is analogous to exponentiation $g\mapsto g^m$ in $\mathbb{Z}^∗_q$ , and is the central operation in ECC, as it is the one-way operation that buries discrete logarithm problems in $E(\mathbb{F}_q)$ .
To efficiently compute the exponentiation $g^m$ in $\mathbb{Z}^∗_q$ , we square-andmultiply, whilst to compute the scalar multiplication $[m]P$ in $E(\mathbb{F}_q)$ , we (because of the additive notation) double-and-add.

</details>

ここで、$E$上の $m$ 乗法写像の例として、次のように定義する。

$$[m]:E\rightarrow E,\hspace{50pt}P \mapsto [m]P,$$

実際の計算方法を簡単に説明する。
この操作は、$\mathbb{Z}^∗_q$ の累乗 $g\mapsto g^m$ に類似しており、$E(\mathbb{F}_q)$ の離散対数問題を埋める一方向操作であるため、ECC の中心的な操作です。
$\mathbb{Z}^∗_q$ の累乗 $g^m$ を効率的に計算するには、2 乗して乗算しますが、$E(\mathbb{F}_q)$ でスカラー乗算 $[m]P$ を計算するには、(加法的表記のため) 2 倍して加算します。

<details><summary>原文</summary>

Example 2.1.8 (Magma script).
Let $E/\mathbb{F}_{1021}:y^2=x^3−3x−3$ so that $r=\#E(\mathbb{F}_q)=1039$ is prime.
Let $P=(379,1011)\in E$ and $m=655$ , and suppose we are to compute $[m]P=[655](379, 1011)$ .
To double-and-add, we write the (10-bit) binary representation of $m$ as $m=(m_9,\ldots ,m_0)_2=(1,0,1,0,0,0,1,1,1,1)$ .
Initialising $T\leftarrow P$ , and starting from the second most significant bit $m_8$ , we successively compute $T\leftarrow [2]T$ for each bit down to $m_0$ , and whenever $m_i=1$ we compute $T\leftarrow T+P$ .
So, in our case it takes 9 doublings $T\leftarrow [2]T$ and 5 additions $T\leftarrow T+P$ to compute $[m]P$ , which ends up being $[655](379,1011)=(388,60)$ .
In general then, this straightforward double-and-add algorithm will take $\log_2 m$ doublings and roughly half as many additions to compute $[m]P$ (if $m$ is randomly chosen).

</details>

例 2.1.8 (Magma script)
$r=\#E(\mathbb{F}_q)=1039$ が素数になるとうに、$E/\mathbb{F}_{1021}:y^2=x^3−3x−3$ とする。
$P=(379,1011)\in E$ と $m=655$ とし、 $[m]P=[655](379,1011)$ を計算するとする。
2倍して加算するには、$m$ の (10 ビット) バイナリ表現を $m=(m_9,\ldots ,m_0)_2=(1,0,1,0,0,0,1,1,1,1)$ として書き込みます。
$T\leftarrow P$ を初期化し、最上位ビット $m_8$ から始めて、$m_0$ までの各ビットについて $T\leftarrow [2]T$ を順次計算し、$m_i=1$ ごとに $T\leftarrow T+P$ を計算する。
つまり、この場合、$[m]P$ を計算するのに9回の倍算 $T\leftarrow [2]T$ と5回の加算 $T\leftarrow T+P$ が必要で、結局、$[655](379,1011)=(388,60)$ となります。
一般に、この単純な 倍算加算（double-and-add）アルゴリズムは、$[m]P$ ( $m$ がランダムに選択された場合) を計算するために $\log_2 m$ 回の倍算と約半分の加算を必要とします。

### 2.1.3 群公理（The group axioms）

<details><summary>原文</summary>

All but one of the group axioms are now concrete.
Namely, for closure, if we start with two points in $E(K)$ , then the chord-and-tangent process gives rise to a cubic polynomial in $K$ for which two roots (the two $x$-coordinates of the points we started with) are in $K$, meaning the third root must also be in $K$; the explicit formulas affirm this.
The identity and inverse axioms are fine, since $P\oplus O=P$ , and the element $\ominus P$ such that $P\oplus (\ominus P)=O$ is clearly unique and well defined for all $P$ .
We also note that the group is abelian, since the process of computing $P\oplus Q$ is symmetric.
The only non-obvious axiom is associativity, i.e. showing $(P\oplus Q)\oplus R=P\oplus (Q\oplus R)$ .
An elementary approach using the explicit formulas above can be used to show associativity by treating all the separate cases, but this approach is rather messy [Fri05].
Silverman gives a much more instructive proof [Sil09, Ch. III.3.4e] using tools that we will develop in the following chapter, but for now we offer some temporary intuition via the illustration in Figures 2.12 and 2.13.

</details>

群公理は1つを除いてすべて具体化されました。
つまり、閉包の場合、$E(K)$ の2点から始めると、弦と接線の過程で $K$ の3次多項式が生じ、その2根（始めた点の $x$ 座標）は $K$ にあり、3根も $K$ になければならず、このことは明示的に式で確認されます。
$P\oplus O=P$ であり、$P\oplus (\ominus P)=O$ となる要素 $\ominus P$ は明らかに一意であり、すべての $P$ に対して明確に定義されているため、恒等公理と逆公理は問題ありません。
また、$P\oplus Q$ の計算過程は対称であるため、この群はアーベル群であることに注意してください。
唯一の自明でない公理は結合性です。つまり、$(P\oplus Q)\oplus R=P\oplus (Q\oplus R)$ を示します。
上記の明示的な式を使用した基本的なアプローチを使用して、すべての個別のケースを処理することで結合性を示すことができますが、このアプローチはやや面倒です [Fri05]。
Silvermanは、次の章で開発するツールを用いて、より有益な証明 [Sil09, Ch.III.3.4e] をしているが、今のところ、図 2.12 と 2.13 の図から一時的な直感を提供します。

![](/images/pfb/figure2_12-2_13.png)

### 2.1.4 楕円曲線計算の高速化（Speeding up elliptic curve computations）

<details><summary>原文</summary>

Group law computations on elliptic curves are clearly more complicated than computations in traditional groups that facilitate discrete logarithm based protocols like $\mathbb{F}^∗_q$; the explicit formulas in $(2.4)$ and $(2.5)$ use many field operations.

However, in the context of cryptography, the more abstract nature of elliptic curve groups actually works in their favour.
This is essentially because attackers aiming to solve the discrete logarithm problem on elliptic curves also face this abstractness.
The subexponential algorithms that apply to finite field discrete logarithms<sup>1</sup> do not translate to the elliptic curve setting, where the best available attacks remain generic, exponential algorithms like Pollard rho [Pol78].
This means that elliptic curve groups of a relatively small size achieves the same conjectured security as multiplicative groups in much larger finite fields, i.e. $E(\mathbb{F}_{q_1})$ and $\mathbb{F}^∗_{q_2}$ achieve similar security when $q_2\gg q_1$ .
For example, an elliptic curve defined over a 160-bit field currently offers security comparable to a finite field of 1248 bits [Sma10, Table 7.2].
Thus, although more field operations are required to perform a group law computation, these operations take place in a field whose operational complexity is much less, and this difference is more than enough to tip the balance in the favour of elliptic curves.
In addition, the smaller group elements in $E(\mathbb{F}_{q^1})$ implies much smaller key sizes, greatly reducing storage and bandwidth requirements.
These are some of the major reasons that elliptic curves have received so much attention in the realm of public-key cryptography; the field of elliptic curve cryptography (ECC) has been thriving since Koblitz [Kob87] and Miller [Mil85] independently suggested their potential as alternatives to traditional groups.

<sup>1</sup>See Diem’s notes on index calculus for a nice introduction [Die12].

</details>

楕円曲線上の群律計算は、$\mathbb{F}^∗_q$ ような離散対数ベースの公式を容易にする従来の群律計算よりも明らかに複雑で、$(2.4)$ と $(2.5)$ の明示的な公式は多くの群演算を使用します。

しかし、暗号技術の文脈では、楕円曲線群の抽象度が高いことが、かえって有利に働く。
これは、楕円曲線上の離散対数問題を解くことを目的とした攻撃者も、この抽象性に直面することが本質的な理由である。
有限体離散対数<sup>1</sup> に適用される劣指数的（subexponential）アルゴリズムは、楕円曲線の設定に変換されません。そこでは、利用可能な最良の攻撃は、Pollard rho [Pol78] のような一般的な指数アルゴリズムのままです。
つまり、比較的小さなサイズの楕円曲線群でも、より大きな有限体における乗法群と同様の安全性、すなわち $q_2\gg q_1$ であれば $E(\mathbb{F}_{q_1})$ と $\mathbb{F}^∗_{q_2}$ が同様の安全性を達成することが予想されるのです。
例えば、160ビットの体上で定義された楕円曲線は、現在、1248ビットの有限体に匹敵するセキュリティを提供しています[Sma10, Table 7.2]。
このように、群律演算を行うにはより多くの体演算が必要ですが、その演算は演算量がはるかに少ない体で行われ、この差は楕円曲線に有利に働くには十分すぎるほどです。
また、$E(\mathbb{F}_{q^1})$ の群要素が小さいため、鍵のサイズが非常に小さくなり、ストレージや帯域幅の要件が大幅に削減されます。
これらは、公開鍵暗号の領域で楕円曲線が注目された大きな理由の一つです。楕円曲線暗号（ECC）の分野は、Koblitz [Kob87] と Miller [Mil85] が独自に従来の群に代わるものとしてその可能性を示唆して以来、盛んになっています。

<sup>1</sup>優れた導入については、指数計算に関する Diem のメモを参照してください [Die12]。

<details><summary>原文</summary>

One avenue of research that has given ECC a great boost is that of optimising the group law computations.
The explicit formulas in affine coordinates ($(2.4)$ and $(2.5)$) would not be used to compute the group law in practice, and in fact the Weierstrass model $E:y^2=x^3+ax+b$ is often not the optimal curve model either.
A huge amount of effort has been put towards investigating other models and coordinate systems in order to minimise the field operations required in group law computations.
One of the initial leaps forward in this line of research was the observation that performing computations in projective space avoids field inversions, which are extremely costly in practice.
We illustrate these techniques in the following examples.

</details>

ECC を大きく後押しした研究の手段の 1 つは、群律計算を最適化することです。
アフィン座標の明示的な式($(2.4)$ および $(2.5)$)は、実際には群律計算には使用されません。実際、Weierstrassモデル $E:y^2=x^3+ax+b$ も最適な曲線モデルではないことがよくあります。
群律計算に必要な体操作を最小限に抑えるために、他のモデルや座標系の調査に多大な労力が費やされてきました。
この一連の研究における最初の飛躍の 1 つは、射影空間で計算を実行すると、実際には非常にコストがかかる体での逆数を回避できるという観察でした。
以下の例でこれらの手法を説明します。

<details><summary>原文</summary>

Example 2.1.9 (Magma script).
Consider a general Weierstrass curve $E(\mathbb{F}_q):y^2=x^3+ax+b$ where $q$ is a large prime, and let $\mathbf{M}$, $\mathbf{S}$ and $\mathbf{I}$ represent the cost of computing multiplications, squarings and inversions in $\mathbb{F}_q$ respectively.
To compute a general affine point doubling $(x_R,y_R)=[2](x_P,yP)$ using $(2.5)$ costs $2\mathbf{M}+2\mathbf{S}+\mathbf{I}$ , and to compute a general affine point addition $(x_R,y_R)=(x_P,y_P)\oplus(x_Q,y_Q)$ using $(2.4)$ costs $2\mathbf{M}+\mathbf{S}+\mathbf{I}$ . 
On the other hand, we can transform the formulas into homogeneous projective space according to the substitutions $x=X/Z$ and $y=Y/Z$ , and we can consider computing $(X_R:Y_R:Z_R)=[2](X_P:Y_P:Z_P)$ and $(X_R:Y_R:Z_R)=(X_P:Y_P:Z_P)\oplus (X_Q:Y_Q:Z_Q)$ on $E:Y^2Z=X^3+aXZ^2+bZ^3$ .
For the addition case, substituting $x_i=X_i/Z_i$ and $y_i=Y_i/Z_i$ for $i\in\{P,Q,R\}$ into the affine formulas

$$
x_R=\left(\frac{y_Q−y_P}{x_Q−x_P}\right)^2−x_P−x_Q;\hspace{50px}y_R=\left(\frac{y_Q−y_P}{x_Q−x_P}\right)(x_P−x_R)−y_P
$$

taken from (2.4), gives

$$
\frac{X_R}{Z_R}=\left(\frac{\frac{Y_Q}{Z_Q}−\frac{Y_P}{Z_P}}{\frac{X_Q}{Z_Q}−\frac{X_P}{Z_P}}\right)^2−\frac{X_P}{Z_P}−\frac{X_Q}{Z_Q};\hspace{50px}\frac{Y_R}{Z_R}=\left(\frac{\frac{Y_Q}{Z_Q}−\frac{Y_P}{Z_P}}{\frac{X_Q}{Z_Q}−\frac{X_P}{Z_P}}\right)\left(\frac{X_P}{Z_P}−\frac{X_R}{Z_R}\right)−\frac{Y_P}{Z_P}.
$$

After a little manipulation, we can then set $Z_R$ to be the smallest value that contains both denominators above, and update the numerators accordingly to give

$$
\begin{align*}
X_R=&(X_PZ_Q−X_QZ_P)(Z_PZ_Q(Y_PZ_Q−Y_QZ_P)^2−(X_PZ_Q−X_QZ_P)^2(X_PZ_Q+X_QZ_P);\\
Y_R=&Z_PZ_Q(X_QY_P−X_PY_Q)(X_PZ_Q−X_QZ_P)^2 \\
&−(Y_PZ_Q−Y_QZ_P)(Y_PZ_Q−Y_QZ_P)^2Z_PZ_Q−(X_PZ_Q+X_QZ_P)(X_PZ_Q−X_QZ_P)^2); \\
Z_R=&Z_PZ_Q(X_PZ_Q−X_QZ_P)^3.
\end{align*}
$$

</details>

例 2.1.9 (Magma script)
$q$ を大きな素数とする一般的なWeierstrass曲線 $E(\mathbb{F}_q):y^2=x^3+ax+b$ を考え、$\mathbf{M}$ 、$\mathbf{S}$ 、$\mathbf{I}$ はそれぞれ $\mathbb{F}_q$ の乗算、平方、逆数の計算コストを表すとする。
(2.5) を使用して一般的なアフィン点の倍加 (xR, yR) = [2](xP , yP ) を計算するには、2M+2S+I のコストがかかり、一般的なアフィン点の加算 (xR, yR) = (xP , yP (2.4) を使用した )⊕(xQ, yQ) のコストは 2M + S + I です。
一方、 $x=X/Z$ と $y=Y/Z$ の代入により、数式を同次射影空間に変換し、$E:Y^2Z=X^3+aXZ^2+bZ^3$ 上で $(X_R:Y_R:Z_R)=[2](X_P:Y_P:Z_P)$ と $(X_R:Y_R:Z_R)=(X_P:Y_P:Z_P)\oplus (X_Q:Y_Q:Z_Q)$ の計算を考えることができる。
加算の場合、$i\in\{P,Q,R\}$ に対して  $x_i=X_i/Z_i$ および $y_i=Y_i/Z_i$ をアフィン公式に代入します。

$$
x_R=\left(\frac{y_Q−y_P}{x_Q−x_P}\right)^2−x_P−x_Q;\hspace{50px}y_R=\left(\frac{y_Q−y_P}{x_Q−x_P}\right)(x_P−x_R)−y_P
$$

$(2.4)$ から以下が得られます。

$$
\frac{X_R}{Z_R}=\left(\frac{\frac{Y_Q}{Z_Q}−\frac{Y_P}{Z_P}}{\frac{X_Q}{Z_Q}−\frac{X_P}{Z_P}}\right)^2−\frac{X_P}{Z_P}−\frac{X_Q}{Z_Q};\hspace{50px}\frac{Y_R}{Z_R}=\left(\frac{\frac{Y_Q}{Z_Q}−\frac{Y_P}{Z_P}}{\frac{X_Q}{Z_Q}−\frac{X_P}{Z_P}}\right)\left(\frac{X_P}{Z_P}−\frac{X_R}{Z_R}\right)−\frac{Y_P}{Z_P}.
$$

少し操作して、上の両分母を含む最小の値を $Z_R$ とし、分子を適宜更新すると、次のようになる。

$$
\begin{align*}
X_R=&(X_PZ_Q−X_QZ_P)(Z_PZ_Q(Y_PZ_Q−Y_QZ_P)^2−(X_PZ_Q−X_QZ_P)^2(X_PZ_Q+X_QZ_P);\\
Y_R=&Z_PZ_Q(X_QY_P−X_PY_Q)(X_PZ_Q−X_QZ_P)^2 \\
&−(Y_PZ_Q−Y_QZ_P)(Y_PZ_Q−Y_QZ_P)^2Z_PZ_Q−(X_PZ_Q+X_QZ_P)(X_PZ_Q−X_QZ_P)^2); \\
Z_R=&Z_PZ_Q(X_PZ_Q−X_QZ_P)^3.
\end{align*}
$$

<details><summary>原文</summary>

The explicit formulas database (EFD) [BL07a] reports that the above formulas can be computed in a total of $12\mathbf{M}+2\mathbf{S}$ .
The real power of adopting projective coordinates for computations becomes apparent when we remark that most optimised implementations of $\mathbf{F}_q$ arithmetic have $\mathbf{I}\gg 20\mathbf{M}$ , and the multiplication to inversion ratio is commonly reported to be $80:1$ or higher.
Thus, the $12\mathbf{M}+2\mathbf{S}$ used for additions in projective space will be much faster than the  $2\mathbf{M}+\mathbf{S}+\mathbf{I}$ for affine additions.
For completeness, we remark that deriving the projective formulas for computing $(X_R:Y_R:Z_R)=[2](X_P:Y_P:Z_P)$ is analogous (but substantially more compact since we only have the projective coordinates of $P$ to deal with), and the EFD reports that this can be done in $5\mathbf{M}+6\mathbf{S}$ , which will again be much faster than the $2\mathbf{M}+2\mathbf{S}+\mathbf{I}$ in affine space.

The Weierstrass model for elliptic curves covers all isomorphism classes, meaning that every elliptic curve can be written in Weierstrass form.
Other models of elliptic curves are usually available if some condition holds, and (ifthis is the case) it can be advantageous to adopt such a model, as the following example shows.

</details>

明示的数式データベース (EFD) [BL07a] は、上記の数式を合計 $12\mathbf{M}+2\mathbf{S}$ で計算できると報告しています。
Fq演算の最適化された実装の多くが $\mathbf{I}\gg 20\mathbf{M}$ であり、乗算と逆数の比率が $80：1$ 以上と報告されていることから、射影座標を用いた計算の真価が明らかになった。
したがって、射影空間での加算に用いる $12\mathbf{M}+2\mathbf{S}$ は、アフィン加算の  $2\mathbf{M}+\mathbf{S}+\mathbf{I}$ よりもはるかに高速になるのである。
完全を期すために、 $(X_R:Y_R:Z_R)=[2](X_P:Y_P:Z_P)$ を計算するための射影公式の導出も類似しており (ただし、扱うのは $P$ の射影座標だけなので実質的によりコンパクト)、EFD はこれを $5\mathbf{M}+6\mathbf{S}$ で行えると報告しており、これもアフィン空間の $2\mathbf{M}+2\mathbf{S}+\mathbf{I}$ よりはるかに速いだろう。

楕円曲線のWeierstrassモデルは、すべての同型性クラスをカバーしており、すべての楕円曲線がWeierstrass形式で記述できることを意味します。
楕円曲線は、ある条件を満たせば、他のモデルも利用できるのが普通で、（その場合は）次の例のように、そのモデルを採用することが有利になることがあります。

<details><summary>原文</summary>

Example 2.1.10 (Magma script).
If $x^3+ax+b$ has a root in $\mathbb{F}_q$ , then Billet and Joye [BJ03, Eq. 8-10] show that instead of working with $E:y^2=x^3+ax+b$ , we can work with the (birationally equivalent) Jacobi-quartic curve $J:v^2=au^4+du^2+1$ , for appropriately defined $a$, $d$ (that depend on the root).
Here we write $J$ using $(u,v)$ coordinates so back-and-forth mappings are defined without confusion.
Thus, consider $E/\mathbb{F}_{97}:y^2=x^3+5x+5$ , for which $x^3+5x+5$ has $34$ as a root, so we will work on the isomorphic curve $J/\mathbb{F}_{97}:v^2=73u^4+46u^2+1$ .
Instead of homogeneous projective coordinates, [BJ03] projectified under the substitution $u=U/W$ and $v=V/W^2$ , which gives the (non-homogeneous) projective closure as $J:V^2=73U^4+46U^2W^2+W^4$ .
Any point $(x,y)\ne\mathcal{O}$ on $E$ can be taken straight to the projective closure of $J$ via

$$
(x,y) \mapsto  \left(2(x−34):(2x+34)(x−34)^2−y^2:y\right) ,
$$

with the reverse mapping given by

$$
(U:V:W) \mapsto \left(2\frac{V + W^2}{U^2}− 17, W\frac{4(V+W^2)−5U^2}{U^3}\right) .
$$

where we immediately highlight the relative simplicity of the above formulas in comparison to the homogeneous projective formulas derived in the previous example. 
Unsurprisingly then, the fastest formulas for Jacobi-quartic additions and doublings outdo those for general Weierstrass curves in homogeneous projective space.
Namely, the current fastest formulas for doublings on Jacobi-quartics cost $2\mathbf{M}+5\mathbf{S}$ and additions cost $6\mathbf{M}+4\mathbf{S}$ [HWCD09], whilst in the previo example we had $5\mathbf{M}+6\mathbf{S}$ for doublings and $12\mathbf{M}+2\mathbf{S}$ for additions.

</details>

例 2.1.10 (Magma script)
$x^3+ax+b$ が $\mathbb{F}_q$ に平方根を持つ場合、Billet and Joye [BJ03, Eq. 8-10] は、 $E:y^2=x^3+ax+b$ を扱う代わりに、（平方根に依存する）適切に定義した $a$, $d$ に対して、（双対等価な）ヤコビ曲線 $J:v^2=au^4+du^2+1$ で作業できることを示しました。
ここでは、$J$ を $(u,v)$ 座標で書くことで、前後の写像を混乱なく定義できるようにしています。
したがって、 $E/\mathbb{F}_{97}:y^2=x^3+5x+5$ を考えます $x^3+5x+5$ は平方根として $34$ を持ちます。そのため、同形曲線 $J/\mathbb{F}_{97}:v^2=73u^4+46u^2+1$ に取り組みます。
同次射影座標の代わりに、[BJ03] では $u=U/W$ と $v=V/W^2$ の代入で射影し、 $J:V^2=73U^4+46U^2W^2+W^4$ として（非同次）射影閉包を与えています。
$E$ 上の任意の点 $(x,y)\ne\mathcal{O}$ は、次の方法で $J$ の射影閉包に直接導くことができます。

$$
(x,y) \mapsto  \left(2(x−34):(2x+34)(x−34)^2−y^2:y\right) ,
$$

であり、逆写像は次式で与えられます。

$$
(U:V:W) \mapsto \left(2\frac{V + W^2}{U^2}− 17, W\frac{4(V+W^2)−5U^2}{U^3}\right) .
$$

ここで、前の例で導かれた同次射影式と比較して、上記の式が相対的に単純であることを直ちに強調するものです。
当然のことながら、ヤコビ型四則演算の加算と倍算の最速公式は、同次射影空間の一般的なWeierstrass曲線の公式を凌駕しています。
すなわち、Jacobi-quarticsの現在の最速公式は倍算は $2\mathbf{M}+5\mathbf{S}$ で、加算は $6\mathbf{M}+4\mathbf{S}$ ですが[HWCD09]、前の例では、倍算は $5\mathbf{M}+6\mathbf{S}$ で、加算は $12\mathbf{M}+2\mathbf{S}$ でした。

<details><summary>原文</summary>

The Jacobi-quartic curves discussed above are just one example of dozens of models that have been successful in achieving fast group law computations, and therefore fast cryptographic implementations.
Other well known models include Edwards curves [Edw07,BL07b], Hessian curves [JQ01,Sma01] and Montgomery curves [Mon87].
We refer to the EFD [BL07a] for a catalogue of all the fastest formulas for the popular curve models, and to Hisil’s thesis [His10] for a general method of (automatically) deriving fast group law algorithms on arbitrary curve models.
For any reader wishing to delve even further into group law arithmetic on elliptic curves, we also recommend the recent, advanced works by Castryck and Vercauteren [CV11], and by Kohel [Koh11].

</details>

上述したヤコビ型4次曲線は、群律演算の高速化、ひいては暗号実装の高速化に成功した数十のモデルの一例に過ぎません。
その他、Edwards曲線[Edw07,BL07b]、Hessian曲線[JQ01,Sma01]、Montgomery曲線[Mon87]などがよく知られたモデルです。
一般的な曲線モデルのすべての最速の式のカタログについては EFD [BL07a] を参照し、任意の曲線モデルで高速な群法則アルゴリズムを (自動的に) 導出する一般的な方法については Hisil の論文 [His10] を参照します。
楕円曲線の群法演算をさらに掘り下げたい読者には、Castryck と Vercauteren [CV11]、および Kohel [Koh11] による最近の高度な研究もお勧めします。

## 2.2 ねじれ、自己準同型、点カウント（Torsion, endomorphisms and point counting）

<details><summary>原文</summary>

We now turn our focus to the behaviour of elliptic curve groups, as they are used in cryptography.
We start by importantly discussing the possible structures exhibited by the finite group $E(\mathbb{F}_q)$ .
It turns out that $E(\mathbb{F}_q)$ is either itself cyclic, or isomorphic to a product of two cyclic groups $\mathbb{Z}_{n_1}\times\mathbb{Z}_{n_2}$ with $n_1|n_2$ [ACD+05, Prop. 5.78].
In cryptography, we would like the group $E(\mathbb{F}_q)$ to be as cyclic as possible, so we usually prefer the former case, or at the very least for $n_1$ to　be very small.
In most cases of practical interest, we can generate curves that are cyclic with relative ease, so throughout this thesis it is to safe assume that E(Fq) is cyclic (but to see the real depth of this question in general, we refer to [MS07]).
The following example illustrates that  $E(\mathbb{F}_q)=\langle P\rangle$ obeys all the usual rules that apply to cyclic groups, and introduces the important notion of $r$-torsion.

</details>

ここで、暗号技術に用いられる楕円曲線群の振る舞いに注目します。
まず、有限群 $E(\mathbb{F}_q)$ が示す可能性のある構造について、重要な議論を行う。
$E(\mathbb{F}_q)$ はそれ自身が環か、 $n_1|n_2$ の2つの環群 $\mathbb{Z}_{n_1}\times\mathbb{Z}_{n_2}$ の積に同型であることがわかります。［ACD+05, Prop.5.78］
暗号技術では、群 $E(\mathbb{F}_q)$ はできるだけ環状であることが望ましいので、通常は前者、少なくとも $n_1$ が非常に小さいことが望ましいとされます。
実際のところ、ほとんどの場合、比較的容易に環の曲線を生成することができるので、本論文では $E(\mathbb{F}_q)$ は環状であると安全に仮定することにします（ただし、一般にこの問題の本当の深さを見るには、[MS07]を参照してください）。
次の例は、 $E(\mathbb{F}_q)=\langle P\rangle$ が環群に適用されるすべての通常の規則に従うことを説明し、$r$-ねじれ という重要な概念を導入している。

<details><summary>原文</summary>

Example 2.2.1 (Magma script). 
Consider $E/\mathbb{F}_{101}:y^2=x^3+x+1$ .
The group order is $\#E(\mathbb{F}_q)=105=3\cdot 5\cdot 7$ , and $P=(47,12)\in E$ is a generator. 
Lagrange’s theorem says that points (and subgroups) over the base field will have order in $\{1,3,5,7,15,21,35,105\}$ .
Indeed, to get a point of order $r|105$ , we simply multiply $P$ by the appropriate cofactor, which is $h=\#E/r$ .
For example, a point of order $3$ is $[35](47,12)=(28,8)$ , a point of order $21$ is $[5](47,12)=(55,65)$ , and a point of order $1$ is $[105](47,12)=\mathcal{O}$ (which is the only such point).
By definition, a point is “killed” (sent to $\mathcal{O}$ ) when multiplied by its order.
Any point over the full closure $E(\mathbb{F}_q)$ that is killed by $r$ is said to be in the $r$-torsion.
So, the point $(55,65)$ above is in the $21$-torsion, as is the point $(28,8)$ .
There are exactly $21$ points in $E(\mathbb{F}_q)$ in the $21$-torsion, but there are many more in  $E(\overline{\mathbb{F}}_q)$ .

</details>

例 2.2.1 (Magma script)
$E/\mathbb{F}_{101}:y^2=x^3+x+1$ を考えます。
位数は、$\#E(\mathbb{F}_q)=105=3\cdot 5\cdot 7$ 、$P=(47,12)\in E$ は生成元です。
ラグランジュの定理では、基底体上の点（および部分群）は $\{1,3,5,7,15,21,35,105\}$ の位数を持つことになります。
実際、$r|105$ の位数の点を得るには、$P$ に適切な係数を掛けるだけで、$h=\#E/r$ となります。
例えば、位数 $3$ の点は  $[35](47,12)=(28,8)$ 、位数 $21$ の点は $[5](47,12)=(55,65)$ 、位数 $1$ の点は $[105](47,12)=\mathcal{O}$（この点のみ）です。
定義によれば、点はその位数を乗算すると「殺される」（ $\mathcal{O}$ に送られます）。
完全閉域 $E(\mathbb{F}_q)$ 上の点で、$r$ によって殺されるものは、$r$-ねじれ であるといいます。
つまり、上の点 $(55,65)$ は点 $(28,8)$ と同じように $21$ねじれ になります。
 $E(\mathbb{F}_q)$ の中で $21$ねじれ に入る点はちょうど $21$ 個ですが、 $E(\overline{\mathbb{F}}_q)$ にはもっと多くの点があります。

<details><summary>原文</summary>

The whereabouts and structure of r-torsion points in E(Fq) (alluded to at the end of Example 2.2.1) plays a crucial role in pairing-based cryptography; we will be looking at this in close detail in Chapter 4.

In ECC we would like the group order $\#E(\mathbb{F}_q)$ to be as close to prime as possible. 
This is because the (asymptotic) complexity of the ECDLP that attackers face is dependent on the size of the largest prime subgroup of $E(\mathbb{F}_q)$ .
Even if the particular instance of the discrete logarithm problem uses a generator of the whole group, the attacker can use the known group order to solve smaller instances in subgroups whose orders are pairwise prime, and then reconstruct the answer using the Chinese Remainder Theorem (CRT).
We make this clear in the following two examples: the first is a toy example, whilst the second shows the difference between two curves of the same cryptographic size; one that is currently considered secure and one that is completely breakable using modern attacks.

</details>

$E(\mathbb{F}_q)$ の $r$-ねじれ点（例2.2.1の最後で触れている）の位置と構造は、ペアリング暗号において重要な役割を担っており、第4章で詳しく見ていきます。

ECCでは、群位数 $\#E(\mathbb{F}_q)$ をできるだけ素数に近づけたいと考えています。
これは、攻撃者が直面するECDLPの（漸近的な）複雑さが、$E(\mathbb{F}_q)$ の最大の素数部分群の大きさに依存するためです。
離散対数問題の特定のインスタンスが群全体の生成元を使用する場合でも、攻撃者は既知の群の位数を使用して、位数が互いに素である部分群の小さなインスタンスを解決し、中国の余弦定理（CRT）を使用して答えを再構築することができます。
1つ目はおもちゃのような例で、2つ目は同じ暗号サイズでも、現在安全とされている曲線と、現代の攻撃で完全に破れる曲線の違いを示しています。

<details><summary>原文</summary>

Example 2.2.2 (Magma script).
Consider $E/\mathbb{F}_{1021}:y^2=x^3+905x+100$ , with group order $\#E(\mathbb{F}_q)=966=2\cdot 3\cdot 7\cdot 23$ , and generator $P=(1006,416)$ .
Suppose we are presented with an instance of the ECDLP: namely, we are given $Q=(612,827)$, and we seek to find $k$ such that $[k]P=Q$ .
For the sake of the example, suppose our best “attack” is trivial: trying every multiple $[i]P$ of $P$ until we hit the correct one ( $i=k$ ).
Rather than seeking $i$ in the full group ( $2\leq i\leq 965$ ), we can map the instance into each prime order subgroup by multiplying by the appropriate cofactor, and then solve for $k_j\equiv k\mod j$, $j\in \{2, 3, 7, 23\}$ .
For $j=2$ , we have $P_j=P_2=[966/2]P=[483](1006,416)=(174,0)$ , and $Q_j=Q_2=[483](612,827)=(174,0)$ , so $Q_2=[k_2]P_2$ gives $k_2=1$ .
For $j=3$ , we have $P_3=[322]P=(147,933)$ and $Q_3=[322]P=\mathcal{O}$ , so $Q_3=[k_3]P_3$ gives $k_3=3$ .
For $j=7$ , we have $P_7=[138]P=(906,201)$ and $Q_7=[138]Q=(906,201)$, so $Q_7=[k_7]P_7$ gives $k_7=1$ .
For $j=23$ , we have $P_{23}=[42]P=(890,665)$ and $Q_{23}=[42]Q=(68, 281)$ .
For $Q_{23}=[k_{23}]P_{23}$ , we exhaust $k_{23}\in\{1,\ldots ,22\}$ to see that $k_{23}=20$ .
Now, we can use the Chinese Remainder Theorem to solve

$$k\equiv k_2=1\mod 2;\quad k\equiv k_3=0\mod 3;\quad k\equiv k_7=1\mod 7;\quad k\equiv k_{23}=20\mod 23,$$

which gives $k\equiv 687\mod\#E$ , solving the ECDLP instance.
Notice that the hardest part was exhausting the set $\{1,\ldots ,22\}$ to find $k_{23}=20$ , so the largest prime order subgroup becomes the bottleneck of the algorithm, giving intuition as to why the largest prime order subgroup defines the attack complexity when groups of a cryptographic size are used.

</details>

例 2.2.2 (Magma script)

$E/\mathbb{F}_{1021}:y^2=x^3+905x+100$、群位数 $\#E(\mathbb{F}_q)=966=2\cdot 3\cdot 7\cdot 23$ 、生成元 $P=(1006,416)$ を考えます。
ECDLP のインスタンスが提示されたとします。つまり、$Q=(612,827)$ が与えられ、$[k]P=Q$ となるような $k$ を見つけようとします。
この例のため、最善の「攻撃」が自明であると仮定します。正しいものに到達するまで、$P$ の倍数 $[i]P$ ごとに試行します ( $i=k$ )。
$i$ を全群 ( $2\leq i\leq 965$ ) で求めるのではなく、適当な補数を掛けて各素数位数群にインスタンスを写像し、 $k_j\equiv k\mod j$ 、 $j\in \{2, 3, 7, 23\}$ を解けばよい。
$j=2$ の場合、$P_j=P_2=[966/2]P=[483](1006,416)=(174,0)$ 、$Q_j=Q_2=[483](612,827)=(174,0)$ となるので、$Q_2=[k_2]P_2$ により $k_2=1$ が得られます。
$j=3$ の場合、$P_3=[322]P=(147,933)$、$Q_3=[322]P=\mathcal{O}$ となるので、$Q_3=[k_3]P_3$ により $k_3=3$ が得られます。
$j=7$ の場合、$P_7=[138]P=(906,201)$、$Q_7=[138]Q=(906,201)$ となるので、$Q_7=[k_7]P_7$ により $k_7=1$ が得られます。
$j=23$ の場合、$P_{23}=[42]P=(890,665)$、$Q_{23}=[42]Q=(68, 281)$ を持ちます。
$Q_{23}=[k_{23}]P_{23}$ の場合、$k_{23}\in\{1,\ldots ,22\}$ を取り尽くし、$k_{23}=20$ であることを確認します。
これで、中国剰余定理を使って解くことができます。

$$k\equiv k_2=1\mod 2;\quad k\equiv k_3=0\mod 3;\quad k\equiv k_7=1\mod 7;\quad k\equiv k_{23}=20\mod 23,$$

これは $k\equiv 687\mod\#E$ を与え、ECDLP インスタンスを解きます。
最も難しいのは集合 $\{1,\ldots ,22\}$ を取り尽くして $k_{23}=20$ を見つけることで、最大の素数位数部分群がアルゴリズムのボトルネックとなることに注目し、暗号サイズの群を使用する場合に最大の素数位数部分群が攻撃の複雑さを定義する理由を直感的に理解することができます。

<details><summary>原文</summary>

Example 2.2.3 (Magma script).
For our real world example, we take the curve P256 from the NIST recommendations [NIS99], which currently achieves a similar security level (resistance against best known attacks) to the 128-bit Advanced Encryption Standard (AES) for symmetric encryption.
The curve is defined as $E/\mathbb{F}_q:y^2=x^3−3x+b$ , with prime order $r=\#E$ , and generator $G=(x_G,y_G)$ , where

$$
\begin{align*}
&q = 115792089210356248762697446949407573530086143415290314195533631308867097853951, \\
&r = 115792089210356248762697446949407573529996955224135760342422259061068512044369, \\
&b = 41058363725152142129326129780047268409114441015993725554835256314039467401291, \\
&x_G = 48439561293906451759052585252797914202762949526041747995844080717082404635286, \\
&y_G = 36134250956749795798585127919587881956611106672985015071877198253568414405109, \\
&x_H = 53987601597021778433910548064987973235945515666715026302948657055639179420355, \\
&y_H = 53690949263410447908824456005055253553237881490194075871737490561466076234637.
\end{align*}
$$

We give another point $H=(x_H,y_H)$ to pose $H=[k]G$ as an intractable instance of the ECDLP; this 256-bit prime field (and group order) is far beyond the reach of current attacks.
For example, there is currently a campaign underway to solve a discrete logarithm problem over a 130-bit field using a cluster of servers that have already been running for two years (see http://ecc-challenge.info/), so (assuming the best known attacks stay exponential) it seems the above ECDLP should be safe for a while yet.
We remark that the prime characteristic $q$ is given by $q=2^{256}−2^{224}+2^{192}+2^{96}−1$ ; such primes are preferred in ECC as they allow for faster finite field multiplication and reduction routines, greatly enhancing the speed of $\mathbb{F}_q$ arithmetic.
We now give a curve over the same field $\mathbb{F}_q$ , for which the ECDLP is well within reach of the best known attacks.
Namely, consider the alternative curve with $b=0$ , namely $\tilde{E}/\mathbb{F}_q:y^2=x^3−3x$ , whose group order $n=\#\tilde{E}$ is given as

$$
\begin{align*}
n &= 115792089210356248762697446949407573530086143415290314195533631308867097853952, \\
&= 2^{96}\cdot 7\cdot 274177\cdot 67280421310721\cdot 11318308927973941931404914103.
\end{align*}
$$

This time, the largest prime divisor of the group order is only 94 bits long, and the complexity of solving the ECDLP in $\tilde{E}(\mathbb{F}_q)$ is governed by the difficulty of solving the ECDLP instance in this largest prime subgroup, which could be done in a small amount of time on a desktop computer.

</details>

実世界の例として、NIST 勧告 [NIS99] から曲線 P256 を採用しています。これは現在、対称暗号化の 128 ビット Advanced Encryption Standard (AES) と同様のセキュリティレベル (最もよく知られている攻撃に対する耐性) を達成しています。
曲線は $E/\mathbb{F}_q:y^2=x^3-3x+b$ と定義され、素数位数 $r=\#E$ 、生成元 $G=(x_G,y_G)$ となります。ここで

$$
\begin{align*}
&q = 115792089210356248762697446949407573530086143415290314195533631308867097853951, \\
&r = 115792089210356248762697446949407573529996955224135760342422259061068512044369, \\
&b = 41058363725152142129326129780047268409114441015993725554835256314039467401291, \\
&x_G = 48439561293906451759052585252797914202762949526041747995844080717082404635286, \\
&y_G = 36134250956749795798585127919587881956611106672985015071877198253568414405109, \\
&x_H = 53987601597021778433910548064987973235945515666715026302948657055639179420355, \\
&y_H = 53690949263410447908824456005055253553237881490194075871737490561466076234637.
\end{align*}
$$

別の点 $H=(x_H,y_H)$ を与えて、$H=[k]G$ を ECDLP の扱いにくいインスタンスとして設定します。 この 256 ビットの素数体 (および群の位数) は、現在の攻撃の範囲をはるかに超えています。
たとえば、すでに 2 年間稼働しているサーバーのクラスターを使用して、130 ビット フィールドの離散対数問題を解くキャンペーンが現在進行中です (http://ecc-challenge.info/ を参照)。 最もよく知られている攻撃が指数関数的であると仮定すると)、上記の ECDLP はまだしばらくの間安全であるように思われます。
素数標数 $q$ は $q=2^{256}−2^{224}+2^{192}+2^{96}−1$ で与えられることに注意してください。 このような素数は、より高速な有限体の乗算および縮小ルーチンを可能にし、$\mathbb{F}_q$ 演算の速度を大幅に向上させるため、ECC で優先されます。
次に、同じ体 $\mathbb{F}_q$ 上の曲線で、ECDLPが最もよく知られた攻撃に十分手が届くものを紹介します。
すなわち、$b=0$ の代替曲線、 $\tilde{E}/\mathbb{F}_q:y^2=x^3−3x$ を考え、その群位数 $n=\#\tilde{E}$ は次のように与えられる。

$$
\begin{align*}
n &= 115792089210356248762697446949407573530086143415290314195533631308867097853952, \\
&= 2^{96}\cdot 7\cdot 274177\cdot 67280421310721\cdot 11318308927973941931404914103.
\end{align*}
$$

今回、群位数の最大素数の約数は94ビットしかなく、$\tilde{E}(\mathbb{F}_q)$ のECDLPを解く複雑さは、この最大素数の部分群におけるECDLPインスタンスを解く難しさに支配されており、デスクトップコンピュータでわずかな時間で解くことができます。

<details><summary>原文</summary>

The above example provides clear motivation as to the importance of counting points on elliptic curves.
The largest prime factor of the group order determines the difficulty that attackers face when trying to solve the ECDLP, so we would like to be able to count points on curves quickly enough to find those whose order is prime or almost prime (i.e. has a small cofactor), or have methods of prescribing such a group order before searching for the curve.
Fortunately, on elliptic curves we have efficient algorithms to do both.

We start our brief discussion on elliptic curve point counting by referring back to the two group orders in Example 2.2.3, and observing that both group orders share the first half of their digits with those of the field characteristic $q$ .
This suggests that the number of points on an elliptic curve is close to $q$, which is indeed the case in general; the Hasse bound [Sil09, Ch. 5, Th. 1.1] says the most that $\#E(\mathbb{F}_q)$ can differ from $q+1$ is $2\sqrt{q}$ , i.e. $|\#E(\mathbb{F}_q)−(q+1)|\leq 2\sqrt{q}$ .
This offset between $\#E(\mathbb{F}_q)$ and $(q+1)$ is called the trace of Frobenius, and is denoted by $t$, so

$$\#E(\mathbb{F}_q)=q+1−t,\qquad |t|\leq 2\sqrt{q} \tag{2.6}$$

We will discuss where $t$ comes from and provide some more intuition behind the above formula in a moment, but what the Hasse bound tells us is that the group order lies somewhere in the interval $[q+1−2\sqrt{q},q+1+2\sqrt{q}]$ .
In fact, Deuring [Deu41] showed that when $q$ is prime<sup>2</sup> , then every value $N\in [q+1−2\sqrt{q},q+1+2\sqrt{q}]$ can be found as a group order $\#E(\mathbb{F}_q)$ for some $E$ .

<sup>2</sup> When q is a prime power, there are a very small number of explicitly described exceptions.

</details>

上記の例は、楕円曲線上の点を数えることの重要性について明確な動機を与えている。
群位数の最大の素因数は、攻撃者がECDLPを解こうとするときに直面する困難を決定します。したがって、群位数の素数またはほぼ素数のもの（つまり、小さな補数を持つもの）を見つけるために、曲線上の点を十分に速く数えることができるか、曲線を検索する前にそのような群位数を規定する方法があればと思います。
幸いなことに、楕円曲線では、その両方を実現する効率的なアルゴリズムがあります。

例 2.2.3 の 2 つの群位数を参照し、両方の群位数が体の特性 $q$ の桁の前半を共有していることを観察することによって、楕円曲線の点の数え方について簡単な議論を始めます。
これは、楕円曲線上の点の数が $q$ に近いことを示唆しており、これは実際に一般的なケースです。 Hasse bound[Sil09, Ch. 5, Th. 1.1] は、$\#E(\mathbb{F}_q)$ が $q+1$ と異なる最大値は $2\sqrt{q}$ 、つまり $|\#E(\mathbb{F}_q)−(q+1)|\leq 2\sqrt{q}$ であると述べています。
この$\#E(\mathbb{F}_q)$ と $(q+1)$ のオフセットはフロベニウスのトレースと呼ばれ、$t$ と表記されるので

$$\#E(\mathbb{F}_q)=q+1−t,\qquad |t|\leq 2\sqrt{q} \tag{2.6}$$

$t$ がどこから来るのか、また上式の背後にある直感的な説明は後述するが、Hasse boundが教えてくれるのは、群位数が区間 $[q+1−2\sqrt{q},q+1+2\sqrt{q}]$ のどこかにあることである。
Deuring [Deu41] は、$q$ が素数<sup>2</sup>のとき、$N\in [q+1-2\sqrt{q},q+1+2\sqrt{q}]$ がある $E$ に対して群位数として見出せることを示しました。

<sup>2<sup> $q$ が素数冪の場合、明示的に記述された例外が非常に少数あります。

<details><summary>原文</summary>

Example 2.2.4 (Magma script).
Let $q=23$, so that the Hasse interval becomes $[q+1−2\sqrt{q},q+1+2\sqrt{q}]=[15,33]$ , meaning that there are exactly 19 different group orders taken by elliptic curves over $\mathbb{F}_{23}$ .
For example, $E/\mathbb{F}_{23}:y^2=x^3+18x+3$ has $\#E=15$ , whilst $\tilde{E}/\mathbb{F}_{23}:y^2=x^3+13x+7$ has $\#\tilde{E}=33$ .
We give $19(a,b)$ pairs such that the corresponding curves $E:y^2=x^3+ax+b$ have group orders in  scending order spanning the whole interval, as follows: $(18, 3), (7, 22), (19, 14), (17, 17), (12, 5), (7, 12), (8, 10), (17, 18), (20, 20), (2, 3), (20, 3), (6, 8), (16, 8), (16, 22), (9, 16), (19, 6), (20, 8), (22, 9), (13, 7)$.

A rough (but elementary and instinctive) argument as to why $\#E\approx q$ is that approximately half of the values $x\in [0,\ldots ,q−1]$ will give a quadratic residue $x^3+ax+b\in \mathrm{QR}(q)$ , which gives rise to two points $(x,\pm\sqrt{x^3+ax+b})\in E(\mathbb{F}_q)$ , the only exception(s) being when $x^3+ax+b=0$ which obtains one point.
The sophisticated explanation requires a deeper knowledge than our introduction　offers, but for the purposes of this introductory text we get almost all that we need from Equation $(2.6)$ ; the derivation of which makes use of the following definition.
If $E$ is defined over $\mathbb{F}_q$ , then the Frobenius endomorphism $\pi$ is defined as

$$\pi :E\rightarrow E,\qquad (x,y)\mapsto (x^q,y^q). \tag{2.7}$$

We note that the Frobenius endomorphism maps any point in $E(\overline{\mathbb{F}}_q)$ to a point in $E(\overline{\mathbb{F}}_q)$ , but the set of points fixed by $\pi$ is exactly the group  $E(\mathbb{F}_q)$ .
Thus, $\pi$ only acts non-trivially on points in $E(\overline{\mathbb{F}}_q)\backslash E(\mathbb{F}_q)$ , and more generally, $\pi^i:(x,y)\mapsto (x^{q^i},y^{q^i})$ only acts non-trivially on points in $E(\overline{\mathbb{F}}_q)\backslash E(\mathbb{F}_{q^i})$ .

</details>

例 2.2.4 (Magma script)
$q=23$ とすると、Hasse区間は $[q+1−2\sqrt{q},q+1+2\sqrt{q}]=[15,33]$ となり、 $\mathbb{F}_{23}$ 上の楕円曲線がとる群位数がちょうど19種類あることになります。
例えば、 $E/\mathbb{F}_{23}:y^2=x^3+18x+3$ は $\#E=15$ であり、 $\tilde{E}/\mathbb{F}_{23}:y^2=x^3+13x+7$ は  $\#\tilde{E}=33$ です。
対応する曲線 $E:y^2=x^3+ax+b$ が区間全体に渡る昇順の群順を持つような $(a,b)$ の組を、以下の $19$ 組与える。$(18, 3), (7, 22), (19, 14), (17, 17), (12, 5), (7, 12), (8, 10), (17, 18), (20, 20), (2, 3), (20, 3), (6, 8), (16, 8), (16, 22), (9, 16), (19, 6), (20, 8), (22, 9), (13, 7)$

なぜ $\#E\approx q$ であるかについての大まかな (しかし初歩的で本能的な) 議論は、値 $x\in [0,\ldots ,q−1]$ の約半分が二次剰余 $x^3+ax+b\in \mathrm{QR}(q)$ を与えるということです。 、これは 2 つの点 $(x,\pm\sqrt{x^3+ax+b})\in E(\mathbb{F}_q)$ を生じさせます。唯一の例外は、1 つの点を取得する $x^3+ax+b=0$ の場合です。
洗練された説明には、導入部が提供するものよりも深い知識が必要ですが、この導入部テキストの目的のために、必要なものはほぼすべて式 $(2.6)$ から得られます。 その派生は、次の定義を利用します。
$E$ が $\mathbb{F}_q$ 上で定義されている場合、フロベニウス自己準同型 $\pi$ は次のように定義されます。

$$\pi :E\rightarrow E,\qquad (x,y)\mapsto (x^q,y^q). \tag{2.7}$$

フロベニウス自己準同型写像は $E(\overline{\mathbb{F}}_q)$ 内の任意の点を $E(\overline{\mathbb{F}}_q)$ 内の点にマッピングしますが、$\pi$ によって固定された点の集合は正確に群 $E(\mathbb{F}_q)$ であることに注意してください。
したがって、$\pi$ は $E(\overline{\mathbb{F}}_q)\backslash E(\mathbb{F}_q)$ の点に対してのみ非自明的に作用し、より一般的には、$\pi^i:(x,y)\mapsto (x^{q^i},y^{q^i})$ は $E(\overline{\mathbb{F}}_q)\backslash E(\mathbb{F}_{q^i})$ の点に対してのみ非自明的に作用します。


<details><summary>原文</summary>

Example 2.2.5 (Magma script).
Let $q=67$ , and consider $E/\mathbb{F}_q:y^2=x^3+4x+3$ , and let $\mathbb{F}_{q^2}=\mathbb{F}_q(u)$ where $u^2+1=0$ , and further let $\mathbb{F}_{q^3}=\mathbb{F}_q(v)$ where $v^3+2=0$ .
For $P_1=(15,50)\in E(\mathbb{F}_q)$ , we have $\pi_q(P_1)=(15^q,50^q)=(15,50)$ .
For $P_2=(2u+16,30u+39)$ , we have $\pi_q(P_2)=((2u+16)^q,(30u+39)^q)=(65u+16,39+37u)$ ; it is easy to see in this example that computing $\pi_q(Q)$ for any $Q\in E(\mathbb{F}_{q^2})$ involves a simple “complex conjugation” on each coordinate, which also agrees with $\pi^2_q(Q)=Q$ .
Let $P_3=(15v^2+4v+8,44v^2+30v+21)$ , $\pi_q(P_3)=(33v^2+14v+8,3v^2+38v+21)$, $\pi^2_q(P_3)=(19v^2+49v+8,20v^2+66v+21)$ , and $\pi^3_q(P_3)=P_3$ .

We can now return to sketch the derivation of Equation $(2.6)$ by skimming over results that are presented in full in Silverman’s book [Sil09, Ch. V, Th. 1.1].
We now know that $P\in E(\mathbb{F}_q)$ if and only if $\pi(P)=P$ (i.e. $([1]−\pi)P=\mathcal{O}$ ), and thus $\#E(\mathbb{F}_q)=\#\mathrm{ker}([1]−\pi)$ .
It is not too hard to show that the map $[1]−\pi$ is separable, which means that $\#E(\mathbb{F}_q)=\#\mathrm{ker}([1]−\pi)=\mathrm{deg}([1]−\pi)$ .
We can then make use of (a special case of) a version of the Cauchy-Schwarz inequality [Sil09][Ch. V, Lemma 1.2], to give $|\mathrm{deg}([1]−\pi)−\mathrm{deg}([1])−\mathrm{deg}(\pi)|\leq 2\sqrt{\mathrm{deg}([1])\mathrm{deg}(\pi)}$ , from which Equation $(2.6)$ follows from $\mathrm{deg}(\pi)=q$ .

The theory of elliptic curves makes constant use of the endomorphism ring of $E$ , denoted $\mathrm{End}(E)$ , which (as the name suggests) is the ring of all maps from $E$ to itself; addition in the ring is natural, i.e. $(\psi_1+\psi_2)(P)=\psi_1(P)+\psi_2(P)$, and multiplication in$\mathrm{End}(E)$ is composition $(\psi_1\psi_2)(P)=\psi_1(\psi_2(P))$ .
The multiplication-by-$m$ map $[m]$ is trivially in End(E) for all $m\in\mathbb{Z}$ , and when $E$ is defined over a finite field, then clearly $\pi$ is too, so we are usually interested in any extra endomorphisms that shed more light on the behaviour of $E$ .

</details>

例 2.2.5 (Magma script)
 $q=67$ とし、 $E/\mathbb{F}_q:y^2=x^3+4x+3$ を考え、$\mathbb{F}_{q^2}=\mathbb{F}_q(u)$ ここで $u^2+1=0$ とし、さらに $\mathbb{F}_{q^3}=\mathbb{F}_q(v)$ ここで $v^3+2=0$ とします。
$P_1=(15,50)\in E(\mathbb{F}_q)$ にたいして、$\pi_q(P_1)=(15^q,50^q)=(15,50)$ が成り立ちます。
$P_2=(2u+16,30u+39)$ の場合、$\pi_q(P_2)=((2u+16)^q,(30u+39)^q)=(65u+16,39+37u)$ となります。：この例から、任意の $Q\in E(\mathbb{F}_{q^2})$ に対して $\pi_q(Q)$ を計算すると、それぞれの座標で単純な「複素共役」が起こり、 $\pi^2_q(Q)=Q$ とも一致することが簡単にわかるでしょう。
$P_3=(15v^2+4v+8,44v^2+30v+21)$ 、 $\pi_q(P_3)=(33v^2+14v+8,3v^2+38v+21)$ 、$\pi^2_q(P_3)=(19v^2+49v+8,20v^2+66v+21)$ であり、 $\pi^3_q(P_3)=P_3$ です。

Silverman の著書 [Sil09, Ch. V, Th. 1.1] で全文が紹介されている結果をざっと読んで、式 $(2.6)$ の導出のスケッチに戻ることができます。
ここで、$P\in E(\mathbb{F}_q)$ は、$\pi(P)=P$ （すなわち $([1]−\pi)P=\mathcal{O}$ ）の場合に限り、 $\#E(\mathbb{F}_q)=\#\mathrm{ker}([1]−\pi)$ であることがわかります。
写像 $[1]−\pi$ が分離可能であること、つまり $\#E(\mathbb{F}_q)=\#\mathrm{ker}([1]−\pi)=\mathrm{deg}([1]−\pi)$ であることを示すのはそれほど難しくありません。
次に、Cauchy-Schwarzの不等式[Sil09][Ch. V, Lemma 1.2]の（特殊な）バージョンを利用すると、$|\mathrm{deg}([1]−\pi)−\mathrm{deg}([1])−\mathrm{deg}(\pi)|\leq 2\sqrt{\mathrm{deg}([1])\mathrm{deg}(\pi)}$ が与えられ、ここから式 $(2.6)$ が、 $\mathrm{deg}(\pi)=q$ ら成り立つことになります。

楕円曲線の理論では、$\mathrm{End}(E)$ で示される $E$ の自己準同型環を常に使用します。これは (名前が示すように) $E$ からそれ自体へのすべての写像の環です。 環の加算は自然、すなわち $(\psi_1+\psi_2)(P)=\psi_1(P)+\psi_2(P)$ であり、$\mathrm{End}(E)$ の乗算は合成 $(\psi_1\psi_2)(P)=\psi_1(\psi_2(P))$ です。
$m$乗写像 $[m]$ は、すべての $m\in\mathbb{Z}$ に対して自明に $\mathrm{End}(E)$ にあります。また、$E$ が有限体上で定義される場合、明らかに $\pi$ もそうです。 $E$ の動作をさらに詳しく説明します。

<details><summary>原文</summary>

Example 2.2.6 (Magma script).
Consider $E/\mathbb{F}_q:y^2=x^3+b$ .
The map $\xi$ , defined by $\xi:(x,y)\mapsto (\xi3x,y)$ with $\xi^3_3=1$ and $\xi_3\ne 1$ , is a non-trivial endomorphism on $E$ , so $\xi\in\mathrm{End}(E)$ .
If $\xi_3\in\mathbb{F}_q$ , then $\xi$ will be defined over $\mathbb{F}_q$ , otherwise $\xi_3\in\mathbb{F}_{q^2}$ in which case $\xi$ is not defined over $\mathbb{F}_q$ , but over $\mathbb{F}_{q^2}$ .
We will observe both cases.
Firstly, cubic roots of unity will be defined in $\mathbb{F}_q$ if and only if $q\equiv 1\mod 3$ , so let us take $q\equiv 19$, $b=5$, which gives $E/\mathbb{F}_{19}:y^2=x^3+5$ .
Let $\xi_3=7$ so that $\xi^3_3=1$ (we could have also taken $\xi^2_3=11$ ), so that $\xi :(x,y)\mapsto (7x,y)$ is an endomorphism on $E$ .
Applying this to, say $P=(−1,2)$ , gives $\xi(P)=(−7,2)\in E$ .
Taking the same curve over $\mathbb{F}_{23}$ , i.e. $E/\mathbb{F}_{23}:y^2=x^3+5$ , for which $P=(−1,2)$ is a again a point, we no longer have a non-trivial $\xi_3\in\mathbb{F}_{23}$ , so we must form a quadratic extension $\mathbb{F}_{q^2}(u)$ , $u^2+1=0$ .
Now, we can take $\xi_3=8u+11$ (the other option is $\xi^2_3=15u+11$ ), so that $\xi(P)=(−(8u+11),2)=(15u+12,2)\in E(\mathbb{F}_{q^2})$ .
Notice that $P$ started in $E(\mathbb{F}_q)$ , but landed in $E(\mathbb{F}_{q^2})$ under $\xi$ .
The endomorphism $\xi$ has an inverse $\xi^{−1}$ (which is defined the same way but with $\xi^2_3$ instead), so $\xi$ is actually an automorphism of $E$ , written as $\xi\in\mathrm{Aut}(E)$ .

</details>

例 2.2.6 (Magma script)
$E/\mathbb{F}_q:y^2=x^3+b$ を考えます。
 $\xi:(x,y)\mapsto (\xi_3x,y)$ と $\xi^3_3=1$ および $\xi_3\ne 1$ で定義される写像 $\xi$ は、$E$ 上で非自明な自己準同型なので、$\xi\in\mathrm{End}(E)$ となります。
$\xi_3\in\mathbb{F}_q$ の場合、$\xi$ は $\mathbb{F}_q$ に対して定義されます。いっぽう $\xi_3\in\mathbb{F}_{q^2}$ この場合、 $\xi$ は $\mathbb{F}_q$ ではなく、 $\mathbb{F}_{q^2}$ に対して定義されます。
両方のケースを観察します。
まず、$\mathbb{F}_q$ において立方根が定義されるのは、$q\equiv 1\mod 3$ のときだけなので、 $q\equiv 19$ 、$b=5$ とすると、 $E/\mathbb{F}_{19}:y^2=x^3+5$ が得られます。
ここで、$\xi_3=7$ とすると、$\xi^3_3=1$ となり（ $\xi^2_3=11$ としてもよい）、$\xi :(x,y)\mapsto (7x,y)$ は $E$ 上の自己準同型となります。
これを $P=(−1,2)$ に適用すると、 $\xi(P)=(−7,2)\in E$ が得られます。
$\mathbb{F}_{23}$ 上の同じ曲線、すなわち $E/\mathbb{F}_{23}:y^2=x^3+5$ をとって、 $P=(−1,2)$ を再び点とすると、もはや非自明な $\xi_3\in\mathbb{F}_{23}$ はもはや存在しないので、2次拡大 $\mathbb{F}_{q^2}(u)$ , $u^2+1=0$ を形成しなければなりません。
ここで、$\xi_3=8u+11$とすると（他に $\xi^2_3=15u+11$ もある）、$\xi(P)=(-(8u+11),2)=(15u+12,2)\in E(\mathbb{F}_{q^2})$ となり、その結果、$\xi_3=8u+11$ となります。
$P$ は $E(\mathbb{F}_q)$ から始まり、 $E(\mathbb{F}_{q^2})$ の下で $xi$ に着地していることに注意してください。
自己準同型写像 $\xi$ は逆数 $\xi^{−1}$ (同じように定義されますが代わりに $\xi^2_3$ を使用します) を持つため、 $\xi$ は実際には $E$ の自己同型写像であり、 $\xi\in\mathrm{Aut}(E)$ と記述されます。

<details><summary>原文</summary>

The definition of $\xi :(x,y)\mapsto (\xi_3x,y)$ in the above example gives an endomorphism on $E:y^2=x^3+b$ regardless of the field that $E$ is defined over.
If there exists a non-trivial map (like $\xi$ ) for an elliptic curve $E$ , we say $E$ has complex multiplication.
To be more precise, all elliptic curve endomorphism rings trivially contain $Z$ , since every $m\in Z$ corresponds to the multiplication-by-$m$ map $[m]\in\mathrm{End}(E)$ .
However, if non-trivial endomorphisms exist that make $\mathrm{End}(E)$ strictly larger than $Z$ , then we say $E$ has complex multiplication (CM).
Thus, by this definition, every elliptic curve defined over $\mathbb{F}_q$ has CM, because the existence of the Frobenius endomorphism $\pi\in\mathrm{End}(E)$ makes $\mathrm{End}(E)$ larger than $Z$ .

However, if we discuss whether $E$ has CM without yet stipulating the underlying finite field, then the question becomes non-trivial in general, because the answer depends on the existence of non-trivial maps.
We use Silverman’s example to illustrate [Sil09, Ch. 3, Eg. 4.4].

</details>

上の例で $\xi :(x,y)\mapsto (\xi_3x,y)$ を定義すると、$E$ が定義される体に関係なく、$E:y^2=x^3+b$ 上の自己準同型が得られます。
楕円曲線 $E$ に対して自明でない写像（ $\xi$ のような）が存在する場合、$E$ は虚数乗法（complex multiplication）を持つと言います。
より正確に言えば、すべての楕円曲線自己準同型環は自明に $\mathbb{Z}$ を含みます。これは、すべての $m\in\mathbb{Z}$ が $m$ 乗算写像 $[m]\in\mathrm{End}(E)$ に対応するためです。
ただし、$\mathrm{End}(E)$ が $\mathbb{Z}$ より厳密に大きくなるような非自明な自己準同型が存在する場合は、$E$ は虚数乗法（CM）を持つと言います。
したがって、この定義により、$\mathbb{F}_q$ 上で定義されたすべての楕円曲線は CM を持ちます。これは、フロベニウス自己準同型 $\pi\in\mathrm{End}(E)$ の存在により、$\mathbb{Z}$ より大きい $\mathrm{End}(E)$  が作成されるためです。

しかし、$E$ がCMを持つかどうかを、その下の有限体についてまだ規定せずに議論すると、答えは非自明写像の存在に依存するため、一般にこの問題は非自明となります。
Silvermanの例を用いて説明します。[Sil09, Ch.3, Eg.4.4]

<details><summary>原文</summary>

Example 2.2.7 (Magma script).
Consider $E/K:y^2=x^3+ax$ .
The map $\zeta:(x,y)\mapsto (−x, iy)$ , where $i^2=−1$ in $K$ is an endomorphism, so $E$ has CM.
Clearly, $\zeta$ will be defined over $K$ if and only if $i\in K$ .
Observe that $\zeta\circ\zeta(x,y)=\zeta(−x,iy)=(x,−y)=−(x,y)$ , so $\zeta\circ\zeta=[−1]$ (i.e. $\zeta^2$ is equivalent to negation).
Thus, there is a ring homomorphism $\mathbb{Z}[i]\rightarrow\mathrm{End}(E)$ defined by $m+ni\mapsto [m]+[n]\circ\zeta$ .
If $\mathrm{Char}(K)\ne 0$ , then this map is an isomorphism, thus $\mathrm{End}(E)\cong Z[i]$ , and $\mathrm{Aut}(E)\cong Z[i]^∗$ .

The trace of Frobenius $t$ in Equation $(2.6)$ is named so because of the role it plays in the characteristic polynomial satisfied by $\pi$ , which is given as

$$\pi^2−[t]\circ\pi+[q]=0\qquad\mathrm{in}\ \mathrm{End}(E),\tag{2.8}$$

meaning that for all $(x,y)\in E(\overline{\mathbb{F}}_q)$ , we have

$$(x^{q^2},y^{q^2})−[t](x^q,y^q)+[q](x, y)=\mathcal{O}.\tag{2.9}$$

</details>

例 2.2.7 (Magma script)
$E/K:y^2=x^3+ax$ を考えます。
写像 $\zeta:(x,y)\mapsto (−x, iy)$ 、ここで、$K$ 上の $i^2=−1$ は自己準同型写像なので、$E$ は CM を持ちます。
明らかに、$i\in K$ の場合に限り、$\zeta$ は $K$ に対して定義されます。
$\zeta\circ\zeta(x,y)=\zeta(−x,iy)=(x,−y)=−(x,y)$ なので、$\zeta\circ\zeta=[−1]$ (つまり、$\zeta^2$ は否定に等しい) となります。
したがって、 $m+ni\mapsto [m]+[n]\circ\zeta$ で定義される環準同型 $\mathbb{Z}[i]\rightarrow\mathrm{End}(E)$ が存在します。
$\mathrm{Char}(K)\ne 0$ の場合、この写像は同型写像であるため、 $\mathrm{End}(E)\cong Z[i]$ と $\mathrm{Aut}(E) \cong Z[i]^∗$ .

式 $(2.6)$ のフロベニウス $t$ のトレースは、$\pi$ によって満たされる特性多項式で果たす役割のために、そのように名付けられ、次のように与えられます。

$$\pi^2−[t]\circ\pi+[q]=0\qquad\mathrm{in}\ \mathrm{End}(E),\tag{2.8}$$

つまり、すべての $(x,y)\in E(\overline{\mathbb{F}}_q)$ に対して、以下となります。

$$(x^{q^2},y^{q^2})−[t](x^q,y^q)+[q](x, y)=\mathcal{O}.\tag{2.9}$$

<details><summary>原文</summary>

Example 2.2.8 (Magma script).
We use our results from Example 2.2.5 to illustrate, so as before $E/\mathbb{F}_{67}:y^2=x^3+4x+3$ , $\mathbb{F}_{q^2}=\mathbb{F}_q(u)$ where $u^2+1=0$ , and $\mathbb{F}_{q^3}=\mathbb{F}_q(v)$ where $v^3+2=0$ .
The trace of Frobenius is $t=−11$ , so $\#E(\mathbb{F}_q)=q+1−t=79$ .
For $P_1=(15,50)\in E(\mathbb{F}_q)$ , we trivially had $\pi^2(P_1)=\pi(P_1)=P_1$ , so $P_1−[t]P_1+[q]P_1=([1]−[t]+[q])P_1=[\#E(\mathbb{F}_q)]P_1=\mathcal{O}$ .
For $P_2=(2u+16, 30u+39)$ , we had $\pi^2(P_2)=P_2$ and $\pi(P_2)=(65u+16,37u+39)$ , so we are computing $P_2−[−11]\pi(P_2)+[67]P_2=[68](2u+16,30u+39)+[11](65u+16,37u+39)$ , which is indeed $\mathcal{O}$ .
$P_3\in E(\mathbb{F}_{q^3})$ is the only case where both $\pi$ and $\pi^2$ act non-trivially, so we compute $(19v^2+49v+8,20v^2+66v+21)−[−11](33v^2+14v+8,3v^2+38v+21)+[67](15v^2+4v+8,44v^2+30v+21)$ , which is $\mathcal{O}$ .

We now give a brief sketch of Schoof’s algorithm for counting points on elliptic curves [Sch85].
Understanding the algorithm is not a prerequisite for understanding pairings, but it certainly warrants mention in any overview text on elliptic curves in cryptography, since it is essentially the algorithm that made ECC practical.
Before Schoof’s polynomial-time algorithm, all algorithms for point counting on elliptic curves were exponential and therefore cryptographically impractical.
Besides, to sketch his idea, we need to introduce the notion of division polynomials, which are a useful tool in general.
Put simply, division polynomials are polynomials whose roots reveal torsion points: namely, for odd<sup>3</sup> $\ell$ , the $\ell$-th division polynomial $\phi_\ell(x)$ on $E$ solves to give the $x$-coordinates of the points of order $\ell$ .
They are defined recursively and depend on the curve constants $a$ and $b$ , but rather than giving the recursions here, we point the reader to [Sil09, Ch. III, Exer. 3.7], and opt instead for an example that illustrates their usefulness.

</details>

例 2.2.5 の結果を使用して説明します。前回同様に、$E/\mathbb{F}_{67}:y^2=x^3+4x+3$ 、$\mathbb{F}_{q^2}=\mathbb{F}_q(u)$ ここで $u^2+1=0$ 、および $\mathbb{F}_{q^3}=\mathbb{F}_q(v)$ ここで $v^3+2=0$ です。
フロベニウスのトレースは $t=-11$ なので、$\#E(\mathbb{F}_q)=q+1−t=79$ となります。
$P_1=(15,50)\in E(\mathbb{F}_q)$ に対して、$\pi^2(P_1)=\pi(P_1)=P_1$ が自明なので、$P_1−[t]P_1+[q]P_1=([1]−[t]+[q])P_1=[\#E(\mathbb{F}_q)]P_1=\mathcal{O}$ となります。
$P_2=(2u+16, 30u+39)$ に対して、$\pi^2(P_2)=P_2$ と $\pi(P_2)=(65u+16,37u+39)$ なので、$P_2−[−11]\pi(P_2)+[67]P_2=[68](2u+16,30u+39)+[11](65u+16,37u+39)$ を計算すると、これは確かに $\mathcal{O}$ となります。
$P_3\in E(\mathbb{F}_{q^3})$ は、$\pi$ と $\pi^2$ の両方が非自明に作用する唯一のケースであるため、 $(19v^2+49v+8,20v^2+66v+21)−[−11](33v^2+14v+8,3v^2+38v+21)+[67](15v^2+4v+8,44v^2+30v+21)$ 、つまり  $\mathcal{O}$ となります。

ここで、楕円曲線上の点を数える Schoof のアルゴリズム [Sch85] の簡単なスケッチを示します。
このアルゴリズムを理解することは、ペアリングを理解するための必須条件ではありませんが、ECCを実用化したアルゴリズムであるため、暗号における楕円曲線に関する概説書では必ず言及する必要があります。
Schoof の多項式時間アルゴリズムが登場する前は、楕円曲線上の点を数えるアルゴリズムはすべて指数関数的であり、暗号学的に実用的ではありませんでした。
さらに、彼のアイデアをスケッチするには、一般的に便利なツールである除算多項式の概念を導入する必要があります。
簡単に言えば、除算多項式は根がねじれ点を示す多項式です: つまり、奇数<sup>3</sup> $\ell$ に対して、$E$ 上の $\ell$ 番目の除算多項式 $\phi_\ell(x)$ を解くと、位数 $\ell$ の $x$ 座標が得られます。
それらは再帰的に定義され、曲線定数 $a$ と $b$ に依存しますが、ここで再帰を与えるのではなく、読者に [Sil09, Ch. III, Exer. 3.7] を紹介します、代わりにその有用性を示す例を選択してください。

<sup>3</sup> When $\ell$ is even, the division polynomial is of the form $\psi_\ell(x,y)=y\cdot\tilde{\psi}_\ell(x)$ since $y=0$ gives points of order two, which are in the $\ell$-torsion.
<sup>3</sup> $\ell$ が偶数の場合、除算多項式は $\psi_\ell(x,y)=y\cdot\tilde{\psi}_\ell(x)$ の形式になります。これは、$y=0$ が $\ell$ ねじれにある位数 2 の点を与えるためです。


<details><summary>原文</summary>

Example 2.2.9 (Magma script). 
Recall the curve $E/\mathbb{F}_{101}:y^2=x^3+x+1$ from Example 2.2.1 with group order $\#E(\mathbb{F}_q)=105=3\cdot 5\cdot 7$ .
The $x$-coordinates of the points of order $2$ are found as the roots of $\psi_2(x)=4x^3+4x+4$ , which is irreducible in $\mathbb{F}_q[x]$ , so there are no $2$-torsion points in $E(\mathbb{F}_q)$ .
For $r=3$ , $\psi_3(x)=3x^4+6x^2+12x+100\in\mathbb{F}_q[x]$ factors into $\psi_3(x)=(x+73)(x+84)(x^2+45x+36)$ , so we get two solutions over $\mathbb{F}_q$ , namely $x=17$ and $x=28$ .
This does not mean that the points implied by both solutions are in $\mathbb{F}_q$ : namely, $x=28$ gives $x^3+x+1\in\mathrm{QR}(q)$ , so two points in the $3$-torsion follow as $(28,8)$ and $(28,93)$ .
Conversely, $x=17$ gives $x^3+x+1\notin\mathrm{QR}(q)$ , so the two points implied by $x=17$ will be defined over $\mathbb{F}_{q^2}$ .
For $\psi_5(x)=5x^{12}+\ldots +16$ , the factorisation in $\mathbb{F}_q[x]$ is $\psi_5(x)=(x+15)(x+55)(x^5+\ldots +1)(x^5+\ldots +100)$ , which gives $x=46$ and $x=86$ as solutions.
This time, both $x$ values give rise to two points, giving four non-trivial $5$-torsion points in total: $(46, 25)$, $(46, 76)$, $(86, 34)$, $(86, 67)$.
$\psi_7(x)$ is degree $24$ , and gives three linear factors in $\mathbb{F}_q[x]$ , all of which result in two $7$-torsion points, giving $6$ non-trivial torsion points in total: $(72, 5)$, $(72, 96)$, $(57, 57)$, $(57, 44)$, $(3, 43)$, $(3, 58)$.
Other division polynomials have roots in $\mathbb{F}_q[x]$ , but these roots will not give rise to points defined over $\mathbb{F}_q[x]$ .
For example, $\psi_{11}(x)$ has $5$ roots over $\mathbb{F}_q$ $(13,18,19,22,63)$ , but none of them give points in $E(\mathbb{F}_q)$ , meaning we will have to extend to $E(\mathbb{F}_{q^2})$ to collect any $11$-torsion points.
The only division polynomials whose roots produce points defined over $\mathbb{F}_q$ are the $\psi_d(x)$ with $d|105$ .
This generalises to imply that the only division polynomials whose roots produce points defined over Fqn are ψd(x), where d | #E(Fqn ).

</details>

例 2.2.9 (Magma script)

群位数 $\#E(\mathbb{F}_q)=105=3\cdot 5\cdot 7$ の例 2.2.1 から曲線、$E/\mathbb{F}_{101}:y^2=x^3+x+1$ を思い出します。
位数 $2$ の点の $x$ 座標は $\psi_2(x)=4x^3+4x+4$ の根として求まり、これは $\mathbb{F}_q[x]$ において既約なので、$E(\mathbb{F}_q)$ には$2$-ねじれ点は存在しないことになります。
$r=3$ にたいして、 $\psi_3(x)=3x^4+6x^2+12x+100\in\mathbb{F}_q[x]$ は $\psi_3(x)=(x+73)(x+84)(x^2+45x+36)$ になるので、$\mathbb{F}_q$ 上の2つの解、すなわち $x=17$ と $x=28$ が得られます。
これは、両方の解によって暗示される点が $\mathbb{F}_q$ にあることを意味するものではありません。つまり、$x=28$ は $x^3+x+1\in\mathrm{QR}(q)$ を与えるため、$3$ ねじれの $2$ つの点は $(28,8)$ および $(28,93)$ のようになります。
逆に、$x=17$ は $x^3+x+1\notin\mathrm{QR}(q)$ となるため、$x=17$ によって暗示される 2 つの点は $\mathbb{F}_{q^2}$ で定義されます。
$\psi_5(x)=5x^{12}+\ldots +16$ の場合、$\mathbb{F}_q[x]$ の因数分解は $\psi_5(x)=(x+15)(x+55)(x^5+\ldots +1)(x^5+\ldots +100)$ となり、解として $x=46$ と $x=86$ が得られます。
今回は、両方の $x$ 値が 2 つのポイントを生成し、合計で 4 つの非自明な $5$ ねじれポイントを与えます: $(46, 25)$、$(46, 76)$、$(86, 34)$、$(86, 67)$。
$\psi_7(x)$ は位数 $24$ で、$\mathbb{F}_q[x]$ に3つの線形因子を与え、そのすべてが2つの $7$ ねじれ点となり、合計 $6$ つの非自明ねじれ点を与えられます： $(72, 5)$, $(72, 96)$, $(57, 57)$, $(57, 44)$, $(3, 43)$, $(3, 58)$。
他の除算多項式は $\mathbb{F}_q[x]$ に根を持ちますが、これらの根は $\mathbb{F}_q[x]$ で定義された点を生じさせません。
例えば、$\psi_{11}(x)$ は $\mathbb{F}_q$ 上で $5$ つの根 $(13,18,19,22,63)$ を持っていますが、どれも $E(\mathbb{F}_q)$ に点を与えないので、 $11$-ねじれ点を集めるには $E(\mathbb{F}_{q^2})$ に拡張しなければならないことを意味します。
$\mathbb{F}_q$ 上で定義された点を根に持つ除算多項式は、$d|105$ の $\psi_d(x)$ のみです。
これは、根が $\mathbb{F}_{q^n}$ 上で定義された点を生成する唯一の除算多項式が $\psi_d(x)$ であることを意味するように一般化されます。 ここで、$d|\#E(\mathbb{F}_{q^n})$ です。

<details><summary>原文</summary>

We are now in a position to shed light on Schoof’s algorithm.
Equation $(2.6)$ means that computing $E(\mathbb{F}_q)$ immediately reduces to computing the (much smaller) trace of Frobenius, $t$ .
At the highest level, Schoof’s idea is to compute $t_\ell\equiv t\mod\ell$ for enough co-prime $\ell$’s to be able to uniquely determine $t$ within the interval $−2\sqrt{q}\leq t\leq 2\sqrt{q}$ via the Chinese Remainder Theorem.
Namely, when $\prod_\ell t_\ell\geq 4\sqrt{q}$ , then we have enough relations to determine the correct $t$ .
To compute $t_\ell$ for various primes $\ell$ , Schoof looked to consider Equation $(2.9)$ “modulo $\ell$”, restricting the points $(x, y)$ to come from the $\ell$-torsion, and trying to solve

$$(x^{q^2},y^{q^2})−[t_{\ell}](x^q,y^q)+[q_\ell](x,y)=\mathcal{O},\tag{2.10}$$

for $t_\ell$ , where $q_\ell\equiv q\mod\ell$ .
The problem for general $\ell$ is, that since we do not know the group order, we cannot explicitly use $\ell$-torsion points in $(2.10)$, nor do we know if they are even defined over $\mathbb{F}_q$ , or where they are defined, so we have to work with $(2.10)$ implicitly.
Namely, we restrict $(2.10)$ to the $\ell$-torsion by working modulo $\psi_\ell(x)$ : we do not work with Equation $(2.10)$ on $E(\mathbb{F}q)$ , but rather in the polynomial ring $R_\ell =\mathbb{F}_q[x,y]/\langle\psi_\ell(x),y^2−(x^3+ax+b)\rangle$ , where the size of the polynomials $f(x,y)$ we deal with in $R_\ell$ are bounded by the degrees of the division polynomials $\psi_\ell(x)$ .
Even for very large prime fields $\mathbb{F}_q$ of cryptographic size, the number of different primes used is small enough to keep this algorithm very practical.
For example, finding the group order of the curve defined over a 256-bit prime q in Example 2.2.3 would require solving $(2.10)$ for the $27$ primes up to $\ell=107$, at which point the product of all the primes used exceeds $4\sqrt{q}$ .
It is not too difficult to deduce that the asymptotic complexity of Schoof’s algorithm is $O((\log q)^8)$ (see [Sil09, Ch. XI.3] for details, and further improvements).

</details>

これで、Schoof のアルゴリズムに光を当てることができるようになりました。
式 $(2.6)$ は、$E(\mathbb{F}_q)$ を計算するとすぐに、フロベニウス $t$ の (はるかに小さい) トレースを計算することになることを意味します。
最高レベルでは、十分な数の互いに素の $\ell$ ついて $t_\ell\equiv t\mod\ell$ を計算し、中国余弦定理によって区間 $−2\sqrt{q}\leq t\leq 2\sqrt{q}$ 内の $t$ を一意に決定できるようにすることがSchoofのアイデアです。
つまり、$\prod_\ell t_\ell\geq 4\sqrt{q}$ のとき、正しい $t$ を決定するのに十分な関係が得られます。
さまざまな素数 $\ell$ の $t_\ell$ を計算するために、Schoof は式 $(2.9)$「モジュロ $\ell$」を考慮し、点 $(x,y)$ を $\ell$-ねじれから来るように制限し、 $t_\ell$ に対して

$$(x^{q^2},y^{q^2})−[t_{\ell}](x^q,y^q)+[q_\ell](x,y)=\mathcal{O},\tag{2.10}$$

を解こうとしました。ここで、 $q_\ell\equiv q\mod\ell$ です。
一般的な $\ell$ の問題は、群の位数がわからないため、$(2.10)$ で $\ell$ ねじれ点を明示的に使用できないこと、またそれらが $\mathbb{F}_q$ で定義されているかどうか、またはそれらがどこで定義されているかがわからないことです。 $(2.10)$ を暗黙的に操作する必要があります。
すなわち、モジュロ $\psi_\ell(x)$ を操作することにより、$(2.10)$ を $\ell$-ねじれに制限します。 $E(\mathbb{F}q)$ の式 $(2.10)$ ではなく、多項式環 $R_\ell =\mathbb{F}_q[x,y]/\langle\psi_\ell(x),y^2−(x^3+ax+b)\rangle$ ここで、$R_\ell$ で扱う多項式 $f(x,y)$ のサイズは、除算多項式 $\psi_\ell(x)$ の位数によって制限されます。
暗号学的な大きさの非常に大きな素数体 $\mathbb{F}_q$ であっても、使用する素数の種類が少ないので、このアルゴリズムは非常に実用的です。
例えば、例2.2.3の256ビットの素数 $q$ で定義された曲線の群位数を求めるには、$\ell=107$ までの $27$ 個の素数について $(2.10)$ を解く必要がありますが、その時点で使用するすべての素数の積が $4\sqrt{q}$ を超えてしまうのです。
Schoofのアルゴリズムの漸近的な複雑さが $O((\log q)^8)$ であることを推論するのはそれほど難しくありません（詳細とさらなる改善については[Sil09, Ch. XI.3] を参照してください）。

<details><summary>原文</summary>

Example 2.2.10 (Magma script).
Consider $E/\mathbb{F}_{13}:y^2=x^3+2x+1$ ; we seek $\#E(\mathbb{F}_{13})$ .
Schoof’s algorithm actually begins with $\ell=3$ [Sil09, Ch. XI.3]; so since $14<4\sqrt{13}<15$ , we only need to solve $(2.10)$ with $\ell=3$ and $\ell=5$ .
For $\ell=3$ , $\psi_3(x)=3x^4+12x^2+12x+9$ , so we work in the ring $R_3=\mathbb{F}_q[x,y]/\langle 3x^4+12x^2+12x+9,y^2−(x^3+2x+1)\rangle$ with $q_\ell=1$ , to find that $t_3=0$ .
For $\ell=5$ , $\psi_5(x)=5x^{12}+\ldots +6x+7$ , so we work in the ring $R_5=\mathbb{F}_q[x,y]/\langle 5x^{12}+\ldots +6x+7,y^2−(x^3+2x+1)\rangle$ with $q_\ell=3$ to find that $t_5=1$ .
For both cases we had to compute $[q_\ell](x,y)$ in $R_\ell$ using the affine formulas $(2.4)$ and $(2.5)$ , compute $(x^q,y^q)$ and $(x^{q^2},y^{q^2})$ in $R_\ell$ , and then test incremental values of $t_\ell$ until $[t_\ell](x^q,y^q)$ (also computed with the affine formulas) satisfies $(2.10)$ .
The CRT with $t\equiv 0\mod{3}$ and $t\equiv 1\mod{5}$ gives $t\equiv 6\mod{15}$ , which combined with $−7\leq t\leq 7$ means $t=6$ , giving $\#E=q+1−t=8$ .

We finish this chapter by briefly discussing one more improvement to ECC that will essentially bring the reader up to speed with major milestones that contribute to the current state-of-the-art implementations.
The technique was introduced by Gallant, Lambert and Vanstone (GLV) [GLV01], and recently generalised by Galbraith, Lin and Scott (GLS) [GLS11].
It exploits the existence of an efficiently computable endomorphism $\psi$ that allows us to instantly move $P$ to a large multiple $\psi(P)=[\lambda]P$ of itself, so that (in the simplest case) the scalar multiplication $[m]P$ can be split into $[m]P=[m_0]P+[m_1]\psi(P)$ , where if $|m|\approx r$ (the large subgroup order), then $|m_0|,|m_1|\approx\sqrt{r}$ .
The values $m_0$ and $m_1$ are found by solving a closest vector problem in a lattice [GLV01, §4].
We apply an example from the GLV paper (which was itself taken from Cohen’s book [Coh96, §7.2.3]) that is actually exploiting a special case of the endomorphism we described in Example 2.2.7.

</details>

例 2.2.10 (Magma script)
$E/\mathbb{F}_{13}:y^2=x^3+2x+1$ を考えます。$\#E(\mathbb{F}_{13})$ を求めます。
Schoof のアルゴリズムは、実際には $\ell=3$ で始まります [Sil09, Ch. XI.3]; $14<4\sqrt{13}<15$ なので、 $\ell=3$ と $\ell=5$ で $(2.10)$ を解くだけで済みます。
$\ell=3$ に対して $\psi_3(x)=3x^4+12x^2+12x+9$ 、なので $q_\ell=1$ の環 $R_3=\mathbb{F}_q[x,y]/\langle 3x^4+12x^2+12x+9,y^2−(x^3+2x+1)\rangle$ で作業し、 $t_3=0$ を求めます。
$\ell=5$ に対して $\psi_5(x)=5x^{12}+\ldots +6x+7$ 、なので $q_\ell=3$ の環 $R_5=\mathbb{F}_q[x,y]/\langle 5x^{12}+\ldots +6x+7,y^2−(x^3+2x+1)\rangle$ で作業し、 $t_5=1$ を求めます。
どちらの場合も、アフィン公式 $(2.4)$ と $(2.5)$ を使用して $R_\ell$ の $[q_\ell](x,y)$ を計算し、$R_\ell$ の $(x^q,y^q)$ と $(x^{q^2},y^{q^2})$ を計算する必要がありました。
次に、 $[t_\ell](x^q,y^q)$ (これもアフィン式で計算されます) が $(2.10)$ を満たすまで、$t_\ell$ の増分値をテストします。
$t\equiv 0\mod{3}$ および $t\equiv 1\mod{5}$ の CRT は $t\equiv 6\mod{15}$ を与え、これを $−7\leq t\leq 7$ と組み合わせると $t=6$ を意味し、 $\#E=q+1−t=8$ が得られます。

この章を締めくくるには、ECC のもう 1 つの改善点について簡単に説明します。これにより、読者は現在の最先端の実装に貢献する主要なマイルストーンを本質的に理解できるようになります。
この手法は、Gallant、Lambert、および Vanstone (GLV) [GLV01] によって導入され、最近 Galbraith、Lin、および Scott (GLS) [GLS11] によって一般化されました。
これは、効率的に計算可能な自己準同型写像 $\psi$ の存在を利用して、$P$ をそれ自体の大きな倍数 $\psi(P)=[\lambda]P$ に即座に移動できるようにするため、(最も単純なケースでは) スカラー倍数 $[m]P$ は次のようになります。 $[m]P=[m_0]P+[m_1]\psi(P)$ に分割されます。ここで $|m\approx r|$ の場合(大きな部分群位数) の場合、$|m_0|,|m_1|\approx\sqrt{r}$ となります。
値 $m_0$ と $m_1$ は、格子の最近接ベクトル問題を解くことによって求められます [GLV01、§4]。
例 2.2.7 で説明した準同形性の特殊なケースを実際に利用している GLV の論文 (Cohen の本 [Coh96, §7.2.3] から引用したもの) の例を適用します。

<details><summary>原文</summary>

Example 2.2.11 (Magma script).
Let $q\equiv 1\mod{4}$ be prime, $E/\mathbb{F}_q:y^2=x^3+ax$ , and let $i^2=−1$ .
The map defined by $\psi:(x,y)\mapsto (−x,iy)$ and $\psi :\mathcal{O}\mapsto\mathcal{O}$ is an endomorphism defined over $\mathbb{F}_q$ ( $\psi=\xi$ from 2.2.7).
Let $P\in E(\mathbb{F}_q)$ have prime order $r$ , then $\psi(Q)=[\lambda]Q$ for all $Q\in \langle P\rangle$ , and $\lambda$ is the integer satisfying $\lambda^2=−1\mod{r}$ .
We give a specific example: $q=1048589$ , $E/\mathbb{F}_q:y^2=x^3+2x$ with $\#E=2r$ , where $r=524053$ ; we further have $i=38993$, and $\lambda=304425$ .
$P=(609782,274272)\in E$ has $|\langle P\rangle|=r$ , so we can take any element in $\langle P\rangle$ , say $Q=(447259,319154)$ , and compute $\psi(Q)=(−447259,i\cdot 319154)=(601330,117670)=[304425](447259, 319154)=[\lambda]Q$ .
Computing a random multiple of $Q$ , say $[m]Q$ with $m=103803$ , can be done by decomposing $m$ into (in this case) $(m_0,m_1)=(509,262)$ , and instead computing $[m]Q=[m_0]Q+[m_1]\psi(Q)$ .
Here $m$ is $17$ bits, whilst $m_0$ and $m_1$ are both $9$ bits.
Doing the scalar multiples $[m_0]Q$ and $[m_1]\psi(Q)$ separately would therefore give no savings, but where the GLV/GLS methods gain a substantial speed-up is in merging the doublings required in both of the multiplications by the “mini-scalars”, which halves the number of doublings required overall; again, see [GLV01, GLS11] for futher details.

</details>

例 2.2.11 (Magma script)
素数 $q\equiv 1\mod{4}$ 、$E/\mathbb{F}_q:y^2=x^3+ax$、$i^2=−1$ とします。
$\psi:(x,y)\mapsto (−x,iy)$ と $\psi :\mathcal{O}\mapsto\mathcal{O}$ で定義される写像は、$\mathbb{F}_q$ (2.2.7 からは $\psi=\xi$) に対して定義された自己同型写像です。
$P\in E(\mathbb{F}_q)$ が素数位数 $r$ を持つとすると、すべての $Q\in \langle P\rangle$ に対して $\psi(Q)=[\lambda]Q$ であり、$\lambda$ は $\lambda^2=−1\mod{r}$ を満たす整数です。
具体的な例として、$q=1048589$ で $E/\mathbb{F}_q:y^2=x^3+2x$ は $\#E=2r$ です、ここで $r=524053$ であるとき、$i=38993$ 、 $\lambda=304425$ となります。
$P=(609782,274272)\in E$ は $|\langle P\rangle|=r$ となるため、 $\langle P\rangle$ の任意の要素を取得できます。たとえば、 $Q=(447259,319154)$ とすると、 $\psi(Q)=(−447259,i\cdot 319154)=(601330,117670)=[304425](447259, 319154)=[\lambda]Q$ と計算されます。
$Q$ のランダムな倍数、たとえば $m=103803$ で $[m]Q$ を計算するには、$m$ を (この場合) $(m_0,m_1)=(509,262)$ に分解し、 代わりに $[m]Q=[m_0]Q+[m_1]\psi(Q)$ を計算します。
ここで、$m$ は $17$ ビットですが、$m_0$ と $m_1$ は両方とも $9$ ビットです。
スカラー倍算 $[m_0]Q$ と $[m_1]\psi(Q)$ を別々にやっても節約にはなりませんが、GLV/GLS メソッドが大幅にスピードアップするのは、「ミニスカラー（mini-scalars）」による両方の乗算で必要な倍算を統合することで、全体で必要な倍算の数を半分にするからです、また、より詳しいことは [GLV01, GLS11] で説明しています。

## 2.3 （章のまとめ）Chapter summary

<details><summary>原文</summary>

We defined the elliptic curve group law ⊕ via the chord-and-tangent method, and discussed that elliptic curve groups are an attractive setting for discrete-log based cryptosystems because of the relative security obtained for the sizes of the fields they are defined over.
We also exemplified many improvements in the context of cryptographic implementations, where the fundamental operation (that creates ECDLP instances) is computing large scalar multiples $[m]P$ of $P\in E$.
Namely, we showed that group law computations in finite fields can be much faster in projective coordinates, i.e. computing $(X_1:Y_1:Z_1)\oplus(X_2:Y_2:Z_2)$ rather than $(x_1,y_1)\oplus (x_2,y_2)$ , and that other (non-Weierstrass) curve models also offer advantages.
We gave an explicit equation for the number of points in $E(\mathbb{F}q)$ , and briefly discussed Schoof’s polynomial-time algorithm that facilitates point counting on curves of cryptographic size.
We also introduced the notion of the endomorphism ring $\mathrm{End}(E)$ of $E$ , and finished by showing that non-trivial elements of  $\mathrm{End}(E)$ can be used to further accelerate ECC.
A reader that is comfortable with the exposition in this chapter is equipped with many of the tools required to tackle the vast literature in this field, and is somewhat up-to-date with the state-of-the-art ECC implementations.
For example, in the context of chasing ECC speed records, some authors have applied alternative projective coordinate systems to the Edwards model to give very fast scalar multiplications [HWCD08], whilst others have investigated higher dimension GLV/GLS techniques (Example 2.2.11 above was 2-dimensional) to gain big speed-ups [HLX12]; visit http://bench.cr.yp.to/supercop.html for comprehensive and up-to-date benchmarkings of a wide number of implementations that are pushing ECC primitives to the limit.

**Relaxed notation.**
Our last order of business before proceeding into the next chapter is to relax some notation in order to agree with the rest of the literature.
Rather than writing “$\oplus$” for the elliptic curve group law, from hereon we simply use “$+$”.
Similarly, for the inverse of the point $P$, we use $−P$ instead of $\ominus P$.

</details>

楕円曲線群律 $\oplus$ を 弦と接線（chord-and-tangent）メソッドで定義し、楕円曲線群はその上で定義される体の大きさに対して得られる相対的安全性から、離散対数ベースの暗号システムにとって魅力的な設定であること説明しました。
また、基本的な操作 (ECDLP インスタンスを作成する) として $P\in E$ の大きなスカラー倍数 $[m]P$ を計算する暗号実装のコンテキストで多くの改善を例示しました。
すなわち、有限体における群律計算は、射影座標において、$(x_1,y_1)\oplus (x_2,y_2)$ ではなく、 $(X_1:Y_1:Z_1)\oplus(X_2:Y_2:Z_2)$ の計算がはるかに速くできること、他の（非Weierstrass）曲線モデルも有利であることを示しました。
 $E(\mathbb{F}q)$ の点の数に関する明示的な方程式を与え、暗号サイズの曲線での点の数の計算を容易にするSchoofの多項式時間アルゴリズムについて簡単に説明しました。
また、$E$ の自己準同型環 $\mathrm{End}(E)$ の概念を導入し、$\mathrm{End}(E)$ の重要な要素を使用して ECC をさらに高速化できることを示して終了しました。
この章の説明に慣れている読者は、この分野の膨大な文献に取り組むために必要な多くのツールを備えており、最先端の ECC 実装についてある程度最新の知識を持っています。
例えば、ECC の速度記録を追い求める文脈で、ある著者は Edwards モデルに別の射影座標系を適用して非常に高速なスカラー倍算を実現しました [HWCD08]。一方、他の著者は高次元の GLV/GLS 技術 (上記の例 2.2.11 は2次元) について調べ、大きな速度向上を達成しました [HLX12] 。ECC 基 本技術に限界を迫る多数の実装に関する包括的で最新のベンチマークに ついては http://bench.cr.yp.to/supercop.html でご確認ください。

**ゆったり表記**
次の章に進む前の最後の仕事は、残りの文献に同意するためにいくつかの表記法を緩和することです。
楕円曲線群律を「$\oplus$」と書かず、以後は単に「$+$」と表記します。
同様に、点 $P$ の逆数には、$\ominus P$ の代わりに $-P$ を使用します。

# まとめ（Conclusion）

<details><summary>原文</summary>
</details>



