---
title: "ペアリングベース暗号システムの実装について 2"
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


# Chapter 2<br>楕円曲線（Elliptic Curves）

<details>
<summary>原文</summary>
We review basic facts about elliptic curves.
Aside from the problem of generating suitable elliptic curves and counting the size of the resulting group, we will cover enough theory to replace finite fields with elliptic curves in cryptosystems based on cyclic groups.
We discuss pairings and algorithms to find pairing-friendly curves in future chapters.
We will not discuss curve-finding and point-counting algorithms geared towards standard elliptic curve cryptography which necessarily requires curves that are not pairing-friendly, but instead direct the interested reader to Blake, Seroussi and Smart [9].
</details>

楕円曲線に関する基本的な事実をおさらいする。
適当な楕円曲線を生成し、得られる群の大きさを数える問題は別として、環状群に基づく暗号システムにおいて有限場を楕円曲線に置き換えるのに十分な理論を網羅する。
ペアリングとペアリングに適した曲線の探索アルゴリズムについては、今後の章で説明する。
ペアリングに適さない曲線を必然的に必要とする標準的な楕円曲線暗号に向けた曲線探索とポイントカウントのアルゴリズムについては議論せず、Blake, Seroussi and Smart [9]を参照してもらうことにする。

## 2.1 楕円曲線に関する非公式な概要（Informal Overview of Elliptic Curves）

<details>
<summary>原文</summary>
We present a highly informal overview to elliptic curve theory, which may aid those that have not encountered them before.
This section can be safely skipped.

Consider a polynomial $C$ in two variables $X$, $Y$.
We are interested in the solutions to $C = 0$ which describe a curve on a two-dimensional plane.

We first observe that if $C′$ is another curve that is an affine transformation of $C$, that is, if we can linearly transform (e.g. rotate, scale, shear) and then translate $C$ to obtain $C′$ then a correspondence exists between the solutions to $C = 0$ and the solutions to $C′ = 0$.
Knowing the solutions of one allows us to easily compute the solutions of the other.
For this reason we consider such curves $C$ and $C′$ to be equivalent.

If every term in $C$ has combined degree of at most $1$, that is, if $C = aX + bY + c$ then $C$ describes a line.
The geometry of lines is too simple to yield anything cryptographically useful.

If every term in $C$ has combined degree at most $2$, then $C$ describes a single line, a pair of lines, an ellipse, a parabola or a hyperbola.
The first two possiblities can be viewed as special cases, occuring when $C$ is reducible or degenerate in some sense.

By adding points of infinity to the plane, we can find affine transformations that change any ellipse, parabola or hyperbola into the unit circle centred at the origin.
Intuitively, the two ends of the parabola can be thought of as meeting at a point at infinity, forming a circle, and similarly opposite ends of hyperbolas connect at infinite points.

Thus to study degree $2$ curves is essentially to study the unit circle, whose geometry is again is too simple for our purposes.

However, degree $3$ curves, called elliptic curves, are nontrivial (for instance, unlike the previous two cases we cannot transform any elliptic curve into any other) and have a rich structure well-suited for cryptography.

For any irreducible elliptic curve C, applying appropriate affine transformations produces an equation $C′$ of a certain form known as the Weierstrass form. In this form, $C′$ always contains exactly one point at infinity.
Transforming C so that no points are infinite is possible but leads to more complicated equations.
Using an equation that has exactly one point at infinity simplifies some of the expressions.
</details>

楕円曲線理論に初めて接する人のために、非常に非公式な概観を提示する。
このセクションは読み飛ばしても構わない。

2変数 $X$, $Y$ の多項式 $C$ を考える。
2次元平面上の曲線を記述する $C = 0$ の解に興味がある。

まず、$C$ のアフィン変換である別の曲線を $C′$ とすると、つまり、$C$を線形変換（回転、拡大縮小、せん断など）して、並進して$C′$とすると、$C = 0$ の解と $C′ = 0$ の解の間に対応関係があることを観察することができます。
片方の解がわかれば、もう片方の解を簡単に計算することができる。
そのため、このような曲線 $C$ と $C′$ は等価であると考える。

$C$ のすべての項が最大でも $1$ の結合次数を持つ場合、つまり $C = aX + bY + c$ の場合、$C$ は直線を記述する。
直線の幾何学は単純すぎて、暗号学的に有用なものは得られない。

$C$ のすべての項が最大で $2$ の結合次数を持つとき、$C$は1本の直線、1組の直線、楕円、放物線、双曲線のいずれかを記述する。
最初の2つの可能性は、$C$ がある意味で還元的か縮退的である場合の特殊なケースと見なすことができる。

平面に無限遠の点を加えることで、任意の楕円、放物線、双曲線を原点を中心とする単位円に変えるアフィン変換を見つけることができます。
直感的には、放物線の両端は無限遠の点で出会って円を形成していると考えることができ、同様に双曲線の両端も無限遠の点でつながっています。

したがって、次数$2$の曲線を研究することは、本質的に単位円を研究することであり、その幾何学はまた我々の目的には単純すぎる。

しかし、楕円曲線と呼ばれる次数3$の曲線は、先の2つの場合とは異なり、任意の楕円曲線を他の楕円曲線に変換することができないなど、非自明であり、暗号解読に適した豊かな構造を持っている。

しかし、次数$3$の曲線は楕円曲線と呼ばれ、自明ではない（例えば、前の2つの場合とは異なり、任意の楕円曲線を他の楕円曲線に変換することはできない）、暗号に適した豊かな構造を持っている。

任意の既約楕円曲線 $C$ に対して、適当なアフィン変換を施すと、Weierstrass形式と呼ばれるある形式の方程式 $C′$ が生成されます。この形式では、$C′$ は常に無限遠にちょうど1つの点を含む。
$C$ を無限大になる点がないように変換することも可能であるが、より複雑な方程式になる。
無限遠にちょうど1つの点を持つ方程式を用いると、いくつかの式が簡単になる。

## 2.2 楕円曲線の点（Points on Elliptic Curves）

<details>
<summary>原文</summary>

Let $\mathbb{F}_q$ be a field for some prime $q > 3$.
Unless otherwise specified we shall always define curves over a field of prime order and of characteristic greater than three.
Elliptic curves can be implemented over fields of characteristic 2 and 3 and enjoy many optimizations, but suffer from specialized discrete log attacks [24] and should generally be avoided.
We do consider a certain family of characteristic three curves in Section 4.16, where we also outline their trade-offs.

An elliptic curve $E$ over such a field $\mathbb{F}_q$ is an equation of the form

$$E : Y^2 = X^3 + aX + b$$

where $a, b \in \mathbb{F}_q$.
Let $\Delta = 4a^3 + 27b^2$ , the discriminant of the cubic in $x$.

Then $E$ is singular if $\Delta = 0$, i.e. the cubic has a repeated root, and nonsingular otherwise, i.e. the cubic has distinct roots.
</details>

