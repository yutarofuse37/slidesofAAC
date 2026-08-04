---
theme: seriph
title: Multiplicative Weights Update for Absolute Algebraic Connectivity
info: |
  ## Multiplicative Weights Update for Absolute Algebraic Connectivity

  布施 祐大郎, 清水 伸高（東京科学大学）
  夏のLAシンポジウム2026
drawings:
  persist: false
comark: true
duration: 35min
colorSchema: light
layout: cover
background: /bg_title.png
class: text-left
---

<img
  src="./logo.png"
  alt="Institute of Science Tokyo"
  class="absolute bottom-6 left-8 h-9"
/>

<div class="absolute inset-0 flex flex-col justify-center pl-12 pr-40 -mt-4 text-black">
  <h1 class="!text-2xl !leading-snug !text-left !text-black !font-normal">
    Multiplicative Weights Update<br>
    for Absolute Algebraic Connectivity
  </h1>

  <div class="pt-6 text-lg text-black">
    布施 祐大郎, 清水 伸高
  </div>
  <div class="pt-1 text-black">
    東京科学大学
  </div>
  <div class="pt-8 text-sm text-black">
    夏のLAシンポジウム2026
  </div>
</div>

---
layout: section
---

# 背景と動機

---

# ネットワーク設計

<v-clicks>

現実の多くのシステムは，ネットワークとして表現できる．

- 通信網：障害が起きても通信を維持したい．
- 交通網・物流網：一部の経路が止まっても全体機能を保ちたい．
- 分散システム：情報共有や合意形成を安定かつ高速に行いたい．

<div class="callout-bar">

この共通課題は，「ネットワークをどれだけ頑健に設計できるか」である．

</div>

本研究ではあるグラフが与えられたときに，各辺に重みを配分することで，その重み付きグラフの**連結性を最大化すること**が目的である．

</v-clicks>

---

# 問題の直感

<v-click>

構造が同じグラフでも，重み配分により「分断されにくさ」は大きく変化する．

</v-click>

<div class="diagram-wrap">
  <GraphWeights />
</div>

<div class="mt-3 text-center">
<v-click>

「分断されにくさ」を測り，その値を重み配分によって**最大化**するか．

</v-click>
</div>

---

# 連結性指標 $\lambda_2$ とその解釈

<v-click>

重み付きラプラシアン $L(w)$ の第二固有値（algebraic connectivity）<a href="https://dml.cz/handle/10338.dmlcz/101168" target="_blank" rel="noopener">&#91;Fiedler 1973&#93;</a>：

</v-click>

$$
\lambda_2(L(w))
= \min_{\substack{x^\top\mathbf{1}=0\\ \|x\|_2=1}}
\sum_{\{u,v\}\in E} w_{\{u,v\}}(x_u-x_v)^2
$$

<div class="grid grid-cols-[1.1fr_0.9fr] gap-5 items-center mt-2">
  <div class="text-sm leading-relaxed">

  **ゲームとしてみる：**

  <v-clicks>

  - **守備側**：総量1の強度（ばね定数）を辺へ配分する（$w\in\Delta_E$）．
  - **攻撃側**：各頂点への引っ張り $x_u$ を選ぶ．符号は向き，$|x_u|$ は力の大きさ．<br>
    （ただし $x^\top\mathbf{1}=0$，$\|x\|_2=1$）
  - $C(x;w)=\sum w_{\{u,v\}}(x_u-x_v)^2$：その引っ張りに対するコスト（弾性エネルギー）
  - **攻撃側**はコスト $C$ を最小化：$\lambda_2(L(w))=\min_x C(x;w)$

  </v-clicks>

  </div>
  <div class="text-center">
    <AttackDefense />
  </div>
</div>

---

# 具体例：パス上の攻撃と守備

<v-click>

パス $1$--$2$--$3$--$4$．攻撃側は中央で左右に引っ張る：
$x=\bigl(-\tfrac12,-\tfrac12,\tfrac12,\tfrac12\bigr)$
（$x^\top\mathbf{1}=0$，$\|x\|_2=1$）．
伸びるのは辺 $\{2,3\}$ のみで，$C(x;w)=w_{23}$．

