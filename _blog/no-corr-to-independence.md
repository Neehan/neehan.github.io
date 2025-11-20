---
title: "When Does No Correlation Imply Independence?"
excerpt: "Zero correlation does not always imply independence. However, if two random variables have exponentially decaying tails and all their mixed polynomial covariances vanish, then they must be independent. The proof uses moment generating functions to extend polynomial factorization to the full distribution."
collection: blog
date: 2025-11-18
tags:
  - math
  - theory
  - proofs
---
*(**Note:** This is a draft. Might contain mistakes)*

It is a well-known fact in probability that zero correlation does not always imply independence. The standard counterexample is trivial: let $X$ be a standard normal random variable and $Y = X^2$. Then $X$ and $Y$ are clearly dependent, since knowing $X$ determines $Y$ completely, yet by symmetry $\operatorname{Cov}(X,Y) = \mathbb E[X^3] = 0$. Independence is a much stronger condition than mere uncorrelatedness.

But what is the relationship between correlation (more generally, covariance) and independence? In this note I will attempt to explain that and also show how a stronger covariance relation with a regularity condition does  imply independence. Specifically, if two random variables have exponentially decaying tails and all their mixed polynomial covariances vanish, then they must be independent. This result applies to many common families of distributions including Gaussian, Gamma, and Exponential.