ある素数 $q > 3$ に対する $\mathbb{F}_q$ を体とする。
特に断らない限り、素数位数で3以上の標数を持つ体に対して常に曲線を定義するものとする。
楕円曲線は標数2と標数3の体に実装でき、多くの最適化を享受できるが、特殊な離散対数攻撃[24]に悩まされるため、一般に避けるべきである。
我々は、セクション4.16で、ある種の特性3曲線の族を検討し、そのトレードオフも概説する。

このような体 $\mathbb{F}_q$ 上の楕円曲線 $E$ は、以下の形式の方程式である。

$$E : Y^2 = X^3 + aX + b$$

ここで、$a, b \in \mathbb{F}_q$である。
ここで、$x$の3乗の判別式を $\Delta = 4a^3 + 27b^2$ とする。

このとき、$E$ は $\Delta = 0$ のとき特異、すなわち、立方が繰り返し根を持つ、それ以外のときは非特異、すなわち、立方が明瞭な根を持つ。

<details>
<summary>原文</summary>

Unless otherwise stated we always consider nonsingular elliptic curves.
Later we will encounter supersingular curves, a particular breed of nonsingular curves that are not singular despite their name.
The antonym of supersingular is ordinary or nonsupersingular.

For any field $\mathbb{F}_{q^k}$ define $E(\mathbb{F}_{q^k})$ to be the set of all solutions of $E$ over $\mathbb{F}_{q^k}$, called the finite points along with a special point denoted $O$, that is called the point at infinity.
We write $\#E(\mathbb{F}_{q^k})$ or $|E(\mathbb{F}_{q^k})|$ for the number of elements of $E(\mathbb{F}_{q^k})$.
Intuitively, the point $O$ can be thought of as the point where all lines parallel to the Y-axis meet. Mathematically, we solve the curve equation using projective coordinates [25] and one can show that $O = (0, 1, 0)$ is always a unique infinite solution to the equation.
</details>

特に断らない限り、我々は常に非特異楕円曲線を考える。
この後、非特異曲線を特殊化した超特異曲線（supersingular curves）を扱いますが、その名前とは裏腹に特異ではありません。
スーパーシンギュラ曲線の反意語は普通または非スーパーシンギュラです。

$E(\mathbb{F}_{q^k})$ は、任意の場の $E$ の解の集合で、有限点と呼ばれるものと、無限遠点と呼ばれる特別な点 $O$ で定義されるものです。
$E(\mathbb{F}_{q^k})$ の要素の数を $\#E(\mathbb{F}_{q^k})$ または $|E(\mathbb{F}_{q^k})|$ と表記する。
直感的には、点 $O$ はY軸に平行な線がすべて交わる点であると考えることができる。数学的には、射影座標を使って曲線方程式を解く [25] と、$O = (0, 1, 0)$ が常に方程式の一意な無限解であることを示すことができる。

<details>
<summary>原文</summary>

We shall never need this fact.

**Theorem (Hasse).**
Let $t = q^k + 1 − \#E(\mathbb{F}_{q^k})$.
Then $|t| \leq 2\sqrt{q^k}$.

Thus the number of points on an elliptic curve in a given field is on the same order as the size of the field. 
The quantity $t$ is called the trace of Frobenius.

**Theorem (Weil).**
Let $t = q + 1 − \#E(\mathbb{F}_q)$ where $q$ is a prime power.
Factor the polynomial $x^2 − tx + q$ as $(x − \alpha)(x − \beta)$ over $\mathbb{C}[x]$.
Then

$$\#E(\mathbb{F}_{q^k}) = q^k + 1 − (\alpha^k + \beta^k).$$

This last theorem is more practical in the following form.
Let $t_0 = 2$, Let $t_1 = q + 1 − \#E(\mathbb{F}_q)$.
Define $t_n$ recursively by

$$t_n = t_1t_{n−1} − qt_{n−2}.$$

Then $\#E(\mathbb{F}_{q^k}) = q^k + 1 − t_k$.

For example, consider the curve $E$ given by

$$Y^2 = X^3 + X + 6$$

over $\mathbb{F}_{19}$, an example used by Balasubramanian and Koblitz [1].
There are 18 points

$$
\begin{array}{llllll}
(0, 5), &(4, 6), &(2, 4), &(3, 6), &(14, 3), &(12, 13),\\
(18, 2), &(10, 3), &(6, 0), &(10, 16), &(18, 17), &(12, 6),\\
(14, 16), &(3, 13), &(2, 15), &(4, 13), &(0, 14), &O\\
\end{array}
$$

thus the trace of Frobenius $t = 2$.
Over $\mathbb{F}_{192}$ , we have 

$$\#E(\mathbb{F}_{192}) = 19^2 + 1 − t_2$$

where $t_2 = 2 \cdot 2 − 19 \cdot 2 = −34$, thus $\#E(\mathbb{F}_{19^2}) = 396$.
</details>

この事実は、決して必要ないだろう。

**定理(Hasse).**
$t = q^k + 1 - \#E(\mathbb{F}_{q^k})$ とする。
すると、$|t| \leq 2\sqrt{q^k}$ となる。

したがって、ある場の楕円曲線上の点の数は、その場の大きさと同じオーダーになる。
この量 $t$ はフロベニウスのトレースと呼ばれる。

**定理(Weil).**
$t = q + 1 - \#E(\mathbb{F}_q)$ ここで、$q$ は素数乗とする。
多項式 $x^2 - tx + q$ を $\mathbb{C}[x]$ の上で $(x - \alpha)(x - \beta)$ と因数分解せよ。
次に

$$\#E(\mathbb{F}_{q^k}) = q^k + 1 − (\alpha^k + \beta^k).$$

この最後の定理は、次のような形がより実用的である。
$t_0 = 2$, $t_1 = q + 1 - \#E(\mathbb{F}_q)$ とする。
$t_n$ を再帰的に次のように定義する。

$$t_n = t_1t_{n−1} − qt_{n−2}.$$

このとき、$\#E( \mathbb{F}_{q^k}) = q^k + 1 - t_k$ となります。

例えば、次のように与えられる曲線 $E$ を考えてみましょう。

$$Y^2 = X^3 + X + 6$$

$\mathbb{F}_{19}$の上で, Balasubramanian and Koblitz [1] が使用した例です。
18個の点

$$
\begin{array}{llllll}
(0, 5), &(4, 6), &(2, 4), &(3, 6), &(14, 3), &(12, 13),\\
(18, 2), &(10, 3), &(6, 0), &(10, 16), &(18, 17), &(12, 6),\\
(14, 16), &(3, 13), &(2, 15), &(4, 13), &(0, 14), &O\\
\end{array}
$$

したがって、フロベニウスのトレースは $t = 2$ となります。
$\mathbb{F}_{192}$ の上で

$$\#E(\mathbb{F}_{192}) = 19^2 + 1 − t_2$$

ここで、$t_2 = 2 \cdot 2 - 19 \cdot 2 = -34$ なので、$\#E( \mathbb{F}_{19^2}) = 396$ となります。