</v-click>

<div class="diagram-wrap">
  <GraphPath />
</div>

<div class="mt-3 text-sm">
<v-clicks>

- 同じ引っ張り方でも，$w_{23}$ の大きさでコストが大きく変わる．
- **守備側**は，あらゆる攻撃に対してコストが高くなるよう $w$ を配分したい．

</v-clicks>
</div>

---

# 守備側の最適化

<v-click>

守備側は，攻撃側の最善（$\min_x$）を見越して $w$ を選ぶ：

</v-click>

$$
\max_{w\in\Delta_E}\ \lambda_2(L(w))
=\max_{w\in\Delta_E} \min_{\substack{x^\top\mathbf{1}=0\\ \|x\|_2=1}}
\sum_{\{u,v\}} w_{\{u,v\}}(x_u-x_v)^2
$$

<div class="diagram-wrap">
  <GraphBridge />
</div>

<div class="mt-2 text-sm">
<v-clicks>

- 戦略：攻撃されやすい辺の強度を高め，どの引っ張りでもコストが同程度になるようにする．
- これが algebraic connectivity の最大化問題である．

</v-clicks>
</div>

---

# 今回扱う問題

<v-clicks>

<div class="callout">

**定義**（Absolute Algebraic Connectivity <a href="https://doi.org/10.1080/03081089008817967" target="_blank" rel="noopener">&#91;Fiedler 1990&#93;</a>）

連結な無向グラフ $G=(V,E)$ に対し：

$$
\begin{aligned}
\text{maximize}\quad & \lambda_2(L(\boldsymbol{w})) \\
\text{subject to}\quad
& \boldsymbol{w} \in \Delta_E := \left\{\boldsymbol{x}\in\mathbb{R}_{\ge 0}^{E} \;\middle|\; \sum_{e\in E}x_e=1\right\}.
\end{aligned}
$$

この最適値を **Absolute Algebraic Connectivity** といい，$\mathsf{OPT}$ と書く．

</div>

<div class="callout">

**定義**（$(1-\varepsilon)$-近似解）

$\bar{w}\in\Delta_E$ が $\lambda_2(L(\bar{w}))\ge(1-\varepsilon)\mathsf{OPT}$ を満たすとき，$\bar{w}$ を $(1-\varepsilon)$-近似解という．

</div>

</v-clicks>

---

# 関連研究

<v-clicks>

- Absolute Algebraic Connectivity は <a href="https://doi.org/10.1080/03081089008817967" target="_blank" rel="noopener">&#91;Fiedler 1990&#93;</a> により導入された．
- <a href="https://web.stanford.edu/~boyd/papers/pdf/fastestmix.pdf" target="_blank" rel="noopener">&#91;Boyd–Diaconis–Xiao 2004&#93;</a> は，ランダムウォークの混合を速める辺重み設計を algebraic connectivity 最大化として定式化し，SDPに基づく多項式時間アルゴリズムを与えた．
- 最適辺重みや双対解の構造，特定グラフクラスの陽な公式も研究されている
  （例：<a href="https://doi.org/10.1016/j.laa.2006.08.026" target="_blank" rel="noopener">&#91;de Abreu 2007&#93;</a>，
  <a href="https://doi.org/10.1109/cdc.2008.4738734" target="_blank" rel="noopener">&#91;Wan et al. 2008&#93;</a>）．
- 我々の知る限り，一般グラフに対する既知の多項式時間近似アルゴリズムはいずれも SDP に依拠しており，組合せ的アルゴリズムは知られていなかった．

<div class="callout-alert">

**本研究の位置づけ**

半正定値計画に依拠せず，乗法的重み更新法（MWU）に基づく組合せ的近似アルゴリズムを与える．

</div>

</v-clicks>

---
layout: section
---

# 問題設定

---

