---
title: "初心者のためのペアリング 4"
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

# 第4章（Chapter 4）
# ペアリング群としての楕円曲線（Elliptic curves as pairing groups）

この章の目的は、暗号化ペアリングで使用される楕円群を定義することです。
最も抽象的な定義 [Sil10] から始めます。ペアリングとは、他のアーベル群 $R$ の値を取るアーベル群 $M$ 上の双線形写像です。

$$
\langle\cdot,\cdot\rangle:M\times M\to R.
$$

$M$ と $R$ のバイナリ群演算をそれぞれ $+$ と $*$ で表すとします。
上の写像の双線形性特性 (ペアリングに分類される) は、$x,y,z\in M$ に対して、次のことを意味します。

$$
\langle x + y, z\rangle = \langle x, z\rangle * \langle y, z\rangle, \\
\langle x, y + z\rangle = \langle x, y\rangle * \langle x, z\rangle.
$$

つまり、写像 $\langle\cdot,\cdot\rangle$ は両方の入力に線形です。

この双線形性の特性により、ペアリングが暗号化における強力な原始となります。
目的のためには、写像内の 2 つの引数が同じ群に由来するという条件をわずかに緩和することが有利であることがよくあります。そして、それらが同じ次数の巡回群（つまり同型） から来ることを許可します。
したがって、暗号化に関連する豊富な文献では、双線形写像に一般的に使用される表記は次のとおりです。

$$
e:\mathbb{G}_1\times\mathbb{G}_2\to\mathbb{G}_T .
$$

この章の主な目的は、2 つの群 $\mathbb{G}_1$ と $\mathbb{G}_2$ を定義することです。
$\mathbb{G}_T$ の定義は、次の章のペアリングの定義に付属します。

現在、暗号化に適したペアリングのインスタンス化が知られているのは、代数曲線の除数類群に関する Weil と Tate のペアリングだけであり、最も単純で効率的な場合は楕円曲線上です。
$\mathbb{F}_{q^k}$ を、$k\geq 1$ における $\mathbb{F}_q$ の有限拡大とする。
群 $\mathbb{G}_1$ と $\mathbb{G}_2$ は $E(\mathbb{F}_{q^k})$ で定義され、ターゲット群 $\mathbb{G}_T$ は乗法群 $\mathbb{F}^*_{q^k}$ で定義されるため、通常は $\mathbb{G}_1$ と $\mathbb{G}_2$ を加算的に記述し、$\mathbb{G}_T$ は乗法的に記述します。
したがって、$P,P'\in\mathbb{G}_1$ および $Q,Q'\in\mathbb{G}_2$ について、$e$ の双線形性は次のことを意味します。

$$
e(P+P',Q)=e(P,Q)\cdot e(P',Q),\\
e(P,Q+Q')=e(P,Q)\cdot e(P,Q'),
$$

このことから、スカラー $a,b\in\mathbb{Z}$ については、次のようになります。

$$
e([a]P,[b]Q)=e(P,[b]Q)^a=e([a]P,Q)^b=e(P,Q)^{ab}=e([b]P,[a]Q). \tag{4.1}
$$

$\mathbb{G}_1$、$\mathbb{G}_2$、または $\mathbb{G}_T$ はまだ定義されておらず、ペアリング $e(P,Q)$ がどのように計算されるかについての議論を開始するにはまだ少し時間がかかりますが、コンテキスト内でペアリングの双線形特性をすぐに確認するのに役立ちます。