## 2.3 点の発見（Finding Points）

<details>
<summary>原文</summary>

Let $E : Y^2 = X^3 +aX +b$ be an elliptic curve over a field $K$.
There always exists an unique infinite solution, namely $O$.
We describe a simple method for finding the finite points of $E$.

For any $x \in K$, we may attempt to solve $Y^2 = x^3 + ax + b$ for $Y$ by finding a square root of the right-hand side.
We momentarily postpone describing the details of square root algorithms.
For now, assume we can find square roots.

When solutions for $Y$ do exist for a given $x$, we have found exactly two points, one for each square root, except in the rare case when the point lies on the $X$-axis, which can happen in at most three places.

Also, recall from an above theorem that the size of $K$ is roughly the same as the number of points on $E(K)$.

Combining these two facts shows that for approximately half of the choices for $x \in K$, a square root exists and we can solve $E$ to find a point.
Thus we have a fast method of finding random points on $E$:

1.Choose $x \in K$ at random.

2.Solve $Y^2 = x^3 + ax + b$ for $Y$.
If there are no solutions then go to the previous step.

3.Flip a coin to decide which solution of $Y$ to use.
</details>

$E : Y^2 = X^3 +aX +b$ を体 $K$ 上の楕円曲線とする。
このとき、必ず一意な無限解、すなわち $O$ が存在する。
本論文では、$E$ の有限点を求める簡単な方法について述べる。

任意の $x \in K$ に対して、$Y^2 = x^3 + ax + b$ を右辺の平方根を求めることで解こうとすることができる。
平方根のアルゴリズムの詳細については、しばらく先送りする。
とりあえず、平方根を求めることができるとする。

ある $x$ に対して $Y$ の解が存在する場合、$X$ 軸上に点がある場合を除いて、それぞれの平方根に対して1つずつ、合計2つの点を見つけることができたが、これは最大で3箇所しか起こらない。

また、上の定理から、$K$ の大きさは $E(K)$ 上の点の数とほぼ同じであることを思い出してください。

この2つの事実を組み合わせると、$x \in K$の選択肢の約半分について、平方根が存在し、$E$を解いて点を見つけることができることがわかります。
このように、$E$ 上のランダムな点を高速に見つける方法がある。

1.ランダムに$x \in K$を選ぶ。

2.$Y$ について $Y^2 = x^3 + ax + b$ を解け。
解がない場合は前のステップに進む。

3.コインで $Y$ のどの解を使うかを決める。

<details>
<summary>原文</summary>

Of course, it is impossible to choose the point at infinity with this method, and points that lie on the $X$-axis have a slightly higher probability of been picked than other points.
For cryptography this is of no concern since the point of infinity is usually unwanted, and the probability of finishing at a point with zero $Y$-coordinate is negligible since there are at most three of them.
Moreover, it is often unimportant which square root is chosen.

If one insists on choosing all points of $E(K)$ uniformly, one could simply add a step before choosing $x$.
Let $n = \#E(K)$.
Then with with $1/n$ probability, choose $O$ or one of the points lying on the $X$-axis, otherwise proceed with the above algorithm, except in the second step, we also go back to the first step if the only solution is $Y = 0$.

Before attempting to find a square root of a given element $x \in K$, we can check that one actually exists first.
When $K$ has prime order, one can compute the Legendre symbol before attempting to square root $x$.
More generally it can be checked that $X^2 − x$ is reducible.

Alternatively, one can omit the check, proceed with a square root algorithm, and compare the square of the output with $x$: if there is a mismatch then $x$ is not a square after all.

It remains to describe how to take square roots. For a field of prime order one can use the Tonelli-Shanks algorithm to compute square roots [9, 47], which we quote below.
For a general finite field, one must use a more complex algorithm.
Perhaps the simplest of these Legendre’s method which can be viewed as using the Cantor-Zassenhaus algorithm of Section 5.8 to factor $X^2 − x$.
Faster algorithms exist, though sometimes require precomputation [8].
</details>

もちろん、この方法では無限遠の点を選ぶことは不可能であり、$X$ 軸上にある点は他の点よりも選ばれる確率が若干高くなる。
暗号技術にとって、無限遠の点は通常不要であり、$Y$ 座標が0の点で終わる確率はせいぜい3つなので、これは無視できる程度である。
さらに、どの平方根が選ばれるかは重要でないことが多い。

$E(K)$ のすべての点を一様に選ぶことにこだわるなら、$x$ を選ぶ前に一段追加すればよい。
$n = \#E(K)$ とする。
このとき、$1/n$ の確率で、$O$ または $X$ 軸上にある点のいずれかを選び、それ以外は上記のアルゴリズムで進め、ただし、第2ステップで、唯一の解が $Y=0$ であれば第1ステップに戻る。

与えられた要素 $x \in K$ の平方根を求めようとする前に、まずそれが実際に存在するかどうかを確認することができる。
$K$ が素数のとき、$x$ の平方根を求める前に、Legendre記号を計算することができる。
より一般的には、$X^2 - x$ が還元可能であることを確認することができる。

また、チェックを省略して平方根アルゴリズムで進め、出力の2乗と $x$ を比較し、不一致であれば $x$ は結局2乗ではない、ということもできる。

あとは、平方根の取り方を説明する。素数階の場では、Tonelli-Shanksアルゴリズムを使って平方根を計算することができる [9, 47]ので、以下にそれを引用する。
一般有限体では、もっと複雑なアルゴリズムが必要です。
最も簡単なのはLegendreの方法で、これは5.8節のCantor-Zassenhausアルゴリズムを使って $X^2 - x$ を因数分解していると見なすことができる。
より高速なアルゴリズムも存在するが，事前の計算が必要な場合もある[8]．

### 2.3.1 Tonelli-Shanksのアルゴリズム（Tonelli-Shanks Algorithm）

<details>
<summary>原文</summary>

Suppose we wish to compute $b = \sqrt{a}$ in a field $\mathbb{F}_q$ for some prime $q$.
(These paragraphs should be viewed as self-contained; the notation from earlier paragraphs does not apply here.)

---

**Algorithm 1** (Tonelli-Shanks) Find $b = \sqrt{a}$ in a prime field

&nbsp;1: Find an element $g \in \mathbb{F}_q$ that is not a square.
&nbsp;2: Since $q$ is odd (unless $q = 2$ in which case square roots are trivial), we may write $q − 1 = 2^st$ for some odd $t$.
&nbsp;3: $e \leftarrow 0$
&nbsp;4: **for** $i \leftarrow 2$ to $s$ **do**
&nbsp;5: &emsp;**if** $(ag^{−e})^{(q−1)/2^i} \ne 1$ **then**
&nbsp;6: &emsp;&emsp;$e \leftarrow 2^{i−1} + e$
&nbsp;7: &emsp;**end if**
&nbsp;8: **end for**
&nbsp;9: $h \leftarrow ag^{−e}$
10: $b \leftarrow g^{e/2}h^{(t+1)/2}$
11: Return $b$