# グラフラプラシアンとFiedlerベクトル

<v-clicks>

無向グラフ $G=(V,E)$ に対し，辺 $e=\{u,v\}$ の辺ラプラシアンを
$L_e := (\chi_u - \chi_v)(\chi_u - \chi_v)^\top$ と定める．

重み $w\in\Delta_E$ に対するラプラシアンは $L(w)=\sum_{e\in E} w_eL_e$ である．

- $L(w)$ の最小固有値は常に $0$（対応固有ベクトルは $\mathbf{1}$）．
- $X := \left\{x \in \mathbb{R}^n \mid x^\top \mathbf{1} = 0, \; \|x\|_2 = 1\right\}$ とおくと，Courant–Fischer より：

$$
\lambda_2(L(w)) = \min_{x\in X} x^\top L(w) x.
$$

この最小値を与える $x$ は $L(w)$ の第二固有ベクトルであり，**Fiedlerベクトル**と呼ばれる．

</v-clicks>

---

# 近似Fiedlerベクトル

<v-clicks>

正確なFiedlerベクトルの計算は難しいので，**近似Fiedlerベクトル**を用いる．

<div class="callout">

**定義**（近似Fiedlerベクトル <a href="https://doi.org/10.1137/130915984" target="_blank" rel="noopener">&#91;Spielman–Teng 2014&#93;</a>）

許容誤差 $\alpha\ge 0$ に対し，$x\in X$ が

$$
x^\top L x \le (1+\alpha)\min_{y\in X} y^\top L y
\quad\bigl(\text{すなわち}\quad x^\top L x \le (1+\alpha)\lambda_2(L)\bigr)
$$

を満たすとき，$x$ を $\alpha$-近似Fiedlerベクトルという．

</div>

<div class="callout">

**補題**（高速オラクル <a href="https://doi.org/10.1137/130915984" target="_blank" rel="noopener">&#91;Spielman–Teng 2014&#93;</a>）

任意の $\alpha>0$ に対し，確率 $1-\frac{1}{m^{10}}$ 以上で $\alpha$-近似Fiedlerベクトルを出力する
乱択アルゴリズムが存在し，計算時間は $\widetilde{O}\!\left(\frac{m}{\alpha}\right)$．

</div>

- 乱択部分はこのオラクルのみであり，各呼び出しの失敗確率は高々 $1/m^{10}$．
- 以降の保証は，すべての呼び出しが成功したという条件のもとで示す．

</v-clicks>

---

# 問題の定式化

<v-clicks>

本研究では，汎用SDPソルバーの代替として，MWUに基づく組合せ的アルゴリズムを提案する．

<div class="callout">

- **入力**：連結無向グラフ $G=(V,E)$（$n$ 頂点，$m$ 辺）と $\varepsilon>0$
- **出力**：次を満たす $\boldsymbol{w}\in\Delta_E$

$$
\lambda_2(L(\boldsymbol{w})) \ge (1-\varepsilon)\cdot \mathsf{OPT}
$$

（確率 $2/3$）．ただし $\mathsf{OPT}=\max_{w\in\Delta_E}\lambda_2(L(w))$．

</div>

また，$1/n^3\le \mathsf{OPT}\le 2/(n-1)$ が成り立つ．

</v-clicks>

---

# 主結果

<v-clicks>

既知の範囲 $1/n^3\le\mathsf{OPT}\le 2/(n-1)$ のもとで，次が成り立つ．

<div class="callout-strong">

**定理**（主定理）

任意の $\varepsilon>0$ に対し，辺数 $m$ の連結グラフを入力とし，確率少なくとも $2/3$ で $(1-\varepsilon)$-近似解 $\bar{w}\in\Delta_E$ を出力するアルゴリズム $A_1$ が存在する．計算量は

$$
\widetilde{O}\!\left(\frac{m}{\varepsilon^3\mathsf{OPT}}\right)
\quad\bigl(\varepsilon\text{ が定数なら }\widetilde{O}(m/\mathsf{OPT})\bigr).
$$

