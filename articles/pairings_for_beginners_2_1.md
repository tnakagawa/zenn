---
title: "初心者のためのペアリング 2-1"
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

