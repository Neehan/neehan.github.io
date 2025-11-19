---
title: "When Does No Correlation Imply Independence"
excerpt: "Zero correlation does not always imply independence, but under exponential tails and vanishing mixed polynomial covariances, it does. We prove this using moment generating functions."
collection: blog
date: 2025-11-18
tags:
  - math
  - theory
  - proofs
---
*(**Note:** This is a draft. Might contain mistakes)*

It is a well-known fact in probability that zero correlation does not always imply independence. The standard counterexample is trivial: let $X$ be a standard normal random variable and $Y = X^2$. Then $X$ and $Y$ are clearly dependent, since knowing $X$ determines $Y$ completely, yet by symmetry $\operatorname{Cov}(X,Y) = \mathbb E[X^3] = 0$. Independence is a much stronger condition than mere uncorrelatedness.

But what is the relationship between covariance and independence? In this note I will attempt to explain that and also how a stronger covariance relation does in fact imply independence. Specifically, if two random variables have exponentially decaying tails and all their mixed polynomial covariances vanish, then they must be independent. This result applies to many common families of distributions including Gaussian, Gamma, and Exponential.

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

One trick is to use moment generating functions. Our strategy is to show that the joint moment generating function $M\_{(X,Y)}(\theta,\eta) = \mathbb E[e^{\theta X + \eta Y}]$ and the product $M\_X(\theta) M\_Y(\eta)$ both exist and are equal in a neighborhood of zero. By the standard uniqueness theorem for MGFs, this equality forces the distributions to be equal, which gives independence.

The proof proceeds in three steps. First, we show that exponential tails imply the moment generating function has positive radius of convergence. Second, we use the covariance condition to show the joint moment generating function factorizes. Finally, we invoke the standard uniqueness theorem for MGFs to conclude that equality of MGFs implies equality of distributions. 

## Proof of the Main Theorem

The moment generating function (MGF) of a random variable $X$ is defined as $M\_X(\theta) = \mathbb E[e^{\theta X}]$ for all $\theta \in \mathbb R$ where this expectation exists. When finite, the MGF encodes all moments of $X$ via its Taylor expansion: $M\_X(\theta) = \sum\_{n=0}^\infty \frac{\theta^n}{n!}\mathbb E[X^n]$. We will show that exponential tails guarantee the MGF exists in a neighborhood of zero.

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

In particular, for all $\|\theta\| < c\_X$ we have $\mathbb E[e^{\|\theta\||X|}] < \infty$, and by symmetry $\mathbb E[e^{\|\eta\||Y|}] < \infty$ for all $\|\eta\| < c\_Y$. For $\|\theta\| < c\_X$, the Taylor series of the MGF converges by the moment bound:

$$
\begin{align}
M_X(\theta) &= \sum_{n=0}^\infty \frac{\theta^n}{n!}\mathbb E[X^n] \\
&\le \sum_{n=0}^\infty \frac{|\theta|^n}{n!}\mathbb E[|X|^n] \\
&\le C_X\sum_{n=0}^\infty \frac{|\theta|^n}{c_X^n} < \infty.
\end{align}
$$

By Lemma 1 applied to both $X$ and $Y$, we have that $M\_X(\theta)$ converges for $\|\theta\| < c\_X$ and $M\_Y(\eta)$ converges for $\|\eta\| < c\_Y$. We now verify that the joint MGF also converges in a rectangle around the origin. Pick $r\_X,r\_Y > 0$ such that $2r\_X < c\_X$ and $2r\_Y < c\_Y$. For $\|\theta\| < r\_X$ and $\|\eta\| < r\_Y$, by Cauchy-Schwarz we have

$$
\begin{align}
\mathbb E[e^{\theta X + \eta Y}] &\le \mathbb E[e^{|\theta||X|+|\eta||Y|}] \\
&\le \sqrt{\mathbb E[e^{2|\theta||X|}]\mathbb E[e^{2|\eta||Y|}]} < \infty,
\end{align}
$$