</div>

- 切除平面法 <a href="https://arxiv.org/abs/2004.04250" target="_blank" rel="noopener">&#91;Jiang et al. 2020&#93;</a> に基づく手法を $A_2$ とおくと，計算量は $\widetilde{O}(m^3\mathrm{polylog}(1/\varepsilon))$．
- $\mathsf{OPT}=\Omega(1/m^{2})$ のとき $A_1$ が高速．$\mathsf{OPT}$ がこれより小さいとき $A_2$ が高速となり得る．
- $P_n$ では $A_2$ が優位．直径 $O(1)$ の密グラフ（完全グラフ・完全二部グラフなど）では $A_1$ が優位．

</v-clicks>

---
layout: two-cols
layoutClass: gap-4
---

# 計算量の比較

<div class="text-sm leading-relaxed pr-2">

<v-clicks>

- $(x,y)=(\log_n m,\log_n\mathsf{OPT})$ に応じて最速アルゴリズムが変化する．

- **例：パス** $P_n$：
  $m=\Theta(n)$，$\mathsf{OPT}=\Theta(n^{-3})$
  → $A_2$ が高速
  （$\widetilde{O}(n^3)$ vs. $\widetilde{O}(n^4)$）

- **例：直径 $O(1)$／密グラフ**：
  $\mathsf{OPT}=\Omega(n^{-2})$ より
  $A_1$ は $\widetilde{O}(mn^2)$．
  密グラフ（$m=\Theta(n^2)$）では $A_1$ が優位．

</v-clicks>

</div>

::right::

<div class="flex flex-col items-center justify-center h-full">
  <ComplexityRegions />
  <p class="caption mt-2 text-center">
    $A_1$（ティール）と $A_2$（オレンジ）の優位領域<br>
    $(x,y)=(\log_n m,\log_n\mathsf{OPT})$
  </p>
</div>

---
layout: section
---

# アルゴリズム：MWU

---

# FiedlerベクトルとMWU更新

<v-clicks>

現在の重み $w^{(t)}$ に対する内側の最小化問題を考える：

$$
\min_{x\in X} x^\top L(w^{(t)})x.
$$

- この最小値を与えるベクトルは $L(w^{(t)})$ のFiedlerベクトルであり，本アルゴリズムではその $\alpha$-近似 $x^{(t)}\in X$ を用いる．
- $x^{(t)}$ を固定すると，目的値は辺重みに関して線形である：

$$
(x^{(t)})^\top L(w)x^{(t)}
=
\sum_{e=\{u,v\}\in E} w_e (x_u^{(t)}-x_v^{(t)})^2 .
$$

- したがって，各辺の係数
$\ell_t(e):=(x_u^{(t)}-x_v^{(t)})^2/4$
をMWUの利得として用いる（$0\le \ell_t(e)\le1$）．
- 乗法更新は，この利得が大きい辺により大きな重みを与える．

</v-clicks>

---

# アルゴリズム $A_1$

<v-clicks>

$\max_{w\in\Delta_E}\min_{x\in X}x^\top L(w)x$
に対し，MWUを適用する <a href="https://theoryofcomputing.org/articles/v008a006/" target="_blank" rel="noopener">&#91;Arora–Hazan–Kale 2012&#93;</a>．

<div class="callout text-sm leading-relaxed">

**アルゴリズム $A_1$**

**入力：** 連結グラフ $G=(V,E)$，パラメータ $\alpha>0$，$0<\eta\le 1/2$，$T\in\mathbb{N}$

**出力：** 辺重み $\bar{w}\in\Delta_E$

