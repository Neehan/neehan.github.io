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

# When No Correlation Implies Independence

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

## Lemma: Exponential Tails Imply Polynomial Density

We first prove that exponential tails guarantee polynomial approximation in $L^2$.

**Lemma.** Let $X$ be a real random variable whose tail satisfies

$$
\mathbb P(|X|>t) \le C e^{-ct}\quad\text{for all }t>0,
$$

for some constants $C,c>0$. Then all moments $\mathbb E[\|X\|^n]$ are finite, and the set of polynomials in $X$,

$$
\{p(X): p \text{ a real polynomial}\},
$$

is dense in $L^2(\mathbb P\_X)$.

*Proof.* The exponential tail bound immediately implies all moments are finite. Indeed, for each integer $n\ge 1$,

$$
\mathbb E[|X|^n]
= \int_0^\infty \mathbb P(|X|^n > t)\,dt
= \int_0^\infty \mathbb P(|X| > t^{1/n})\,dt.
$$

Making the change of variables $u = t^{1/n}$, so $t = u^n$ and $dt = n u^{n-1} du$, we obtain

$$
\mathbb E[|X|^n]
= \int_0^\infty \mathbb P(|X| > u)\, n u^{n-1}\,du
\le nC \int_0^\infty u^{n-1} e^{-cu}\,du,
$$

which is a finite Gamma integral. Thus $\mathbb E[\|X\|^n]<\infty$ for every $n$.

In particular, every polynomial $p(x) = \sum\_{k=0}^d a\_k x^k$ satisfies $p(X)\in L^2(\mathbb P)$: the square $p(x)^2$ is a polynomial of degree at most $2d$, and its expectation is a finite linear combination of moments $\mathbb E[\|X\|^m]$ with $m\le 2d$. Hence all polynomials belong to $L^2(\mathbb P\_X)$.

We now prove density. Let $\mu = \mu\_X$ be the law of $X$. We can view everything in $L^2(\mathbb R,\mu)$. Take any $f\in L^2(\mu)$ and $\varepsilon>0$.

First, truncate $f$ to a large compact interval. For $R>0$ define

$$
f_R(x) = f(x)\,\mathbf 1_{\{|x|\le R\}}.
$$

Then

$$
\|f-f_R\|_{L^2(\mu)}^2 = \int_{|x|>R} |f(x)|^2 \, d\mu(x).
$$

