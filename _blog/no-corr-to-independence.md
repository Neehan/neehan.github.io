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

The zero covariance condition $\mathbb E[XY] = \mathbb E[X]\mathbb E[Y]$ only gives factorization for the linear functions, which is vastly weaker than what is required for independence. This explains why the standard counterexample $Y = X^2$ works: we only have factorization for linear functions, not for nonlinear functions like $f(x) = x^2$.

## The Main Theorem

**Theorem.** *Let $X$ and $Y$ be real random variables such that*
1. *$\mathbb P(\|X\|>t) \le C\_X e^{-c\_X t}$ and $\mathbb P(\|Y\|>t) \le C\_Y e^{-c\_Y t}$ for all $t>0$ and some constants $C\_X,c\_X,C\_Y,c\_Y>0$,*
2. *$\operatorname{Cov}(X^m,Y^n) = 0$ for all positive integers $m,n$.*

*Then $X$ and $Y$ are independent.*

## Proof Strategy

Our covariance hypothesis gives us $\mathbb E[X^m Y^n] = \mathbb E[X^m]\mathbb E[Y^n]$ for all positive integers $m,n$, which extends by linearity to all polynomials: $\mathbb E[p(X)q(Y)] = \mathbb E[p(X)]\mathbb E[q(Y)]$. To prove independence, we need to extend this factorization from polynomials to all bounded measurable functions.