例 4.0.1 (Magma script).
$q=7691$ とし、$E/\mathbb{F}_q:y^2=x^3+1$ とします。
$\mathbb{F}_{q^2}$ が $\mathbb{F}_{q^2}=\mathbb{F}_q(u)$ ($u^2+1=0$) として構築されるとします。
$P=(2693,4312)\in E(\mathbb{F}_q)$ および $Q=(633u+6145,7372u+109)\in E(\mathbb{F}_{q^2})$ とします。
$\#E(\mathbb{F}_q)=2^2\cdot3\cdot641$ および $\#E(\mathbb{F}_{q^2})=2^4\cdot3^2\cdot641^2=\#E(\mathbb{F}_q)^2$。
$P$ と $Q$ は、同じ素数次数 $r=|\langle P\rangle|=|\langle Q\rangle|=641$ の異なる部分群に属するように特別に選択されました (理由は後で説明します)。
$P$ と $Q$ の Weil ペアリング $e(\cdot,\cdot)$ は $e(P,Q)=6744u+5677\in\mathbb{F}^*_{q^2}$ です。
実際には、$r|\#\mathbb{F}_{q^2}$ 、そして $e(P,Q)$ は $\mathbb{F}_{q^2}$ の部分群に属します、つまり $\mu_r\in\mathbb{F}_{q^2}$ の $r$ 乗根にあり、これは $e(P,Q)^r=1$ を意味します。
これで、双線形性の例をいくつか説明できるようになりました。
したがって、任意の $a\in\mathbb{Z}_r$ および $b\in\mathbb{Z}_r$ 、たとえば $a=403$ および $b=135$ をとり、$[a]P=(4903,2231)$ および $[b]Q=(5806u+1403,6091u+2370)$ であることがわかります。
$e([a]P,Q)=3821u+7025$ を計算し、$e([a]P,Q)=3821u+7025=(6744u+5677)^{403}=e(P, Q)^a$ であることを確認できます。 または $e(P,[b]Q)=248u+5$ とすると、 $e(P,[b]Q)=248u+5=(6744u+5677)^{135}=e(P,Q)^b$ ; または $e([a]P,[b]Q)=2719u+2731=(6744u+5677)^{561}=e(P,Q)^{a\cdot b}\mod{r}$。
$e(P,Q)\ne 1\in\mu_r$ であるため、$e([a]P,[b]Q)$ は次の場合にのみ自明になることに注意してください。これは $r|ab$ を意味します。 $r|a$ または $r|b$ は、$[a]P$ または $[b]Q$ のいずれか (または両方) が $\mathcal{O}$ でなければならないことを意味します。
したがって、 $e(P,Q)\ne 1$ は、自明ではない引数に対する自明ではないペアリングを保証します。 これは、非退化と呼ばれる、暗号化に必要な特性です。

上記の例 4.0.1 に従って、ペアリング $e$ が双線形で非退化で効率的に計算できる場合、$e$ は認容的ペアリングと呼ばれます。

備考 4.0.1 (ECC 対 PBC)
この非公式な発言は、PBC の新規参入者に対する説明を目的としています。
膨大な量の文献を消化する初期の段階での混乱は、ECC と PBC の関係をより広い文脈で捉えるのに役立つ Lynn の論文の 1 つの段落によって部分的に軽減されました。
暗号化に適した既知の認容的ペアリングは、代数曲線上の Weil と Tate のペアリングだけです。
これらのペアリングが楕円曲線上で定義できるという事実は、ペアリングが登場する前からすでに非常に魅力的な暗号設定でしたが、Lynn 氏が言うように、「幸せな偶然」です。
暗号学者は、適切な形式であれば安全で認容的ペアリングを歓迎したでしょうが、それらが代数幾何学の領域から引き継がれ、楕円曲線上で計算されるという事実により、ペアリングは「さらに魅力的」になります [Lyn07、§2.9]。

暗号化では、認容的ペアリングを構成する 3 つよりも多くのプロパティが必要です。
ペアリングベースのプ原始に従来の原始よりも高い機能を与える (4.1) の双線形特性の魔法は、3 つの群すべて内の離散対数関連の問題が依然として解決できない場合を除き、役に立ちません。
例 4.0.1 は認容的ペアリングを示していますが、$\mathbb{G}_1$、$\mathbb{G}_2$、$\mathbb{G}_T$ のおもちゃのサイズはそれぞれの離散対数問題に関して明らかに抵抗がないため、このようなペアリングのインスタンスは明らかに使用されません。
ただし、3 つの群すべてのサイズ $r$ がさらに大きくなるように拡張された場合 (たとえば、512 ビット)、対応するペアリングは現在のセキュリティ要件を満たし、すべての既知の攻撃に耐えることができます。
許容要件を満たす代替の双線形ペアリングを提案しますが、(群サイズがどれほど大きいかに関係なく) PBC には依然として適していません。
この例も、Lynn の論文 [Lyn07、§1.9] から引用されています。

