---
title: "ペアリングベース暗号システムの実装について 1"
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


# Chapter 1<br>双線型写像（Bilinear Maps）

<details>
<summary>原文</summary>
Asymmetric ciphers, or public-key cryptosystems, are perhaps the most celebrated contribution of modern cryptography.
They certainly have had the most impact.
It is hard to imagine what the world would be like without their revolutionary approach to key distribution.

All public-key cryptosystems in wide use today can trace their roots to the DiffieHellman key exchange protocol [27] or the RSA cryptosystem [54].
The former depends on cyclic groups with particular properties.
The latter, though using similar arithmetic operations, relies on different principles. For example, RSA uses groups that are not cyclic
and requires that the order of the group to be unknown to the attacker.

Roughly speaking, bilinear maps, or pairings, give cyclic groups additional properties.
Initially, in the 1990s, these additional properties were seen as detrimental as they could be exploited to break cryptosystems [46, 32, 31], but it was later discovered that they could also be exploited to build cryptosystems.
Rather than avoiding pairings, one can seek them out to construct new schemes.

Boneh and Franklin’s identity-based encryption scheme [13] is perhaps the most famous early example of what could be achieved using bilinear maps, though not the first [57, 40].
Shamir first discussed identity-based encryption in 1984 [61], but researchers were unable to build a practical scheme by conventional means for approximately twenty years.
Boneh and Franklin found an elegant solution using bilinear maps [13].
Extending the basic idea leads to identity-based schemes with additional useful properties such as authenticated or hierarchical identity-based encryption [45, 39].
More generally, so many cryptographic applications of the pairing have been identified that this area of research is sometimes considered its own field called pairing-based cryptography [3].

We note that an identity-based scheme based on quadratic residues and not bilinear maps has since been proposed [20, 14], albeit one that is signifcantly less practical.
However, this is the exception rather than the rule. In general it is not known how to find conventional equivalents of a given pairing-based cryptosystem.
</details>

非対称暗号、すなわち公開鍵暗号は、おそらく現代暗号の中で最も有名な貢献である。
確かに最も大きな影響を与えた。
公開鍵暗号方式がなければ、世界はどうなっていたか想像もつかない。

現在広く使われているすべての公開鍵暗号は、DiffieHellman鍵交換プロトコル[27]やRSA暗号[54]にそのルーツを求めることができます。
前者は、特殊な性質を持つ巡回群に依存する。
後者は、似たような演算を用いるものの、異なる原理に基づくものである。例えば、RSA暗号は環状でない群を使い
また、攻撃者がグループの順序を知らないことが必要である。

大雑把に言えば、双線形写像（ペアリング）により、巡回群に新たな性質が付与される。
当初、1990年代には、これらの特性は暗号システムを破るために悪用されるとして有害視されていましたが[46, 32, 31]、後に暗号システムを構築するためにも悪用できることが判明しています。
ペアリングを避けるのではなく、ペアリングを探し出して新しい方式を構築することができる。

BonehとFranklinのIDベースの暗号化方式[13]は、おそらく双線形写像を使って何が実現できるかを示す最も有名な初期の例であるが、これが最初ではない[57, 40]。
Shamirは1984年に初めてIDベースの暗号化について議論したが[61]、研究者は約20年間従来の手段で実用的な方式を構築することができなかった。
BonehとFranklinは双線形写像を用いたエレガントな解決策を発見した[13]。
この基本的な考え方を拡張すると、認証された、あるいは階層的なIDベースの暗号化[45, 39]など、さらに有用な特性を持つIDベースのスキームが生まれる。
より一般的には、ペアリングの非常に多くの暗号的応用が確認されており、この研究領域はペアリングに基づく暗号[3]という独自の分野とみなされることもある。

なお、双線形写像ではなく平方剰余に基づくIDベース暗号も提案されている[20, 14]が、実用性は著しく低いものの、これは例外である。
しかし、これは一般的ではなく、例外的である。一般に、ペアリングに基づく暗号システムの従来の同等品を見つける方法は知られていない。

## 1.1 巡回群（Cyclic Groups）

<details>
<summary>原文</summary>
Let $G = \langle g \rangle$ of prime order $r$.
Let $g$ be a generator of $G$ and let $x$, $y$, $z$ be integers in $[0, r − 1]$ .
Consider the following problems.

**Discrete Log Problem.**
Given $g$, $g^x$, compute $x$.

**Computational Diffie-Hellman Problem.**
Given $g$, $g^x$, $g^y$, compute $g^{xy}$.

**Decisional Diffie-Hellman Problem.**
Given $g$, $g^x$, $g^y$, $g^z$, determine if $xy = z$.

A vast array of cryptosystems including the Diffie-Hellman key exchange protocol can be built assuming one of these problems, or one of many other related problems, is difficult to solve.
The discrete log problem is the most important, for if it could be solved, all other related problems could also be solved.
</details>

素数位数$r$ の群 $G = \langle g \rangle$ とする。
$g$ は $G$の生成元であり、$x$, $y$, $z$ は $[0, r − 1]$ の整数とする。
次のような問題を考える。

**離散対数問題（Discrete Log Problem）**
$g$, $g^x$　が与えられたとき、$x$ を求める。

**CDH問題（Computational Diffie-Hellman Problem）**
$g$, $g^x$, $g^y$　が与えられた時、$g^{xy}$ を求める。

**DDH問題（Decisional Diffie-Hellman Problem）**
Given $g$, $g^x$, $g^y$, $g^z$, determine if $xy = z$.

## 1.2 形式的なセキュリティの定義（Formal Security Definitions）

<details>
<summary>原文</summary>
For completeness we include formal definitions of difficult problems commonly used by cryptographers, even though we do not need them as we do not provide any security proofs.
Our focus is on the implementation of pairing-based cryptosystems, not their correctness.

There are two flavours of definitions. One is asymptotic and the other uses fixed security parameters.

We first describe the asymptotic definition. Roughly speaking, a problem is hard if there is no efficient algorithm that can solve it.
We say a function $f:\mathbb{Z}_{\geq 0} \rightarrow \mathbb{R}_{\geq 0}$ is negligible if for all $c > 0$ there exists a $\lambda_0 > 0$ such that for all $\lambda \geq \lambda_0$, $f(\lambda) < 1/\lambda_c$.
Intuitively a function is negligible if it is smaller than any polynomial function for sufficiently large inputs.
Examples of negligible functions are $2^{−\lambda}$ and $\lambda^{− \log \lambda}$, while $λ^{−10000000}$ is nonnegligible.
</details>