**Notations.** We work on a fixed probability space $(\Omega,\mathcal F,\mathbb P)$. For a random variable $X$, we denote its [law](https://en.wikipedia.org/wiki/Probability_distribution) by $\mu\_X$. For a pair $(X,Y)$, $\mu\_{(X,Y)}$ is the joint law and $\mu\_X \otimes \mu\_Y$ is the product measure. The covariance is $\operatorname{Cov}(X,Y) = \mathbb E[XY] - \mathbb E[X]\mathbb E[Y]$, and the moment generating function is $M\_X(\theta) = \mathbb E[e^{\theta X}]$.

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

Our covariance hypothesis gives us $\mathbb E[X^m Y^n] = \mathbb E[X^m]\mathbb E[Y^n]$ for all positive integers $m,n$, which extends by linearity to all polynomials with real coefficients: $\mathbb E[p(X)q(Y)] = \mathbb E[p(X)]\mathbb E[q(Y)]$. To prove independence, we need to extend this factorization from polynomials to all bounded measurable functions.

One trick is to use moment generating functions. Our strategy is to show that the joint moment generating function $M\_{(X,Y)}(\theta,\eta) = \mathbb E[e^{\theta X + \eta Y}]$ and the product $M\_X(\theta) M\_Y(\eta)$ both exist and are equal in a neighborhood of zero. By the standard uniqueness theorem for MGFs, this equality forces the distributions to be equal, which gives independence.

The proof proceeds in three steps. First, we show that exponential tails imply the moment generating function has positive radius of convergence. Second, we use the covariance condition to show the joint moment generating function factorizes. Finally, we invoke the standard uniqueness theorem for MGFs to conclude that equality of MGFs implies equality of distributions. 

## Proof of the Main Theorem

First, we will prove three lemmas.

**Lemma 1.** *For all $n \ge 0$, we have $\mathbb E[\|X\|^n] \le C\_X n! c\_X^{-n}$. Moreover, $M\_X(\theta)$ converges absolutely for all $\|\theta\| < c\_X$.*

*Proof.* For any $n \ge 0$, we have

$$
\begin{align}
\mathbb E[|X|^n] &= \int_0^\infty nt^{n-1}\mathbb P(|X| > t)\,dt \\
&\le \int_0^\infty nt^{n-1}C_Xe^{-c_Xt}\,dt \\
&= C_Xn! c_X^{-n} < \infty.
\end{align}
$$

Thus, for every $\|\lambda\| < c\_X$, we have

$$
\mathbb E[e^{\lambda |X|}] = \sum_{n=0}^\infty \frac{\lambda^n}{n!}\mathbb E[|X|^n] \le C_X\sum_{n=0}^\infty \left(\frac{|\lambda|}{c_X}\right)^n < \infty.
$$

In particular, for all $\|\theta\| < c\_X$ we have $\mathbb E[e^{\|\theta\|\|X\|}] < \infty$, and by symmetry $\mathbb E[e^{\|\eta\|\|Y\|}] < \infty$ for all $\|\eta\| < c\_Y$. For $\|\theta\| < c\_X$, the Taylor series of the MGF converges by the moment bound:

$$
\begin{align}
M_X(\theta) &= \sum_{n=0}^\infty \frac{\theta^n}{n!}\mathbb E[X^n] \\
&\le \sum_{n=0}^\infty \frac{|\theta|^n}{n!}\mathbb E[|X|^n] \\
&\le C_X\sum_{n=0}^\infty \frac{|\theta|^n}{c_X^n} < \infty.
\end{align}
$$

By Lemma 1 applied to both $X$ and $Y$, we have that $M\_X(\theta)$ converges for $\|\theta\| < c\_X$ and $M\_Y(\eta)$ converges for $\|\eta\| < c\_Y$. We now verify that the joint MGF also converges in a rectangle around the origin. For $\|\theta\| < c\_X/2$ and $\|\eta\| < c\_Y/2$, by Cauchy-Schwarz we have

$$
\begin{align}
\mathbb E[e^{\theta X + \eta Y}] &\le \mathbb E[e^{|\theta||X|+|\eta||Y|}] \\
&\le \sqrt{\mathbb E[e^{2|\theta||X|}]\mathbb E[e^{2|\eta||Y|}]} < \infty,
\end{align}
$$

where the right side is finite since $2\|\theta\| < c\_X$ and $2\|\eta\| < c\_Y$, so $\mathbb E[e^{2\|\theta\|\|X\|}] < \infty$ and $\mathbb E[e^{2\|\eta\|\|Y\|}] < \infty$ by Lemma 1. Thus the joint MGF $M\_{(X,Y)}(\theta,\eta) = \mathbb E[e^{\theta X + \eta Y}]$ converges for all $\|\theta\| < c\_X/2$ and $\|\eta\| < c\_Y/2$. The covariance condition now implies factorization of the joint MGF.

**Lemma 2.** *For $\|\theta\| < c\_X/2$ and $\|\eta\| < c\_Y/2$, we have $\mathbb E[e^{\theta X}e^{\eta Y}] = \mathbb E[e^{\theta X}]\mathbb E[e^{\eta Y}]$.*

*Proof.* For $\|\theta\| < c\_X/2$ and $\|\eta\| < c\_Y/2$, we may expand $e^{\theta X} = \sum\_{m=0}^\infty \frac{\theta^m}{m!}X^m$ and $e^{\eta Y} = \sum\_{n=0}^\infty \frac{\eta^n}{n!}Y^n$. The partial sums are dominated by the integrable function $e^{\|\theta\|\|X\|+\|\eta\|\|Y\|}$, which has finite expectation as shown above. By [dominated convergence](https://en.wikipedia.org/wiki/Dominated_convergence_theorem),

$$
\begin{align}
\mathbb E[e^{\theta X}e^{\eta Y}] &= \mathbb E\left[\sum_{m=0}^\infty \frac{\theta^m}{m!}X^m \sum_{n=0}^\infty \frac{\eta^n}{n!}Y^n\right] \\
&= \sum_{m=0}^\infty\sum_{n=0}^\infty \frac{\theta^m\eta^n}{m!n!}\mathbb E[X^m Y^n] \\
&= \sum_{m=0}^\infty\sum_{n=0}^\infty \frac{\theta^m\eta^n}{m!n!}\mathbb E[X^m]\mathbb E[Y^n] \\
&= \mathbb E[e^{\theta X}]\mathbb E[e^{\eta Y}],
\end{align}
$$

where the third equality uses the factorization $\mathbb E[X^m Y^n] = \mathbb E[X^m]\mathbb E[Y^n]$ for all $m,n \ge 0$. For $m,n \ge 1$, this follows from the covariance assumption $\operatorname{Cov}(X^m,Y^n) = 0$. For $m=0$ or $n=0$, the factorization is trivial since $X^0 = Y^0 = 1$, giving $\mathbb E[X^0Y^n] = \mathbb E[Y^n] = \mathbb E[X^0]\mathbb E[Y^n]$ and $\mathbb E[X^mY^0] = \mathbb E[X^m] = \mathbb E[X^m]\mathbb E[Y^0]$. $\square$

The final step is to show that factorization of the MGF implies factorization of the distribution.

**Lemma 3.** *If $M\_{(X,Y)}(\theta,\eta)$ and $M\_X(\theta)M\_Y(\eta)$ both exist and are equal for all $\|\theta\| < c\_X/2$ and $\|\eta\| < c\_Y/2$, then $\mu\_{(X,Y)} = \mu\_X \otimes \mu\_Y$.*

*Proof.* This is a standard result in probability theory. See Khoshnevisan, [*Moment-generating functions and independence*](https://www.math.utah.edu/~davar/math6010/2014/MGFIndependence.pdf), Theorem 6 for a proof. $\square$

We can now complete the proof of the main theorem. By Lemma 2, for $\|\theta\| < c\_X/2$ and $\|\eta\| < c\_Y/2$, we have

$$
M_{(X,Y)}(\theta,\eta) = M_X(\theta)M_Y(\eta).
$$

By Lemma 3, this implies $\mu\_{(X,Y)} = \mu\_X \otimes \mu\_Y$, so $X$ and $Y$ are independent. $\square$