The key observation is that if the moments of a distribution do not grow too fast, they uniquely determine the distribution. This is known as [Carleman's condition](https://en.wikipedia.org/wiki/Carleman%27s_condition). A classic counterexample where moments fail to determine the distribution is the [log-normal distribution](https://en.wikipedia.org/wiki/Log-normal_distribution), whose moments grow factorially fast. Our exponential tail condition ensures that moments grow slowly enough for Carleman's condition to hold.

We exploit this using moment generating functions. Our strategy is to show that the joint moment generating function $M\_{(X,Y)}(\theta,\eta) = \mathbb E[e^{\theta X + \eta Y}]$ and the product $M\_X(\theta) M\_Y(\eta)$ both exist and are equal in a neighborhood of zero. Since moment generating functions encode all moments via their Taylor expansions, equality of the moment generating functions implies equality of all moments. By Carleman's condition, this forces the distributions to be equal, which gives independence.

The proof proceeds in three steps. First, we show that exponential tails imply the moment generating function has positive radius of convergence. Second, we use the covariance condition to show the joint moment generating function factorizes. Finally, we verify that Carleman's condition holds under exponential tails, so that moment generating function uniqueness implies distributional uniqueness. 

## Proof of the Main Theorem

The moment generating function (MGF) of a random variable $X$ is defined as $M\_X(\theta) = \mathbb E[e^{\theta X}]$ for all $\theta \in \mathbb R$ where this expectation exists. When finite, the MGF encodes all moments of $X$ via its Taylor expansion: $M\_X(\theta) = \sum\_{n=0}^\infty \frac{\theta^n}{n!}\mathbb E[X^n]$. We will show that exponential tails guarantee the MGF exists in a neighborhood of zero.

**Lemma 1.** *For all $n \ge 0$, we have $\mathbb E[\|X\|^n] \le C\_X n! c\_X^{-n}$. Moreover, $M\_X(\theta)$ is finite for all $\|\theta\| < c\_X/2$.*

*Proof.* For any $n \ge 0$, we have

$$
\mathbb E[|X|^n] = \int_0^\infty nt^{n-1}\mathbb P(|X| > t)\,dt \le \int_0^\infty nt^{n-1}C_Xe^{-c_Xt}\,dt = C_Xn! c_X^{-n} < \infty.
$$

For $\|\theta\| < c\_X/2$, the Taylor series of the MGF converges by the moment bound:

$$
M_X(\theta) = \sum_{n=0}^\infty \frac{\theta^n}{n!}\mathbb E[X^n] \le \sum_{n=0}^\infty \frac{|\theta|^n}{n!}\mathbb E[|X|^n] \le C_X\sum_{n=0}^\infty \frac{|\theta|^n}{c_X^n} < \infty.
$$

By Lemma 1 applied to both $X$ and $Y$, we have that $M\_X$ and $M\_Y$ are finite in neighborhoods of zero. The covariance condition now implies factorization of the joint MGF.

**Lemma 2.** *For $\|\theta\|,\|\eta\|$ sufficiently small, $\mathbb E[e^{\theta X}e^{\eta Y}] = \mathbb E[e^{\theta X}]\mathbb E[e^{\eta Y}]$.*

*Proof.* For $\|\theta\|,\|\eta\|$ sufficiently small, we may expand $e^{\theta X} = \sum\_{m=0}^\infty \frac{\theta^m}{m!}X^m$ and $e^{\eta Y} = \sum\_{n=0}^\infty \frac{\eta^n}{n!}Y^n$. By Lemma 1, the MGFs are finite, so these series converge absolutely in $L^1(\mathbb P)$. By dominated convergence,

$$
\begin{align}
\mathbb E[e^{\theta X}e^{\eta Y}] &= \mathbb E\left[\sum_{m=0}^\infty \frac{\theta^m}{m!}X^m \sum_{n=0}^\infty \frac{\eta^n}{n!}Y^n\right] \\
&= \sum_{m=0}^\infty\sum_{n=0}^\infty \frac{\theta^m\eta^n}{m!n!}\mathbb E[X^m Y^n] \\
&= \sum_{m=0}^\infty\sum_{n=0}^\infty \frac{\theta^m\eta^n}{m!n!}\mathbb E[X^m]\mathbb E[Y^n] \\
&= \mathbb E[e^{\theta X}]\mathbb E[e^{\eta Y}],
\end{align}
$$

where the third equality uses $\operatorname{Cov}(X^m,Y^n) = 0$, which gives $\mathbb E[X^m Y^n] = \mathbb E[X^m]\mathbb E[Y^n]$. $\square$

The final step is to show that factorization of the MGF implies factorization of the distribution. This follows from Carleman's condition.

**Lemma 3.** *The joint distribution $\mu\_{(X,Y)}$ is uniquely determined by its MGF.*

*Proof.* By Lemma 1, the moments satisfy $\mathbb E[\|X\|^n] \le C\_X n! c\_X^{-n}$. This moment bound implies that Carleman's condition holds:

$$
\sum_{n=1}^\infty \left(\mathbb E[|X|^{2n}]\right)^{-1/(2n)} \ge \sum_{n=1}^\infty (C_Xn!c_X^{-n})^{-1/(2n)} \ge \sum_{n=1}^\infty C_X^{-1/(2n)}(n!)^{-1/(2n)}c_X^{1/2} = \infty,
$$

where the divergence follows from [Stirling's approximation](https://en.wikipedia.org/wiki/Stirling%27s_approximation): $(n!)^{-1/(2n)} \sim (ne)^{-1/2}$. The same holds for $Y$. By [Carleman's theorem](https://en.wikipedia.org/wiki/Carleman%27s_condition), the moment sequences of $X$ and $Y$ uniquely determine their distributions, and hence the joint moment sequence uniquely determines $\mu\_{(X,Y)}$. Since the MGF has positive radius of convergence by Lemma 1, it uniquely determines the moments, and therefore uniquely determines the distribution. $\square$

We can now complete the proof of the main theorem. By Lemma 2, for $(\theta,\eta)$ in a neighborhood of $(0,0)$, the joint MGF satisfies

$$
M_{(X,Y)}(\theta,\eta) = \mathbb E[e^{\theta X + \eta Y}] = \mathbb E[e^{\theta X}e^{\eta Y}] = M_X(\theta)M_Y(\eta).
$$

Now consider the product measure $\mu\_X \otimes \mu\_Y$ on $\mathbb R^2$. The MGF of this product measure is given by

$$
\int_{\mathbb R^2} e^{\theta x + \eta y}\,d(\mu_X \otimes \mu_Y)(x,y) = \int_{\mathbb R} e^{\theta x}\,d\mu_X(x) \cdot \int_{\mathbb R} e^{\eta y}\,d\mu_Y(y) = M_X(\theta)M_Y(\eta).
$$

Therefore $\mu\_{(X,Y)}$ and $\mu\_X \otimes \mu\_Y$ have identical MGFs on a neighborhood of $(0,0)$. By Lemma 3, the MGF uniquely determines the distribution, so $\mu\_{(X,Y)} = \mu\_X \otimes \mu\_Y$. This is precisely the definition of independence. $\square$

**Corollary.** *If $X$ satisfies the exponential tail condition $\mathbb P(\|X\|>t) \le Ce^{-ct}$, then polynomials are dense in $L^2(\mu\_X)$.*

*Proof.* By Lemma 1, all moments are finite, so the polynomials $\{1, X, X^2, X^3, \ldots\}$ all lie in $L^2(\mu\_X)$. To show density, suppose $f \in L^2(\mu\_X)$ is orthogonal to all polynomials, meaning $\mathbb E[f(X)X^n] = 0$ for all $n \ge 0$. Let $Y = f(X)$. Then $\mathbb E[Y] = 0$ and $\operatorname{Cov}(X^m, Y) = \mathbb E[X^m Y] - \mathbb E[X^m]\mathbb E[Y] = \mathbb E[X^m Y] = 0$ for all $m \ge 0$. By the main theorem, $X$ and $Y$ are independent. But $Y = f(X)$ is a function of $X$, so the only way they can be independent is if $Y$ is constant almost surely. Since $\mathbb E[Y] = 0$, we have $Y = 0$ almost surely, hence $f = 0$ in $L^2$. $\square$