また、暗号技術者がよく使う難しい問題については、安全性の証明をしないので必要ないが、完全性を期すために正式な定義をしている。
我々の焦点はペアリング暗号の実装であり、その正しさではない。

定義には2種類ある。一つは漸近的な定義で、もう一つは固定的なセキュリティパラメータを用いた定義である。

まず、漸近的な定義について説明する。大雑把に言うと、ある問題を解くことのできる効率的なアルゴリズムが存在しない場合、その問題は難しいということになります。
関数 $f:\mathbb{Z}_{\geq 0} \rightarrow \mathbb{R}_{\geq 0}$ が無視できるのは、すべての $c > 0$ に対して、$\lambda \geq \lambda_0$ が存在したとき、すべての $\lambda \geq \lambda_0$ に対して、 $f(\lambda) < 1/\lambda_c$となる。
直観的に、十分に大きな入力に対して任意の多項式関数より小さい場合、その関数は無視できる。
例として、無視できる関数は$2^{−\lambda}$と$\lambda^{− \log \lambda}$で、$λ^{−10000000}$は無視できません。

<details>
<summary>原文</summary>
Let $\mathcal{G} = \{G_1, G_2, \ldots\}$ be a family of cyclic groups where the order of $G_i$ is an $i$-bit number.
For an algorithm $A$ define the _advantage_ of $A$ to be

$$ Adv_A(t)=\mathrm{Pr}[A(g,g^x)=x] $$

is negligible, where $g$ is randomly chosen (uniformly) from $G_t$ and $x$ from $[0,|G_t|−1]$.
Then we say that the discrete log problem is hard in $\mathcal{G}$ if $Adv_A$ is negligible for all probabilistic expected polynomial-time algorithms $A$.
</details>

$i$ ビットである $G_i$ を位数とする、巡回群の族を $\mathcal{G} = \{G_1, G_2, \ldots\}$ とする。
あるアルゴリズム$A$に対して、$A$の _advantage_ を次のように定義する。

$$ Adv_A(t)=\mathrm{Pr}[A(g,g^x)=x] $$

は無視でき、$g$ は $G_t$ から(一様に)ランダムに選ばれ、$x$ は $[0,|G_t|-1]$ から選ばれる。
そして、すべての確率的期待多項式時間アルゴリズム $A$ に対して $Adv_A$ が無視できるとき、離散対数問題は $\mathcal{G}$ において困難であるという。

<details>
<summary>原文</summary>
Decisional variants of problems must be handled differently.
For example, we say the decisional Diffie-Hellman problem is hard if for all probabilistic expected polynomial-time algorithms $A$

$$Adv_A(t) = |\mathrm{Pr}[A(g, g^x, g^y, g^z) = B(x, y, z)] − 1/2|$$

is negligible, where $g$ is randomly chosen from $G_t$ , $x$, $y$ from $[0, r − 1]$, and $z$ from $[0, r − 1]$ with probability $1/2$ and set to $xy$ otherwise, and where $B$ returns $1$ if $xy = z$ and 0 otherwise.
</details>

決定論的な問題の変形は異なる方法で扱われなければならない。
例えば、すべての確率的期待多項式時間アルゴリズム $A$ に対して、決定版 Diffie-Hellman 問題が困難であると言う。

$$Adv_A(t) = |\mathrm{Pr}[A(g, g^x, g^y, g^z) = B(x, y, z)] − 1/2|$$

は無視でき、$g$ は $G_t$ から、$x$, $y$ は $[0, r - 1]$ から確率 $1/2$ でランダムに選ばれ、それ以外は$xy$とし、$B$ は $xy = z$ なら $1$、それ以外は $0$ を返すものとします。

<details>
<summary>原文</summary>
The other kind of definition on the other hand is couched explicitly in terms of running times and probabilities.
It is useful when examining groups of a particular size, and similarly in cases where there are no families of infinite size.
A classic example is the DES cipher, where an asymptotic definition cannot be used since DES keys are always 56 bits long.
When using fixed security parameters, we say the discrete log problem is $(t, \varepsilon)$-hard in a given group $G$ if for all probabilistic $t$-time algorithms $A$ we have

$$\mathrm{Pr}[A(g, g^x) = x] < \varepsilon$$

where $g$ is uniformly chosen from $G$ and $x$ from $[0, |G| − 1]$.
</details>

一方、もう一つの定義は、実行時間や確率という言葉で明示的に表現されるものである。
これは、ある特定の大きさのグループを調べるときに有効であり、同様に無限の大きさの族が存在しない場合にも有効である。
典型的な例はDES暗号で、DESの鍵は常に56ビット長であるため漸近的定義は使えない。
固定セキュリティパラメータを用いる場合、全ての確率的 $t$-時間アルゴリズム$A$に対して、以下を満たすとき、離散対数問題は与えられた群$G$において $(t, \varepsilon)$-困難であるという。

$$\mathrm{Pr}[A(g, g^x) = x] < \varepsilon$$

ここで、$g$は$G$から、$x$は$[0, |G| - 1]$から一様に選ばれる。

<details>
<summary>原文</summary>
The security definitions for other problems in either flavour are similarly constructed and are omitted here.

Of course, there is always another requirement for cyclic groups used in cryptography.
Efficient algorithms for multiplying group elements, inverting group elements, and hashing to a group element must exist.
Exponentiating and choosing a random element should be efficient as well, but this is implied by fast multiplication: exponentiation by repeated squaring is efficient, and for random element generation one can simply choose a random integer and raise a generator by this power.

In practice, “efficient” means fast enough so that on the platform that the system is deployed the user will not notice any slowdown.
With pairing-based cryptography, the bilinear map must also be efficient.
</details>

他の問題の安全性の定義も同様であり、ここでは省略する。

もちろん、暗号に用いられる巡回群には、常に別の要件が存在する。
群要素の乗算、群要素の反転、群要素へのハッシュ化などの効率的なアルゴリズムが存在しなければならない。
指数化やランダム要素の選択も効率的であるべきですが、これは高速な乗算が意味するところです。二乗を繰り返す指数化は効率的ですし、ランダム要素の生成は単に乱整数を選び、この乗で生成器を上げればよいのです。

実際のところ、「効率的」とは、システムが導入されたプラットフォームで、ユーザーが速度低下を感じない程度の速さを意味する。
ペアリング暗号の場合、双線型写像も効率的である必要がある。

## 1.3 具体的な巡回群（Concrete Cyclic Groups）

<details>
<summary>原文</summary>
There are two mathematical settings to choose from when implementing cyclic groups: finite fields and elliptic curves.
We give a short summary here but will expound on elliptic curves later.