1. 各 $e\in E$ に対し $w_e^{(0)}\leftarrow 1/|E|$ とおく．
2. $t=0,1,\dots,T-1$ について：
   1. $L(w^{(t)})$ の $\alpha$-近似Fiedlerベクトル $x^{(t)}\in X$ を求める．
   2. 各辺 $e=\{u,v\}$ に対し $\ell_t(e)\leftarrow (x_u^{(t)}-x_v^{(t)})^2/4$ とおく．
   3. 各 $e\in E$ に対し
   $$
   w_e^{(t+1)}
   \leftarrow
   \frac{w_e^{(t)}\exp\bigl(\eta\,\ell_t(e)\bigr)}
   {\sum_{a\in E}w_a^{(t)}\exp\bigl(\eta\,\ell_t(a)\bigr)}
   $$
   と更新する．
3. $\bar{w}\leftarrow\dfrac{1}{T}\sum_{t=0}^{T-1}w^{(t)}$ を返す．

</div>

</v-clicks>

---

# 解析：local norm technique による改善

<v-clicks>

通常のMWUの解析では，必要な反復回数は $O(1/\mathsf{OPT}^2)$ である．

<div class="callout-alert">

**local norm technique** <a href="https://doi.org/10.1561/2200000018" target="_blank" rel="noopener">&#91;Shalev-Shwartz 2012&#93;</a>

local norm technique を用いると，反復回数を $O(1/\mathsf{OPT})$ まで減らせる．
定数 $\varepsilon$ のもとでは，計算量 $\widetilde{O}(m/\mathsf{OPT})$ が得られる．

</div>

<div class="callout">

**補題**

$0<\eta\le1/2$ のとき，任意の $u\in\Delta_E$ に対して

$$
\sum_{t=0}^{T-1}\langle w^{(t)},\ell_t\rangle
\ge (1-\eta)\sum_{t=0}^{T-1}\langle u,\ell_t\rangle - \tfrac{\log m}{\eta}.
$$

とくに最適解 $w^\star$ に対して $u=w^\star$ とおくと，$\langle w^\star,\ell_t\rangle\ge\mathsf{OPT}/4$ が成り立つ．

</div>

</v-clicks>

---

# 近似保証

<v-clicks>

<div class="callout">

**補題**

任意の $\alpha>0$，$0<\eta\le1/2$，$T\in\mathbb{N}$ に対し，出力 $\bar{w}$ は確率 $1-T/m^{10}$ 以上で

$$
\lambda_2(L(\bar{w}))
\ge
\frac{(1-\eta)\mathsf{OPT}-4\log m/(\eta T)}{1+\alpha}
$$

を満たす．また，1反復あたりの計算時間は $\widetilde{O}(m/\alpha)$ である．

</div>

- オラクルを $T$ 回呼び出すため，いずれかが失敗する確率は高々 $T\cdot(1/m^{10})$．
- 主定理の確率 $2/3$：アルゴリズム全体の成功を「定数確率で成功する」と述べたもの．必要なら独立反復で $1-\delta$ まで上げられ，計算量は $O(\log(1/\delta))$ 倍で済む．

下界 $\gamma\le\mathsf{OPT}$ が既知の場合：

$$
\alpha=\eta=\varepsilon/4,\qquad
T=\Bigl\lceil\frac{64\log m}{\varepsilon^2\gamma}\Bigr\rceil
\;\Longrightarrow\;
\lambda_2(L(\bar{w}))\ge (1-\varepsilon)\mathsf{OPT}.
$$

</v-clicks>

---

# 未知の $\mathsf{OPT}$：Doubling Trick

<v-clicks>

前スライドの設定は，下界 $\gamma\le \mathsf{OPT}$ が既知であることを仮定していた：

$$
\alpha=\eta=\varepsilon/4,\qquad
T=\left\lceil\frac{64\log m}{\varepsilon^2\gamma}\right\rceil .
$$

<div class="callout">

**Doubling Trick**

1. 既知の範囲 $1/n^3\le \mathsf{OPT}\le 2/(n-1)$ を用いる．
2. $\gamma_0=2/(n-1)$ から始め，$\gamma$ を $\gamma\leftarrow \gamma/(1+\varepsilon/2)$ と順に小さくする．
3. 各 $\gamma$ に対して上のパラメータでアルゴリズムを実行し，
   $\lambda_2(L(\bar{w}))\ge(1-\varepsilon)\gamma$ が確認できた最初の出力を返す．