Since $\|f\|^2\in L^1(\mu)$ and the sets $\{\|x\|>R\}$ decrease to the empty set as $R\to\infty$, the [dominated convergence theorem](https://en.wikipedia.org/wiki/Dominated_convergence_theorem) gives $\|f-f\_R\|_{L^2(\mu)}\to 0$. Choose $R$ large enough that

$$
\|f-f_R\|_{L^2(\mu)} < \varepsilon/3.
$$

Next, approximate $f\_R$ by a continuous function with compact support. The measure $\mu$ restricted to $[-R,R]$ is a finite Borel measure on a compact metric space. It is a standard fact from measure theory that continuous functions on $[-R,R]$ are dense in $L^2([-R,R],\mu)$: simple functions are dense, and each indicator of a measurable set can be approximated in $L^2$ by continuous functions using regularity of finite Borel measures. Therefore there exists a continuous function $g$ supported in $[-R,R]$ such that

$$
\|f_R - g\|_{L^2(\mu)} < \varepsilon/3.
$$

Now apply [Weierstrass approximation](https://en.wikipedia.org/wiki/Stone%E2%80%93Weierstrass_theorem) to $g$. On the compact interval $[-R,R]$, the Weierstrass approximation theorem guarantees that for any $\delta>0$, there exists a polynomial $p$ such that

$$
\sup_{|x|\le R} |g(x) - p(x)| < \delta.
$$

Since $g$ vanishes outside $[-R,R]$, we have

$$
\|g-p\|_{L^2(\mu)}^2
= \int_{|x|\le R} |g(x)-p(x)|^2\,d\mu(x)
\le \delta^2\,\mu([-R,R])
\le \delta^2,
$$

because $\mu$ is a probability measure. Choosing $\delta = \varepsilon/3$ gives $\|g-p\|_{L^2(\mu)} < \varepsilon/3$.

Combining these approximations and using the triangle inequality in $L^2$,

$$
\|f - p\|_{L^2(\mu)}
\le \|f - f_R\|_2 + \|f_R - g\|_2 + \|g - p\|_2
< \frac{\varepsilon}{3} + \frac{\varepsilon}{3} + \frac{\varepsilon}{3}
= \varepsilon.
$$

Thus for any $f\in L^2(\mu)$ and any $\varepsilon>0$, we can find a polynomial $p$ such that $\|f-p\|_{L^2(\mu)}<\varepsilon$. This shows that polynomials are dense in $L^2(\mathbb P\_X)$. $\square$

## Proof of the Main Theorem

We now prove that exponential tails combined with vanishing polynomial covariances force independence.

The exponential tail assumption implies all moments of $X$ and $Y$ exist. Indeed, we applied this argument in the lemma to $X$, and the same applies to $Y$. In particular, every polynomial in $X$ lies in $L^2(\mathbb P\_X)$, and every polynomial in $Y$ lies in $L^2(\mathbb P\_Y)$. By the lemma, polynomials are dense in $L^2(\mathbb P\_X)$ and $L^2(\mathbb P\_Y)$.

The covariance assumption can be rewritten directly as

$$
\mathbb E[X^m Y^n] = \mathbb E[X^m]\mathbb E[Y^n]
\quad\text{for all integers }m,n\ge 1.
$$

For $m=0$ or $n=0$, this equality is trivial, since $X^0=Y^0=1$ and $\mathbb E[1]=1$. Thus in fact

$$
\mathbb E[X^m Y^n] = \mathbb E[X^m]\mathbb E[Y^n]
\quad\text{for all integers }m,n\ge 0.
$$

Let $p(x) = \sum\_{i=0}^k a\_i x^i$ and $q(y) = \sum\_{j=0}^\ell b\_j y^j$ be arbitrary polynomials. Using linearity of expectation and the identity for monomials, we obtain

$$
\mathbb E[p(X)q(Y)]
= \mathbb E\Big[\sum_{i,j} a_i b_j X^i Y^j\Big]
= \sum_{i,j} a_i b_j\,\mathbb E[X^i Y^j]
= \sum_{i,j} a_i b_j\,\mathbb E[X^i]\mathbb E[Y^j].
$$

On the other hand,

$$
\mathbb E[p(X)]\mathbb E[q(Y)]
= \Big(\sum_i a_i\,\mathbb E[X^i]\Big)\Big(\sum_j b_j\,\mathbb E[Y^j]\Big)
= \sum_{i,j} a_i b_j\,\mathbb E[X^i]\mathbb E[Y^j].
$$

Therefore

$$
\mathbb E[p(X)q(Y)] = \mathbb E[p(X)]\,\mathbb E[q(Y)]
$$

for all polynomials $p$ and $q$.

We now extend this factorization from polynomials to all square-integrable functions of $X$ and $Y$. Let $L^2\_X\subset L^2(\mathbb P)$ denote the closed subspace consisting of all random variables of the form $f(X)$ with $f\in L^2(\mathbb P\_X)$; similarly, let $L^2\_Y$ be the subspace of all $g(Y)$ with $g\in L^2(\mathbb P\_Y)$. The lemma, applied separately to $X$ and $Y$, implies that the sets $\{p(X): p\text{ polynomial}\}$ and $\{q(Y): q\text{ polynomial}\}$ are dense in $L^2\_X$ and $L^2\_Y$ respectively.

Take arbitrary $f\in L^2(\mathbb P\_X)$ and $g\in L^2(\mathbb P\_Y)$. Consider them as random variables $f(X)\in L^2\_X$ and $g(Y)\in L^2\_Y$. Since polynomials are dense, for each $n$ we can choose polynomials $p\_n,q\_n$ such that

$$
\|f(X) - p_n(X)\|_{L^2(\mathbb P)} \to 0,\qquad
\|g(Y) - q_n(Y)\|_{L^2(\mathbb P)} \to 0
\quad\text{as }n\to\infty.
$$

We may choose these approximations in a norm-nonincreasing way, for example by letting $p\_n(X)$ be the orthogonal projection of $f(X)$ onto the finite-dimensional subspace $\operatorname{span}\{1,X,\dots,X^n\}\subset L^2\_X$, and similarly for $q\_n(Y)$. Orthogonal projections in a Hilbert space do not increase norms, so

$$
\|p_n(X)\|_{L^2(\mathbb P)} \le \|f(X)\|_{L^2(\mathbb P)},\qquad
\|q_n(Y)\|_{L^2(\mathbb P)} \le \|g(Y)\|_{L^2(\mathbb P)}
$$

for all $n$. In particular, the sequences $(\|p\_n(X)\|\_2)$ and $(\|q\_n(Y)\|\_2)$ are bounded.

We compare expectations using the fact that convergence in $L^2$ implies convergence in $L^1$ under a probability measure. For any $h\in L^2(\mathbb P)$, [Cauchy–Schwarz](https://en.wikipedia.org/wiki/Cauchy%E2%80%93Schwarz_inequality) yields

$$
\|h\|_{L^1(\mathbb P)} \le \|h\|_{L^2(\mathbb P)}.
$$

Thus $p\_n(X)\to f(X)$ in $L^1$ and $q\_n(Y)\to g(Y)$ in $L^1$, which implies

$$
\mathbb E[p_n(X)] \to \mathbb E[f(X)],\qquad
\mathbb E[q_n(Y)] \to \mathbb E[g(Y)].
$$

We also need convergence of the mixed expectations. Write

$$
f(X)g(Y) - p_n(X)q_n(Y)
= f(X)(g(Y)-q_n(Y)) + q_n(Y)(f(X)-p_n(X)).
$$

Taking $L^1$ norms and using Cauchy–Schwarz,

$$
\|f(X)g(Y) - p_n(X)q_n(Y)\|_{L^1}
\le \|f(X)\|_{L^2}\,\|g(Y)-q_n(Y)\|_{L^2}
+ \|q_n(Y)\|_{L^2}\,\|f(X)-p_n(X)\|_{L^2}.
$$

The first term tends to zero because $\|g(Y)-q\_n(Y)\|\_2\to0$. The second term tends to zero because $\|f(X)-p\_n(X)\|\_2\to0$ and $\|q\_n(Y)\|\_2$ is bounded. Therefore

$$
\|f(X)g(Y) - p_n(X)q_n(Y)\|_{L^1(\mathbb P)} \to 0,
$$

and hence

$$
\mathbb E[p_n(X)q_n(Y)] \to \mathbb E[f(X)g(Y)].
$$

For each fixed $n$, we already have

$$
\mathbb E[p_n(X)q_n(Y)] = \mathbb E[p_n(X)]\,\mathbb E[q_n(Y)]
$$

because $p\_n$ and $q\_n$ are polynomials and we have established factorization for polynomials. Passing to the limit as $n\to\infty$, we obtain

$$
\mathbb E[f(X)g(Y)]
= \lim_{n\to\infty} \mathbb E[p_n(X)q_n(Y)]
= \lim_{n\to\infty} \mathbb E[p_n(X)]\,\mathbb E[q_n(Y)]
= \mathbb E[f(X)]\,\mathbb E[g(Y)].
$$

Thus for every $f\in L^2(\mathbb P\_X)$ and $g\in L^2(\mathbb P\_Y)$,

$$
\mathbb E[f(X)g(Y)] = \mathbb E[f(X)]\,\mathbb E[g(Y)].
$$

Since we work under a probability measure, every bounded measurable function belongs to $L^2$, so the factorization holds in particular for all bounded Borel functions $f$ and $g$. Now fix Borel sets $A,B\subset\mathbb R$ and choose $f=\mathbf 1\_A$, $g=\mathbf 1\_B$. Then

$$
\mathbb P(X\in A, Y\in B)
= \mathbb E[\mathbf 1_A(X)\mathbf 1_B(Y)]
= \mathbb E[\mathbf 1_A(X)]\,\mathbb E[\mathbf 1_B(Y)]
= \mathbb P(X\in A)\,\mathbb P(Y\in B).
$$

This is exactly the definition of independence of $X$ and $Y$. The theorem is proved. $\square$