For finite fields, one picks a prime $n$ and uses a subgroup $G$ of $\mathbb{Z}^∗_n$ of prime order $r$, so the group operation is field multiplication.
Note an RSA cryptosystem arises when $n$ is instead chosen to be the product of two large primes $P$, $Q$, in which case computations are still possible in $\mathbb{Z}^∗_n$ even if its order is unknown.

For elliptic curves, one takes an elliptic curve $E$ over some finite field $K$ and takes some subgroup $G$ of the group of points $E(K)$ with prime order $r$, so the group operation is point addition.
There is no elliptic curve version of RSA, but we shall encounter a case which may seem confusingly similar to RSA: we sometimes choose a curve whose group of points has order $n = PQ$.

Finite fields were proposed first, and much has been published on implementing finite fields and their potential weaknesses.
Of particular interest are subexponential discrete log algorithms such as index calculus, resulting in recommendations that field sizes be at least 1024 bits.

Elliptic curves were investigated later, and their use in cryptography was at first viewed as experimental.
However, they are now much less controversial thanks to the efforts of many researchers.
A myriad of optimizations and algorithms were discovered for elliptic curve cryptography, and encouragingly, a specialized elliptic curve discrete log algorithm has yet to be found.
In other words, the fastest known way to break discrete log for a general elliptic curve is to use a algorithm for a generic cyclic group, such as the Pollard rho and lambda methods, implying that the security of a 160-bit elliptic curve group and a 1024-bit finite field are roughly equivalent.

Thus currently elliptic curves present many advantages over finite fields.
Despite more complex algorithms for basic operations, they are much faster and more compact due to a smaller group size.

In Section 2.8 we find that finite fields can be viewed as a special case of elliptic curves due to a correspondence between certain singular curves and multiplicative groups of finite fields.
</details>

巡回群の実装には、有限体と楕円曲線の2つの数学的設定がある。
ここでは、楕円曲線について簡単に説明する。

有限体では、素数 $n$ を選び、素位数 $r$ の$\mathbb{Z}^∗_n$ の部分群 $G$ を用いるので、群演算は体の乗算である。
なお、RSA暗号は $n$ を2つの大きな素数 $P$, $Q$ の積にすると、位数が分からなくても $\mathbb{Z}^∗_n$ で計算が可能になる。

楕円曲線では、ある有限体$K$上の楕円曲線$E$を選び、その点群$E(K)$の素数位数$r$の部分群$G$を取るので、群演算は点加算となる。
RSAの楕円曲線版は存在しないが、RSAと紛らわしいケースに遭遇することがある：点群の位数が$n=PQ$である曲線を選ぶことがある。

有限体が最初に提案され、有限体の実装とその潜在的な弱点について多くの論文が発表されている。
特に、指数計算のような指数以下の離散対数アルゴリズムは、フィールドサイズを少なくとも1024ビットにすることを推奨する結果になった。

楕円曲線は後から研究されたもので、暗号技術への利用は当初は実験的なものと見られていた。
しかし、多くの研究者の努力により、現在ではかなり議論の余地は少なくなっている。
楕円曲線暗号の最適化とアルゴリズムは無数に発見されたが、心強いことに、楕円曲線に特化した離散対数アルゴリズムはまだ見つかっていない。
つまり、一般的な楕円曲線に対する離散対数を破る最速の方法は、Pollard rho法やlambda法などの一般的な巡回群に対するアルゴリズムを用いることであり、160ビット楕円曲線群と1024ビット有限群の安全性はほぼ同等であることを意味している。

このように、現在のところ楕円曲線は有限体に対して多くの利点を有している。
基本的な操作のアルゴリズムはより複雑ですが、群サイズが小さいため、より速く、よりコンパクトになります。

セクション2.8では、ある特異曲線と有限体乗法群の対応から、有限体が楕円曲線の特殊なケースと見なすことができることを発見した。

## 1.4 対象ペアリング（The Symmetric Pairing）

<details>
<summary>原文</summary>
We begin with the original and simplest abstract definition of the pairing.
Let $G$, $G_T$ be cyclic groups of prime order $r$.
Let $g$ be a generator of $G$.
A _bilinear pairing_ or _bilinear map_ $e$ is an efficiently computable function

$$e : G \times G \rightarrow G_T$$

such that

1.(Nondegeneracy) $e(g, g) \ne 1$
2.(Bilinearity) $e(g^a, g^b) = e(g, g)^{ab}$ for all $a, b \in \mathbb{Z}$
</details>

まず、ペアリングの原型となる最も簡単な抽象的定義から始める。
$G$, $G_T$ を素位数 $r$ の巡回群であるとする。
$g$ を $G$ の生成元とする。
_双線形ペアリング_ または _双線形写像_ $e$ は効率的に計算可能な関数である

$$e : G \times G \rightarrow G_T$$

このような

1.（_非幾何性_） $e(g, g) \ne 1$ である。
2.（_双線形性_） すべての $a, b \in \mathbb{Z}$ に対して、$e(g^a, g^b) = e(g, g)^{ab}$

<details>
<summary>原文</summary>

A symmetric bilinear map is completely defined by the value it takes at $e(g, g)$.
Essentially there is always exactly one bilinear map for any given cyclic group: we have the degenerate case when $e(g, g) = 1$ while the other $r−1$ maps are equivalent up to a constant, in the sense that if $e_1$, $e_2$ are nondegenerate bilinear maps then for some constant $c$ we have then $e_1(g, h) = e_2(g, h)^c$ for all $g, h \in G$.
The difficulty lies in finding such a map that is efficiently computable.

We can immediately demonstrate power of such a map.
Given $g$, $g^x$, $g^y$, $g^z$, by bilinearityand nondegeneracy, $z = xy$ if and only if $e(g, g^z) = e(g^x, g^y)$.
In other words we can solve the decisional Diffie-Hellman problem.
Thus cryptosystems relying on the intractability of this problem cannot be constructed on a cyclic group with a symmetric pairing, though later we present a more general pairing definition where DDH can still be difficult despite the presence of a bilinear map.

Note it is not known how to solve the computational Diffie-Hellman or discrete log problem using a pairing.
</details>

対称双線形写像は、$e(g, g)$でとる値によって完全に定義されます。
つまり、$e(g, g) = 1$のときは退化ており、$e_1$, $e_2$が退化していない双線形写像であれば、ある定数$c$に対して、$e_1(g, h) = e_2(g, h)^c$ がすべての $g, h \in G$ に対して成立するのです。
このような効率的に計算できる写像を見つけることが難しい。

