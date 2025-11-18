---
title: "When No Correlation Implies Independence"
excerpt: "Zero correlation does not imply independence, but under exponential tails and vanishing mixed polynomial covariances, it does. We prove this using polynomial density in L² and a continuity argument."
collection: blog
date: 2025-11-18
tags:
  - math
  - theory
  - proofs
---
*(**Note:** This is a draft. Might contain mistakes)*

It is a well-known fact in probability that zero correlation does not always imply independence. The standard counterexample is trivial: let $X$ be a standard normal random variable and $Y = X^2$. Then $X$ and $Y$ are clearly dependent (knowing $X$ determines $Y$ completely), yet by symmetry $\operatorname{Cov}(X,Y) = \mathbb E[X^3] = 0$. Independence is a much stronger condition than mere uncorrelatedness.

In this note, however, I will show that under certain regularity conditions, a stronger version of the zero correlation property does in fact imply independence. Specifically, if two random variables have exponentially decaying tails and zero correlation between all their mixed moments, then they must be independent. This result can be applied to many common families of distributions including Gaussian, Gamma, Exponential and so on.

**Notations.** We work throughout on a fixed probability space $(\Omega,\mathcal F,\mathbb P)$. All random variables are real-valued and defined on this space. For a random variable $X:\Omega\to\mathbb R$, we denote its [law](https://en.wikipedia.org/wiki/Probability_distribution) by $\mu\_X$, the pushforward of $\mathbb P$ under $X$. The space $L^2(\mathbb P\_X)$ means $L^2(\mathbb R,\mathcal B(\mathbb R),\mu\_X)$, and we freely identify a function $f\in L^2(\mathbb P\_X)$ with the random variable $f(X)\in L^2(\mathbb P)$.

## The Main Theorem

**Theorem.** Let $X$ and $Y$ be real random variables on $(\Omega,\mathcal F,\mathbb P)$ such that for some constants $C\_X,c\_X,C\_Y,c\_Y>0$,

$$
\mathbb P(|X|>t) \le C_X e^{-c_X t},\qquad
\mathbb P(|Y|>t) \le C_Y e^{-c_Y t}
\quad\text{for all }t>0.
$$

Assume furthermore that for every pair of positive integers $m,n$,

$$
\operatorname{Cov}(X^m,Y^n)
= \mathbb E[X^m Y^n] - \mathbb E[X^m]\mathbb E[Y^n]
= 0.
$$

Then $X$ and $Y$ are independent.

In other words, exponential tails plus vanishing mixed monomial covariances imply that the joint law of $(X,Y)$ is the product of the marginals.

## What Independence Actually Means

Before proving this theorem, let us recall what independence truly means. Two random variables $X$ and $Y$ are independent if and only if their joint distribution factorizes: for all Borel sets $A,B\subset\mathbb R$,

$$
\mathbb P(X\in A, Y\in B) = \mathbb P(X\in A)\cdot\mathbb P(Y\in B).
$$

This is equivalent to saying that the expectation of the product of any two measurable functions of $X$ and $Y$ factorizes: for all bounded Borel functions $f,g:\mathbb R\to\mathbb R$,

$$
\mathbb E[f(X)g(Y)] = \mathbb E[f(X)]\cdot\mathbb E[g(Y)].
$$

To see this equivalence, note that if the joint distribution factorizes, then for any bounded Borel $f,g$ we have

$$
\mathbb E[f(X)g(Y)] = \int_{\mathbb R^2} f(x)g(y)\,d(\mu_X\otimes\mu_Y)(x,y) = \int_{\mathbb R} f(x)\,d\mu_X(x) \cdot \int_{\mathbb R} g(y)\,d\mu_Y(y) = \mathbb E[f(X)]\cdot\mathbb E[g(Y)].
$$

Conversely, if the factorization holds for all bounded Borel $f,g$, then taking $f=\mathbf 1\_A$ and $g=\mathbf 1\_B$ gives $\mathbb P(X\in A, Y\in B) = \mathbb P(X\in A)\mathbb P(Y\in B)$, which is independence.

This is a tensor product condition: the joint law is the product measure of the marginals. Our covariance condition gives us this factorization only for polynomial functions $f(x) = x^m$ and $g(y) = y^n$. To prove independence, we need to extend this factorization to all measurable functions, or at least to indicator functions of Borel sets.

## Intuition Behind the Proof

The proof strategy is to approximate general measurable functions by polynomials. If we can show that polynomials are dense in the appropriate function space, and if we know that all polynomial pairs satisfy the factorization property, then by a limiting argument the factorization must hold for all functions in that space. This requires two key ingredients.

First, we need polynomial factorization of covariances to extend to arbitrary polynomials, not just monomials. This is straightforward by linearity: if $\mathbb E[X^m Y^n] = \mathbb E[X^m]\mathbb E[Y^n]$ for all monomials, then the same holds for all polynomial pairs $p(X)$ and $q(Y)$ by expanding and using linearity of expectation.

Second, and more importantly, we need polynomials to actually approximate all measurable functions of interest. This is where the exponential tail condition enters. The exponential decay ensures that all moments are finite and that polynomial approximants can be dominated by integrable functions, which allows us to apply the [dominated convergence theorem](https://en.wikipedia.org/wiki/Dominated_convergence_theorem). This domination property is essential for proving that polynomials are dense in $L^2(\mathbb P\_X)$. Once we have this density, we can extend the factorization from polynomials to all square-integrable functions via a continuity argument, and obtain factorization for indicator functions in particular, which gives independence.

## Lemma 1: Exponential Tails Allow Truncation

We first show that exponential tails allow us to approximate any $L^2$ function by compactly supported ones.

**Lemma 1.** Let $\mu$ be a probability measure on $\mathbb R$ with exponentially decaying tail:

$$
\mu(\{|x|>t\}) \le C e^{-ct}\quad\text{for all }t>0,
$$

for some constants $C,c>0$. Then:
1. All moments are finite: $\int\_{\mathbb R} |x|^n\,d\mu(x) < \infty$ for every $n\ge 1$.
2. For any $f\in L^2(\mu)$ and $\varepsilon>0$, there exists $R>0$ such that the truncation $f\_R(x) = f(x)\mathbf 1\_{\{|x|\le R\}}$ satisfies $\|f-f\_R\|_{L^2(\mu)} < \varepsilon$.

*Proof.* For (1), use the layer cake representation: for each integer $n\ge 1$,

$$
\int_{\mathbb R} |x|^n\,d\mu(x)
= \int_0^\infty \mu(\{|x|^n > t\})\,dt
= \int_0^\infty \mu(\{|x| > t^{1/n}\})\,dt.
$$

Change variables $u = t^{1/n}$, so $t = u^n$ and $dt = n u^{n-1} du$:

$$
\int_{\mathbb R} |x|^n\,d\mu(x)
= \int_0^\infty \mu(\{|x| > u\})\, n u^{n-1}\,du
\le nC \int_0^\infty u^{n-1} e^{-cu}\,du
= nC\,\Gamma(n)/c^n < \infty.
$$

For (2), note that

$$
\|f-f_R\|_{L^2(\mu)}^2 = \int_{|x|>R} |f(x)|^2 \, d\mu(x).
$$

Since $|f|^2\in L^1(\mu)$ and the sets $\{|x|>R\}$ decrease to the empty set as $R\to\infty$, the [dominated convergence theorem](https://en.wikipedia.org/wiki/Dominated_convergence_theorem) gives $\|f-f\_R\|_{L^2(\mu)}\to 0$. $\square$

## Lemma 2: Polynomial Approximation on Compact Sets

Next we show that compactly supported measurable functions can be approximated by polynomials.

**Lemma 2.** Let $\mu$ be a finite Borel measure on $\mathbb R$ and $R>0$. For any measurable function $f:\mathbb R\to\mathbb R$ supported in $[-R,R]$ with $f\in L^2(\mu)$, and any $\varepsilon>0$, there exists a polynomial $p$ such that $\|f-p\|_{L^2(\mu)} < \varepsilon$.

*Proof.* First apply [Lusin's theorem](https://en.wikipedia.org/wiki/Lusin%27s_theorem): there exists a continuous function $g:[-R,R]\to\mathbb R$ and a measurable set $E\subset[-R,R]$ with $\mu(E)<\delta$ such that $f(x)=g(x)$ for all $x\in[-R,R]\setminus E$. Extend $g$ to vanish outside $[-R,R]$. Then

$$
\|f - g\|_{L^2(\mu)}^2
= \int_E |f(x) - g(x)|^2\,d\mu(x)
\le \int_E (|f(x)| + |g(x)|)^2\,d\mu(x).
$$

Since $f\in L^2(\mu)$ and $g$ is continuous on a compact set (hence bounded by some $M$), we have

$$
\|f - g\|_{L^2(\mu)}^2 \le 4M^2 \mu(E) < 4M^2\delta.
$$

Choosing $\delta$ small enough gives $\|f - g\|_{L^2(\mu)} < \varepsilon/2$.

Next, apply [Weierstrass approximation](https://en.wikipedia.org/wiki/Stone%E2%80%93Weierstrass_theorem): there exists a polynomial $p$ such that $\sup\_{x\in[-R,R]} |g(x) - p(x)| < \delta'$. Then

$$
\|g-p\|_{L^2(\mu)}^2
\le \delta'^2\,\mu([-R,R])
\le \delta'^2 \mu(\mathbb R).
$$

Choosing $\delta'$ small enough gives $\|g-p\|_{L^2(\mu)} < \varepsilon/2$.

By the triangle inequality, $\|f-p\|_{L^2(\mu)} < \varepsilon$. $\square$

## Proof of the Main Theorem

**Step 1: Polynomial factorization.** By linearity, the assumption $\mathbb E[X^m Y^n] = \mathbb E[X^m]\mathbb E[Y^n]$ extends from monomials to all polynomials: $\mathbb E[p(X)q(Y)] = \mathbb E[p(X)]\mathbb E[q(Y)]$.

**Step 2: Density and continuity.** By Lemmas 1 and 2, polynomials are dense in $L^2(\mu\_X)$ and $L^2(\mu\_Y)$. Take $f\in L^2(\mu\_X)$ and $g\in L^2(\mu\_Y)$, and choose polynomial sequences $p\_n \to f$ in $L^2(\mu\_X)$ and $q\_n \to g$ in $L^2(\mu\_Y)$.

Under a probability measure, $L^2\subset L^1$ with $\|h\|_{L^1} \le \|h\|_{L^2}$ by Cauchy–Schwarz. Thus $p\_n(X)\to f(X)$ and $q\_n(Y)\to g(Y)$ in $L^1(\mathbb P)$. For the product,

$$
\|f(X)g(Y) - p_n(X)q_n(Y)\|_{L^1}
\le \|f(X)\|_{L^2}\,\|g(Y)-q_n(Y)\|_{L^2}
+ \|q_n(Y)\|_{L^2}\,\|f(X)-p_n(X)\|_{L^2}
\to 0,
$$

so $\mathbb E[p\_n(X)q\_n(Y)] \to \mathbb E[f(X)g(Y)]$. Since $\mathbb E[p\_n(X)q\_n(Y)] = \mathbb E[p\_n(X)]\mathbb E[q\_n(Y)]$, we obtain $\mathbb E[f(X)g(Y)] = \mathbb E[f(X)]\mathbb E[g(Y)]$.

**Step 3: Independence.** Apply the factorization to $f=\mathbf 1\_A$ and $g=\mathbf 1\_B$ (bounded, hence in $L^2$):

$$
\mathbb P(X\in A, Y\in B)
= \mathbb E[\mathbf 1_A(X)\mathbf 1_B(Y)]
= \mathbb E[\mathbf 1_A(X)]\,\mathbb E[\mathbf 1_B(Y)]
= \mathbb P(X\in A)\,\mathbb P(Y\in B).
\quad\square
$$