</div>

- 繰り返しのある時点で $\gamma\le\mathsf{OPT}\le(1+\varepsilon/2)\gamma$ となるため，その時点で $\gamma=\Theta(\mathsf{OPT})$ である．
- よって全体の計算量は $\widetilde{O}(m/(\varepsilon^3\mathsf{OPT}))$ である．

</v-clicks>

---
layout: two-cols
layoutClass: gap-4
---

# まとめ

<div class="text-sm leading-relaxed pr-2">

<v-clicks>

<div class="callout-alert">

**主結果**

任意の $\varepsilon>0$ に対し，辺数 $m$ の連結グラフを入力とし，
確率少なくとも $2/3$ で辺重み $\bar{w}\in\Delta_E$ を出力する
MWUに基づくアルゴリズムが存在し，

$$
\lambda_2\bigl(L(\bar{w})\bigr)
\;\ge\;
(1-\varepsilon)\mathsf{OPT}.
$$

計算量は $\widetilde{O}\!\bigl(m/(\varepsilon^3\mathsf{OPT})\bigr)$．

</div>

切除平面法に基づく手法（計算量 $\widetilde{O}(m^3\mathrm{polylog}(1/\varepsilon))$）と比較すると，
$\mathsf{OPT}=\Omega(1/m^2)$ のとき本手法が高速である．
直径 $O(1)$ の密グラフでは本手法が優位．

</v-clicks>

</div>

::right::

<div class="flex flex-col items-center justify-center h-full">
  <ComplexityRegions />
  <p class="caption mt-2 text-center">
    $A_1$（ティール）と $A_2$（オレンジ）の優位領域<br>
    $(x,y)=(\log_n m,\log_n\mathsf{OPT})$
  </p>
</div>

---
layout: center
class: text-center
---

# 参考文献

<div class="text-left text-sm max-w-3xl mx-auto space-y-1.5">

1. <a href="https://dml.cz/handle/10338.dmlcz/101168" target="_blank" rel="noopener">&#91;Fiedler 1973&#93;</a> M. Fiedler. Algebraic connectivity of graphs. *Czechoslovak Math. J.*, 1973.
2. <a href="https://doi.org/10.1080/03081089008817967" target="_blank" rel="noopener">&#91;Fiedler 1990&#93;</a> M. Fiedler. Absolute algebraic connectivity of trees. *Linear Multilinear Algebra*, 1990.
3. <a href="https://web.stanford.edu/~boyd/papers/pdf/fastestmix.pdf" target="_blank" rel="noopener">&#91;Boyd–Diaconis–Xiao 2004&#93;</a> S. Boyd, P. Diaconis, and L. Xiao. Fastest mixing Markov chain on a graph. *SIAM Rev.*, 2004.
4. <a href="https://doi.org/10.1016/j.laa.2006.08.026" target="_blank" rel="noopener">&#91;de Abreu 2007&#93;</a> N. M. M. de Abreu. Old and new results on algebraic connectivity of graphs. *Linear Algebra Appl.*, 2007.
5. <a href="https://doi.org/10.1137/130915984" target="_blank" rel="noopener">&#91;Spielman–Teng 2014&#93;</a> D. A. Spielman and S.-H. Teng. Nearly linear time algorithms for preconditioning and solving symmetric, diagonally dominant linear systems. *SIAM J. Matrix Anal. Appl.*, 2014.
6. <a href="https://theoryofcomputing.org/articles/v008a006/" target="_blank" rel="noopener">&#91;Arora–Hazan–Kale 2012&#93;</a> S. Arora, E. Hazan, and S. Kale. The multiplicative weights update method: a meta-algorithm and applications. *Theory of Computing*, 2012.
7. <a href="https://arxiv.org/abs/2004.04250" target="_blank" rel="noopener">&#91;Jiang et al. 2020&#93;</a> H. Jiang, Y. T. Lee, Z. Song, and S. C.-W. Wong. An improved cutting plane method for convex optimization, convex-concave games, and its applications. *STOC*, 2020.
8. <a href="https://doi.org/10.1561/2200000018" target="_blank" rel="noopener">&#91;Shalev-Shwartz 2012&#93;</a> S. Shalev-Shwartz. Online learning and online convex optimization. *Found. Trends Mach. Learn.*, 2012.
9. <a href="https://doi.org/10.1007/BF01787694" target="_blank" rel="noopener">&#91;Mohar 1991&#93;</a> B. Mohar. Eigenvalues, diameter, and mean distance of graphs. *Graphs Combin.*, 1991.