---

The first step can be accomplished by choosing random elements $g \in \mathbb{F}_q$ until $g^{(q−1)/2} =−1$.
Clearly this $g$ can be stored for use in future square root computations in the samea field.

We can briefly explain the Tonelli-Shanks algorithm as follows.
Observe square roots ina a cyclic group of order $t$ where $t$ is odd can be computed by exponentiating by $(t + 1)/2$.
Then using $\mathbb{F}^∗_q \cong \mathbb{Z}^+_{2^s} \times \mathbb{Z}^+_t$ for some odd $t$ leads to the above.
</details>

ある素数 $q$ の体 $\mathbb{F}_q$ で $b = \sqrt{a}$ を計算したいとする。
(この段落は自己完結していると考えるべきで、先の段落の表記法はここでは適用されない)。

---

**アルゴリズム 1** (Tonelli-Shanks) 素数体において$b = \sqrt{a}$ を求めよ。

&nbsp;1: 平方根ではない要素 $g \in \mathbb{F}_q$ を求めよ。
&nbsp;2: $q$ は奇数なので（ただし、$q = 2$の場合は平方根は自明）、ある奇数 $t$ に対して $q - 1 = 2^st$ と書くことができる。
&nbsp;3: $e \leftarrow 0$
&nbsp;4: **for** $i \leftarrow 2$ to $s$ **do**
&nbsp;5: &emsp;**if** $(ag^{−e})^{(q−1)/2^i} \ne 1$ **then**
&nbsp;6: &emsp;&emsp;$e \leftarrow 2^{i−1} + e$
&nbsp;7: &emsp;**end if**
&nbsp;8: **end for**
&nbsp;9: $h \leftarrow ag^{−e}$
10: $b \leftarrow g^{e/2}h^{(t+1)/2}$
11: $b$ を返す。

---

$g^{(q−1)/2} =−1$ になるまで、$g \in \mathbb{F}_q$ をランダムに選べばよいのです。
この $g$ を保存しておけば、将来同じ分野で平方根を計算するときに利用できることは明らかである。

Tonelli-Shanksアルゴリズムを簡単に説明すると、次のようになる。
$t$ が奇数の周期群における平方根は、$(t + 1)/2$ の指数で計算できることを確認する。
そして、ある奇数 $t$ に対して  $\mathbb{F}^∗_q \cong \mathbb{Z}^+_{2^s} \times \mathbb{Z}^+_t$ を用いると、上記のようになる。

### 2.3.2 ハッシュ点（Hashing to Points）

<details>
<summary>原文</summary>
Finding points by choosing an $X$-coordinate and solving for $Y$ suggests an efficient algorithm for hashing to a point in $E$.
The input is hashed to some $x \in K$, and then a corresponding $y$ is sought.
On failure, a new $x$-coordinate is deterministically generated from $x$, and again we attempt to solve $E$ for $y$. 
Repeating this process as many times as necessary eventually yields a valid point $(x, y) \in E(K)$.
</details>

$X$ 座標を選んで $Y$ を解くことで点を見つけることは、$E$ の点へのハッシュ化の効率的なアルゴリズムを示唆している。
入力は $K$ のある $x$ にハッシュされ、次に対応する$y$ が求められる。
失敗すると、$x$ から新しい $x$ 座標を決定論的に生成し、再び $E$ を $y$ に対して解こうとする。
これを必要なだけ繰り返すと、最終的に $E(K)$ に有効な点 $(x, y) \in E(K)$ を得ることができる。

## 2.4 点の圧縮と縮小（Point Compression and Reduction）

<details>
<summary>原文</summary>
Another implication of taking square roots to find a $Y$ corresponding to a particular $X$ value is that a point $(x, y)$ can be represented by $x$ along with a single bit indicating which solution of $y$ to take.
This technique is known as point compression.

Alternatively, for some cases we can simply use $x$ alone, which is sometimes called point reduction.
For example, suppose Alice needs to send Bob a point in some cryptosystem.
She sends Bob $x$, who guesses the solution $y$.
Bob attempts to proceed normally.
If the protocol fails e.g. the signature does not verify, then Bob tries again with $−y$.
This does not cost much more since the solutions are related by negation, and in Section 6.13 we show how to check both possibilities with only one operation.

Trivially we can take this principle further.
Alice can omit $k$ bits of $x$, and leave Bob to try all $2^k$ possibilities.
</details>

また、ある特定の $X$ 値に対応する $Y$ を求めるために平方根を取ることのもう一つの意味は、点 $(x, y)$ を $x$ と、どの $y$ の解を取るかを示す1ビットで表すことができることである。
この手法は点圧縮と呼ばれる。

あるいは、単純に$x$だけを使うことができる場合もあり、これをポイント・リダクションと呼ぶこともある。
例えば、アリスがある暗号システムでボブに点を送る必要があるとする。
彼女はボブに $x$ を送り、ボブはその解 $y$ を推測する。
ボブは正常に処理を進めようとする。
もしプロトコルが失敗したら、例えば署名が検証されなかったら、ボブ は $-y$ で再試行する。
これは、解が否定によって関連しているので、それほどコストがかからない。 第6.13節では、たった1回の操作で両方の可能性をチェックする方法を示す。

自明なことだが、この原則をさらに推し進めることができる。
アリスは $x$ の $k$ ビットを省略し、ボブに $2^k$ 個の可能性をすべて試させることができる。


## 2.5 Chord-Tangent Lawの合成（The Chord-Tangent Law of Composition）

<details>
<summary>原文</summary>
We define an operation $+$ on $E(\mathbb{F}_{q^k})$.
Let $P = (a, b)$, $Q = (c, d) \in E(\mathbb{F}_{q^k})$ be finite points.

If $P \ne Q$, then it is not hard to show that if $a \ne c$ then the line through $P$ and $Q$ must intersect $E$ at another point $(x, y)$ where $x, y \in \mathbb{F}_{q^k}$.
Note that $(x, −y)$ also is a solution of $E$.
Define $P + Q = (x, −y)$ for $a \ne c$.
If $a = c$ (in which case we must have $b = −d$), then define $P + Q = O$.

Now suppose $Q = P$.
In this case, consider the tangent line going through $P$.
It turns out it must intersect $E$ at another point $(x, y)$ where $x, y \in \mathbb{F}_{q^k}$ unless $b = 0$.
Define $P + P = (x, −y)$ for $b \ne 0$.
For $b = 0$ define $P + P = O$.

Lastly define $P + O = P$, $O + O = O$.

This operation turns $E(\mathbb{F}_{q^k})$ into a group.
The point $O$ is the identity, and the inverse of a point $P = (x, y)$ is the point $−P = (x, −y)$.

As usual, define $0P = O$, $1P = P$, $nP = (n − 1)P + P$ for integers $n > 1$ and $nP = −(−n)P$ for integers $n < 0$.
This operation is termed point multiplication.
Point multiplication can be performed efficiently via carefully chosen point additions, in a process that mirrors the repeated squaring technique for exponentiation in finite fields.

