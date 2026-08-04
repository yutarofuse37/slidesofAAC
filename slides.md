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

本研究ではあるグラフが与えられたときに，各辺に重みを配分し，その重み付きグラフの連結性を最大化する問題を考える．

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

「分断されにくさ」を測り，その値を重み配分によって**最大化**できるか．

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

  - **守備側**：総量1の強度を辺へ配分（$w\in\Delta_E$）
  - **攻撃側**：頂点の引っ張り $x$ を選ぶ<br>
    （$x^\top\mathbf{1}=0$，$\|x\|_2=1$）
  - 利得 $C(x;w)=\sum w_e(x_u-x_v)^2$（弾性エネルギー）
  - 攻撃側は $C$ を最小化し，$\lambda_2(L(w))=\min_x C(x;w)$

  </v-clicks>

  </div>
  <div class="text-center">
    <AttackDefense />
  </div>
</div>

---

# 具体例：パス上の攻撃と守備

<v-click>

パス $1$--$2$--$3$--$4$．攻撃：$x=\bigl(-\tfrac12,-\tfrac12,\tfrac12,\tfrac12\bigr)$．
伸びるのは辺 $\{2,3\}$ のみで，$C(x;w)=w_{23}$．

</v-click>

<div class="diagram-wrap">
  <GraphPath />
</div>

<div class="mt-3 text-sm">
<v-clicks>

- 同じ引っ張りでも，$w_{23}$ の大きさでコストが大きく変わる．
- 守備側は，あらゆる攻撃に対してコストが高くなるよう $w$ を配分したい．

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

- グラフ構造は固定し，辺重みを変数として最適化する．
- 制約は「非負」かつ「総和1」：$w\in\Delta_E$．
- 目的は $\lambda_2(L(w))$ の最大化である．

<div class="callout">

**定義**（Absolute Algebraic Connectivity <a href="https://doi.org/10.1080/03081089008817967" target="_blank" rel="noopener">&#91;Fiedler 1990&#93;</a>）

$$
\begin{aligned}
\text{maximize}\quad & \lambda_2(L(\boldsymbol{w})) \\
\text{subject to}\quad
& \boldsymbol{w} \in \Delta_E := \left\{\boldsymbol{x}\in\mathbb{R}_{\ge 0}^{E} \;\middle|\; \sum_{e\in E}x_e=1\right\}.
\end{aligned}
$$

この最適値を **absolute algebraic connectivity** という．

</div>

</v-clicks>

---

# この問題の難しさ

<v-clicks>

- 目的関数は固有値に基づく非平滑関数であり，解析・計算の両面で取り扱いが難しい．
- SDP定式化は可能であるが，実装負荷および計算負荷が高くなりやすい．
- 汎用ブラックボックス解法ではなく，グラフ構造を直接活用する解法が望まれる．

<div class="callout-alert">

**本研究の目標**

一般グラフに対して，理論保証を伴う簡潔な組合せ的アルゴリズムを構築する．

</div>

</v-clicks>

---
layout: section
---

# 問題設定

---

# 準備：ラプラシアンとFiedlerベクトル

<v-clicks>

無向グラフ $G=(V,E)$ に対し，辺 $e=\{u,v\}$ の辺ラプラシアンを
$L_e := (\chi_u - \chi_v)(\chi_u - \chi_v)^\top$ と定める．

重み $w\in\Delta_E$ に対するラプラシアンは $L(w)=\sum_{e\in E} w_eL_e$ である．

- $L(w)$ の最小固有値は常に $0$（対応固有ベクトルは $\mathbf{1}$）．
- $X := \left\{x \in \mathbb{R}^n \mid x^\top \mathbf{1} = 0, \; \|x\|_2 = 1\right\}$ とおくと：

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

$x\in X$ が
$x^\top L x \le (1+\alpha)\lambda_2(L)$
を満たすとき，$x$ を $\alpha$-近似Fiedlerベクトルという．

</div>

<div class="callout">

**補題**（高速オラクル <a href="https://doi.org/10.1137/130915984" target="_blank" rel="noopener">&#91;Spielman–Teng 2014&#93;</a>）