例 4.0.2 (Magma script).
$r>1$ を整数とします。
$e:\mathbb{G}_1\times\mathbb{G}_2\to\mathbb{G}_T$ は $\mathbb{G}_1=\mathbb{G}_T=Z^*_r$ および $\mathbb{G}_2=Z^+_{r−1}$ を持ち、 $e:(g,a)=g^a$ で定義されるとします。
$g,g'\in\mathbb{G}_1$ の場合は $e(g\cdot g',a)=e(g,a)\cdot e(g',a)$ となり、$a,a'\in\mathbb{G}_2$ の場合は $e(g,a+a')=e(g,a)\cdot e(g,a')$。
$e$ は明らかに双線形ですが、$\mathbb{G}_2$ の離散対数問題は簡単なので、双線形写像の検出力はいくぶん冗長になります。
しかしながら、上記の組み合わせに関して古典的な問題のいくつかを依然として述べることができるのは興味深いことです。
たとえば、 $r$ を大きな素数に設定すると、標準の離散対数問題は次のようになります。$g\in\mathbb{G}_1$、$h\in\mathbb{G}_T$ が与えられた場合、$e(g,a)=h$ となる $a\in\mathbb{G}_2$ を求めます。

## 4.1 $r$ ねじれ（The $r$-torsion）

次に、群 $\mathbb{G}_1$ と $\mathbb{G}_2$ を具体的に定義することに焦点を当てます。
ペアリングがどのように計算されるかをまだ見ていないため、$\mathbb{G}_1$ と $\mathbb{G}_2$ から何が必要かについていくつかのステートメントを作成する必要があります。これは、次の章で Weil と Tate のペアリングの定義が登場する場合にのみ実際に結合します。
そのような主なステートメントは、Weil または Tate sの意味で、ペアリング $e(P,Q)$ を計算するには、$P$ と $Q$ が同じ素数<sup>1</sup>次数 $r$ の互いに素な巡回部分群に由来する必要があるということです。
現時点では、Weil の相反性 (定理 3.1) の記述で行われた互いに素なサポートの規定を参照し、$P$ と $Q$ が同じ巡回部分群内にある場合に次のように主張することによって、その理由を示唆することしかできません。この場合、関連する約数のサポートが望ましくない一致を強いられるため、ペアリングの計算は本質的に失敗します。

---

<sup>1</sup> $r$ が複合値の場合に追加の機能を利用する研究がいくつかありました。 RSA モジュラス $n=pq$ ですが、これはそれほど一般的ではなく、効率性も非常に低い設定であるとは考えていません。詳細については、[BGN05、Fre10、BRS11、Lew12] を参照してください。

---

$E(\mathbb{F}_q)$ 自体に部分群が 1 つしか含まれていない場合に、次数 $r$ の複数の巡回部分群を見つける方法の例 (4.0.1) をすでに見てきました。
つまり、$\mathbb{F}_q$ を $\mathbb{F}_{q^2}$ に拡張し、$E(\mathbb{F}_{q^2})\backslash E(\mathbb{F}_q)$ に次数 $r$ の他の部分群が少なくとも 1 つあることがわかり、そこで $Q$ を定義し、その後 $e(P,Q)$ を計算できました。
これはまさに、一般に 2 つの異なる次数 $r$ 部分群を取得する方法です。$E(\mathbb{F}_{q^k})$ が次数 $r$ のより多くの点を捕捉するような $\mathbb{F}_q$ の最小拡張 $\mathbb{F}_{q^k}$ を見つけます。
これを実現する整数 $k\geq 1$ は埋め込み次数と呼ばれ、ペアリングの計算において重要な役割を果たします。
また、議論の中心となるのは、$E(\mathbb{F}_q)$ 上の次数 $r$ の点の群全体であり、$r$ ねじれと呼ばれます。これは $E[r]$ で示され、$E[r]=\set{P\in E:[r]P=\mathcal{O}}$ として定義されます。

次の結果 ([ACD+05, Th. 13.13] または [Sil09, Ch. III, Cor. 6.4(b)] を参照) は非常に注目に値します。 それは $E[r]$ の濃度だけでなく、その構造も教えてくれます。
$K$ が特性 0 または $r$ の素数を持つ任意の体の場合、次のようになります。

$$
E[r]\cong\mathbb{Z}_r\times\mathbb{Z}_r. \tag{4.2}
$$

これは、一般に $\#E[r]=r^2$ であることを意味します。
さらに、無限大 $\mathcal{O}$ の点はすべての次数 $r$ の部分群に重なるため、式 (4.2) は、(素数 $r$ の場合) $r$ ねじれが次数 $r$ の $r+1$ 個の巡回部分群から構成されることを意味します。
埋め込み次数 $k$ の次の等価条件も、$E[r]$ が全体のどこにあるかを正確に示します。
埋め込み次数は実際には $q$ と $r$ の関数 $k(q,r)$ であることに注意してください。ただし、通常はコンテキストが明らかであるため、単に $k$ と書きます。

- $k$ は、$r|(q^k−1)$ となるような最小の正の整数です。
- $k$ は、$\mathbb{F}_{q^k}$ が $\bar{\mathbb{F}}_q$ 内の単位の $r$ 乗根をすべて含むような最小の正の整数です (つまり、 $\mu_r\sub\mathbb{F}_{q^k}$ )。
- $k$ は、$E[r]\sub E(\mathbb{F}_{q^k})$ となる最小の正の整数です。

$r||\#E(\mathbb{F}_q)$ (つまり、$r|\#E(\mathbb{F}_q)$ だが $r^2\nmid\#E(\mathbb{F}_q)$) の場合、$E(\mathbb{F}_q)$ の $r$ ねじれ部分群は一意です。
この場合、$k>1$ および (4.2) は、$\mathbb{F}_{q^k}$ が $E(\mathbb{F}_{q^k})\backslash E(\mathbb{F}_q)$ に属する $r$ ねじれ点をさらに生成する $\mathbb{F}_q$ の最小の拡大体であることを意味します。
言い換えれば、拡大体が次数 $r$ の点 (基本体上で定義されていないもの) をもう 1 つ見つけるのに十分な大きさになると、実際には $E[r]\cong\mathbb{Z}_r\times\mathbb{Z}_r$ 内のすべての点が見つかります。
Scott [Sco04] はこの現象をより詩的に説明しています。

「...同じ方程式を持つ曲線が、$k$ の特定の値に対して体 $\mathbb{F}_{q^k}$ 上で考慮されると、かなり魔法のようなことが起こります。群構造は奇妙な開花を遂げ、より新しく、よりエキゾチックな性格を帯びます。」

また、Scott によるねじり部分群 $E[r]$ の描写は特に有益であることがわかりました [Sco04, Sco07a]。そのため、以下の例とこの章の残りの部分全体でそれを使用します。

Example 4.1.1 (Magma script).
Let q = 11, and consider E/Fq : y2 = x3 + 4.
E(Fq) has 12 points, so take r = 3 and note (from Equation (4.2)) that there are 9 points in the 3-torsion.
Only 3 of them are found in E(Fq), namely (0, 2) (0, 9) and O, which agrees with the fact that the embedding degree k 6= 1, since (q1 − 1) 6≡ 0 mod r. 
However, (q2 − 1) ≡ 0 mod r which means that the embedding degree is k = 2, so we form Fq2 = Fq(u), with u2 + 1.
Thus, we are guaranteed to find the whole 3-torsion in E(Fq2 ), and it is structured as 4 cyclic subgroups of order 3; O overlaps into all of them – see Figure 4.1.

例 4.1.1 (Magma script).
$q=11$ として、$E/\mathbb{F}_q:y^2=x^3+4$ を考えます。
$E(\mathbb{F}_q)$ には $12$ 個の点があるため、$r=3$ とし、(式 (4.2) より) $3$ ねじれには $9$ 個の点があることに注意してください。
そのうちの $3$ つだけ、つまり $(0,2)(0,9)$ と $\mathcal{O}$ が $E(\mathbb{F}_q)$ で見つかります。これは、$(q^1−1)\not\equiv0 \mod r $であるため、埋め込み次数 $k\ne 1$ であるという事実と一致します。
ただし、$(q^2−1)\equiv 0 \mod r$ は、埋め込み次数が $k=2$ であることを意味するため、$u^2+1$ で $\mathbb{F}_{q^2}=\mathbb{F}_q(u)$ を形成します。
したがって、$E(\mathbb{F}_{q^2})$ で $3$ ねじれ全体を見つけることが保証されており、それは次数 $3$ の $4$ つの巡回部分群として構造化されています。 $\mathcal{O}$ はそれらすべてに重なっています – 図 4.1 を参照してください。

![](/images/pfb/figure4_1.png)

$\mathcal{O}$ は $3$ ねじれにありますが、次数 $3$ を持たないことを指摘します。
次数 $1$ ではなく、次数 $d|r$ の点が自動的に $r$ ねじれに含まれます。
同じ部分群内になく、どちらも $\mathcal{O}$ ではない任意の $2$ つの点 $P,Q\in E[3] \backslash \set{\mathcal{O}}$ を考えます。
式 (4.2) を翻訳すると、$E[3]$ 内の他の点は $[i]P+[j]Q,i,j\in\set{0, 1, 2}$ として取得できることになりま
$P\ne\mathcal{O}$ を固定し、$j$ に $0,1,2$ を実行させると、$E[3]$ の他の $3$ つの部分群に $P+[j]Q$ が配置されます ($\langle Q\rangle$ ではありません。これは $P=\mathcal{O}$ に対応します)。

Example 4.1.2 (Magma script).

例 4.1.2 (Magma script).

In the rare case that r2| #E, it is possible that the entire r-torsion can be found over E(Fq), i.e. that the embedding degree is k = 1.
Consider E/F31 : y2 = x3 + 13, which has 25 points, so take r = 5.

$r^2 | \#E$ のようなまれなケースでは、$r$ ねじれ全体が $E(\mathbb{F}_q)$ 上で見つかる可能性があります。つまり、埋め込み次数は $k=1$ です。
$E/\mathbb{F}_{31}:y^2=x^3+13$ を考えます。これには $25$ 点があるため、$r=5$ とします。
$r|q−1, k=1$ であるため、$E[r] \subseteq E(\mathbb{F}_q)$; 図 4.2 は、$E[5]\cong\mathbb{Z}_5\times\mathbb{Z}_5$ を構成する次数 $5$ の $6$ つの環状部分群を示しています。
もちろん、次数 $r^2$ の点が可能であるため、$r^2|\#E(\mathbb{F}_q)$ は必ずしも $E[r]\subseteq E(\mathbb{F}_q)$ を意味するわけではありません。

![](/images/pfb/figure4_2.png)

次の例の前に、$r$ ねじれ 部分群内で複雑な役割を果たす重要な写像を紹介します。
$\mathbb{F}_q$ の有限拡大体について研究しているので、ガロア理論からの有用な貢献が見つかるのは当然です。
つまり、点 $P=(x,y)\in E(\mathbb{F}_{q^k})$ のトレース写像は次のように定義されます。

$$
\mathrm{Tr}(P)=\sum_{\sigma\in\mathrm{Gal}(\mathbb{F}_{q^k}/\mathbb{F}_q)}\sigma(P)=\sum_{i=0}^{k-1}\pi^i(P)=\sum_{i=0}^{k-1}(x^{q^i},y^{q^i}),
$$

ここで、$\pi$ は式 (2.7) で定義された $q$ 乗のフロベニウス準同型写像です。

ガロア理論では、 $\mathrm{Tr}:E(\mathbb{F}_{q^k})\to E(\mathbb{F}_q)$ であるため、  $r||\#E(\mathbb{F}_q)$  (これは今後も常に当てはまります) の場合、この写像マップは次のようになります。
これは実際には群準同型性であり、すべてのねじれ点を $r$ ねじれの $1$ つの部分群に送ります。
全体像を描く前に、例 4.1.3 で説明します。

例 4.1.3 (Magma script).
再び $q=11$ を使用しますが、今回は $E/\mathbb{F}_q:y^2=x^3+7x+2$ を使用します。
$E(\mathbb{F}_q)$ には $7$ 点があるため、$r=7$ とします。
すでに $E(\mathbb{F}_q)[r]$ がありますが、 $E[r]$ を全体として収集するには、 $\mathbb{F}_q$ を $\mathbb{F}_{q^k}$ に拡張する必要があります。
今回は、$(q^k−1)\mod 7\equiv 0$ となる最小の整数 $k$ は $k=3$ であるため、$u^3+u+4=0$ で $\mathbb{F}_{q^3}=\mathbb{F}_q(u)$ を形成し、$E[7]\subset E(\mathbb{F}_{q^3})$ が保証されます。
図 4.3 に示すように、$7$ ねじれは濃度 $49$ を持ち、$8$ つの循環部分群に分割されます。
点を当てはめるために、$\mathbb{F}_{q^3}=\mathbb{F}_q(u)$ の要素のべき乗表現を使用します。
この場合、$P\in E(\mathbb{F}_{q^3})$ の場合、$E$ 上のトレース写像は $\mathrm{Tr}(P)=(x,y)+(x^q,y^q)+(x^{q^2},y^{q^2})$ になります。
固有のねじれ部分群 $E(\mathbb{F}_q)[r]$ の場合、フロベニウス準同型性は自明 $(\pi(P)=P)$ であるため、トレース写像は明らかに $k$ による乗算として機能します、つまり $\mathrm{Tr}(P)=[k]P$ です。
ただし、$\mathrm{Tr}$ はねじれ内の 1 つおきの要素を $E(\mathbb{F}_q)[r]$ に送信します。
たとえば、$Q=(u^{481},u^{1049})$ (上向きの部分群内) の場合、$\mathrm{Tr}(Q)=(8,8)$ になります。 $R=(u^{423},u^{840})$ (右下の部分群) の場合、$\mathrm{Tr}(R)=(10,7)$ になります。 $S=(u^{1011},u^{1244})$ の場合、$\mathrm{Tr}(S)=(8,3)$ となります。
ただし、$E[7]$ には別の特異な部分群があり、トレース写像は各要素を $\mathcal{O}$ に送信します。
これは一般的に発生しており、これが PBC に重要な結果をもたらすことがわかりつつあります。この場合、この部分群は $T=(u^{1315},u^{1150})$ を含む右上の群です。つまり、 $\mathrm{Tr}(T)=\mathcal{O}$ なので、$\mathcal{Tr}:\langle T\rangle\to\set{\mathcal{O}}$ となります。
最後に注意すべき点は、埋め込み次数 $k=3$ は、(6つの) 自明ではない $1$ の $7$ 乗根がすべて $\mathbb{F}_{q^3}$ で (ただし、それ以前ではなく) 見つかっていることを意味していることです。つまり、$\mu_7\backslash\set{1}\in\mathbb{F}_{q^3}\backslash\mathbb{F}_{q^2}$ 。

![](/images/pfb/figure4_3.png)

次に、$r$ ねじれ $E[r]$ の一般的な説明をします。
そのためには、これから遭遇するシナリオに最も一般的に当てはまるいくつかの前提について話し合う必要があります。
まず、$r||\#E(\mathbb{F}_q)$ が素数であり、埋め込み次数 $k$ ($r$ に関して) が $k>1$ であると仮定します。
したがって、$E[r]$ には、基礎体 部分群と呼ばれる、$\mathbb{F}_q$ に対して定義される次数 $r$ の固有の部分群が存在します。 それは $\mathcal{G}_1$ で示されます。
フロベニウスの同型写像 $\pi$ は $\mathcal{G}_1$ には自明に作用しますが、$E[r]$ の他の場所には作用しないため、$\mathcal{G}_1=E[r]\cap\mathrm{Ker}(\pi−[1])$ と定義できます。
つまり、$\mathcal{G}_1$ は $E[r]$ に制限された $\pi$ の $[1]$ 固有空間です。
$\pi$ の固有空間を使用して表現できる $E[r]$ の別の部分群があります。
式 (2.8) に戻ると、$\pi$ の他の固有値が $q$ であることが容易に推測でき、$E[r]$ の別の部分群 $\mathcal{G}_2$ を $\mathcal{G}_2=E[r]\cap\mathrm{Ker}(\pi−[q])$ として定義します。
この部分群は、まさに例 4.1.3 でほのめかした特異な部分群であることがわかります。
すべての $P\in\mathcal{G}_2$ は $\mathrm{Tr}(P)=\mathcal{O}$ であるため、$\mathcal{G}_2$ をトレース ゼロ 部分群と呼びます。 この結果は Dan Boneh によるものです [Gal05, Lemma IX.16]。
図 4.4 に示します。

![](/images/pfb/figure4_4.png)

また、アンチトレース 写像 $a\mathrm{Tr}:P\mapsto P'=[k]P−\mathrm{Tr}(P)$ を介して、任意の $P\in E[r]$ をトレース ゼロ 部分群 $\mathcal{G}_2$ に写像することもできます。 $\mathrm{Tr}(P')=\mathcal{O}$ であることを示すことは、読者にとって価値のある演習です。