Recall the previous chapter used multiplicative group notation to emphasize the connection between discrete log cryptosystems and pairing-based cryptosystems, as we are using elliptic curve groups where one would use the multiplicative group of a finite field.
Mathematicians use additive group notation for the elliptic curve group since the group is Abelian, and we will adhere to this convention in this section.
</details>

$E(\mathbb{F}_{q^k})$ 上に演算 $+$ を定義する。
$E(\mathbb{F}_{q^k})$ の $P = (a, b)$, $Q = (c, d)$ を有限点とする。

$P \ne Q$ で $a \ne c$ ならば、$P$ と $Q$ を通る線は、$x, y \in \mathbb{F}_{q^k}$ で別の点 $(x, y)$ で $E$ と交わらなければならないことを示すことは難しくない。
ただし、$(x, -y)$ も $E$ の解である。
$P + Q = (x, -y)$ を $a \ne c$ に対して定義する。
$a = c$ のとき（この場合、$b = -d$ でなければならない）、$P + Q = O$ と定義する。

最後に $P + O = P$, $O + O = O$ を定義する。

この操作により、$E(\mathbb{F}_{q^k})$ は群になる。
点 $O$ は恒等式であり、点 $P = (x, y)$ の逆は点 $-P = (x, -y)$ である。

例によって、整数 $n > 1$ に対して $0P = O$, $1P = P$, $nP = (n - 1)P + P$, 整数 $n < 0$ に対して $nP = -(-n)P$ と定義します。
この演算を点の乗算という。
点の乗算は、有限体における指数計算の反復二乗法と同じように、注意深く選んだ点の加算によって効率的に行うことができる。

前章では、離散対数暗号とペアリング暗号の関連性を強調するために、有限体の乗法群を用いるところを楕円曲線群を用いて乗法群表記を行ったことを思い出してください。
楕円曲線群はAbelianであるため、数学者は加群表記を用いるが、本章ではこの慣習を踏襲する。

## 2.6 明示的な計算式（Explicit Formulas）

<details>
<summary>原文</summary>
Let $E : y^2 = x^3 + ax + b$ be an elliptic curve over $\mathbb{F}_{q^k}$.
Let $P_1, P_2 \in E(\mathbb{F}_{q^k})$ and suppose we wish to find $P_3 = P_1 + P_2$.
Note the case where at least one of $P_1$, $P_2$ is $O$ is trivial, so assume both $P_1$ and $P_2$ are finite.
Then write $P_1 = (x_1, y_1)$, $P_2 = (x_2, y_2)$.

If $x_1 = x_2$ and $y_1 = −y_2$ then the line $V$ through $P_1$ and $P_2$ is vertical and can be given by

$$V : X − x_1 = 0.$$

In this case $P_3 = O$ (and $P_1 = −P_2$).

Otherwise $P_3$ also must be finite, hence write $P_3 = (x_3, y_3)$.
We have two cases.
If $P_1 = P_2$ then the tangent line $T$ at $P_1$ has slope

$$\lambda=\left[ \frac{\partial E/\partial X}{\partial E/\partial Y} \right]_{(x_1,y_1)} = \frac{3x^2_1+a}{2y_1}.$$

and if $P_1 \ne P_2$ then the line $L$ through $P_1$ and $P_2$ has slope

$$\lambda = (y_2 − y_1)/(x_2 − x_1).$$

If we set $\mu = y_1 − \lambda x_1$ then the equation of $L$ or $T$ is given by:

$$L, T : Y − (\lambda X + \mu) = 0$$

We can find the $x$-coordinate of the other point of intersection $(x_3, −y_3)$ by substituting $Y = \lambda X + \mu$  into $E$.
We find

$$E(X, \lambda X + \mu) = X^3 − \lambda^2X^2 + a_1X + a_0 = 0$$

for some $a_1, a_0 \in \mathbb{F}_{q^k}$ , thus the sum of the roots $x_1 + x_2 + x_3 = \lambda^2$.

This allows us to compute $x_3, y_3$.
Explicitly we have for $P_1 \ne P_2$:

$$
\begin{align*}
\lambda &\leftarrow (y_2 − y_1)/(x_2 − x_1) \\
x_3 &\leftarrow \lambda^2 − x_1 − x_2 \\
y_3 &\leftarrow (x_1 − x_3)\lambda − y_1 \\
\end{align*}
$$

and for $P_1 = P_2$ :

$$
\begin{align*}
\lambda &\leftarrow (3x^2_1 + a)/(2y_1) \\
x_3 &\leftarrow \lambda^2 − 2x_1 \\
y_3 &\leftarrow (x_1 − x_3)\lambda − y_1 \\
\end{align*}
$$

The most expensive step is the division in the computation of $\lambda$.

The expressions for the lines $L, T, V$ will be used later in the computation of a pairing.

For the curve $Y^2 = X^3 + X + 6$ over $\mathbb{F}_{19}$, using these formulas, it can be verified for instance that $(0, 5) + (0, 5) = (4, 6)$, and $(0, 5) + (2, 15) = (4, 13)$.
</details>

$E : y^2 = x^3 + ax + b$ を$\mathbb{F}_{q^k}$上の楕円曲線であるとする。
$E(\mathbb{F}_{q^k})$ に $P_1, P_2$ があるとし、$P_3 = P_1 + P_2$ を求めたいものとする。
なお、$P_1$, $P_2$ の少なくとも一方が $O$ の場合は些細なことなので、$P_1$ と $P_2$ はともに有限であると仮定する。
そして、$P_1 = (x_1, y_1)$, $P_2 = (x_2, y_2)$と書けばよい。

$x_1 = x_2$ と $y_1 = -y_2$ のとき、$P_1$ と $P_2$ を通る直線 $V$ は垂直であり、次式で与えることができる。

$$V : X − x_1 = 0.$$

この場合、$P_3 = O$となる（$P_1 = -P_2$となる）。

そうでなければ、 $P_3$ も有限でなければならないので、 $P_3 = (x_3, y_3)$ と書きます。
2つの場合がある。
$P_1 = P_2$ のとき、 $P_1$ の接線 $T$ は傾き


$$\lambda=\left[ \frac{\partial E/\partial X}{\partial E/\partial Y} \right]_{(x_1,y_1)} = \frac{3x^2_1+a}{2y_1}.$$

であり、$P_1 \ne P_2$ ならば $P_1$ と $P_2$ を通る直線 $L$ の傾きは

$$\lambda = (y_2 − y_1)/(x_2 − x_1).$$

ここで、$\mu = y_1 - \lambda x_1$ とすれば、$L$ または $T$ の方程式は次式で与えられる。

$$L, T : Y − (\lambda X + \mu) = 0$$

$E$ に $Y = \lambda X + \mu$ を代入すれば、交点 $(x_3, -y_3)$ のもう一つの $x$ 座標を求めることができます。
このとき

$$E(X, \lambda X + \mu) = X^3 − \lambda^2X^2 + a_1X + a_0 = 0$$