任意の $\alpha>0$ に対し，確率 $1-\frac{1}{m^{10}}$ 以上で $\alpha$-近似Fiedlerベクトルを出力する
ランダム化アルゴリズムが存在し，計算時間は $\widetilde{O}\!\left(\frac{m}{\alpha}\right)$．

</div>

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

<div class="callout-strong">

**定理**（主定理）

任意の $\varepsilon>0$ に対し，$(1-\varepsilon)$-近似解を確率 $2/3$ で出力するアルゴリズム $A_1$ が存在する：

$$
\widetilde{O}\!\left(\frac{m}{\varepsilon^3\mathsf{OPT}}\right)
\quad\bigl(\varepsilon\text{ が定数なら }\widetilde{O}(m/\mathsf{OPT})\bigr).
$$

</div>

- 比較対象：<a href="https://arxiv.org/abs/2004.04250" target="_blank" rel="noopener">&#91;Jiang et al. 2020&#93;</a> の切除平面法 **$A_2$** → $\widetilde{O}(m^3\mathrm{polylog}(1/\varepsilon))$
- $\mathsf{OPT}\gtrsim 1/m^2$ では $A_1$ が優位，$\mathsf{OPT}$ が小さい領域では $A_2$ が優位となり得る

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
</div>

---
layout: section
---

# アルゴリズム：MWU

---

# 変分的な見方

<v-clicks>

Absolute Algebraic Connectivity は次の鞍点問題として表される：

$$
\mathsf{OPT}
= \max_{\boldsymbol{w} \in \Delta_E} \min_{\boldsymbol{x}\in X} \boldsymbol{x}^\top L(\boldsymbol{w}) \boldsymbol{x}
$$

- $\boldsymbol{x}^\top L(\boldsymbol{w})\boldsymbol{x} = \sum_{e=\{u,v\}} w_e (x_u-x_v)^2$
- 各辺の利得は $(x_u-x_v)^2$ で評価される
- この構造に合わせて **MWU** を適用する <a href="https://theoryofcomputing.org/articles/v008a006/" target="_blank" rel="noopener">&#91;Arora–Hazan–Kale 2012&#93;</a>

</v-clicks>

---

# アルゴリズム $A_1$：近似Fiedlerオラクル付きMWU

<v-clicks>

<div class="callout">

**更新則**

1. 初期化：$w_e^{(0)}=1/m$（一様），学習率 $\eta$，近似度 $\alpha$，反復回数 $T$．
2. 各時刻 $t=0,\dots,T-1$ について：
   1. $L(w^{(t)})$ の $\alpha$-近似Fiedlerベクトル $x^{(t)}$ を求める．
   2. 損失 $\ell_t(e):=(x_u^{(t)}-x_v^{(t)})^2/4$（$0\le\ell_t(e)\le1$）．
   3. 乗法的更新：$w_e^{(t+1)} \propto w_e^{(t)}\exp\bigl(\eta\,\ell_t(e)\bigr)$．
3. 出力は平均 $\bar{w} = \frac{1}{T}\sum_{t=0}^{T-1}w^{(t)}$．

</div>

</v-clicks>

---

# 解析：local norm technique による改善

<v-clicks>

通常のMWUのリグレット解析では，必要な反復回数は $O(1/\mathsf{OPT}^2)$ である．

<div class="callout-alert">

**local norm technique** <a href="https://doi.org/10.1561/2200000018" target="_blank" rel="noopener">&#91;Shalev-Shwartz 2012&#93;</a>

反復回数を $O(1/\mathsf{OPT})$ まで減らせる．
定数 $\varepsilon$ のもとでは $\widetilde{O}(m/\mathsf{OPT})$ が得られる．

</div>

<div class="callout">

**補題**（リグレットバウンド・概略）

$0<\eta\le1/2$ のとき，任意の $u\in\Delta_E$ に対して

$$
\sum_{t=0}^{T-1}\langle w^{(t)},\ell_t\rangle
\ge (1-\eta)\sum_{t=0}^{T-1}\langle u,\ell_t\rangle - \tfrac{\log m}{\eta}.
$$