このような写像の威力はすぐに証明できる。
$g$, $g^x$, $g^y$, $g^z$ が与えられたとき、双線形性と非幾何性により、 $e(g, g^z) = e(g^x, g^y)$ のときだけ $z = xy$ となります。
つまり、DDH-問題（decisional Diffie-Hellman problem）を解くことができる。
このため、この問題の難解さを利用した暗号システムは、対称的なペアリングを持つ双曲線写像では構築できない。ただし、後に、双線形写像が存在してもDDHが困難である、より一般的なペアリングの定義を示す。

なお、ペアリングを用いたCDH問題や離散対数問題の解法は不明である。

## 1.5 BLS署名スキーマ（The BLS Signature Scheme）

<details>
<summary>原文</summary>

With the abstract definition and some assumptions, we can readily construct cryptosystems.
We have just noted that if cyclic group with such a bilinear map exists, then we have a group where the computational Diffie-Hellman problem is thought to be hard, yet the decisional variant is easy to solve.
Such groups are sometimes called Gap Diffie-Hellman groups [51], and imply a signature scheme, often referred to as the BLS Signature Scheme [17]:

**Setup.** Choose a Gap Diffie-Hellman group $G$ of prime order $r$. Publish a generator $g \in G$.

**Key Generation.** Choose a random $x$ in $\{1, \ldots, r − 1\}$. Output the public key $g^x$ and the private key $x$.

**Signing.** Given a message $h \in G$, output $h^x$.

**Verify.** Given a message-signature pair $h$, $\sigma$ and public key $g^x$, check that $\langle g, h, g^x, \sigma \rangle$ is a Diffie-Hellman tuple.

It can be shown that under the computational Diffie-Hellman assumption, this signature scheme is secure against existential forgery under a chosen-message attack in the random oracle model [17].
Hence an abstract view of the pairing is all that is needed to build a cryptosystem, and frequently this suffices. However, we shall see that it can be useful to understand pairings in more detail.
For example, we need to know more to show the above signature scheme is short in length, which in fact was the original motivation for its proposal.
This signature scheme in fact has other useful properties, including batched verification and allowing the the simple construction of aggregate, ring and verifiably-encrypted signatures[15].

</details>

このような抽象的な定義といくつかの仮定があれば、容易に暗号システムを構築することができる。
このような双線形写像を持つ循環群が存在する場合、計算上のDiffie-Hellman問題は困難であると考えられているが、決定論的変形は容易に解ける群が存在することを指摘したところである。
このような群はギャップDiffie-Hellman群[51]と呼ばれることがあり、BLS署名方式[17]と呼ばれる署名方式を意味する。

**Setup.** 素位数 $r$ のギャップDiffie-Hellman群 $G$ を選ぶ。$g \in G$ を公開する。

**Key Generation.** 公開鍵 $g^x$ と秘密鍵 $x$ を出力する。

**Signing.** $h \in G$ のメッセージ $h$ が与えられ、$h^x$ を出力する。

**Verify.** メッセージと署名のペア $h$ と $\sigma$ と公開鍵 $g^x$ が与えられたとき、$\langle g, h, g^x, \sigma \rangle$ が Diffie-Hellman tuple であることを確認する。


CDH（computational Diffie-Hellman）の仮定の下で、この署名方式はランダムオラクルモデルにおける選択メッセージ攻撃に対して存在的な偽造に対して安全であることを示すことができる[17]。
したがって、暗号システムを構築するために必要なのは、ペアリングの抽象的なビューだけであり、しばしばこれで十分である。しかし、ペアリングをより詳細に理解することが有用であることがわかるだろう。
例えば、上記の署名方式が長さが短いことを示すには、より多くの情報が必要であり、実際、これが提案の当初の動機であった。
この署名方式は、実は他にもバッチ検証や、集約署名、リング署名、検証可能暗号化署名の簡単な構成が可能であるなど、有用な性質を持っている[15]。

## 1.6 新しい困難仮定（New Hardness Assumptions）

<details>
<summary>原文</summary>

Classic problems have natural counterparts in pairing-based cryptography.
In the examples below, let $g$ be a generator for a group $G$ of prime order $r$, and let $e$ be a bilinear map on $G$.

**Bilinear Diffie-Hellman Problem [13].** Given $g$, $g^x$, $g^y$, $g^z$ compute $e(g, g)^{xyz}$. This assumption was implicitly used before it was formally defined [57, 40].

**Decisional Bilinear Diffie-Hellman Problem [10].** Given $g$, $g^x$, $g^y$, $g^z$, $e(g, g)^w$, determine if w = xyz. This has also been referred to as BDDH [13].

q **-Strong Diffie-Hellman Problem [11].** Given $g, g^x, \ldots, g^{(x^q)}$, compute $c$, $g^{1/(x_c)}$ for any $c \in \{1, \ldots, r − 1\}$.

Just as an algorithm that solves discrete log can be used to solve the computational and decisional Diffie-Hellman problems, security reductions exist between some of these new problems [41].

The BLS signature scheme does not use any of these new assumptions, and merely employs the pairing to solve the decisional Diffie-Hellman problem.
In contrast, other pairing-based cryptosystems such as the identity-based encryption scheme of Boneh and Franklin [13] do rely on the hardness of one or more problems involving pairings.

Sometimes a conventional cryptosystem suggests a pairing-based counterpart. For example, the above pairing-based signature scheme and a ring signature scheme due to Rivest et al. [55] were presented in adjacent sessions at a conference.
Someone taking the ideas from both talks would not have much difficulty in synthesizing their results.
Indeed, a pairing-based ring signature scheme was presented about one year later[15].

As before, both asymptotic and fixed-security-parameter formal security definitions can be constructed for each of the above problems.

</details>

古典的な問題には、ペアリングに基づく暗号に対応する自然な問題がある。
以下の例では、$g$ を素数位数 $r$ の群 $G$ の生成元とし、$e$ を $G$ 上の双線形写像とする。

**DDH問題（Bilinear Diffie-Hellman Problem [13].）** 与えられた $g$, $g^x$, $g^y$, $g^z$ から $e(g, g)^{xyz}$ を計算する。この仮定は、正式に定義される前に暗黙のうちに用いられていた[57, 40]。

**BDDH問題（Decisional Bilinear Diffie-Hellman Problem [10].）** $g$, $g^x$, $g^y$, $g^z$, $e(g, g)^w$ が与えられたとき、w = xyz かどうかを決定せよ。これはBDDHとも呼ばれる[13]。