に対して、ある $a_1, a_0 \in \mathbb{F}_{q^k}$ であるから、根の和 $x_1 + x_2 + x_3 = \lambda^2$ が得られる。

これによって、$x_3, y_3$を計算することができる。
明示的に$P_1 \ne P_2$のときがある。

$$
\begin{align*}
\lambda &\leftarrow (y_2 − y_1)/(x_2 − x_1) \\
x_3 &\leftarrow \lambda^2 − x_1 − x_2 \\
y_3 &\leftarrow (x_1 − x_3)\lambda − y_1 \\
\end{align*}
$$

で、$P_1 = P_2$ の場合。

$$
\begin{align*}
\lambda &\leftarrow (3x^2_1 + a)/(2y_1) \\
x_3 &\leftarrow \lambda^2 − 2x_1 \\
y_3 &\leftarrow (x_1 − x_3)\lambda − y_1 \\
\end{align*}
$$

最もコストがかかるのは、$\lambda$の計算における除算のステップである。

線分$L, T, V$の式は、後でペアリングの計算で使うことになる。

$\mathbb{F}_{19}$ 上の $Y^2 = X^3 + X + 6$ の曲線について、これらの公式を用いて、例えば、 $(0, 5) + (0, 5) = (4, 6)$, $(0, 5) + (2, 15) = (4, 13)$ を確認することができる。

## 2.7 楕円曲線暗号（Elliptic Curve Cryptography）

<details>
<summary>原文</summary>

We now possess enough theory to show how elliptic curves may be used in cryptography.
Let $E$ be an elliptic curve over a field $K$.
The group operation described above means that every point on $E$ generates a cyclic group $G$.
Then we can use $G$ for cyclic group cryptography provided that its order is prime, that the basic operations, namely group operation, inversion, hashing, are efficient, and that problems such as discrete log are difficult.

The formulas above show that only a few operations in $K$ are required for point addition and negation.
We previously saw how to hash to points in $E(K)$.
Thus it seems elliptic curve cryptography can replace cryptography in finite fields by using points in $E(K)$ instead of elements of some $F^∗$ for some finite field $F$, and the group operation is point addition instead of modular multiplication.
The only obstacle is ensuring that randomly chosen points and hashed points lie in $G$, and not all of $E(K)$.

Recall that cryptographic schemes in $F^∗$ for some finite field $F$ often operate within a subgroup $G$ of a particular order $r$, so elements chosen at random and hashed to must have order $r$, or a factor of $r$.
But an element of $F^∗$ in general does not have such an order.
Thus after using some algorithm to choose or hash to some element $x \in F^∗$, to obtain an element of a suitable order one simply exponentiates $x$ by $n/r$ where $n = \#F^∗$.
On elliptic curves, the construction of a point of order $r$, or a factor of $r$, from some given point $P \in E(K)$ can be accomplished similarly by multiplying $P$ by $n/r$ where $n = \#E(K)$.

</details>

楕円曲線が暗号にどのように使われるかを示すのに十分な理論を持っている。
$E$ を $K$ 体上の楕円曲線とする。
前述の群演算は、$E$ 上のすべての点が巡回群 $G$ を生成することを意味する。
そうすると、$G$ は、その位数が素数であること、群演算、反転、ハッシュという基本演算が効率的であること、離散対数などの問題が困難であることを条件に、巡回群暗号に利用することができる。

上の式から、点の加算と否定のために必要な操作は $K$ におけるわずかなものであることがわかる。
前回、$E(K)$ の点へのハッシュの方法を見た。
したがって、楕円曲線暗号は、ある有限体 $F$ に対するある$F^∗$の要素の代わりに $E(K)$ の点を用い、群演算はモジュラー掛け算の代わりに点加算を用いることで、有限体における暗号を置き換えることができるようです。
唯一の障害は、ランダムに選ばれた点とハッシュ化された点が $E(K)$ の全てではなく、$G$ に存在することを保証することである。

ある有限体 $F$ の $F^∗$ における暗号化方式は、しばしば特定の位数 $r$ の部分群 $G$ 内で動作するので、ランダムに選ばれてハッシュ化される要素は位数 $r$ か、$r$ の因子を持たなければならないことを思い出して下さい。
しかし、一般に $F^∗$ の要素はそのようなオーダーを持たない。
したがって、あるアルゴリズムで $F^∗$ の要素 $x$ を選んだりハッシュしたりした後、適当な次数の要素を得るには $x$ を $n = \#F^∗$ で指数化するだけでよいのです。
楕円曲線では、$E(K)$ の点 $P$ から $r$ の位数、または $r$ の因子を作るには、$P$ に $n/r$ を掛ければよい（$n = \#E(K)$ ）。

<details>
<summary>原文</summary>

Let $n = \#E(K)$.
Then from Abelian group theory for any prime $r$ dividing $n$, there exists a point $P \in E(K)$ of order $r$, and furthermore, if $r^2$ does not divide $n$ then there is exactly one subgroup $G$ of $E(K)$ of order $r$.

This suggests the following procedure for implementing any cryptographic scheme based on cyclic groups of prime order:

1.Choose any curve $E(K)$ and somehow work out $n = \#E(K)$.
2.Find a prime $r$ dividing $n$, such that $r^2 \nmid n$.
We shall work in the unique cyclic subgroup $G \subset E(K)$ of points of order $r$.
3.When a random group element of $G$ is required, first choose a random point of $E(K)$ and then multiply by $n/r$.
Similarly, when hashing to a point of $G$, first hash to a point in $E(K)$ and then multiply by $n/r$.
4.Other operations are straightforward: every time a group operation is required, we perform a point addition.
To find an inverse of a group element, we negate the ycoordinate of a point.
When an exponentiation is called for we carry out a point multiplication.

This easily generalizes to squarefree $r$.

In some applications it does not matter if the group is not cyclic.
For example, many cryptosystems function equally well in a group isomorphic to $\mathbb{Z}^+_r \times \mathbb{Z}^+_r$.
In these cases we allow $r^2$ to divide $n$.

We quote another well-known theorem which implies that we never have more than two copies of  $\mathbb{Z}^+_r$ in $E(K)$.
Note our most general abstract definition of the pairing in fact permits groups with at least three copies of a cyclic group, even though this theorem shows this is impossible for elliptic curves.

**Theorem** (See Silverman [62]).

$$E(K) = \mathbb{Z}^+_r \times \mathbb{Z}^+_s$$

for some integers $r$, $s$ with $r|s$.

In the running example, $E : Y^2 = X^3 + X + 6$ over $\mathbb{F}_{19}$, the order of the group is $n = 18 = 3^2 \times 2$, and it so happens that the group is cyclic: it can be checked that $(0, 5)$ is a generator.
Thus this curve can be used for cryptography on a cyclic group of order $r=3$.
However, in general, if $r^2|n$ there may be more than one subgroup of order $r$ and the above procedure cannot be followed.

