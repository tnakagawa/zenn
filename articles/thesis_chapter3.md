---
title: "ペアリングベース暗号システムの実装について 3"
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


# Chapter 3<br>WeilとTateのペアリング（The Weil and Tate Pairings）

<details>
<summary>原文</summary>
The Weil and Tate pairings take $r$-torsion points as input, and in the case of the Weil pairing, both inputs are $r$-torsion points.
They can be defined using rational functions.
They output an element of a finite field that is an rth root of unity.
We shall state facts whose proofs can be found in many textbooks on elliptic curves such as Silverman [62].
</details>

WeilとTateのペアリングは$r$-ねじれ点を入力とし、Weilのペアリングの場合、両方の入力が$r$-ねじれ点である。
有理関数を用いて定義することができる。
また、有限体において、$r$-番目の統一根となる要素を出力する。
その証明はSilverman [62]などの楕円曲線に関する多くの教科書に載っている事実を述べよう。

## 3.1 ねじれ点（Torsion Points）

<details>
<summary>原文</summary>
Let $K$ be a finite field of characteristic $q$, so that $K = \mathbb{F}_{q^m}$ for some natural number $m$.
Let $E$ be an elliptic curve defined over $K$.
Let $n = \#E(K)$ Suppose $P \in E(K)$ satisfies $rP = O$, so that $P$ has order $r$ or a factor of $r$.
We call $P$ an $r$-torsion point.
We denote the set of $r$-torsion points in $E(K)$ by $E(K)[r]$.

For the curves we shall consider, $n$ and $q$ are always coprime thus $r$ is also coprime to $q$ (since $r|n$).
The case when $r$ is not coprime to $q$ leads to the anomalous attack [9, Section V.3] which breaks discrete log in linear time.