とくに $u=w^\star$ とおくと，$\langle w^\star,\ell_t\rangle\ge\mathsf{OPT}/4$．

</div>

</v-clicks>

---

# 近似保証

<v-clicks>

<div class="callout">

**補題**

出力 $\bar{w}$ は確率 $1-T/m^{10}$ 以上で

$$
\lambda_2(L(\bar{w}))
\ge
\frac{(1-\eta)\mathsf{OPT}-4\log m/(\eta T)}{1+\alpha}
$$

を満たす．1反復あたり $\widetilde{O}(m/\alpha)$．

</div>

パラメータ例（下界 $\gamma\le\mathsf{OPT}$ が既知）：

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

学習率と反復回数は未知の $\mathsf{OPT}$ に依存する．
そこで **Doubling Trick** により，下界 $\gamma$ を段階的に推定する．

<div class="callout">

**手順**

- 既知の範囲：$1/n^3\le \mathsf{OPT}\le 2/(n-1)$
- $\gamma_0=2/(n-1)$ から始め，
  $\gamma\in\bigl\{\gamma_0,(1+\varepsilon/2)^{-1}\gamma_0,\ldots\bigr\}$
  を順に試し，条件を満たした最初の出力を返す

</div>

- 成功する最小の推定値は $\Theta(\mathsf{OPT})$．失敗試行のコストは等比級数に収まる
- 全体の計算量は $\widetilde{O}(m/(\varepsilon^3\mathsf{OPT}))$（定数 $\varepsilon$ では $\widetilde{O}(m/\mathsf{OPT})$）

</v-clicks>

---

# まとめ

<v-clicks>

- absolute algebraic connectivity に対し，汎用SDPを用いず，組合せ的な近似アルゴリズムを与えた．
- MWU・近似Fiedlerオラクル・local norm technique を組み合わせ，定数 $\varepsilon$ のもとで $\widetilde{O}(m/\mathsf{OPT})$ を達成した．
- $\mathsf{OPT}$ が比較的大きい場合（例：直径 $O(1)$ の密グラフ）には，$A_2$ より有利になり得る．

</v-clicks>

---
layout: center
class: text-center
---

# 参考文献

<div class="text-left text-sm max-w-2xl mx-auto space-y-2">

1. <a href="https://dml.cz/handle/10338.dmlcz/101168" target="_blank" rel="noopener">&#91;Fiedler 1973&#93;</a> M. Fiedler. Algebraic connectivity of graphs. *Czechoslovak Math. J.*, 1973.
2. <a href="https://doi.org/10.1080/03081089008817967" target="_blank" rel="noopener">&#91;Fiedler 1990&#93;</a> M. Fiedler. Absolute algebraic connectivity of trees. *Linear Multilinear Algebra*, 1990.
3. <a href="https://doi.org/10.1137/130915984" target="_blank" rel="noopener">&#91;Spielman–Teng 2014&#93;</a> D. A. Spielman and S.-H. Teng. Nearly linear time algorithms for preconditioning and solving symmetric, diagonally dominant linear systems. *SIAM J. Matrix Anal. Appl.*, 2014.
4. <a href="https://theoryofcomputing.org/articles/v008a006/" target="_blank" rel="noopener">&#91;Arora–Hazan–Kale 2012&#93;</a> S. Arora, E. Hazan, and S. Kale. The multiplicative weights update method: a meta-algorithm and applications. *Theory of Computing*, 2012.
5. <a href="https://arxiv.org/abs/2004.04250" target="_blank" rel="noopener">&#91;Jiang et al. 2020&#93;</a> H. Jiang, Y. T. Lee, Z. Song, and S. C.-W. Wong. An improved cutting plane method for convex optimization, convex-concave games, and its applications. *STOC*, 2020.
6. <a href="https://doi.org/10.1561/2200000018" target="_blank" rel="noopener">&#91;Shalev-Shwartz 2012&#93;</a> S. Shalev-Shwartz. Online learning and online convex optimization. *Found. Trends Mach. Learn.*, 2012.

</div>