q **-SDH問題（Strong Diffie-Hellman Problem [11].）** 与えられた $g, g^x, \ldots, g^{(x^q)}$ に対して、$c$, $g^{1/(x_c)}$ を任意の $c \in {1, \ldots, r - 1}$ について計算しなさい。

離散対数を解くアルゴリズムが計算上および決定上のDiffie-Hellman問題を解くために使用できるように、これらの新しい問題の間に安全性の削減が存在する[41]。

BLS署名方式は、これらの新しい仮定を一切使用せず、単に決定論的Diffie-Hellman問題を解くためにペアリングを使用しています。
一方、Boneh and FranklinのIDベース暗号方式[13]などの他のペアリングに基づく暗号方式は、ペアリングを含む1つ以上の問題の困難性に依存している。

従来の暗号方式が、ペアリングに基づく対応策を示唆することもある。例えば、上記のペアリング署名方式とRivestら[55]によるリング署名方式は、ある学会で隣り合わせのセッションで発表された。
両方の講演からアイデアを得た人が、その結果を合成するのはそれほど難しくないだろう。
実際、約1年後にペアリングに基づくリング署名方式が発表された[15]。

前述と同様に、上記の各問題に対して、漸近的な安全性パラメータと固定的な安全性パラメータの両方の形式的安全性定義が構築可能である。

1.7 ゆるいペアリング定義（Loosening the Pairing Definition）

<details>
<summary>原文</summary>
In practice, symmetric pairings can be instantiated by using suitable supersingular elliptic curves.
However, in order to allow a wider range of curves to be used, the definition must be modified.
One early pairing-based cryptography publication by Boneh, Lynn and Shacham suggested the following [17]:
Let $G_1, G_2, G_T$ be cyclic groups of prime order $r$.
Assume the Diffie-Hellman problem is hard in $G_1$.
Let $\phi : G_2 \rightarrow G_1$ be an efficiently computable group isomorphism.
Let $g_2$ be a generator of $G_2$.
Set $g_1 = \phi(g_2)$ (so $g_1$ generates $G_1$).
A bilinear pairing $e$ is an efficiently computable function

$$e : G_1 \times G_2 \rightarrow G_T$$

such that $e(g_1, g_2) \ne 1$ and $e(g^a_1, g^b_2) = e(g_1, g_2)^{ab}$ for all $a, b \in \mathbb{Z}$.

This version of the bilinear map is sometimes called the asymmetric pairing.

This definition allows a greater variety of pairings to be used, notably those constructed on ordinary curves.
Additionally, because of the map $\phi$ security proofs require only minimal changes.
For example, a scheme based on the CDH assumption in $G$ under the symmetric pairing definition may now be based on the CDH assumption in $G_2$.
However, it is advantageous to further loosen the definition.
</details>

実際には、適当な超特異楕円曲線を用いれば、対称的な対を実現することができる。
しかし、より広い範囲の曲線を使用できるようにするためには、定義を修正する必要がある。
Boneh、Lynn、Shachamが発表した初期のペアリングに基づく暗号の1つに、以下のような提案がある[17]。
$G_1, G_2, G_T$ を素数位数 $r$ の巡回群であるとする。
$G_1$ において Diffie-Hellman 問題が困難であると仮定する。
また、$phi : G_2 \rightarrow G_1$ を効率的に計算可能な群同型とする。
また、$g_2$ を$G_2$の生成子とする。
また、$g_1 = \phi(g_2)$とする（したがって、$g_1$ は $G_1$ を生成する）。
双線形ペアリング $e$ は効率的に計算できる関数であり

$$e : G_1 \times G_2 \rightarrow G_T$$

このとき、$e(g_1, g_2) \ne 1$ かつ $e(g^a_1, g^b_2) = e(g_1, g_2)^{ab}$ がすべての $a, b \in \mathbb{Z}$ に対して成り立つようなものでなければならない。

この双線形写像を非対称ペアリングと呼ぶことがある。

この定義により、通常の曲線で構成されるペアリングを筆頭に、より多様なペアリングが使用できるようになります。
また、写像 $\phi$ を用いるため、安全性の証明は最小限の変更で済む。
例えば、対称ペアリングの定義で $G$ のCDH仮定に基づく方式は、$G_2$ のCDH仮定に基づく方式になる可能性がある。
しかし、さらに定義を緩めることが有利である。

1.7.1 ハッシュの問題（A Problem With Hashing）

<details>
<summary>原文</summary>
Suppose we have chosen an ordinary curve that does not yield a symmetric pairing, and suppose that we wish to redesign the cryptosystem to instead use an asymmetric pairing defined above.

It turns out that there is no known method to hash to an element of $G_2$ such that its discrete log to some fixed base is unknown.
We can hash to elements of $G_1$ and $G_T$ , and perform all other cryptographically useful operations in G2 such as picking a random element, multiplication and inversion.
We can map an element of $G_2$ to $G_1$.
We just cannot hash a string to an element of $G_2$.

For some cryptosystems, this issue can complicate their design, and in extreme cases we may have to give up and stick to symmetric pairings only.
However, with a more general pairing definition, we can use ordinary curves and still be able to hash to $G_2$.
</details>

対称対を生成しない通常の曲線を選択し、その代わりに上で定義した非対称対を使用するように暗号システムを再設計したいとする。

このとき、$G_2$ の要素にハッシュをかけ、その離散対数がある固定塩基に対して未知であるような方法は知られていないことが判明した。
$G_1$ と $G_T$ の要素にハッシュし、G2においてランダム要素の選択、乗算、反転など暗号学的に有用な全ての演算を実行することが可能である。
$G_2$ の要素を $G_1$ にマップすることができる。
$G_2$ の要素を $G_1$ にマッピングすることはできるが、文字列を $G_2$ の要素にハッシュすることはできない。

暗号システムによっては、この問題が設計を複雑にし、極端な場合には諦めて対称的なペアリングのみにこだわらなければならないこともあります。
しかし、より一般的なペアリングの定義があれば、普通の曲線を使っても $G_2$ にハッシュすることができる。

1.8 一般的な双線形ペアリング（The General Bilinear Pairing）

<details>
<summary>原文</summary>
To enable certain optimizations (see Section 6.5) and to permit hashing to any element of any group including $G_2$, we prefer the following definition.
Additionally, its more flexible nature allows a greater number of assumptions which in turn allow more cryptosystems to be built.

Let $r$ be a prime.
Let $G_1, G_T$ be cyclic groups of order $r$.
Let $G_2$ be a group where each element has order dividing $r$.
In particular $G_2$ is not necessarily cyclic.
Again we use multiplicative group notation.
A bilinear pairing $e$ is an efficiently computable function