ペアリングを定義するには、2 つの群 $\mathbb{G}_1$ と $\mathbb{G}_2$ を指定する必要があります。$\mathbb{G}_1$ と $\mathbb{G}_2$ は $E[r]$ の $r+1$ 群のいずれかとして定義できるため、これらの $\mathbb{G}$ を 2 つの特定の $r$ ねじれ部分群を表す $\mathcal{G}$ と混同しないでください。
ただし、後で説明するように、特に $\mathbb{G}_1=\mathcal{G}_1$ および $\mathbb{G}_2=\mathcal{G}_2$ を設定したい理由はたくさんあります。しかし、後で説明するように、これを望まない理由もあります。
異なるねじれ部分群への、または異なるねじれ部分群からの写像の存在は、暗号作成者がペアリングベースのプロトコルに望む特定の機能に影響を与えます。
これらの機能と利用可能な選択肢についてはすぐに説明しますが、最初に特別な類の曲線に利用できる最後の写像を確認する必要があります。
$\#E(\mathbb{F}_q)=q+1$ の場合、素体上で楕円曲線 $E$ 超特異<sup>2</sup>と呼びます。
他にも同等の条件がいくつかあります [Sil09, Ch. V, Th. 3.1(a)]が、私たちの目的にとって最も意味のある特性は、超特異曲線に歪みマップ $\phi$ が備わっていることです。
これは、$E(\mathbb{F}_q)$ 内の点を $E(\mathbb{F}_{q^k})$ 内の点に取る非 ($\mathbb{F}_q$-) 有理写像です [Gal05, §IX.7.2]。
超特異点ではない曲線は通常曲線と呼ばれ、そのような写像は存在しません [Ver01, Th. 11]。
超特異点である楕円曲線の 2 つの例を示し、ねじれ内の歪み写像 $\phi$ の動作を示します。

