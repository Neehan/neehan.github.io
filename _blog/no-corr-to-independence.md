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

It is a well-known fact in probability that zero correlation does not always imply independence. The standard counterexample is trivial: let $X$ be a standard normal random variable and $Y = X^2$. Then $X$ and $Y$ are clearly dependent, since knowing $X$ determines $Y$ completely, yet by symmetry $\operatorname{Cov}(X,Y) = \mathbb E[X^3] = 0$. Independence is a much stronger condition than mere uncorrelatedness.

But what if we ask for more than just zero correlation? Instead of requiring only that $\operatorname{Cov}(X,Y) = 0$, suppose we require that $\operatorname{Cov}(X^m,Y^n) = 0$ for every pair of positive integers $m$ and $n$. This is a vastly stronger condition. It says that not only are $X$ and $Y$ uncorrelated, but so are all powers of $X$ with all powers of $Y$. In this note I will show that under certain regularity conditions, this stronger property does in fact imply independence. Specifically, if two random variables have exponentially decaying tails and all their mixed polynomial covariances vanish, then they must be independent. This result applies to many common families of distributions including Gaussian, Gamma, and Exponential.

**Notations.** We work throughout on a fixed probability space $(\Omega,\mathcal F,\mathbb P)$. All random variables are real-valued and defined on this space. For a random variable $X:\Omega\to\mathbb R$, we denote its [law](https://en.wikipedia.org/wiki/Probability_distribution) by $\mu\_X$, the pushforward of $\mathbb P$ under $X$. The space $L^2(\mathbb P\_X)$ means $L^2(\mathbb R,\mathcal B(\mathbb R),\mu\_X)$, and we freely identify a function $f\in L^2(\mathbb P\_X)$ with the random variable $f(X)\in L^2(\mathbb P)$.

## What Independence Means

Two random variables $X$ and $Y$ are independent if their joint distribution factorizes: 

$$
\mathbb P(X\in A, Y\in B) = \mathbb P(X\in A)\cdot\mathbb P(Y\in B)
$$ 

for all Borel sets $A,B\subset\mathbb R$. This is equivalent to expectation factorizing for all bounded measurable functions: 

$$
\mathbb E[f(X)g(Y)] = \mathbb E[f(X)]\cdot\mathbb E[g(Y)].
$$ 

To see the equivalence, note that if the joint law factorizes then integrating $f(x)g(y)$ against $\mu\_X\otimes\mu\_Y$ gives factorization by [Fubini's theorem](https://en.wikipedia.org/wiki/Fubini%27s_theorem). Conversely, taking $f=\mathbf 1\_A$ and $g=\mathbf 1\_B$ recovers the probabilistic definition.

The zero covariance only gives this condition for linear $f$ and $g$, which is vastly inferior to what is required. 

## The Main Theorem

**Theorem.** *Let $X$ and $Y$ be real random variables such that*
1. *$\mathbb P(\|X\|>t) \le C\_X e^{-c\_X t}$ and $\mathbb P(\|Y\|>t) \le C\_Y e^{-c\_Y t}$ for all $t>0$ and some constants $C\_X,c\_X,C\_Y,c\_Y>0$,*
2. *$\operatorname{Cov}(X^m,Y^n) = 0$ for all positive integers $m,n$.*

*Then $X$ and $Y$ are independent.*

## Proof Strategy

Our covariance hypothesis gives us $\mathbb E[X^m Y^n] = \mathbb E[X^m]\mathbb E[Y^n]$ for monomials, which extends by linearity to all polynomials: $\mathbb E[p(X)q(Y)] = \mathbb E[p(X)]\mathbb E[q(Y)]$. To prove independence, we need to extend this to all bounded measurable functions. One idea is to approximate all such functions with polynomials. This requires a stronger assumption as I will explain later.

First note that, on compact sets, polynomials are dense in measurable functions. This follows from [Weierstrass approximation](https://en.wikipedia.org/wiki/Stone%E2%80%93Weierstrass_theorem) (polynomials dense in continuous) and [Lusin's theorem](https://en.wikipedia.org/wiki/Lusin%27s_theorem) (continuous dense in measurable). So locally we can approximate any measurable function by polynomials in $L^2$ norm, and the factorization passes to general functions via triangle inequality.

However, $X$ and $Y$ live on all of $\mathbb R$ and not just compact sets, so we need to control the tails. The exponential decay ensures all moments are finite and, crucially, allows us to bound the tail contribution. This permits truncation of any $L^2$ function to a compact set with arbitrarily small error. Combining truncation with polynomial density on compact sets gives that polynomials are dense in $L^2(\mu\_X)$ and $L^2(\mu\_Y)$ globally. The factorization then extends to all square-integrable functions by continuity, and applying to indicator functions yields independence.

## Lemma 1: Finite Moments

**Lemma 1.** *Under the exponential tail condition, all moments are finite: $\mathbb E[\|X\|^n] < \infty$ for every $n\ge 1$.*

*Proof.* By the layer cake representation,

$$
\mathbb E[|X|^n]
= \int_0^\infty \mathbb P(|X|^n > t)\,dt
= \int_0^\infty \mathbb P(|X| > t^{1/n})\,dt.
$$

Substituting $u = t^{1/n}$ gives $t = u^n$ and $dt = n u^{n-1} du$, hence

$$
\mathbb E[|X|^n]
= \int_0^\infty \mathbb P(|X| > u)\, n u^{n-1}\,du
\le nC \int_0^\infty u^{n-1} e^{-cu}\,du
= \frac{nC}{c^n}\cdot\Gamma(n) < \infty.
\quad\square
$$

## Lemma 2: Exponential Tails Dominate Polynomials

**Lemma 2.** *Under the exponential tail condition, for any polynomial $p$ of degree $d$ and any $R>0$,*

$$
\int_{|x|>R} |p(x)|^2 d\mu(x) \le C' e^{-cR/2}
$$

*for some constant $C'$ depending on $p$, $C$, and $c$.*

*Proof.* Let $p$ have degree $d$ and let $A = \sum \|a\_i\|$ where $p(x) = \sum a\_i x^i$. Then $\|p(x)\| \le A\|x\|^d$ for $\|x\|>1$. For $R\ge 1$,

$$
\begin{align}
\int_{|x|>R} |p(x)|^2 d\mu(x)
&\le A^2 \int_{|x|>R} |x|^{2d} d\mu(x) \\
&= A^2 \int_R^\infty t^{2d} d\mu(\{|x|>t\}) \\
&\le A^2 \left[ R^{2d}Ce^{-cR} + 2dC\int_R^\infty t^{2d-1} e^{-ct} dt \right]
\end{align}
$$

by integration by parts. Since $t^k e^{-ct} \le C\_k e^{-ct/2}$ for all $k\ge 0$ and $t$ large enough, both terms are $O(e^{-cR/2})$. $\square$

## Lemma 3: Polynomial Density

**Lemma 3.** *Under the exponential tail condition, polynomials are dense in $L^2(\mu)$.*

*Proof.* Fix $f\in L^2(\mu)$ and $\varepsilon>0$. Throughout this proof, $\|\cdot\|$ denotes the $L^2(\mu)$ norm unless otherwise specified.

**Step 1: Truncation.** Define $f\_R(x) = f(x)\mathbf 1\_\{\{\|x\|\le R\}\}$. By dominated convergence, $\|f - f\_R\| \to 0$ as $R\to\infty$. Choose $R\_0$ so that $\|f - f\_\{R\_0\}\| < \varepsilon/3$.

**Step 2: Continuous approximation.** By Lusin's theorem, there exists a continuous function $g$ supported on $[-R\_0,R\_0]$ with $\|f\_\{R\_0\} - g\| < \varepsilon/3$. Since $g$ is continuous on the compact set $[-R\_0,R\_0]$, it is bounded: $\|g\|\_\infty \le M$.

**Step 3: Polynomial approximation.** By Weierstrass approximation, for any $\delta>0$ there exists a polynomial $p$ with $\|g - p\|\_\infty \le \delta$ on $[-R\_0,R\_0]$. By construction, $\|p\|\_\infty \le M + \delta$ on $[-R\_0,R\_0]$, and we may choose $p$ with degree at most $d\_0$ (independent of $\delta$ for $\delta\le 1$). Then

$$
\|g - p\|^2
= \int_{|x|\le R_0} |g - p|^2 d\mu + \int_{|x|>R_0} |p|^2 d\mu
\le \delta^2 + C_0 e^{-cR_0/2},
$$

where $C\_0$ is given by Lemma 2 applied to polynomials of degree $d\_0$ with supremum bound $M+1$. Choose $\delta$ small enough that $\|g-p\| < \varepsilon/3$.

By the triangle inequality, $\|f-p\| < \varepsilon$. $\square$

## Proof of the Main Theorem

By linearity, the assumption extends from monomials to polynomials: $\mathbb E[p(X)q(Y)] = \mathbb E[p(X)]\mathbb E[q(Y)]$. By Lemma 3, polynomials are dense in $L^2(\mu\_X)$ and $L^2(\mu\_Y)$. For $f\in L^2(\mu\_X)$ and $g\in L^2(\mu\_Y)$, choose polynomial sequences $p\_n \to f$ and $q\_n \to g$ in the respective $L^2$ spaces.

We show $\mathbb E[p\_n(X)q\_n(Y)] \to \mathbb E[f(X)g(Y)]$. By Cauchy–Schwarz,

$$
\begin{align}
|\mathbb E[f(X)g(Y) - p_n(X)q_n(Y)]|
&\le \mathbb E[|f(X) - p_n(X)| \cdot |g(Y)|] + \mathbb E[|p_n(X)| \cdot |g(Y) - q_n(Y)|] \\
&\le \mathbb E[|f(X) - p_n(X)|^2]^{1/2} \mathbb E[|g(Y)|^2]^{1/2} \\
&\quad + \mathbb E[|p_n(X)|^2]^{1/2} \mathbb E[|g(Y) - q_n(Y)|^2]^{1/2} \\
&\to 0.
\end{align}
$$

Since $\mathbb E[p\_n(X)q\_n(Y)] = \mathbb E[p\_n(X)]\mathbb E[q\_n(Y)] \to \mathbb E[f(X)]\mathbb E[g(Y)]$, we obtain

$$
\mathbb E[f(X)g(Y)] = \mathbb E[f(X)]\mathbb E[g(Y)]
$$

for all $f\in L^2(\mu\_X)$ and $g\in L^2(\mu\_Y)$. Taking $f=\mathbf 1\_A$ and $g=\mathbf 1\_B$ for Borel sets $A,B$ gives

$$
\mathbb P(X\in A, Y\in B) = \mathbb P(X\in A)\mathbb P(Y\in B).
\quad\square
$$