$$e : G_1 \times G_2 \rightarrow G_T$$

such that

1.(_Nondegeneracy_) $e(g_1, g_2) = 1_{G_T}$ for all $g_2 \in G_2$ if and only if $g_1 = 1_{G_1}$ , and similarly $e(g_1, g_2) = 1_{G_T}$ for all $g_1 \in G_1$ if and only if $g_2 = 1_{G_2}$.

2.(_Bilinearity_) for all $g_1 \in G_1$ and $g_2 \in G_2$ : $e(g^a_1, g^b_2) = e(g_1, g_2)^{ab}$ for all $a, b \in \mathbb{Z}$

In this setting the hardness assumptions must be altered further.
Depending on the scheme, we may have to assume certain problems are hard in both $G_1$ and $G_2$, or a combination of the two.
For example, we may need to assume that given $g_1, g^x_1 \in G_1$ and $g_2 \in G_2$, there is no efficient algorithm to compute $g^x_2$.
This last example is sometimes referred to as the computational co-Diffie-Hellman assumption [17].

This definition also allows assumptions that were previously impossible.
For example, some schemes such as the group signature scheme of Boneh, Boyen and Shacham [12] require discrete log, CDH and co-CDH to be hard in $G_1$, $G_2$ and also DDH to be hard in $G_1$.
Other cryptosystems still require in addition DDH to be hard in $G_2$, require $G_2$ to be cyclic, or require an efficiently computable group isomorphism $\phi : G_2 \rightarrow G_1$, all of which are variations on this formal definition, and in Section 6.4.2 we learn how to construct pairings for each case.

We have not required $r$ to be prime.
A composite group order is useful for some cryptosystems[16], but we must be aware that in this case even if $g_1$ and $g_2$ have order $r$, $e(g_1, g_2)$ may not be a generator of $G_T$ , but rather a generator of some subgroup of $G_T$ whose order is a factor of $r$.

When $r$ is composite, it is incorrect to say all nondegenerate pairings are equivalent up to a constant.
Indeed, if $d>1$ is a divisor of $r$ and $g_1$, $g_2$ are generators of $G_1$, $G_2$ respectively, then a bilinear map that takes $(g_1, g_2)$ to a $d$-th root of unity is still nondegenerate.
Since there is more than one choice for $d$ for a composite $r$, these maps cannot be equivalent up to a constant.
They are however still completely determined by $e(g_1, g_2)$ where $g_1$, $g_2$ are generators.
We present a concrete example later in Section 4.2.1.

This also means that $e(g_1, g_2)=1$ does not imply at least one of $g_1$, $g_2$ is the identity element, in contrast to the case when $r$ is prime.

Thus some care is needed when dealing with composite $r$.
Facts about the pairing that are true for prime $r$ do not always carry over.
We shall encounter a concrete example of this phenomenon later.
Note when $r$ is chosen to be a product of two primes, the resulting scheme is still substantially different to RSA as the groups are cyclic and of known order.
</details>

ある種の最適化(セクション6.5参照)を可能にし、$G_2$を含むあらゆる群の あらゆる要素へのハッシュを可能にするため、以下の定義が望ましいと考える。
さらに、この定義がより柔軟であるため、より多くの仮定が可能となり、その結果、より多くの暗号システムを構築することができる。

$r$ を素数とする。
$G_1, G_T$ を位数 $r$ の巡回群であるとする。
$G_2$ を、各要素が $r$ を分割する位数を持つ群とする。
特に $G_2$ は環状である必要はない。
ここでも乗法群の表記を用いる。
双線形ペアリング $e$ は効率的に計算できる関数である。

$$e : G_1 \times G_2 \rightarrow G_T$$

というように

1.(_Nondegeneracy_) $e(g_1, g_2) = 1_{G_T}$ は $g_1 = 1_{G_1}$ のときのみ全ての $g_2 \in G_2$ に対して成り立つ。同様に $e(g_1, g_2) = 1_{G_T}$ は $g_2 = 1_{G_2}$ のときだけ全ての $g_1 \in G_1$ に対して成り立つ。

2.(_Bilinearity_) 全ての $g_1 \in G_1$ と $g_2 \in G_2$ : $e(g^a_1, g^b_2) = e(g_1, g_2)^{ab}$ 全ての $a, b \in \mathbb{Z}$.

この設定では、困難仮定をさらに変更する必要がある。
スキームによっては、ある問題が$G_1$と$G_2$の両方、またはその組み合わせで難しいという仮定が必要になることがあります。
例えば、$g_1, g^x_1 \in G_1$と$g_2 \in G_2$が与えられたとき、$g^x_2$を計算する効率的なアルゴリズムが存在しないと仮定しなければならないかもしれない。
この最後の例は、co-CDH（computational co-Diffie-Hellman）仮定と呼ばれることがある[17]。

また、この定義により、従来不可能であった仮定が可能になる。
例えば、Boneh, Boyen and Shacham [12]のグループ署名方式などでは、離散対数、CDH、co-CDHが $G_1$, $G_2$ で難しいこと、またDDHが $G_1$ で難しいことが要求されている。
他の暗号方式では、さらにDDHが $G_2$ で難しいこと、 $G_2$ が環状であること、効率的に計算できる群同型性 $\phi : G_2 \rightarrow G_1$ を要求しているが、これらはすべてこの正式な定義の変形であり、6.4.2節でそれぞれの場合のペアリングの構築方法を説明する。

$r$ が素数であることは要求していない。
暗号システムによっては複合群位が有用であるが[16]、この場合、$g_1$ と $g_2$ の位数が$r$であっても、$e(g_1, g_2)$ は $G_T$ の生成元ではなく、$G_T$ の部分群の生成元で位数が $r$ の因子である場合があるので注意しなければならない。

$r$ が合成数であるとき、すべての非正規対が定数まで等価であるというのは誤りである。
実際、$d>1$ が $r$ の約数で、$g_1$, $g_2$ がそれぞれ $G_1$, $G_2$ の生成元ならば、$(g_1, g_2)$ を $d$番目の統一根にする双線形写像はまだ非正規である。
合成数 $r$ に対して $d$ は複数選択できるので、これらの写像は定数まで等価になることはない。
しかし、$e(g_1, g_2)$ ($g_1$, $g_2$ は生成元)によって完全に決定される。
具体的な例は4.2.1節で後述する。