---

<sup>2</sup>この用語を、上で説明した図 2.1 ～ 2.4 の単数形と非単数形の定義と混同しないでください。

---

例 4.1.4 (Magma script).
$q=59$ とします。この場合、$E/\mathbb{F}_q:y^2=x^3+1$ は超特異であり、$\#E(\mathbb{F}_q)=q+1=60$ を意味するため、$r=5$ とします。
埋め込み次数は $k=2$ であるため、$\mathbb{F}_{q^2}=\mathbb{F}_q(i)$、$i^2+1=0$ として拡張を構築します。
$\xi_3=24i+29$ は 1 の立方根であり、それに関連する歪みマップは $\phi:(x,y)\mapsto(\xi_3x,y)$ となります。
$\phi^3$ が $E$ 上の恒等写像と等価であるという事実は、図 4.5 に示されています。

![](/images/pfb/figure4_5.png)

例 4.1.5 (Magma script).
前の例と同じ体 ($q=59,\mathbb{F}_{q^2}=\mathbb{F}_q(i),i^2+1=0$) を使用しますが、代わりに超特異曲線 $E/\mathbb{F}_q:y^2=x^3+x$ を使用します。したがって、これも $\#E(\mathbb{F}_q)=60$ となります。
今回のディストーション（歪み）写像は $\phi :(x,y)\mapsto (−x,iy)$ であり、$\phi^4$ が $E$ 上の恒等写像に相当することが容易にわかります。
図 4.6 では、(この場合) ディストーション（歪み）写像が常に要素を部分群の外に移動させるわけではないことがわかります。しかし、むしろ $\phi$ を、たとえば $(28i+51,25i+49)$ によって生成されるねじれ部分群に制限すると、$\langle(28i+51,25i+49)\rangle$ 上の準同型写像が得られます。
これは、ペアリング計算における主要な最適化の 1 つを示唆しています。
つまり、第 2 章では、ECC に適用される準同型性の威力を見ました (特に例 2.2.11)。そして第 7 章では、ねじり部分群の準同型性 (上記のような) が PBC で大きな効果を発揮できることを見ていきます。