</div>

---
layout: section
---

# 補足：$\mathsf{OPT}$ の上下界

---

# $\mathsf{OPT}$ の上下界

<v-clicks>

Doubling Trick では，次の既知の範囲を用いた：

$$
\frac{1}{n^3}
\;\le\;
\mathsf{OPT}
\;\le\;
\frac{2}{n-1}.
$$

- **上界**：$\mathsf{OPT}\le 2/(n-1)$．
  トレース $\mathrm{Tr}(L(w^\star))=2$ と，$\lambda_2$ が残り固有値の平均以下であることから従う．
- **下界**：直径 $D=\mathrm{diam}(G)$ に対し
  $\mathsf{OPT}\ge 2/(n(n-1)D)$．
  とくに $D\le n-1$ より $\mathsf{OPT}=\Omega(1/n^3)$．

</v-clicks>

---

# 上界：$\mathsf{OPT}\le 2/(n-1)$

<v-clicks>

<div class="callout">

**命題**（上界）

任意の $n$ 頂点連結グラフに対し，
$\displaystyle\mathsf{OPT}\le\frac{2}{n-1}$．

</div>

**証明の概略.**
最適解を $w^\star\in\Delta_E$ とする．
$L(w^\star)$ は半正定値で $\lambda_1=0$ だから，

$$
\sum_{i=2}^{n}\lambda_i(L(w^\star))
=
\mathrm{Tr}(L(w^\star))
=
2\sum_{e\in E}w^\star_e
=
2.
$$

$\lambda_2$ は $\lambda_2,\dots,\lambda_n$ のうち最小なので，その平均以下である：

$$
\mathsf{OPT}=\lambda_2(L(w^\star))\le\frac{2}{n-1}.
$$

</v-clicks>

---

# 下界：直径による評価

<v-clicks>

<div class="callout">

**命題**（下界）

直径 $D=\mathrm{diam}(G)$（$1\le D\le n-1$）の $n$ 頂点グラフに対し，
$\displaystyle\mathsf{OPT}\ge\frac{2}{n(n-1)D}=\Omega\!\left(\frac{1}{n^2 D}\right)$．

</div>

根 $r$ に関する BFS木 $T=(V,E_T)$ をとり，木辺へ一様重みを配分する：

$$
w^T_e
:=
\begin{cases}
1/(n-1) & (e\in E_T),\\
0 & (e\notin E_T)
\end{cases}
\qquad
\bigl(w^T\in\Delta_E\bigr),\qquad
L(w^T)=\tfrac{1}{n-1}L(T).
$$

三角不等式より $\mathrm{diam}(T)\le 2D$．さらに
$\lambda_2(L(T))\ge 4/(n\,\mathrm{diam}(T))$ <a href="https://doi.org/10.1007/BF01787694" target="_blank" rel="noopener">&#91;Mohar 1991&#93;</a>
より

$$
\lambda_2(L(w^T))
\ge
\frac{1}{n-1}\cdot\frac{4}{n\cdot\mathrm{diam}(T)}
\ge
\frac{2}{n(n-1)D}.
$$

よって $\mathsf{OPT}\ge\lambda_2(L(w^T))$．
とくに $D\le n-1$ より $\mathsf{OPT}=\Omega(1/n^3)$ であり，Doubling Trick では $1/n^3$ を用いる．

</v-clicks>