これは、$e(g_1, g_2)=1$ が $g_1$, $g_2$ の少なくとも一方が恒等式要素であることを意味しないことも意味しており、$r$ が素数のときとは対照的である。

このように、合成された $r$ を扱うときには、多少の注意が必要である。
素数 $r$ のときに成り立つ対の事実が、そのまま引き継がれるとは限らない。
この現象については、後で具体的な例を挙げる。
なお、$r$ を2つの素数の積とした場合でも、群は環状で既知の次数であるため、結果としてRSAとは大きく異なる方式となる。

## 1.9 双線形ペアリングとしての指数（Exponentiation as a Bilinear Pairing）

<details>
<summary>原文</summary>
Given some integer $r > 1$, let us take $G_1 = G_T = \mathbb{Z}^∗_r$ and $G_2 = \mathbb{Z}^+_{r−1}$.
Define $e : G_1 \times G_2 \rightarrow G_T$ by $e(g, a) = g^a$.

Then it is easily seen that if we relax the condition that the groups are cyclic, then exponentiation satisfies the definition of a bilinear pairing.
Thus we may view all discrete log and RSA cryptosystems as pairing-based cryptosystems, though there are significant differences in this setting.
For example, the discrete log problem is easy in $G_2$, and $G_1 = G_T$.
Classic problems can be restated in terms of pairings.
For example:

**Discrete Log Problem.** Given $g \in G_1$, $c \in G_T$ , find $a \in G_2$ such that $e(g, a) = c$.
**RSA Problem.** Given $a \in G_2$, $c \in G_T$ , find $g \in G_1$ with $e(g, a) = c$.
**Strong RSA Problem.** Given $c \in G_T$ , find $g \in G_1$, $a \in G_2$ with $e(g, a) = c$

Thinking of conventional cryptosystems as pairing-based cryptosystems can inspire new ideas.
</details>

ある整数 $r > 1$ が与えられたとき、$G_1 = G_T = \mathbb{Z}^∗_r$, $G_2 = \mathbb{Z}^+_{r-1}$ とする。
$e : G_1 \times G_2 \rightarrow G_T$ を $e(g, a) = g^a$ で定義する。

そして、群が環状であるという条件を緩和すれば、指数関数が双線形ペアリングの定義を満たすことが容易にわかる。
このように、離散対数暗号やRSA暗号はすべてペアリングに基づく暗号と見ることができるが、この設定には大きな違いがある。
例えば、離散対数問題は $G_2$ では簡単で、$G_1 = G_T$ である。
古典的な問題はペアリングで言い直すことができる。
例えば

**離散対数問題（Discrete Log Problem）** 与えられた $g \in G_1$, $c \in G_T$ , $e(g, a) = c$ を満たす $a \in G_2$ を求めよ。
**RSA問題（RSA Problem）** 与えられた $a \in G_2$, $c \in G_T$ , $e(g, a) = c$となる $g \in G_1$ を求めよ。
**強いRSA問題（Strong RSA Problem）** 与えられた $c \in G_T$ 、$e(g, a) = c$ をとなる $g \in G_1$, $a \in G_2$ を求めよ。

従来の暗号をペアリング型暗号と考えることで、新しい発想が生まれます。

1.10 定義の選択（Choosing a Definition）

<details>
<summary>原文</summary>
The symmetric pairing with cyclic groups into which strings can be hashed is often selected as it allows simpler and briefer mathematical statements and definitions, and the symmetry can be useful.
However, there are few families of elliptic curves that allow such a pairing.

In practice, one can usually trivially modify symmetric-pairing-based schemes and their corresponding security proofs to use a more general pairing definition, so that more families of curves can be used.
For example, one may have to replace the the Computation DiffieHellman assumption with the co-Computation Diffie-Hellman problem.
Or perhaps a minor reworking allows the cryptosystem to function when $G_2$ is not cyclic.

Ideally a cryptosystem should use the most general definition when first proposed, allowing maximum choice for the pairing in an implementation.
If a more restrictive definition is necessary, one must pick the extra assumptions carefully and be aware that perhaps only a few pairing families may satisfy them.
For example, there is no known pairing such that

1.Both input groups $G_1$, $G_2$ are cyclic.
2.One can hash strings to $G_1$ and $G_2$ without knowing the discrete log of the output to some fixed base.
3.There is an efficiently computable isomorphism $\phi : G_2 \rightarrow G_1$ whose inverse is difficult to compute.

yet for any two of the three conditions, we can find pairings that satisfy both.

Familiarizing oneself with the mathematics behind pairings has several advantages over merely memorizing valid assumptions and properties of the pairings that correspond to them.
Being ignorant of the inner workings of available pairings means we do not know when we have made one assumption too many.
For instance, it is not obvious that we can only stipulate up to two of the three conditions in the above list.
Knowledge of the computations being performed also enables us to estimate the time and space taken by cryptographic operations, which is affected by the pairing choice in subtle ways.
For example, if we use a symmetric pairing, then typically elements will be 512 bits long.
If we forgo symmetry and use two groups $G_1$, $G_2$, then it turns out we may achieve a 160-bit $G_1$ and a 320-bit $G_2$, though pairings are slower.
</details>

文字列をハッシュ化できる環状群との対称的な組合せは、数学的な記述や定義をより単純かつ簡潔にできるためしばしば選択され、その対称性は有用である。
しかし、楕円曲線にはこのような組合せを可能にする族がほとんどありません。

実際には、より一般的なペアリングの定義を使用するように、対称ペアリングに基づくスキームとそれに対応する安全性証明を些細なことから修正することができるため、より多くの曲線のファミリーを使用することができるようになるのが普通である。
例えば、CDH（Computation DiffieHellman）仮定をco-CDH（co-Computation Diffie-Hellman）問題と置き換える必要があるかもしれません。
また、$G_2$ が環状でない場合にも、ちょっとした工夫で暗号が機能するようになるかもしれません。

理想的には、暗号システムが最初に提案されたときに最も一般的な定義を使用し、 実装においてペアリングを最大限選択できるようにすることである。
より限定的な定義が必要な場合は、余分な仮定を慎重に選び、それを満たすペアリングが少数であることを認識する必要がある。
例えば、次のようなペアリングは知られていない。

1.入力群$G_1$, $G_2$がともに巡回型である。
2.出力の離散対数を知らなくても、$G_1$と$G_2$に文字列をハッシュし、ある固定基底に到達させることができる。
3.効率的に計算できる同型性 $\phi : G_2 \rightarrow G_1$ があり、その逆は計算が困難である。

しかし、3つの条件のうち任意の2つについては、両方を満たす組合せを見つけることができる。