We have yet to describe how to compute $\#E(K)$ for a given curve.
Fast algorithms exist for this [9] but it turns out that if a pairing is desired, we must seek out elliptic curves whose orders satisfy various conditions.
As a result, instead of choosing a curve first and counting the number of points $n$ and hoping for a large prime factor $r$ of $n$, we must use families of curves where the size of the group is known in advance and has the requisite properties, a subject of a later chapter.

For now, we exhibit one case where $\#E(K)$ is always easy to determine and furthermore $E(K)$ is always cyclic.
</details>

$n = \#E(K)$とする。
$n$ を割る任意の素数 $r$ に対して、アーベル群論から、位数 $r$ の点 $P \in E(K)$ が存在し、$r^2$ が $n$ を割れないなら、$E(K)$ の部分群 $G$ のうち、位数 $r$ のものがちょうど1つ存在することになる。

このことから、素数の環状群に基づく暗号化方式を実現するためには、以下のような手順が考えられます。

1.任意の曲線 $E(K)$ を選び、$n = \#E(K)$ を何とかして求めよ。
2.$n$ を割る素数 $r$ で、$r^2 \nmid n$ となるようなものを探す。
ここでは、位数 $r$ の点の一意な環状部分群 $G \subset E(K)$ で解くものとする。
3.$G$ のランダムな群要素が必要なとき、まず $E(K)$ のランダムな点を選び、$n/r$ を掛ける。
同様に、$G$ の点にハッシュする場合は、まず $E(K)$ の点にハッシュし、次に $n/r$ を乗じる。
4.その他の操作は簡単で、群演算が必要な場合はその都度、点の加算を行う。
群要素の逆数を求めるには、ある点のy座標を否定する。
指数演算が必要なときは点の乗算を行う。

これは無方程式 $r$ に簡単に一般化できる。

いくつかの応用では、群が環状でなくても問題ではありません。
例えば、多くの暗号は $\mathbb{Z}^+_r$ に同型の群でも等しく機能する。
このような場合、$r^2$ が $n$ を割り切れるようにします。

$E(K)$ において $\mathbb{Z}^+_r$ のコピーを2つ以上持つことはないことを意味する、もう一つのよく知られた定理を引用する。
この定理は楕円曲線では不可能であることを示しているが、我々の最も一般的な対の定義では、実際には少なくとも3つの環状群のコピーを持つ群を許容していることに注意。

**定理** （Silverman [62]参照）。

$$E(K) = \mathbb{Z}^+_r \times \mathbb{Z}^+_s$$

を、ある整数 $r$, $s$ で $r|s$ とする。

実行例  $\mathbb{F}_{19}$ 上の $E : Y^2 = X^3 + X + 6$  では、群の位数は $n = 18 = 3^2 \times 2$ で、たまたま群が巡回しており、 $(0, 5)$ が生成元であることが確認できる。
したがって、この曲線は、次数 $r=3$ の巡回群に対する暗号に用いることができる。
しかし、一般に $r^2|n$ の場合、位数 $r$ の部分群が複数存在する可能性があり、上記の手順はとれない。

与えられた曲線に対してどのように $\#E(K)$ を計算するかについてまだ説明していない。
このための高速なアルゴリズムは存在するが[9]、ペアリングが必要な場合、様々な条件を満たす位数を持つ楕円曲線を探し出さなければならないことがわかった。
その結果、最初に曲線を選んで点数$n$を数え、$n$ の大きな素因数 $r$ に期待するのではなく、群の大きさがあらかじめ分かっていて必要な性質を持つ曲線の族を使わなければならない。このことは後の章の課題である。

とりあえず、$\#E(K)$ が常に決定しやすく、さらに $E(K)$ が常に環状である場合を1つ示す。

## 2.8 特異な楕円曲線（Singular Elliptic Curves）

<details>
<summary>原文</summary>
Using singular elliptic curves is equivalent to conventional cryptography.
It can be shown that the set $E_{ns}$-of nonsingular points (all but one) of a singular elliptic curve over a field $K$ also form a group under the chord-tangent law, and $E_{ns} \cong K^∗$ or $E_{ns} \cong K^+$ [62, Proposition 2.5].
Furthermore these isomorphisms are efficiently computable in either direction.
Note the latter case is useless for cryptographically as discrete log is easy in $K^+$.

For example, consider the curve

$$E : y^2 = x^3 + x^2$$

This has a singular point at $(0, 0)$.
Then the other solutions to this curve (including the point at infinity) form a group $E_{ns}(\mathbb{F}_{q^k})$ for any finite field $\mathbb{F}_{q^k}$.

Furthermore, there is an isomorphism $E_{ns}(\mathbb{F}_{q^k}) \rightarrow \mathbb{F}^∗_{q^k}$ given by

$$(x, y) \mapsto \frac{y − x}{y + x}$$

(and maps $O$ to $1$) which is efficienty computable in either direction.
A little algebra shows the reverse map is

$$z \mapsto \left( x' , \frac{1+z}{1-z}x' \right)$$

where $x′ = 4z/(1 − z)^2$ for $z \ne 1$, and $1 \mapsto O$.

This example has little practical value as point additions are slower than modular multiplications, and keeping both coordinates of a point in memory takes twice as much space.
However, it does suggest that cryptography in finite fields can be viewed as a special case of elliptic curve cryptography, and strictly speaking, those that complain that elliptic curve cryptography is controversial, too experimental and untested must qualify their remarks by making exceptions for singular curves!

Needless to say, it is entirely possible that researchers may one day discover special discrete log algorithms that work only for singular curves, in which case we are left only with finite fields for cryptography.
Similarly, if a subexponential discrete log algorithm applicable to any elliptic curve were found, then again finite fields would be the only reasonable choice because elliptic curves are less efficient for the same field size.

It is clear that if an efficiently bilinear nondegenerate pairing were found for singular curves then the Decisional Diffie-Hellman problem could be broken in finite fields.
</details>

特異な楕円曲線を利用することは、従来の暗号と等価である。
$K$ 体上の特異楕円曲線の非特異点（1点を除くすべて）の集合 $E_{ns}$ も和音接線則（chord-tangent）の下で群を形成し、$E_{ns} \cong K^∗$ または $E_{ns} \cong K^+$ となることが示される。 [62, Proposition 2.5]
さらにこれらの同型はどちらの方向にも効率的に計算可能である。
なお、後者の場合は、$K^+$では離散対数が簡単なので、暗号的には意味がない。

例えば、次のような曲線を考えてみましょう。

$$E : y^2 = x^3 + x^2$$

これは $(0, 0)$ に特異点を持っている。
そして、この曲線の他の解（無限遠点にある点を含む）は、任意の有限体 $\mathbb{F}_{q^k}$ に対して群 $E_{ns}(\mathbb{F}_{q^k})$ を形成します。

さらに、次式で与えられる同型性 $E_{ns}(\mathbb{F}_{q^k}) \rightarrow \mathbb{F}^∗_{q^k}$ が存在する。

$$(x, y) \mapsto \frac{y − x}{y + x}$$