![](/images/pfb/figure4_6.png)

$r$ ねじれ内で利用可能な写像を要約します。
G1 または G2 ではない E[r] 内の任意のサブグループから、
トレース写像とアンチトレース写像プを介して、いつでも $\mathcal{G}_1$ または $\mathcal{G}_2$ に写像できます。
$E$ が普通の場合、$\mathcal{G}_1$ または $\mathcal{G}_2$ から計算可能な写像はありません。それ以外の場合、$E$ が超特異である場合、ディストーション（歪み）写像 $\phi$ はこれら 2 つの部分群からの準同型写像になります。

## 4.2 ペアリングの種類（Pairing types）

As we mentioned before the previous two examples, the interplay between the maps that are available in any given scenario gives rise to different functionalities within a pairing-based protocol.

前の 2 つの例で述べたように、任意のシナリオで利用可能な写像間の相互作用により、ペアリングベースのプロトコル内でさまざまな機能が発生します。

Galbraith et al. [GPS08] were the first to identify that all of the potentially desirable properties in a protocol cannot be achieved simultaneously, and therefore classified pairings into certain types.

ガルブレイスら。 [GPS08] は、プロトコル内の潜在的に望ましい特性をすべて同時に達成することはできないことを最初に特定し、したがってペアリングを特定のタイプに分類しました。

There are now four pairing types in the literature; Galbraith et al. originally presented three, but a fourth type was added soon after by Shacham [Sha05].
The pairing types essentially arise from observing the (practical) implications of placing G1 and G2 in different subgroups of E[r]; in fact, it will soon become obvious that it is always best to set G1 = G1, so the four types really are tied to the definition of G2.
The main factors affecting the classification are the ability to hash and/or randomly sample elements of G2, the existence of an isomorphism ψ : G2 → G1 which is often required to make security proofs work (see [GPS08]), and (as always) issues concerning storage and efficiency.

# まとめ（Conclusion）

<details><summary>原文</summary>
</details>