ペアリングの背後にある数学に精通することは、単に有効な仮定とそれに対応するペアリングの特性を暗記するよりも、いくつかの利点があります。
ペアリングの内部構造を知らないということは、仮定を1つ多くしすぎたということを知らないということである。
例えば、上記の3つの条件のうち、2つまでしか規定できないことは自明ではない。
また、実行される計算を知ることで、暗号演算にかかる時間と空間を推定することができますが、これはペアリングの選択によって微妙に影響を受けます。
例えば、対称性のあるペアリングを使用する場合、一般的に要素の長さは512ビットになる。
対称性を捨てて2つの群 $G_1$, $G_2$ を使うと、ペアリングは遅くなりますが、160ビットの $G_1$ と320ビットの $G_2$ を実現できることがわかりました。

## 1.11 具体的な双線形写像（Concrete Bilinear Maps）

<details>
<summary>原文</summary>
There is only one known setting for cryptographically useful bilinear maps, namely elliptic curves with an efficiently computable Tate pairing.

Strictly speaking, one can use hyperelliptic curves where much of the theory generalizes, but this is beyond our scope.
We can find hyperelliptic curves [33, 56, 29] suitable for pairings, and although hyperelliptic curve cryptography appears to be a promising avenue for future research, there are no compelling reasons to use them yet.
To the author’s knowledge, there are no implementations of pairing-based cryptosystems based on hyperelliptic curves, and in practical terms,  $G$, or $G_1$ and $G_2$ are always groups of points on an elliptic curve, and $G_T$ is always a subgroup of a multiplicative group of a finite field.

Mathematicians have traditionally employed additive notation for the group of points on an elliptic curve, since it is Abelian.
As a result, publications in the area often use additive group notation along with uppercase letters for elements for the input groups.

We have used multiplicative notation in this chapter to highlight the Diffie-Hellman heritage of pairing-based cryptography.
This notation also suggests the right way to compare elliptic curve and finite field operations: point multiplications should be compared with exponentiations, not integer multiplications.
Later we will switch to additive notation when discussing the underlying mathematics.

When deploying a pairing-based cryptosystem one must select a type of curve to use.
The correct choice depends on several factors, which we discuss in later chapters.
</details>

暗号学的に有用な双線形写像の設定としては、効率的に計算可能なTateペアリングを持つ楕円曲線しか知られていない。

厳密に言えば、理論の多くが一般化される超楕円曲線を使うこともできるが、これは我々の範囲外である。
ペアリングに適した超楕円曲線[33, 56, 29]を見つけることができ、超楕円曲線暗号は将来の研究の有望な道と思われるが、それを使う説得力のある理由はまだない。
筆者の知る限り、超楕円曲線に基づくペアリングに基づく暗号システムの実装はなく、実用上、$G$、あるいは $G_1$、$G_2$ は常に楕円曲線上の点の群であり、$G_T$は常に有限体多倍長群の部分群である。

楕円曲線上の点群はAbelianであるため、数学者は伝統的に加法的な表記法を用いてきた。
そのため、この分野の出版物では、入力群の要素に大文字を用いた加法群表記がしばしば用いられている。

本章では、ペアリングに基づく暗号のDiffie-Hellmanの立場を強調するために、乗法的表記法を使用した。
この表記法はまた、楕円曲線と有限体演算を比較する正しい方法を示唆している：点の乗算は、整数の乗算ではなく、指数関数と比較されるべきである。
後ほど、基礎となる数学について議論する際には、加法的な表記に切り替えます。

ペアリングベースの暗号システムを導入する場合、使用する曲線の種類を選択する 必要がある。
正しい選択はいくつかの要因に依存するが、それについては後の章で説明する。

<details>
<summary>原文</summary>
We conclude with a toy example of the BLS signature scheme.
Let $q = 427211363219$.
Consider the curve $E : y^2 = x^3 + x$ over $\mathbb{F}_q$.
Then $E(\mathbb{F}_q)$ contains a subgroup $G$ of order $r = 524353$ (which is a prime factor of $427211363219 + 1$).
Take a random generator

$$g = (359473638793, 293998693014)$$

for the system parameter, and a random secret key

$$x = 66995.$$

Then the corresponding public key is

$$g^x = (166505572345, 357692656519).$$

Suppose a message hashes to

$$h = (370499138522, 78458612837).$$

Then the corresponding signature is

$$\sigma = h^x = (278647014375, 78919786700).$$

To verify, we must check that $e(g, \sigma) = e(g^x, h)$ which turns out to be

$$85791756805 + 99975533880i,$$

an element of $K[i]$ that has order $r$.

The exact value of the last result may vary from implementation to implementation since the pairing is only unique up to a constant and different implementations may result in different constants, but both sides of the equation always match.

The order $r$ in the above example is roughly 20 bits in length, while the order $q$ of the field, and the space required by compressed elements of $G_1$ and $G_2$ is roughly 40 bits.
In real life, one would require a 160-bit $r$ and 512-bit $q$ for secure signatures.
</details>

最後に、BLS署名方式のおもちゃの例で締めくくる。
$q = 427211363219$ とする。
$\mathbb{F}_q$ の上で、$E : y^2 = x^3 + x$  という曲線があるとする。
すると $E(\mathbb{F}_q)$ は位数 $r = 524353$ の部分群 $G$ を含む（これは $427211363219 + 1$ の素因数である）。
ランダムジェネレータを使う

$$g = (359473638793, 293998693014)$$

をシステムパラメータとし、ランダムな秘密鍵

$$x = 66995$$

とすると、対応する公開鍵は

$$g^x = (166505572345, 357692656519)$$

あるメッセージのハッシュが次のようなものだったとします。

$$h = (370499138522, 78458612837)$$

このとき、対応するシグネチャは

$$\sigma = h^x = (278647014375, 78919786700)$$

検証するには、 $e(g, \sigma) = e(g^x, h)$ であることを確認する必要があるのですが、その結果、次のようになります。

$$85791756805 + 99975533880i$$

は、$K[i]$の要素で、位数が $r$ であることがわかります。

ペアリングは定数までしか一意でなく、実装が異なると定数も異なるため、最後の結果の正確な値は実装によって異なる可能性がありますが、式の両辺は常に一致します。

上の例のオーダー $r$ はおおよそ20ビット長で、フィールドのオーダー $q$ と $G_1$ と $G_2$ の圧縮された要素が必要とする空間はおおよそ40ビットである。
現実には、安全な署名のために160ビットの $r$ と512ビットの $q$ が必要であろう。