(そして $O$ を $1 $に写像する)で与えられるが、これはどちらの方向にも効率的に計算可能である。
ちょっとした代数学で、逆の写像は

$$z \mapsto \left( x' , \frac{1+z}{1-z}x' \right)$$

ここで、$x′ = 4z/(1 - z)^2$ for $z \ne 1$, $1 \mapsto O$ です。

点の加算はモジュラー乗算より遅いし、点の両方の座標をメモリに保持するのは2倍の容量を必要とするので、この例はほとんど実用的でない。
しかし、有限体における暗号は楕円曲線暗号の特殊な例と見ることができ、厳密に言えば、楕円曲線暗号が議論を呼び、実験的で未検証だと言う人は、特異曲線の例外を認めることでその発言を修飾しなければならないのです!

もちろん、特異な曲線にのみ有効な特殊な離散対数アルゴリズムが発見される可能性は十分にあり、その場合は、暗号技術としては有限体しか残されていないことになります。
同様に、どんな楕円曲線にも適用できる亜指数関数的な離散対数アルゴリズムが発見された場合、同じフィールドサイズでは楕円曲線の方が効率が悪いので、やはり有限体しかないでしょう。

もし、特異曲線の効率的な双線形非正規対が見つかれば、決定版Diffie-Hellman問題を有限体において破ることができることは明らかである。


## 2.9 セキュリティ（Security）

<details>
<summary>原文</summary>
We have not yet shown any benefits to using elliptic curves instead of finite fields.
On the contrary, although a group inversion is almost free, the group operation, hashing, and random element generation are considerably more expensive for the same field.

Their strength derives from the fact that no discrete log algorithm for general elliptic curves that outperforms generic methods has ever been discovered.
The best generic methods are based on the birthday paradox, and have $O(\sqrt{n})$ expected running time, where n is the group order.
A group order around 160 bits in length is sufficient to defeat such attacks.
In contrast, subexponential discrete log algorithms for finite fields mean that one must use at least 1024-bit finite fields for security.

Hence one may work with fields over six times smaller in length if elliptic curves are used.
The increase in speed in the underlying field arithmetic easily compensates for the more complex group operations.
Thus choosing elliptic curves instead of finite fields results in savings in both space and time.

The most important attribute of elliptic curves for our purposes is that without them, we cannot construct a cryptographically useful bilinear map.
That elliptic curves happen to be more efficient than finite fields is a happy coincidence and makes pairings even more　attractive.
</details>

有限体でなく楕円曲線を使うことのメリットはまだ示されていない。
それどころか、群反転はほとんど無料であるが、群演算、ハッシュ化、ランダム要素生成は同じ場でもかなり高価である。

その強みは、一般楕円曲線に対する離散対数アルゴリズムで、一般的な手法を凌駕するものが発見されていないことに起因する。
最も優れた一般的な方法は、誕生日のパラドックスに基づくもので、nを群順とした期待実行時間が $O( \sqrt{n})$ となるものです。
このような攻撃には160ビット程度の長さのグループオーダーがあれば十分である。
一方、有限体に対する指数関数的な離散対数アルゴリズムでは、安全性のために少なくとも1024ビットの有限体を使用する必要があることを意味する。

楕円曲線を用いれば、その6倍以上の長さの場を扱うことができる。
このように、楕円曲線は有限体演算の代わりに楕円体演算を用いることで、空間と時間を節約することができます。
このように、有限体でなく楕円曲線を選ぶことで、空間と時間の両方を節約することができる。

楕円曲線の最も重要な特徴は、楕円曲線がなければ暗号学的に有用な双線形写像を構成できないことである。
楕円曲線が有限体よりも効率的であることは嬉しい偶然であり、ペアリングをより魅力的なものにしている。


## 2.10 短い署名（Short Signatures）

<details>
<summary>原文</summary>
At this point we can see why the BLS signature scheme introduced in the first chapter features a short signature length.
Recall in abstract terms, a signature is a single element of a cyclic group $G$ of prime order $r$ and we need a bilinear map to exist on $G$.

In Section 4.16 we give examples of pairings where one input group is a subgroup of points on an elliptic curve $E$ over some field $\mathbb{F}_q$ where $q$ is about 160 bits long.

A point has two coordinates, both taking about 160 bits to represent.
We can use point reduction, that is, discard the $y$-coordinate entirely, and represent signatures by their $x$-coordinate only if we modify verification as follows:

1.Given an $x$-coordinate of a signature, find any solution $y$ in the curve equation $E$.
2.If the signature $(x, y)$ does not verify, check if the signature $(x, −y)$ verifies.
3.If the signature still does not verify then reject it.

Thus signatures are elements of $\mathbb{F}_q$ and hence roughly 160 bits in length.

Verification of the signature involves computing $e(P, Q)$ for some points $P, Q$.
By the bilinearity of the pairing, if we have guessed the point $Q$ wrongly, we can obtain the value of $e(P, −Q) = e(P, Q)^{−1}$ by performing an inversion rather than recompute another pairing.

In Section 6.12 we will see we can do better than this by using a technique known as pairing compression, and in effect check both cases at once.
Instead of discarding the $y$-coordinate, we can replace it with a single bit signifying which solution of $y$ to take, a technique known as point compression [9, Section IV.4].
</details>

この時点で、第1章で紹介したBLS署名方式が短い署名長を特徴としている理由がわかる。
抽象的な言い方をすれば、署名は素数位数 $r$ の環状群 $G$ の一つの要素であり、$G$ 上に双線形写像が存在することが必要であると思い起こす。

4.16節では、一方の入力群が、$q$ が約160ビットの長さのあるある場 $\mathbb{F}_q$ 上の楕円曲線 $E$ 上の点の部分群である場合のペアリングの例を示した。

点には2つの座標があり、どちらも約160ビットで表現できる。
検証を以下のように修正すれば、点削減、つまり $y$-座標を完全に捨てて、$x$-座標だけで署名を表現することができる。

1.署名の $x$-座標が与えられたとき、曲線方程式 $E$ の任意の解 $y$ を求めよ。
2.署名 $(x, y)$ が検証されない場合、署名 $(x, -y)$ が検証されるかどうかを確認する。
3.それでも検証できない場合、その署名を拒否する。

したがって、署名は $\mathbb{F}_q$ の要素であり、その長さはおよそ160ビットである。

署名の検証には、ある点 $P, Q$ に対する $e(P, Q)$ の計算が必要である。
ペアリングの双線形性により、点$Q$を間違って推測した場合は、別のペアリングを再計算するのではなく、反転を行うことで $e(P, -Q) = e(P, Q)^{-1}$ という値を得ることができる。

6.12節では、ペアリングコンプレッションと呼ばれる手法で、事実上一度に両方のケースをチェックすることで、これよりうまくいくことを確認する。
$y$-座標を捨てる代わりに、$y$ のどの解を取るかを示す1ビットに置き換えることができる。これは点圧縮として知られる手法である[9, Section IV.4]。