where the right side is finite since $2\|\theta\| < c\_X$ and $2\|\eta\| < c\_Y$, so $\mathbb E[e^{2\|\theta\||X|}] < \infty$ and $\mathbb E[e^{2\|\eta\||Y|}] < \infty$ by Lemma 1. Thus the joint MGF $M\_{(X,Y)}(\theta,\eta) = \mathbb E[e^{\theta X + \eta Y}]$ converges for all $\|\theta\| < r\_X$ and $\|\eta\| < r\_Y$. The covariance condition now implies factorization of the joint MGF.

**Lemma 2.** *For $\|\theta\| < r\_X$ and $\|\eta\| < r\_Y$, we have $\mathbb E[e^{\theta X}e^{\eta Y}] = \mathbb E[e^{\theta X}]\mathbb E[e^{\eta Y}]$.*

*Proof.* For $\|\theta\| < r\_X$ and $\|\eta\| < r\_Y$, we may expand $e^{\theta X} = \sum\_{m=0}^\infty \frac{\theta^m}{m!}X^m$ and $e^{\eta Y} = \sum\_{n=0}^\infty \frac{\eta^n}{n!}Y^n$. The partial sums are dominated by the integrable function $e^{\|\theta\|\|X\|+\|\eta\|\|Y\|}$, which has finite expectation as shown above. By dominated convergence,

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

**Lemma 3.** *If $M\_{(X,Y)}(\theta,\eta) = M\_X(\theta)M\_Y(\eta)$ for all $\|\theta\| < r\_X$ and $\|\eta\| < r\_Y$, then $X$ and $Y$ are independent.*

*Proof.* The MGF $M\_X(\theta)$ is analytic in $\|\theta\| < r\_X$, hence extends to a holomorphic function on the complex disk $\|z\| < r\_X$. Setting $\theta = it$ for real $t$ with $\|t\| < r\_X$, we have $M\_X(it) = \phi\_X(t)$, the characteristic function of $X$. Similarly for $Y$. The factorization hypothesis gives $M\_{(X,Y)}(is,it) = M\_X(is)M\_Y(it)$, or equivalently $\phi\_{(X,Y)}(s,t) = \phi\_X(s)\phi\_Y(t)$, for all $\|s\| < r\_X$ and $\|t\| < r\_Y$. Since characteristic functions uniquely determine distributions and $\phi\_{(X,Y)} = \phi\_X \otimes \phi\_Y$ on a nonempty open set, by analytic continuation this holds for all $(s,t) \in \mathbb R^2$. By the inverse Fourier transform,

$$
\begin{align}
\mu_{(X,Y)}(dx,dy) &= \frac{1}{(2\pi)^2}\iint e^{-i(v_1 x + v_2 y)}\phi_{(X,Y)}(v_1,v_2)\,dv_1\,dv_2 \\
&= \frac{1}{2\pi}\int e^{-iv_1 x}\phi_X(v_1)\,dv_1 \cdot \frac{1}{2\pi}\int e^{-iv_2 y}\phi_Y(v_2)\,dv_2 \\
&= \mu_X(dx)\mu_Y(dy),
\end{align}
$$

which is the definition of independence. $\square$

We can now complete the proof of the main theorem. By Lemma 2, for $\|\theta\| < r\_X$ and $\|\eta\| < r\_Y$, the joint MGF satisfies

$$
M_{(X,Y)}(\theta,\eta) = \mathbb E[e^{\theta X + \eta Y}] = \mathbb E[e^{\theta X}e^{\eta Y}] = M_X(\theta)M_Y(\eta).
$$

Now consider the product measure $\mu\_X \otimes \mu\_Y$ on $\mathbb R^2$. The MGF of this product measure is given by

$$
\int_{\mathbb R^2} e^{\theta x + \eta y}\,d(\mu_X \otimes \mu_Y)(x,y) = \int_{\mathbb R} e^{\theta x}\,d\mu_X(x) \cdot \int_{\mathbb R} e^{\eta y}\,d\mu_Y(y) = M_X(\theta)M_Y(\eta).
$$

Therefore $\mu\_{(X,Y)}$ and $\mu\_X \otimes \mu\_Y$ have identical MGFs on the rectangle $\|\theta\| < r\_X$, $\|\eta\| < r\_Y$. By Lemma 3, this implies $\mu\_{(X,Y)} = \mu\_X \otimes \mu\_Y$ and we are done. $\square$