It can be proved that for some integer $k \geq 1$, $E(\mathbb{F}_{q^k})[r]$ contains exactly $r^2$ points and is in fact the direct product of two cyclic groups of order $r$, that is, isomorphic to $\mathbb{Z}_r \times \mathbb{Z}_r$.
Furthermore, for all $k′ \geq k$ we have $E(\mathbb{F}_{q^{k^′}})[r] = E(\mathbb{F}_{q^k})[r]$.
Roughly speaking, there are no other $r$-torsion points beyond the first $r^2$ points found, no matter how many times we extend the field.
(That is, if $\tilde{K}$ is the algebraic closure of $K$ then $\#E(\tilde{K})[r] = r^2$.)
We define $E[r] = E(\mathbb{F}_{q^k})[r]$, the set of $r$-torsion points of $E$.

Thus the above isomorphism may be written

$$E[r] \cong \mathbb{Z}_r \times \mathbb{Z}_r.$$

When $r=2$ this is easy to see: a point has order $2$ if and only if it has a zero y-coordinate.
Since $E$ is nonsingular, $x^3 + ax + b = 0$ has three distinct solutions, thus we can always find some field $\mathbb{F}_{q^k}$ here $E$ has four points of order $2$: the point $O$ and three points of the form $(\alpha, 0)$ where $\alpha$ is a root of the cubic.
Since the line through any two of the finite points is simply the line $Y = 0$, which certainly intersects $E$ at the other finite point, we have $E[2] \cong \mathbb{Z}_2 \times \mathbb{Z}_2$.
The proof is much less trivial for general $r$.

The Weil pairing is a bilinear map takes pairs of elements from $E[r]$ and outputs an rth root of unity in $\mathbb{F}_{q^k}$ .
The Tate pairing is similar but only the first input is from $E(\mathbb{F}_q)[r]$.
</details>

$K$ を標数 $q$ の有限体であるとし、ある自然数 $m$ に対して $K = \mathbb{F}_{q^m}$ となるようにする。
$E$ を $K$ 上で定義された楕円曲線とする。
$n = \#E(K)$ とすると $P \in E(K)$ は $rP = O$ を満たすので、$P$ は $r$ の位数または因数を持っているとする。
ここで、$P$ を $r$-ねじれ点と呼ぶ。
$E(K)$ における $r$-ねじれ点の集合を $E(K)[r]$ と表記する。

これから考える曲線では、$n$と$q$は常に互いに素なので、$r$も$q$と互いに素である（$r|n$のため）。
$r$ が $q$ と互いに素でない場合、線形時間で離散対数を破る異常攻撃[9, Section V.3]が発生する。

$E(\mathbb{F}_{q^k})[r]$ はある整数 $k \geq 1$ に対して、ちょうど $r^2$ 個の点を含み、実際 $r$ 位の2つの巡回群の直積、すなわち $\mathbb{Z}_r \times \mathbb{Z}_r$ と同型のものであることが証明されました。
さらに、すべての $k^{\prime} \geq k$ に対して、 $E(\mathbb{F}_{q^{k^\prime}})[r] = E(\mathbb{F}_{q^k})[r]$ が成立することがわかります。

大雑把に言うと、最初に見つかった $r^2$ 点以上の $r$-ねじれ点は、何度場を拡張しても存在しないのです。
(つまり、$\tilde{K}$ が $K$ の代数的閉包であれば、$\#E( \tilde{K})[r] = r^2$ となる)。
$E$ の $r$ 個のねじれ点 の集合である $E[r]=E( \mathbb{F}_{q^k})[r]$ を定義します。

したがって、上記の同型性は次のように書くことができる。

$$E[r] \cong \mathbb{Z}_r \times \mathbb{Z}_r.$$

$r=2$ のとき、これは簡単で、点は $y$座標が0であるときのみ、位数 $2$ となる。
$E$ は非特異なので、$x^3 + ax + b = 0$ は3つの異なる解を持ち、したがって、ここで $E$ には必ず $\mathbb{F}_{q^k}$ の4つの点（点 $O$ と $\alpha$ が立方根の形の3点$( \alpha, 0)$ ）があって位数 $2$のものがあることが分かる。
有限点のうち任意の2点を通る線は単に直線 $Y=0$ であり、これは必ず他の有限点で $E$ と交差するので、$E[2] \cong \mathbb{Z}_2 \times \mathbb{Z}_2$ が成り立つ。
一般的な $r$ では、証明はもっと簡単である。

Weilペアリングは、$E[r]$ から要素の組を取り、$\mathbb{F}_{q^k}$ の $r$-番目の単根を出力する双線形写像である。
Tateペアリングも同様ですが、最初の入力は $E(\mathbb{F}_q)[r]$ からのもののみです。

## 3.2 有理関数（Rational Functions）

<details>
<summary>原文</summary>
We study the behaviour of quotients of polynomials in two variables on points on an elliptic curve.
Denote the ring of polynomials in two variables $X$, $Y$ with coefficients in $\mathbb{F}_{q^k}$ by $\mathbb{F}_{q^k}[X, Y]$.

Let $E$ be an elliptic curve $Y^2 = X^3+aX +b$ over $\mathbb{F}_{q^k}$.
Write $E(X, Y)$ for the polynomial $Y^2 − X^3 − aX − b$.
It can be shown that a polynomial $f$ satisfies $f(X, Y) = 0$ whenever $E(X, Y) = 0$ if and only if $f(X, Y)$ is a multiple of $E(X, Y)$

Thus define $\mathbb{F}_{q^k}[E] = \mathbb{F}_{q^k}[X, Y]_{E(X,Y)}$.
In other words we look at all possible polynomials and consider two of them to be equivalent if they take the same values on all points of $E$.
Every element $f(X, Y)$ in $\mathbb{F}_{q^k}[E]$ can be written in the form $f_x(X)+Yf_y(X)$ for polynomials $f_x, f_y \in \mathbb{F}_{q^k}[X]$ because we may replace $Y^2$ by $X^3 + aX + b$.

Define $\mathbb{F}_{q^k}(E)$ to be the field of fractions of $\mathbb{F}_{q^k}[E]$.
Elements of $\mathbb{F}_{q^k}(E)$ can be written in the form $f(X,Y)/g(X,Y)$ where $f$, $g$ are polynomials in two variables $X$, $Y$ and $g$ is not a multiple of $E$.
</details>

楕円曲線上の点における2変数の多項式の商の振る舞いを研究する。
2変数 $X$, $Y$ で係数が $\mathbb{F}_{q^k}$ の多項式の環を $\mathbb{F}_{q^k}[X, Y]$と定義する。

$E$ を $\mathbb{F}_{q^k}$ 上の $Y^2 = X^3+aX +b$ 楕円曲線とする。
多項式 $Y^2 - X^3 - aX - b$ を $E(X, Y)$ と書く。
多項式 $f$ が $f(X, Y) = 0$ を満たすのは $f(X, Y)$ が $E(X, Y)$ の倍数のときだけであり、$E(X, Y) = 0$ のときはいつでも満たすことが示される。

したがって、$\mathbb{F}_{q^k}[E] = \mathbb{F}_{q^k}[X, Y]_{E(X,Y)}$ と定義します。
つまり、すべての可能な多項式を調べて、$E$ のすべての点で同じ値をとるものを等価と見なすのです。
$\mathbb{F}_{q^k}[E]$ のすべての要素 $f(X, Y)$ は、$Y^2$ を $X^3 + aX + b$ で置き換えれられるので、多項式 $f_x, f_y \in \mathbb{F}_{q^k}[X]$ に対して $f_x(X)+Yf_y(X)$ という形で書くことができる。

$\mathbb{F}_{q^k}(E)$ を $\mathbb{F}_{q^k}[E]$ の分数の場と定義する。
ここで、$f$, $g$ は2変数 $X$, $Y$ の多項式であり、$g$ は $E$ の倍数でないものとする。

## 3.3 曲線の自己準同型（Curve Endomorphisms）

<details>
<summary>原文</summary>
Given a point $P$ and integer $m$ consider the ”multiplication-by-m” map $[m]$ given by

$$P \mapsto mP.$$

If $P$ is viewed as a pair of variables $(X, Y)$, this map can be written as $(f(X, Y), g(X, Y))$ for some rational functions $f$, $g$.
Using the explicit formulas for point addition and doubling given in the previous chapter, we can write down explicit formulas for $f$ and $g$ for any $m$, but even for relatively small $m$ the expressions are unwieldy.

This is an example of a curve endomorphism, because points of $E(\mathbb{F}_{q^k})$ are mapped to points of $E(\mathbb{F}_{q^k})$, and furthermore the map preserves point addition.
Another important example is the $q$-th power Frobenius map $\Phi$, given by $(X, Y) \mapsto (X^q, Y^q)$. 
In general one defines $q^k$-th power Frobenius maps but we only define the $q$-th power map, and if we need higher powers k we shall write $\Phi^k$.

We shall also need the trace map, which is defined by

$$\mathrm{tr} P = P + \Phi (P) + \ldots + \Phi^{k−1}(P)$$

which takes points of $E(\mathbb{F}_{q^k})$ to $E(\mathbb{F}_q)$.

We state another well-known theorem from the theory of elliptic curves.
Below, $f \circ g$ denotes the map defined by $P \mapsto f(g(P))$.

**Theorem** (Hasse).

$$\Phi^k \circ \Phi^k − [t] \circ \Phi^k + [q^k] = [0]$$

where $t = q^k + 1 − \#E(\mathbb{F}_{q^k})$.

Since $E[r] \cong \Z_r \times \Z_r$ any curve endomorphism that maps $E[r]$ to $E[r]$ (which is true for Frobenius and multiplication-by-m maps) can be viewed as a $2 \times 2$ matrix when restricted to $E[r]$.

In particular for a prime $r$ dividing $\#E(\mathbb{F}_q)$ the eigenvalues of $\Phi$ are the solutions to $x^2 − tx + q \pmod{r}$.
The polynomial factorizes as $(x−q)(x−1)$ thus any eigenvalues must be $q$ or $1$.
We can describe the corresponding eigenspaces.

Clearly $E(\mathbb{F}_q)[r]$ is a 1-eigenspace, hence if $E[r] \subset E(\mathbb{F}_q)$ then $\Phi$ is simply the identity.
Otherwise consider the set $G=\{P|\mathrm{tr}P=O\}$.

We know this set contains more than just the point at infinity since for any $P \in E[r] \backslash E(\mathbb{F}_q)[r]$, the point $P− \Phi(P)$ is nontrivial and has trace zero.
Straightforward computations show that $G$ is closed under the group laws, and also show that $\Phi(G)=G$.
Hence $G$ is an eigenspace.

As $G$ cannot be the $1$-eigenspace (which has already been accounted for; $E(\mathbb{F}_q)[r]$ is the 1-eigenspace), $G$ must be the $q$-eigenspace, which implies $|G|=r$.
This result is important in Section 6.4, so we restate it:

**Theorem.** The set of points of trace zero $G={P| \mathrm{tr} P = O}$ is a cyclic group of order $r$, and every $P \in G$ satisfies $\Phi(P) = qP$.
</details>

点 $P$ と整数 $m$ が与えられたとき、次のように与えられる「mによる乗算」写像$[m]$を考える。

$$P \mapsto mP.$$

$P$ を変数 $(X, Y)$ のペアと見なすと、この写像はある有理関数 $f$, $g$ に対して $(f(X, Y), g(X, Y))$ と書くことができる。
前章で示した点の加算と倍加の明示式を使って、任意の $m$ に対して $f$ と $g$ の明示式を書き下せばよいが、比較的小さな $m$ でも式は扱いにくい。

これは、$E(\mathbb{F}_{q^k})$ の点が $E(\mathbb{F}_{q^k})$ の点に写像され、さらに点の加算を保存することから、曲線内同型の例と言えます。
また、$(X, Y) \mapsto (X^q, Y^q)$ で与えられる $q$ 乗フロベニウス写像 $\Phi$ も重要な例である。
一般に $q^k$ 乗フロベニウス写像が定義されるが、ここでは $q$ 乗写像のみを定義し、それ以上の $k$ 乗が必要な場合は $\Phi^k$ と書くことにする。

また、トレースマップが必要で、これは次のように定義される。

$$\mathrm{tr} P = P + \Phi (P) + \ldots + \Phi^{k−1}(P)$$

で定義され、$E( \mathbb{F}_{q^k})$ の点を $E(\mathbb{F}_q)$ に取るものです。

楕円曲線論でよく知られているもう一つの定理を述べる。
以下、$f \circ g$ は $P \mapsto f(g(P))$ で定義される写像を表します。

**定理** (Hasse)。

$$\Phi^k \circ \Phi^k − [t] \circ \Phi^k + [q^k] = [0]$$

ここで、$t = q^k + 1 - \#E(\mathbb{F}_{q^k})$ です。

$E[r] \cong \Z_r \times \Z_r$ なので $E[r]$ を $E[r]$ に写す任意の曲線内同型（フロベニウス写像や $m$ 乗写像に当てはまる）は $E[r]$ に限定すると $2 \times 2$ の行列と見なすことができる。

特に、$E(\mathbb{F}_q)$  を割る素数 $r$ のとき、$\Phi$ の固有値は $x^2 - tx + q \pmod{r}$ の解となります。
多項式は $(x-q)(x-1)$ と因数分解されるので、固有値は $q$ か $1$ でなければならない。
対応する固有空間を記述することができる。

$E(\mathbb{F}_q)[r]$ が $1$-固有空間であることは明らかなので、$E[r] \subset E(\mathbb{F}_q)$ ならば $\Phi$ は単に恒等式となる。
それ以外の場合は、$G=\{P|\mathrm{tr}P=O\}$ の集合を考える。

この集合は無限遠の点以外も含むことがわかる。なぜなら、任意の $P \in E[r] \backslash E(\mathbb{F}_q)[r]$ に対して、点 $P-\Phi(P)$ は非自明でトレースが 0であるからである。
$G$ は群律のもとで閉じていることが簡単な計算でわかり、また$\Phi(G)=G$であることがわかる。
よって、$G$ は固有空間である。

$G$ は $1$ 固有値空間にはなりえないので（すでに説明した通り、 $E(\mathbb{F}_q)[r]$ は 1 固有値空間）、 $G$ は $q$ 固有値空間でなければならず、 $|G|=r$ を意味することになる。
この結果は、6.4節で重要なので、再掲する。


**定理**：トレースゼロの点集合 $G=\{P| \mathrm{tr}P=O\}$ は次数 $r$ の環状群であり、すべての$P \in G$ は　$\Phi(P) = qP$ を満たす。

## 3.4 ゼロと極（Zeroes and Poles）

<details>
<summary>原文</summary>
When we study a polynomial $f(X)$, the roots, or zeroes, of $f(X)$ play an important role.
Given the zeroes of a polynomial $f$, we can instantly write down an expression for $f$ that is unique up to a constant multiple.
Extending this idea, when considering the quotient of two polynomials $f(X)/g(X)$, we see the zeroes are the roots of $f$ and the poles are the roots of $g$, and $f/g$ is completely determined up to a constant by its zeroes and poles and their multiplicities.
Indeed, suppose the zeroes and poles are $\alpha_1,\ldots,\alpha_n$ with multiplicites $a_1,\ldots,a_n$ where $\alpha_i$ is a zero of multiplcity $a_i$.
We consider a zero of negative multiplicity $a_i$ to be a pole of multiplicity $−a_i$.
Then we may write $f(X)/g(X)$ immediately by multiplying equations of appropriate vertical lines:

$$f(X)/g(X) = (X − \alpha_i)^{a_i}$$

One can view roots of a polynomial f as places where the curve $Y=f(X)$ intersects with the curve $Y=0$.
We now generalize by replacing $Y=0$ with an elliptic curve.
When studying an element $f(X,Y) \in \mathbb{F}_q^k[E]$, we take note of points of intersection between $f(X,Y)$ and $E$.
Again, we call these points zeroes of $f$.

Complications occur due to the point at infinity $O$.
We omit the details since we do not wish to discuss projective geometry, but it turns out that every line $\alpha X+\beta Y+\gamma$ that intersects a given elliptic curve $E$ has a pole at $O$ of some multiplicity, though this is not immediately suggested from its equation.
The following two facts are all that we will say on this topic, and they need not be retained, nor even read. 
Firstly, if a vertical line $X−\alpha$ intersects the curve $E$ then it has a pole at $O$ of multiplicity $2$.
Secondly, if a non-vertical line $\alpha X+\beta Y+\gamma$ intersects $E$ then it has a pole of multiplicity $3$ at $O$.
From now we will ignore the behaviour of rational functions at $O$.

Recall an element of $\mathbb{F}_{q^k}(E)$ can be written as $f(X,Y)/g(X,Y)$.
In this case the zeroes are the zeroes of $f$ and the poles of $g$, and the poles are the poles of $f$ and the zeroes of $g$.
Just as expressions of the form $X−\alpha$ can be viewed as building blocks of quotients of polynomials, we view equations of lines $\alpha X+\beta Y+\gamma$ as building blocks of rational functions on elliptic curves.
As with quotients of polynomials, knowing the zeroes and poles of a rational function on $E$ allows us to instantly write its equation, up to multiplication by a constant.
We also build the function by multiplying together equations of lines, but need more than just vertical ones.

In what follows, by abuse of notation we often refer to a single rational function $f$ when in fact we are considering a whole family of rational functions $cf$ for some nonzero constant $c$.
To add to the confusion, having arbitrarily chosen a representative of a family, we will sometimes subsequently insist on using that same representative.
The reasons for doing so will become clear.
</details>

多項式 $f(X)$ を研究するとき、$f(X)$ の根、やゼロが重要な役割を果たす。
多項式 $f$ のゼロがあれば、定数倍まで一意である $f$ の式を即座に書き下すことができる。
この考えを発展させて、二つの多項式 $f(X)/g(X)$ の商を考えると、ゼロが $f$ の根、極が $g$ の根で、$f/g$ はそのゼロと極とその乗数で定数まで完全に決定されることがわかります。
実際、ゼロと極が 多重度 $a_1,\ldots,a_n$ で $\alpha_1, \ldots,\alpha_n$ とすると、$\alpha_i$ は 乗法 $a_i$ のゼロであるとする。
負の多重度 $a_i$ のゼロを多重度 $-a_i$ の極と考える。
そうすると、適当な縦線の方程式を掛け合わせることで、すぐに $f(X)/g(X)$ と書くことができる。

$$f(X)/g(X) = (X − \alpha_i)^{a_i}$$

多項式 $f$ の根は、曲線 $Y=f(X)$ が曲線 $Y=0$ と交差する場所と見なすことができる。
ここで、$Y=0$ を楕円曲線に置き換えて一般化する。
ある要素 $f(X,Y) \in \mathbb{F}_q^k[E]$ を調べるとき、$f(X,Y)$ と $E$ の交点に注目する。
ここでも、これらの点を $f$ のゼロ点と呼ぶ。

無限遠の点 $O$ のために複雑な問題が発生する。
射影幾何学の議論をしたくないので詳細は省略するが、与えられた楕円曲線 $E$ と交わるすべての直線 $\alpha X+\beta Y+\gamma$ は、ある多重度の極を $O$ に持つことが判明したが、これはその方程式からすぐには示唆されない。
この話題については、次の2つの事実がすべてであり、保持する必要はないし、読む必要さえない。
まず、垂直線 $X-\alpha$ が曲線 $E$ と交差する場合、それは $O$ に多重度 $2$ の極を持つ。
次に、非垂直線 $\alpha X+\beta Y+\gamma$ が $E$ と交差する場合、$O$ に多重度 $3$ の極を持つ。
これから、$O$ での有理関数の振る舞いは無視する。

$\mathbb{F}_{q^k}(E)$ の要素は $f(X,Y)/g(X,Y)$ と書けることを思い出してください。
このとき、ゼロは $f$ のゼロと $g$ の極であり、極は $f$ の極と $g$ のゼロである。
$X-\alpha$ の形の式が多項式の商のビルディングブロックと見なせるように、線分 $\alpha X+\beta Y+\gamma$ の方程式を楕円曲線上の有理関数のビルディングブロックと見なします。
多項式の商と同様に、$E$ 上の有理関数のゼロと極を知ることで、定数を掛けるまでの方程式を瞬時に書くことができる。
また、直線の方程式を掛け合わせて関数を作るが、垂直なものだけでなく、もっといろいろなものが必要である。

以下では、表記法の乱用により、実際にはあるゼロでない定数 $c$ に対する有理関数 $cf$ の族全体を考えているのに、単一の有理関数 $f$ と表記することがよくあります。
さらに混乱させるのは、ある族の代表を任意に選択した後、その代表を使うことにこだわることがあることです。
その理由は、これから明らかになる。

## 3.5 因子（Divisors）

<details>
<summary>原文</summary>
Divisors are the standard way to notate zeroes and poles and their multiplicities, or orders.
If we wish to record zeroes and poles $P_1,\ldots ,P_n$ of orders $a_1,\ldots ,a_n$ (where $P_i$ is a zero of order ai if $a_i>0$ and is a pole of order $−a_i$ otherwise), then we may write:

$$D=a_1 \langle P_1 \rangle +\ldots + a_n \langle P_n \rangle .$$

With this notation, if we add the divisors of two functions together, then the resulting divisor reflects the zeroes and poles of the product of the two functions.

For this chapter we break with tradition and use multiplicative notation for divisors instead.
In addition, we will omit the number of poles or zeroes at $O$, as we are ignoring the behaviour of rational functions at that point.
For divisors of rational functions, we actually do not lose any information because if we cared, we could always recover the order of $(O)$ simply by negating of the sum of the orders of the finite points.

Let $D$ be the divisor with zeroes and poles $P_1,\ldots ,P_n$ of multiplicities $a_1,\ldots ,a_n$.
We shall write $D$ as

$$D=(P_1)^{a_1}\ldots(P_n)^{a_n}.$$

Our unusual notation has several advantages.
Using additive group notation for the elliptic curve and multiplicative notation for divisors emphasizes the difference between them and reduces the likelihood of confusing the two.
When rational functions are multiplied together, their divisors can be multiplied together, which is arguably more natural than adding them.
Leaving out the $(O)$ term allows us to focus less on bookkeeping and more on the task at hand, and the order of $(O)$ can be readily calculated anyway.

We use $(f)$ to denote the divisor of $f$, thus in our notation for any rational functions $f$, $g$ we have $(f)(g)=(fg)$.

On an elliptic curve, in our unorthodox notation:

**Lines:** a line $L$ through the points $P$, $Q$ intersects $E$ at a third point $−P−Q$ by the chord-tangent composition law, and so has divisor

$$(L)=(P)(Q)(−P−Q).$$

**Tangents:** when $Q=P$ we have a tangent line $T$ at $P$ with divisor

$$(T)=(P)^2(−2P).$$

**Verticals:** when $Q=−P$, we have a vertical line $V$ through the point $P$ with divisor

$$(V) = (P)(−P).$$
</details>

ゼロと極とその倍数、つまり位数を表記する標準的な方法は因子である。
位数 $a_1,\ldots ,a_n$ のゼロと極 $P_1,\ldots ,P_n$ を記録したい場合（ここで $P_i$ は $a_i>0$ ならば位数 $a_i$ のゼロ、それ以外は位数 $-a_i$ の極となる）、次のように書くことができます。

$$D=a_1 \langle P_1 \rangle +\ldots + a_n \langle P_n \rangle .$$

この表記法では、2つの関数の因子を足し合わせると、できた因子には2つの関数の積のゼロと極が反映される。

この章では、伝統にとらわれず、因子には乗法表記を用いる。
また、$O$ における極やゼロの数は、その点での有理関数の振る舞いを無視するため、省略することにします。
有理関数の約数については、有限点の位数の和を否定するだけで、いつでも $(O)$ の位数を求めることができるので、実は何の情報も失わないのです。

$D$ を多重度 $a_1,\ldots ,a_n$ のゼロと極を持つ約数であるとする。
ここで、$D$ を次のように書く。

$$D=(P_1)^{a_1}\ldots(P_n)^{a_n}.$$

変わった表記法にはいくつかの利点がある。
楕円曲線には加群表記、約数には乗法表記を用いることで、両者の違いが強調され、混同する可能性が低くなる。
有理関数が掛け合わされるとき、その因子も掛け合わせることができ、足し算よりも自然であることは間違いない。
$(O)$ の項を省くことで、帳尻合わせではなく、目の前の作業に集中できますし、$(O)$ の位数はどうせすぐに計算できるのですから。

$(f)$ は $f$ の因子を表すので、有理関数 $f$, $g$ の記法では $(f)(g)=(fg)$ となります。

楕円曲線上では、非正統的な記法で

**線：** 点 $P$, $Q$ を通る線 $L$ は、弦接線合成則（chord-tangent composition law）により第3点 $-P-Q$ で $E$ と交差するので、因子を持つ。

$$(L)=(P)(Q)(−P−Q).$$

**接線:** $Q=P$ のとき、$P$ で因子を持つ接線 $T$ がある。

$$(T)=(P)^2(−2P).$$

**垂直線：** $Q=-P$ のとき、点 $P$ を通る垂直線 $V$ があり、その因子は

$$(V) = (P)(−P).$$


## 3.6 Weilペアリング（The Weil Pairing）

<details>
<summary>原文</summary>
Earlier papers on pairing-based cryptosystems advocated the Weil pairing.
We shall see that the related Tate pairing is preferable for our purposes.
Nonetheless we first describe the Weil pairing for historical reasons, and also because its simpler description serves well as an introduction.
The inputs to the Tate pairing come from different groups, and the definition involves quotient groups, which can obscure the basic idea.
However, the actual computation of the Tate pairing is simpler.

Let $E$ be an elliptic curve containing $n$ points over a field $\mathbb{F}_q$.
Let $G$ be a cyclic subgroup of $E(\mathbb{F}_q)$ of order $r$ with $r$, $q$ coprime.
Let $k$ be the smallest positive integer such that $E(\mathbb{F}_{q^k})$ contains all of $E[r]$.

We define the Weil pairing $f : E[r] \times E[r] \rightarrow \mathbb{F}_{q^k}$ as follows.

For a pair of points $P,Q\in E[r]$, choose any $R,S\in E[\mathbb{F}_{q^k}]$ such that $S\ne R$, $P+R$, $P+R−Q$, $R−Q$.
Let $f_P$ be a rational function with divisor $(f_P)=(P+R)^r/(R)^r$, (In other words, we have $r$ zeroes at $P+R$ and $r$ poles at $R$. In standard notation this is $r\langle P+R \rangle − r\langle R\rangle$.) and similarly let $f_Q$ be a rational function with divisor $(f_Q)=(Q+S)^r/(S)^r$.

Define

$$f(P,Q)=\frac{f_P(Q+S)/f_P(S)}{f_Q(P+R)/f_Q(R)}$$

It can be shown that this value is independent of the choices for $R$, $S$.
The conditions for choosing $R$, $S$ ensure we never evaluate $f_P$ nor $f_Q$ at a zero or pole.
Also both $f_P$ , $f_Q$ are only unique up to a constant, but this has no effect on the final answer since quotients of these functions are computed and the constants cancel out.
This is an example of the grey area mentioned earlier: at first we are free to choose any function as long as they have certain zeroes and poles, but we cannot alter it once we have made a choice.
Finding explicit expressions for these functions is infeasible for cryptographically useful pairings.
Instead, we will soon describe an algorithm, known as Miller’s algorithm [48, 49] that evaluates these functions in a lazy fashion at the required points.
This algorithm resembles a typical exponentiation routine that performs repeating squaring.
One can prove that

1.$f(aP,bQ)=f(P,Q)^{ab}$ for all $P,Q\in E[r]$ and all integers $a,b$.
2.$f(P,P)=1$ for all $P\in E[r]$.
3.$f(P,Q)=1$ for all $P\in E[r]$ if and only if $Q=O$.
4.$f(P,Q)=1$ for all $Q\in E[r]$ if and only if $P=O$.
5.$f(P,Q)=f(Q,P)^{−1}$ for all $P,Q\in E[r]$.
6.$f(\Phi(P),\Phi(Q))=f(P,Q)^q$ for all $P,Q\in E[r]$. where $\Phi$ denotes the Frobenius map.

(The last property is usually stated more generally: $f(\alpha(P),\alpha(Q))=f(P,Q)^{\deg \alpha}$ for any nonzero endomorphism $\alpha$.)
The second to last property, known as antisymmetry, is a property the Tate pairing does not have. 
However it does not yet have any use in cryptography.
</details>

ペアリング暗号に関する先行論文ではWeilペアリングを提唱している。
しかし、我々の目的には、関連するTateペアリングの方が望ましいことがわかるだろう。
しかし、歴史的な理由と、より簡単な記述であるため、まずWeilペアリングを説明する。
Tate対の入力は異なる群から来るし、定義には商群が含まれるので、基本的な考え方が曖昧になる可能性がある。
しかし、Tateペアリングの実際の計算はもっと単純です。

$E$ を、$\mathbb{F}_q$ 体上の $n$ 点を含む楕円曲線とする。
$G$ を、$E(\mathbb{F}_q)$ の巡回部分群とし、$r$, $q$ を共線とする位数 $r$ とする。
$k$ を、$E(\mathbb{F}_{q^k})$ が $E[r]$ のすべてを含むような最小の正の整数をとする。

Weilのペアリング $f : E[r] \times E[r] \rightarrow \mathbb{F}_{q^k}$ を次のように定義する。

一対の点 $P,Q\in E[r]$ に対して、$S\ne R$、$P+R$、$P+R-Q$、$R-Q$ が成り立つような任意の $R,S\in E[\mathbb{F}_{q^k}]$ を選ぶ。
$f_P$ を因子 $(f_P)=(P+R)^r/(R)^r$ の有理関数とする（つまり、$P+R$ で $r$ 個のゼロを持ち、$R$ で $r$ 個の極を持つ。標準記法では、これは $\langle P+R \rangle - r \langle R \rangle$である）。同様に、$f_Q$ を、因子 $(f_Q)=(Q+S)^r/(S)^r$ を持つ有理関数とする。

定義

$$f(P,Q)=\frac{f_P(Q+S)/f_P(S)}{f_Q(P+R)/f_Q(R)}$$

この値は、$R$, $S$ の選択に依存しないことが示される。
$R$, $S$ を選ぶ条件は、$f_P$ も $f_Q$ もゼロや極で評価しないことを保証しています。
また、$f_P$, $f_Q$ ともに定数までしか一意ではありませんが、これらの関数の商が計算され、定数は打ち消されるので、最終的な答えには影響しません。
これは、先ほどのグレーゾーンの例で、最初は一定のゼロと極があればどんな関数でも自由に選べるが、一度選んだ関数を変更することはできない。
このような関数の明示的な表現を見つけることは、暗号的に有用なペアリングでは実行不可能である。
その代わりに、これらの関数を必要な箇所で遅延的に評価する Miller's algorithm [48, 49] として知られるアルゴリズムをすぐに説明する予定である。
このアルゴリズムは、繰り返し二乗を行う典型的な指数計算ルーチンに似ている。
次のことが証明できる。

1.すべての $P,Q\in E[r]$ とすべての整数 $a,b$ に対して $f(aP,bQ)=f(P,Q)^{ab}$ となる。
2.すべての $P\in E[r]$ に対して $f(P,P)=1$ となる。
3.$Q=O$ の場合に限り、すべての $P\in E[r]$ に対して、 $f(P,Q)=1$ となる。
4.$P=O$ の場合に限り、すべての $Q\in E[r]$ に対して、 $f(P,Q)=1$ となる。
5.すべての $P,Q\in E[r]$ に対して、 $f(P,Q)=f(Q,P)^{−1}$ となる。
6.すべての $P,Q\in E[r]$ に対して、$f(\Phi(P),\Phi(Q))=f(P,Q)^q$ となる。ここで $\Phi$ はフロベニウス写像を表す。


(最後の性質は通常より一般的に述べると、任意の非ゼロ内同型写像 $\alpha$ に対して $f(\alpha(P),\alpha(Q))=f(P,Q)^{\deg \alpha}$ となる)。
最後から2番目の性質は「非対称性」と呼ばれるもので、Tateペアリングが持たない性質である。
しかし、この性質はまだ暗号技術に利用されていません。


### 3.6.1 Weil相互律（Weil Reciprocity）

<details>
<summary>原文</summary>
Many of the previous statements, including the fact that the pairing is well-defined (in other words, the choices of $R$, $S$ above do not matter) and several of the listed properties, can be proved using a fact known as Weil reciprocity [13].

First, we define evaluation of a rational function $f$ at a divisor $\prod_pP^{\alpha_P}$ by

$$f(\prod_P (P)^{a_P}) = \prod_P f(P)^{a_P}$$

Weil reciprocity states that for any rational functions $f$, $g$,

$$f((g)) = g((f)),$$

that is, evaluating $g$ at the divisor of $f$ produces the same result as evaluating $f$ at the divsor of $g$.

Intuitively, Weil reciprocity can be thought of as a generalization of the much simpler version of this statement for polynomials: given two polynomials $p$, $q$, the result of evaluating $p$ at the zeroes of $q$ and multiplying has the same absolute value as evaluating $q$ at the zeroes of $p$ and multiplying.
</details>

ペアリングがよく定義されていること(言い換えれば、上記の $R$ , $S$ の選択は問題ではない)や、列挙したいくつかの性質など、これまでの記述の多くは、Weil相互律（Weil reciprocity）として知られる事実を用いて証明することができる[13]。

まず、有理関数 $f$ の 因子 $\prod_pP^{\alpha_P}$ での評価を以下のように定義する。

$$f(\prod_P (P)^{a_P}) = \prod_P f(P)^{a_P}$$

Weil相互律（Weil reciprocity）とは、任意の有理関数 $f$, $g$ について、次のようなものである。

$$f((g)) = g((f)),$$

直感的には、Weil相互律（Weil reciprocity）は、多項式に対するこの文のもっと単純な版の一般化と考えることができる。二つの多項式 $p$, $q$ が与えられたとき、$p$ を $q$ のゼロで評価して掛けた結果は $p$ のゼロで評価して掛けた結果と同じ絶対値になる。


## 3.7 Tateペアリング（The Tate Pairing）

<details>
<summary>原文</summary>
Let $E$ be an elliptic curve containing $n$ points over a field $\mathbb{F}_q$.
Let $G$ be a cyclic subgroup of $E(\mathbb{F}_q)$ of order $r$ with $r,q$ coprime.
Let $k$ be the smallest positive integer such that $r|q^k−1$.
For brevity write $K$ = $\mathbb{F}_{q^k}$.
An equivalent characterization is that $K=\mathbb{F}_{q^k}$ is the smallest extension of $\mathbb{F}_q$ containing the $r$-th roots of unity.

The Tate (or Tate-Lichtenbaum) pairing

$$e : E[r] \cap E(K) \times E(K)/rE(K) → K^∗/K^{∗r}$$

is defined as follows.

Let $f_P$ be a rational function with divisor $(f_P)=(P)^r$.
Choose an $R \in E(K)$ such that $R \ne P$, $P−Q$, $O$, $−Q$.
The define

$$f(P,Q)=f_P(Q+R)/f_P(R)$$

It can be shown that the above value is independent of the choice of $R$, and:
1.$f(aP,bQ)=e(P,Q)^{ab}$ for all $P$, $Q$, $a$, $b$.
2.$f(P,Q)=1$ for all $P$ if and only if $Q=O$.
3.$f(P,Q)=1$ for all $Q$ if and only if $P=O$.
4.$f(\Phi(P),\Phi(Q))=f(P, Q)^q$ for all $P, Q \in E[r]$, where $\Phi$ denotes the Frobenius map.

The output of this pairing is some $x \in K^∗$ that represents the coset $xK^{∗r}$.
To standardize the coset representative, we exponentiate the output of the Tate pairing by $(q^k−1)/r$, which can take a substantial amount of time.
On the positive side, the second input to the Tate pairing is also a coset representative.
This means it can be any point of $E(K)$ and may be of any order.
(For example, if the order of a point $Q$ is not a multiple of $r$ then $Q$ represents the coset $O + rE(K)$. Otherwise $Q$ represents some nonidentity element.)
In contrast, the Weil pairing requires that the second input $Q$ satisfies $rQ=O$.
</details>

$E$ を、$\mathbb{F}_q$ 体上の $n$ 個の点を含む楕円曲線とする。
$G$ を 位数 $r$ とする $E(\mathbb{F}_q)$ の巡回部分群とし、$r,q$ は互いに素とする。
$k$ を $r|q^k−1$ となるような最小の正の整数とする。
簡潔のため $K$ = $\mathbb{F}_{q^k}$　と記す。

これと等価な性質は、$K=\mathbb{F}_{q^k}$ が $r$ 番目の単一根を含む $\mathbb{F}_q$ の最小の拡張であることである。

Tate（または Tate-Lichtenbaum）のペアリング

$$e : E[r] \cap E(K) \times E(K)/rE(K) → K^∗/K^{∗r}$$

は以下のように定義されます。

$f_P$ を、因子 $(f_P)=(P)^r$ を持つ有理関数とする。
$E(K)$ において、$R \ne P$, $P-Q$, $O$, $-Q$ となるような $R$ を選べばよい。
定義

$$f(P,Q)=f_P(Q+R)/f_P(R)$$

上記の値は、$R$ の選択に依存しないことが示される、そして
1.すべての $P$, $Q$, $a$, $b$ に対して、$f(aP,bQ)=e(P,Q)^{ab}$ が成り立つ。
2.$Q=O$ ならば、すべての $P$ にたいして、$f(P,Q)=1$ が成り立つ。
3.$P=O$ ならば、すべての $Q$ にたいして、$f(P,Q)=1$ が成り立つ。
4.すべての $P, Q \in E[r]$ に対して、$f(\Phi(P),\Phi(Q))=f(P, Q)^q$ が成り立つ、ここで $\Phi$ フロベニウス写像を表す。

このペアリングの出力は、コセット（傍系、coset） $xK^{∗r}$ を表すいくつかの $x \in K^∗$ である。
代表元（coset representative）標準化するために、Tate ペアリングの出力を $(q^k-1)/r$ で指数化するが、これにはかなりの時間がかかることがある。
ポジティブな面では、Tete ペアリングの2番目の入力も代表元（coset representative）である。
つまり、$E(K)$ の任意の点であり、任意の位数である可能性があります。
(例えば、ある点 $Q$ の位数が $r$ の倍数でない場合、$Q$ はコセット（傍系、coset） $O + rE(K)$ を表す。そうでなければ、$Q$ は何らかの非同一要素を表している。)
これに対し、Weilペアリングでは、2番目の入力 $Q$ が $rQ=O$ を満たすことが必要である。


## 3.8 理論と実践の融合（Merging Theory with Practice）

<details>
<summary>原文</summary>
We have now described the Weil and Tate pairings.
It remains to show how they fit the abstract definitions given in Chapter 1.
Let $E$ be some elliptic curve defined over $\mathbb{F}_q$ and let $G$ be some cyclic subgroup of points in $E(\mathbb{F}q)$ of order $r$. 
Suppose the embedding degree of $G$ is $k>1$, which is usually the case in practice.

Then by defining $G_1=G$, $G_2=E[r]$, and $G_T$ to be the $r$-th roots of unity in $\mathbb{F}_{q^k}$ , the Weil pairing satisfies the abstract definition of a bilinear map given in Section 1.8.
Similarly, the Tate pairing fits the definition.

With high probability, a random point $Q \in G_2$, does not lie in $G_1$, does not have trace zero, and generates a subgroup $H$ in $G_2$ of order $r$.
Then if we replace $G_2$ with $H$, we have now satisfied the asymmetric pairing definition of Section 1.7 because the trace map nontrivially maps $G_2$ to $G_1$.

We see now why we cannot hash into $G_2$ in general if it is required to be cyclic.
In the case of the Weil pairing, we can easily hash to a point of $E[r]$, but it is not known how to hash to a point of some designated cyclic subgroup of $E[r]$ of order $r$ that is not $G$.
For the Tate pairing we have a similar problem.
There is one exception that we discuss in Section 6.4.2: we can hash to the trace zero group.
(In fact, the proof in the last section contains a statement that suggests how we might do this.)

Cryptosystems that require the symmetric pairings of Section 1.4 and require input groups to which strings can be hashed can only be implemented using supersingular curves and distortion maps as in Section 6.6.

Some kinds of symmetric pairings can be obtained without resorting to supersingular curves.
We could treat $G_2$ as the first and second input groups and apply the map from $G_2$ to $G_1$ on the first input before a pairing computation.
However, one must relinquish the ability to hash, or the requirement that the input groups be cyclic, or a combination of the two.
Additionally, one must compute more in larger fields for the same level of security.
The details can be found in Section 6.4.2.
</details>

ここまでで、WeilとTateのペアリングを説明した。
あとは、それらが第1章で与えられた抽象的な定義にどのように適合するかを示すだけである。
$E$ を $\mathbb{F}_q$ 上で定義されたある楕円曲線とし、$G$ を $E(\mathbb{F}q)$ の点のある巡回部分群で位数が $r$ であるとする。
$G$ の埋め込み位数は $k>1$ であり、これは実際によくあることである。

そこで、$G_1=G$, $G_2=E[r]$, $G_T$ を $\mathbb{F}_{q^k}$ における$r$番目の統一根と定義すると、Weil ペアリングはセクション1.8 で与えられた双線型写像の抽象定義を満たすようになります。
同様に、Tate ペアリングも定義に合致する。

高い確率で、ランダムな点 $Q$ は $G_2$ にあり、$G_1$ にはなく、トレースゼロでもなく、$G_2$ に位数 $r$ の部分群 $H$ を生成する。
このとき、$G_2$ を $H$ に置き換えると、トレース写像は非自明的に $G_2$ を $G_1$ に写すので、これで1.7節の非対称対の定義を満たしたことになる。

$G_2$ が環状であることが要求される場合、一般に $G_2 $にハッシュできない理由がわかりました。
Weilペアリングの場合、$E[r]$ の点へのハッシュは簡単にできますが、$G$ でない $E[r]$ の位数 $r$ の指定された巡回部分群の点へのハッシュはどうすればよいか分かっていません。
Tateペアリングについても同様の問題がある。
ただし、6.4.2節で説明するように、トレースゼロ群にハッシュできる例外があります。
(実際、前節の証明には、この方法を示唆する記述がある)。

セクション1.4の対称ペアリングを必要とし、文字列をハッシュ化できる入力群を必要とする暗号システムは、セクション6.6のように超特異曲線と歪曲写像を使ってのみ実装可能である。

ある種の対称的なペアリングは、超特殊曲線に頼ることなく得ることができる。
$G_2$ を第1と第2の入力群として扱い、ペアリング計算の前に、第1の入力に対して $G_2$ から $G_1$ への写像を適用することができる。
しかし、ハッシュの機能、あるいは入力群が環状であるという要件、あるいはその2つの組み合わせを放棄する必要がある。
さらに、同じレベルの安全性を確保するためには、より大きな体でより多くの計算をしなければならない。
詳細はセクション6.4.2に記載されている。

## 3.9 ミラーアルゴリズム（Miller’s Algorithm）

<details>
<summary>原文</summary>
For the Weil pairing we need to evaluate some rational function $f_P$ with divisor $(P+R)^r/(R)^r$.
For the Tate pairing we need to evaluate some rational function $f_P$ with divisor $(P)^r$.
In both cases, we can do this using Miller’s algorithm [48, 49].

For any points $U,V$ ,
 let $L_{U,V}(X,Y)$ be an equation for a line through $U$ and $V$ ,
 let $T_U(X,Y)$ be an equation for the tangent through $U$,
 let $V_U(X,Y)$ be an equation for avertical line through $U$ (e.g. $X−x$, where $U=(x,y)$).
Since $T_U=L_{U,U}$ and $V_U=L_{U,−U}$ , defining $T_U$ and $V_U$ is unnecessary but they aid comprehension. Note if $U=−U$ then $T_U$ is the same as $V_U$.
</details>

Weilペアリングでは、因子 $(P+R)^r/(R)^r$ を持つ有理関数 $f_P$ を評価する必要がある。
Teteペアリングでは、因子 $(P)^r$ を持つ有理関数 $f_P$ を評価する必要がある。
どちらの場合も、ミラーのアルゴリズム[48,49]を用いて行うことができる。

任意の点 $U,V$ に対して．
 $L_{U,V}(X,Y)$ を $U$ と $V$ を通る直線の方程式とする ,
 $T_U(X,Y)$ を $U$ を通る接線の方程式とする。
 $V_U(X,Y)$ を $U$ を通る垂線（例：$X-x$、$U=(x,y)$）の式とする。

$T_U=L_{U,U}$、$V_U=L_{U,-U}$ なので、$T_U$ と $V_U$ の定義は不要だが、理解の助けになる。
なお、$U=-U$ のときは $T_U$ は $V_U$ と同じである。

### 3.9.1 Weilペアリング（The Weil Pairing）

<details>
<summary>原文</summary>
For an integer $k$, let $f_k$ be a rational function with divisor

$$(f_k)=\frac{(P+R)^k}{(R)^k(kP)}.$$

Observe $f_r=f_P$ , since $rR=O$.
It can be checked that

$$\left(f_a\cdot f_b\cdot \frac{L_{aP,bP}}{V_{(a+b)P}} \right)= (f_{a+b})$$

via a simple computation on the divisors:

$$\frac{(P+R)^a}{(R)^a(aP)}\cdot\frac{(P+R)^b}{(R)^b(bP)}\cdot\frac{(aP)(bP)(−(a + b)P)}{(a + b)P(−(a + b)P)}=\frac{(P+R)^{a+b}}{(R)^{a+b}((a + b)P)}.$$

This leads to the formula:

$$\left(f^2_k\cdot\frac{T_{kP}}{V_{2kP}}\right)=(f_{2k})$$

A straightforward manipulation of divisors also shows $(V_{P+R}/L_{P,R})=(f_1)$.
Thus we may compute $f_P(Q)=f_r(Q)$ using Algorithm 2, where the binary representation of $r$ is $r_t\ldots r_0$.

---

**Algorithm 2** Miller’s algorithm for Weil pairing. $x=f_P(Q)$

&nbsp;1: $x_1\leftarrow V_{P+R}(Q)/L_{P,R}(Q)$
&nbsp;2: $x\leftarrow x_1$
&nbsp;3: $Z\leftarrow P$
&nbsp;4: **for** $i\leftarrow t−1,\ldots,0$ **do**
&nbsp;5: &emsp;$x\leftarrow x^2$· $T_Z(Q)/V_{2Z}(Q)$
&nbsp;6: &emsp;$Z\leftarrow 2Z$
&nbsp;7: &emsp;**if** $r_i = 1$ **then**
&nbsp;8: &emsp;&emsp;$x\leftarrow x\cdot x_1\cdot L_{Z,P}(Q)/V_{Z+P}(Q)$
&nbsp;9: &emsp;&emsp;$Z\leftarrow Z+P$
10: &emsp;**end if**
11: **end for**

---

On termination, we have $x=f_P(Q)$ (and $Z=rP=O$).
Throughout the algorithm we may multipy the equation of any line $L,T,V$ by an arbitrary constant, thus more precisely, this algorithm really computes $f_P(Q)$ for one possible choice of $f_P$.
In the Weil pairing, this causes no problems provided the same equations are chosen the second time this algorithm is run, so that the exact same $f_P$ is evaluated.

Note that

$$\left(f_k\cdot \frac{L_{P+R,kP}}{L_{R,(k+1)P}}\right)=(f_{k+1}).$$

so we could replace step 8 with $x\leftarrow x$·
$L_{P+R,Z}(Q)/L_{R,Z+P}(Q)$ and avoid computing and storing $x_1$, but unless $r$ has very low Hamming weight this is not a good trade because vertical lines are much cheaper to compute than general lines.
</details>

整数 $k$ に対して、$f_k$ を因子を持つ有理関数とする。

$$(f_k)=\frac{(P+R)^k}{(R)^k(kP)}.$$

$f_r=f_P$ を観察すると、$rR=O$ なので
ということが確認できる。

$$\left(f_a\cdot f_b\cdot \frac{L_{aP,bP}}{V_{(a+b)P}} \right)= (f_{a+b})$$

を、因子に関する簡単な計算で求めることができます。

$$\frac{(P+R)^a}{(R)^a(aP)}\cdot\frac{(P+R)^b}{(R)^b(bP)}\cdot\frac{(aP)(bP)(−(a + b)P)}{(a + b)P(−(a + b)P)}=\frac{(P+R)^{a+b}}{(R)^{a+b}((a + b)P)}.$$

という数式が導き出される。

$$\left(f^2_k\cdot\frac{T_{kP}}{V_{2kP}}\right)=(f_{2k})$$

また、素直な因子の操作により、$(V_{P+R}/L_{P,R})=(f_1)$が示される。
したがって、$f_P(Q)=f_r(Q)$ をアルゴリズム2により計算することができる。ここで、$r$ の2進表現は $r_t\ldots r_0$ である。

---

**アルゴリズム 2** Weilペアリングのミラーアルゴリズム $x=f_P(Q)$

&nbsp;1: $x_1\leftarrow V_{P+R}(Q)/L_{P,R}(Q)$
&nbsp;2: $x\leftarrow x_1$
&nbsp;3: $Z\leftarrow P$
&nbsp;4: **for** $i\leftarrow t−1,\ldots,0$ **do**
&nbsp;5: &emsp;$x\leftarrow x^2$· $T_Z(Q)/V_{2Z}(Q)$
&nbsp;6: &emsp;$Z\leftarrow 2Z$
&nbsp;7: &emsp;**if** $r_i = 1$ **then**
&nbsp;8: &emsp;&emsp;$x\leftarrow x\cdot x_1\cdot L_{Z,P}(Q)/V_{Z+P}(Q)$
&nbsp;9: &emsp;&emsp;$Z\leftarrow Z+P$
10: &emsp;**end if**
11: **end for**

---

終了時には、$x=f_P(Q)$ となる（そして $Z=rP=O$ となる）。
このアルゴリズムを通して、任意の直線 $L,T,V$ の方程式に任意の定数を掛けることができるので、より正確には、このアルゴリズムは $f_P$ の一つの可能な選択に対して本当に $f_P(Q)$ を計算することになる。
Weilペアリングでは、このアルゴリズムを2回目に実行したときに、同じ方程式が選択されていれば、全く同じ $f_P$ が評価されるので、問題はない。

なお

$$\left(f_k\cdot \frac{L_{P+R,kP}}{L_{R,(k+1)P}}\right)=(f_{k+1}).$$

ということで、ステップ8を$x\leftarrow x\cdot L_{P+R,Z}(Q)/L_{R,Z+P}(Q)$ に置き換えれば、$x_1$ の計算と保存を回避できるが、$r$ が非常に低いハミング重みでなければ、垂直線の計算が一般線よりずっと安くなるので、これは良いトレードではない。

### 3.9.2 Tateペアリング（The Tate Pairing）

<details>
<summary>原文</summary>

We now consider the Tate pairing.
Recall we wish to compute $f_P(Q)$ where $f_P$ has divisor $(P)^r$.
We can view our current situation as a special case of the above with $R=O$.
Thus define the intermediate functions $f_k$ by

$$(f_k)=(P)^k/(kP).$$

We have $(f_r)=(f_P)$.
The following identities can be obtained by simplifying earlier formulas using $R=O$, but it is easy enough to check them directly.

For example, we can show

$$(f_k)=\left(\prod^{k−1}_{i=1}\frac{L_{iP}}{V_{(i+1)P}}\right)$$

via

$$(f_k)=\frac{(P)(P)}{(2P)}\cdot \frac{(P)(2P)}{(3P)}\cdots \frac{(P)((k − 1)P)}{(kP)}.$$

We could compute $f_r(Q)$ using this formula, but this is clearly impractical for large $r$.

We find

$$(f_{2k})=(f^2_kT_{kP}/V_{2kP})$$

which can be shown with direct calculation:

$$\frac{(P)^{2k}}{(2kP)}=\frac{(P)^{2k}}{(kP)^2}\cdot \frac{(kP)^2(−2kP)}{(2kP)(−2kP)}$$

and similarly we can show

$$(f_{k+1})=(f_kL_{kP,P}/V_{(k+1)P}).$$

leading to the following algorithm that computes $f_P(Q)$ given points $P,Q$ (where $P$ has order $r$).
Let the binary representation of $r$ be $r_t\ldots r_0$.

---

**Algorithm 3** Miller’s algorithm for Tate pairing. $x=f_P(Q)$

&nbsp;1: $x\leftarrow 1$
&nbsp;2: $Z\leftarrow P$
&nbsp;3: **for** $i\leftarrow t−1,\ldots ,0$ **do**
&nbsp;4: &emsp;$x\leftarrow x^2\cdot T_Z(Q)/V_{2Z}(Q)$
&nbsp;5: &emsp;$Z\leftarrow 2Z$
&nbsp;6: &emsp;**if** $r_i=1$ **then**
&nbsp;7: &emsp;&emsp;$x\leftarrow x\cdot L_{Z,P}(Q)/V_{Z+P}(Q)$
&nbsp;8: &emsp;&emsp;$Z\leftarrow Z+P$
&nbsp;9: &emsp;**end if**
&nbsp;10: **end for**

---

When the algorithm finishes we have $x=f_r(Q)$ (and $Z=rP=O$).
Note this algorithm can be viewed as the previous algorithm with $x_1=1$.

</details>

ここで、Tateペアリングを考える。
ここで、$f_P$ が因子 $(P)^r$ であるとき、$f_P(Q)$ を計算したいことを思い出してください。
今の状況は、上記の $R=O$ の特殊なケースと見ることができる。
そこで、中間関数 $f_k$ を次のように定義する。

$$(f_k)=(P)^k/(kP).$$

$(f_r)=(f_P)$ となる。
以下の恒等式は、先の式を $R=O$ を使って簡略化することで得られますが、直接確認するのも簡単です。


例えば、以下のようになります。

$$(f_k)=\left(\prod^{k−1}_{i=1}\frac{L_{iP}}{V_{(i+1)P}}\right)$$

によって

$$(f_k)=\frac{(P)(P)}{(2P)}\cdot \frac{(P)(2P)}{(3P)}\cdots \frac{(P)((k − 1)P)}{(kP)}.$$

この式を使って $f_r(Q)$ を計算することもできるが、$r$ が大きいと明らかに非現実的である。

以下のようになる。

$$(f_{2k})=(f^2_kT_{kP}/V_{2kP})$$

となり、これは直接計算で示すことができる。

$$\frac{(P)^{2k}}{(2kP)}=\frac{(P)^{2k}}{(kP)^2}\cdot \frac{(kP)^2(−2kP)}{(2kP)(−2kP)}$$

を示すことができ、同様に

$$(f_{k+1})=(f_kL_{kP,P}/V_{(k+1)P}).$$

は、点 $P,Q$（ここで $P$ は位数 $r$ ）を与えて $f_P(Q)$ を計算する以下のアルゴリズムを導出する。

$r$ の2進表現を $r_t\ldots r_0$ とする。

---

**アルゴリズム 3** Tateペアリングのミラーアルゴリズム $x=f_P(Q)$

&nbsp;1: $x\leftarrow 1$
&nbsp;2: $Z\leftarrow P$
&nbsp;3: **for** $i\leftarrow t−1,\ldots ,0$ **do**
&nbsp;4: &emsp;$x\leftarrow x^2\cdot T_Z(Q)/V_{2Z}(Q)$
&nbsp;5: &emsp;$Z\leftarrow 2Z$
&nbsp;6: &emsp;**if** $r_i=1$ **then**
&nbsp;7: &emsp;&emsp;$x\leftarrow x\cdot L_{Z,P}(Q)/V_{Z+P}(Q)$
&nbsp;8: &emsp;&emsp;$Z\leftarrow Z+P$
&nbsp;9: &emsp;**end if**
10: **end for**

---

アルゴリズムが終了すると、$x=f_r(Q)$ となる（そして $Z=rP=O$ となる）。
なお、このアルゴリズムは、先ほどのアルゴリズムに $x_1=1$ を加えたものと見なすことができる。

### 3.9.3 中間の極とゼロ（Intermediate Poles and Zeroes）

<details>
<summary>原文</summary>

We have in fact glossed over some complications.
The intermediate functions evaluated during Miller’s algorithm have zeroes and poles that eventually cancel out.
If we were dealing with algebraic expressions for the rational function then we could manipulate them to remove these zeroes and poles at some stage.
But since we are evaluating the function iteratively, this cannot be done, and attempting to evaluate an intermediate function at a zero or pole will cause problems.

Recall we have an almost unrestricted choice for the points $R,S$ in the Weil pairing and also for the point $R$ in the Tate pairing.
To use Miller’s algorithm, we choose them to avoid the zeroes and poles of the intermediate functions.
We can simply take points at random since there are only $O(t)$ bad choices.

Alternatively we may ensure their coordinates do not lie in the field that the functions are defined in, but rather in some field extension, so that in the case of a Weil pairing, $P+R,R,Q+S,S$ cannot possibly be a zero or pole in the intermediate functions though we must still avoid choices such as $R=S$, and in the case of a Tate pairing, neither $R$ nor $Q+R$ can be a zero or pole.
We shall see that under some conditions there is a much simpler fix for the Tate pairing.
</details>

実際、私たちはいくつかの複雑な問題を無視してきました。
ミラーアルゴリズムで評価される中間関数にはゼロと極があり、最終的には相殺される。
もし、有理関数の代数的な式を扱っているのであれば、ある段階でこれらのゼロや極を取り除くように操作することが可能です。
しかし、関数を繰り返し評価するのだから、そんなことはできないし、中間関数をゼロや極で評価しようとすると、問題が生じる。

Weilペアリングの点 $R,S$ とTateペアリングの点 $R$ はほぼ無制限に選択できることを思い出して欲しい。
ミラーアルゴリズムを使うには、中間関数のゼロと極を避けるように選ぶ。
悪い選択は $O(t)$ 個だけなので、ランダムに点を取ればよい。

その場合、Tateペアリング場合は、$P+R,R,Q+S,S$ は中間関数のゼロや極になりえないが、$R=S$ のような選択は避けなければならないし、Tateペアリングの場合は $R$ も $Q+R$ もゼロや極になりえないのだ。
ある条件下では、Tateペアリングにはもっと簡単な固定があることを見ることにしよう。


## 3.10 動作例（Worked Examples）

<details>
<summary>原文</summary>

Consider the curve $E:Y^2=X^3+X$ over $\mathbb{F}_{59}$.
This has 60 points.
Let $G$ be the subgroup of size $r=5$.
One generator of $G$ is $P=(25,30)$:

$$G=E(\mathbb{F}_{59})[5] =\{P=(25,30),2P=(35,31),3P=(35,28),4P=(25,29),5P=O\}$$

Feeding any two of these points as inputs to the Weil pairing is pointless as the output will be 1 since they are linearly dependent.
We must first find other 5-torsion points.
The Tate pairing also does not function at the moment, because $5\nmid 59−1$,

But $5|59^2−1$, and we will see that $E(\mathbb{F}_{59^2})$ contains all 25 points of $E[5]$, so in the field extension $\mathbb{F}_{59^2}$ both the Tate pairing and Weil pairing exist and are nontrivial.
Note $−1$ is a quadratic nonresidue in $\mathbb{F}_{59}$ so we may write $\mathbb{F}_{59^2}=\mathbb{F}_{59}[i]$ where $i=\sqrt{−1}$.

It turns out that the distortion map given by $\phi(x,y)=(−x,iy)$ enables us to write down all of $E[5]$ easily.
This map $\phi$ takes $G$ to another subgroup $G'$ in $E[5]$:

$$G'=\{Q=(−25,30i),\ldots ,4Q=(−25,29i),5Q=O\}$$

where $Q=\phi(P)$, and every element of $E[5]$ can be written as a sum of an element in $G$ and an element in $G'$.

</details>

$\mathbb{F}_{59}$ 上の 曲線 $E:Y^2=X^3+X$  を考えよう。
これは60点（ポイント）ある。
サイズ $r=5$ の部分群を $G$ とする。
$G$ の1つの生成元は $P=(25,30)$ である。

$$G=E(\mathbb{F}_{59})[5] =\{P=(25,30),2P=(35,31),3P=(35,28),4P=(25,29),5P=O\}$$

これらの点のうち、どれか2つをWeilペアリングの入力として与えても、線形従属なので出力は1になり、無意味である。
まず、他の 5-ねじれ点 を見つけなければならない。
Tateペアリングも今は機能していない、なぜなら $5\nmid 59-1$ があるから。

しかし、$5|59^2-1$ とすると、$E(\mathbb{F}_{59^2})$ は $E[5]$ の25点すべてを含むので、拡大体 $\mathbb{F}_{59^2}$ ではTateペアリングとWeilペアリングはともに存在し、非自明であることがわかる。
なお、$-1$ は $\mathbb{F}_{59}$ の平方非剰余（quadratic nonresidue）なので、$i=\sqrt{-1}$ のとき $\mathbb{F}_{59^2}=\mathbb{F}_{59}[i]$ と書くことができる。

このとき、$\phi(x,y)=(-x,iy)$ で与えられる歪曲写像によって、$E[5]$ のすべてを簡単に書き下すことができることがわかった。
この写像 $\phi$ は、$G$ を $E[5]$ の別の部分群 $G'$ へと導くものである。

$$G'=\{Q=(−25,30i),\ldots ,4Q=(−25,29i),5Q=O\}$$

ここで、$Q=\phi(P)$ であり、$E[5]$ のすべての要素は $G$ の要素と $G'$ の要素の和として書くことができる。


### 3.10.1 Weilペアリング（The Weil Pairing）

<details>
<summary>原文</summary>
</details>

Let us first consider the Weil pairing, which we denote by $f$.
We walk through the computation of $f((25, 30),(−25, 30i))$.
The two inputs points are linearly independent thus the output is not $1$.

Computing $f(P,Q)$ entails evaluating several rational functions at various points that depend on two picked points $R$ and $S$.
These points $R$ and $S$ must be selected so that we never encounter a zero or pole while evaluating these functions.

In practice, the probability that randomly chosen $R,S$ are unsuitable is negligible. This is not the case for our toy example, but it turns out that choosing $R=(40,54)$ and $S=(48+55i,28+51i)$ will work.
Note that $R,S$ do not need to lie in $E[5]$.

We describe in detail how Miller’s algorithm arrives at $f_P(Q+S)=f_5(Q+S)$:

1.We compute $f_1=V_{P+R}/L_{P,R}$.
Using the point addition formulas we find $P+R=(6,24)$ and $Q+S=(19+17i,46+18i)$.
Then from the explicit formulas given earlier we have

$$
V_{P+R}:X+53 \\
L_{P,R}:35X+15Y+32
$$

Evaluating both equations at $Q+S$ and taking their quotient gives $x_1=f_1(Q+S)=25+ 31i$.

2.We now enter the main loop. Firstly, $f_2=f^2_1T_P/V_{2P}$ where

$$
T_P:12X+Y+24 \\
V_{2P}:X+24
$$

.
Then we assign $x\leftarrow x^2_1T_P(Q+S)/V_{2P}(Q+S)$ which turns out to be $(18+16i)(3+45i)/(43+17i)=33+22i$ .
3.Since the second bit of $5=101_2$ is zero, the condition for the if statement is false and the next iteration begins.
Now $f_4=f^2_2T_2/V_4$, where

$$
T_{2P}:53X+Y+2\\
V_{4P}:X+34
$$

and we compute $x\leftarrow x^2T_{2P}(Q+S)/V_{4P}(Q+S)$.
It can be checked $x=58+23i$ now.

4.The last bit is 1, so this time we do execute the contents of the if block, namely $x=\leftarrow x\cdot x_1\cdot L_{4P,P}(Q+S)/V_{5P}(Q+S)$.
Since $5P=O$ we discard the denominator in this case, and since $P=−4P$, $L_{4P,P}=V_P$ which is given by 

$$V_P:X+34$$

thus $x=(58+23i)(25+31i)(53+17i)=18+2i$, and the algorithm stops as we have exhausted all the bits.

Thus we have found that $f_P(Q+S)=18+2i$, and yet we never derived an algebraic expression for the rational function $f_P$.
We can repeat the above to find $f_P(S)=10+30i$, but a better way is to compute $f_P (S)$ concurrently to avoid repeating the same operations.
In other words, we simultaneously calculate $f_P(Q+S)$ and $f_P(S)$, and as each equation for a certain line is found, we evaluate at $Q+S$ and also at $S$.

The computation of $f_Q(P+R)$ and $f_Q(R)$ is similar (note since $Q$ does not lie in $F_q$ the $L, T, V$ equations will involve imaginary numbers), and it can be verified that $f_Q(P+R)=27+25i$ and $f_Q(R)=10+41i$.
At last we have

$$f(P,Q)=\frac{f_P(Q+S)/f_P(S)}{f_Q(P+R)/f_Q(R)}=46+56i.$$

Thankfully $(46+56i)^5 = 1$ as expected.


まず、$f$ と表記するWeilペアリングを考えてみましょう。
$f((25, 30),(-25, 30i))$ の計算を順を追って見ていく。
2つの入力点は線形独立なので、出力は $1$ にはならない。

$f(P,Q)$ を計算するには、2つの点 $R$ と $S$ に依存するいくつかの有理関数をさまざまな点で評価しなければならない。
これらの点 $R$ と $S$ は、これらの関数の評価中にゼロや極に遭遇しないように選択されなければならない。

実際には、ランダムに選んだ $R,S$ が不適当である確率は無視できます。
このおもちゃの例ではそうではありませんが、 $R=(40,54)$ と $S=(48+55i,28+51i)$ を選べばうまくいくことがわかります。
なお、$R,S$ は $E[5]$ にある必要はない。

ミラーアルゴリズムがどのようにして $f_P(Q+S)=f_5(Q+S)$ に到達したかを詳しく説明する。

1.$f_1=V_{P+R}/L_{P,R}$ を計算する。
点の加算式から $P+R=(6,24)$、$Q+S=(19+17i,46+18i)$ と求まる。
次に、先ほどの明示的な式から、次のようになります。

$$
V_{P+R}:X+53 \\
L_{P,R}:35X+15Y+32
$$

Evaluating both equations at $Q+S$ and taking their quotient gives $x_1=f_1(Q+S)=25+ 31i$.


<details>
<summary>原文</summary>
</details>
