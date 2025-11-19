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

First note that, on compact sets, polynomials are dense in measurable functions. This follows from [Weierstrass approximation](https://en.wikipedia.org/wiki/Stone%E2%80%93Weierstrass\_theorem) (polynomials dense in continuous) and [Lusin's theorem](https://en.wikipedia.org/wiki/Lusin%27s\_theorem) (continuous dense in measurable). So locally we can approximate any measurable function by polynomials in $L^2$ norm, and the factorization passes to general functions by a limiting argument.

However, $X$ and $Y$ live on all of $\mathbb R$ and not just compact sets, so we need to control the tails. The exponential decay ensures all moments are finite and, crucially, allows us to dominate the tail contribution. This permits truncation of any $L^2$ function to a compact set with arbitrarily small error. Combining truncation with polynomial density on compact sets gives that polynomials are dense in $L^2(\mu\_X)$ and $L^2(\mu\_Y)$ globally. The factorization then extends to all square-integrable functions by continuity, and applying to indicator functions yields independence.

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
= nC\,\Gamma(n)/c^n < \infty.
\quad\square
$$

## Lemma 2: Truncation

**Lemma 2.** *Under the exponential tail condition, for any $f\in L^2(\mu)$ and $\varepsilon>0$, there exists $R>0$ such that $\|f-f\_R\|_{L^2(\mu)} < \varepsilon$, where $f\_R(x) = f(x)\mathbf 1\_{\{\|x\|\le R\}}$.*

*Proof.* We have $\|f-f\_R\|_{L^2(\mu)}^2 = \int\_{\|x\|>R} \|f(x)\|^2 \, d\mu(x)$. Since $f\in L^2(\mu)$, we can dominate the integrand by the integrable function $\|f(x)\|^2$. The sets $\{\|x\|>R\}$ decrease to $\emptyset$ as $R\to\infty$, so by the [dominated convergence theorem](https://en.wikipedia.org/wiki/Dominated\_convergence\_theorem), $\|f-f\_R\|_{L^2(\mu)}\to 0$. $\square$

## Lemma 3: Polynomial Density on Compact Sets

**Lemma 3.** *Let $\mu$ be a finite Borel measure on $\mathbb R$ and $R>0$. For any measurable $f:\mathbb R\to\mathbb R$ supported in $[-R,R]$ with $f\in L^2(\mu)$ and any $\varepsilon>0$, there exists a polynomial $p$ such that $\|f-p\|_{L^2(\mu)} < \varepsilon$.*

*Proof.* By Lusin's theorem, there exists a continuous function $g:[-R,R]\to\mathbb R$ and a measurable set $E\subset[-R,R]$ with $\mu(E)<\delta$ such that $f(x)=g(x)$ for all $x\in[-R,R]\setminus E$. Extend $g$ to vanish outside $[-R,R]$. Since $f\in L^2(\mu)$ and $g$ is bounded by some $M$ on the compact set $[-R,R]$, we have

$$
\|f - g\|_{L^2(\mu)}^2
= \int_E |f(x) - g(x)|^2\,d\mu(x)
\le \int_E (|f(x)| + |g(x)|)^2\,d\mu(x)
\le 4M^2 \mu(E) < 4M^2\delta.
$$

Choosing $\delta$ small enough gives $\|f - g\|_{L^2(\mu)} < \varepsilon/2$. By Weierstrass approximation, there exists a polynomial $p$ with $\sup\_{x\in[-R,R]} \|g(x) - p(x)\| < \delta'$, hence $\|g-p\|_{L^2(\mu)}^2 \le \delta'^2 \mu(\mathbb R)$. Choosing $\delta'$ small enough gives $\|g-p\|_{L^2(\mu)} < \varepsilon/2$. By the triangle inequality, $\|f-p\|_{L^2(\mu)} < \varepsilon$. $\square$

## Proof of the Main Theorem

By linearity, the assumption $\mathbb E[X^m Y^n] = \mathbb E[X^m]\mathbb E[Y^n]$ extends from monomials to all polynomials: $\mathbb E[p(X)q(Y)] = \mathbb E[p(X)]\mathbb E[q(Y)]$. We now extend this to all square-integrable functions.

By Lemmas 2 and 3, polynomials are dense in $L^2(\mu\_X)$ and $L^2(\mu\_Y)$. Indeed, given $f\in L^2(\mu\_X)$, Lemma 2 allows us to truncate to a compact set with small error, and Lemma 3 approximates the truncation by polynomials. Take $f\in L^2(\mu\_X)$ and $g\in L^2(\mu\_Y)$, and choose polynomial sequences $p\_n \to f$ in $L^2(\mu\_X)$ and $q\_n \to g$ in $L^2(\mu\_Y)$.

Under a probability measure, $L^2\subset L^1$ with $\|h\|\_\{L^1\} \le \|h\|\_\{L^2\}$ by [Cauchy–Schwarz](https://en.wikipedia.org/wiki/Cauchy%E2%80%93Schwarz\_inequality), so $p\_n(X)\to f(X)$ and $q\_n(Y)\to g(Y)$ in $L^1(\mathbb P)$. For the product, we have

$$
\|f(X)g(Y) - p_n(X)q_n(Y)\|_{L^1}
\le \|f(X)\|_{L^2}\,\|g(Y)-q_n(Y)\|_{L^2}
+ \|q_n(Y)\|_{L^2}\,\|f(X)-p_n(X)\|_{L^2}
\to 0,
$$

hence $\mathbb E[p\_n(X)q\_n(Y)] \to \mathbb E[f(X)g(Y)]$. Since $\mathbb E[p\_n(X)q\_n(Y)] = \mathbb E[p\_n(X)]\mathbb E[q\_n(Y)]$ by polynomial factorization, we obtain $\mathbb E[f(X)g(Y)] = \mathbb E[f(X)]\mathbb E[g(Y)]$ for all $f\in L^2(\mu\_X)$ and $g\in L^2(\mu\_Y)$.

To conclude independence, apply this to $f=\mathbf 1\_A$ and $g=\mathbf 1\_B$ for any Borel sets $A,B\subset\mathbb R$. These indicators are bounded, hence in $L^2$, giving

$$
\mathbb P(X\in A, Y\in B)
= \mathbb E[\mathbf 1_A(X)\mathbf 1_B(Y)]
= \mathbb E[\mathbf 1_A(X)]\,\mathbb E[\mathbf 1_B(Y)]
= \mathbb P(X\in A)\,\mathbb P(Y\in B).
\quad\square
$$