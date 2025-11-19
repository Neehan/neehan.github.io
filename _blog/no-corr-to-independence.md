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

To see the equivalence, note that if the joint law factorizes then integrating $f(x)g(y)$ against $\mu\_X\otimes\mu\_Y$ gives factorization by Fubini's theorem. Conversely, taking $f=\mathbf 1\_A$ and $g=\mathbf 1\_B$ recovers the probabilistic definition.

An important special case is when both $X$ and $Y$ possess moment generating functions $M\_X(\theta) = \mathbb E[e^{\theta X}]$ and $M\_Y(\eta) = \mathbb E[e^{\eta Y}]$ that are finite in neighborhoods of zero. In this case, independence is equivalent to factorization of the joint moment generating function:

$$
M_{(X,Y)}(\theta,\eta) = \mathbb E[e^{\theta X + \eta Y}] = M_X(\theta)M_Y(\eta)
$$

for all $(\theta,\eta)$ in a neighborhood of $(0,0)$. The intuition is that the MGF $M\_X(\theta) = \mathbb E[e^{\theta X}]$ encodes all moments of $X$ via the Taylor expansion $M\_X(\theta) = \sum\_{n=0}^\infty \frac{\theta^n}{n!}\mathbb E[X^n]$. When the MGF is finite in a neighborhood of zero, this power series has positive radius of convergence, which by analyticity uniquely determines the function. Since the distribution is characterized by its moments (under suitable regularity), the MGF uniquely determines the distribution. Thus factorization of the joint MGF implies factorization of the joint distribution.

The zero covariance only gives this condition for linear $f$ and $g$, which is vastly inferior to what is required. 

## The Main Theorem

**Theorem.** *Let $X$ and $Y$ be real random variables such that*
1. *$\mathbb P(\|X\|>t) \le C\_X e^{-c\_X t}$ and $\mathbb P(\|Y\|>t) \le C\_Y e^{-c\_Y t}$ for all $t>0$ and some constants $C\_X,c\_X,C\_Y,c\_Y>0$,*
2. *$\operatorname{Cov}(X^m,Y^n) = 0$ for all positive integers $m,n$.*

*Then $X$ and $Y$ are independent.*

## Proof Strategy

Our covariance hypothesis gives us $\mathbb E[X^m Y^n] = \mathbb E[X^m]\mathbb E[Y^n]$ for monomials, which extends by linearity to all polynomials: $\mathbb E[p(X)q(Y)] = \mathbb E[p(X)]\mathbb E[q(Y)]$. To prove independence, we need to extend this to all bounded measurable functions. 

## Proof of the Main Theorem

We first establish that exponential tails guarantee the existence of moment generating functions.

**Lemma 1.** *If $\mathbb P(\|X\|>t) \le Ce^{-ct}$ for all $t>0$, then the moment generating function $M\_X(\theta) = \mathbb E[e^{\theta X}]$ is finite for all $\|\theta\| < c/2$.*

*Proof.* Fix $\|\theta\| < c/2$. Then

$$
\begin{align}
\mathbb E[e^{\theta X}] &= \mathbb E[e^{\theta X}\mathbf 1_{\|X\|\le 1}] + \mathbb E[e^{\theta X}\mathbf 1_{\|X\|> 1}] \\
&\le e^{\|\theta\|} + \int_1^\infty e^{\theta t}\,d\mathbb P(\|X\| > t) \\
&\le e^{\|\theta\|} + C\int_1^\infty e^{\theta t}ce^{-ct}\,dt \\
&= e^{\|\theta\|} + Cc\int_1^\infty e^{-t(c-\|\theta\|)}\,dt < \infty. \quad \square
\end{align}
$$

By Lemma 1, both $M\_X$ and $M\_Y$ are finite in neighborhoods of zero. The covariance condition now implies factorization of the joint MGF.

**Lemma 2.** *If $\operatorname{Cov}(X^m,Y^n) = 0$ for all positive integers $m,n$, and both $M\_X(\theta)$ and $M\_Y(\eta)$ are finite for $\|\theta\|,\|\eta\|$ sufficiently small, then $\mathbb E[e^{\theta X}e^{\eta Y}] = \mathbb E[e^{\theta X}]\mathbb E[e^{\eta Y}]$ for such $\theta,\eta$.*

*Proof.* For $\|\theta\|,\|\eta\|$ sufficiently small, we may expand $e^{\theta X} = \sum\_{m=0}^\infty \frac{\theta^m}{m!}X^m$ and $e^{\eta Y} = \sum\_{n=0}^\infty \frac{\eta^n}{n!}Y^n$, both converging in $L^2(\mathbb P)$ by finiteness of the MGFs. By dominated convergence,

$$
\begin{align}
\mathbb E[e^{\theta X}e^{\eta Y}] &= \mathbb E\left[\sum_{m=0}^\infty \frac{\theta^m}{m!}X^m \sum_{n=0}^\infty \frac{\eta^n}{n!}Y^n\right] \\
&= \sum_{m=0}^\infty\sum_{n=0}^\infty \frac{\theta^m\eta^n}{m!n!}\mathbb E[X^m Y^n] \\
&= \sum_{m=0}^\infty\sum_{n=0}^\infty \frac{\theta^m\eta^n}{m!n!}\mathbb E[X^m]\mathbb E[Y^n] \\
&= \mathbb E[e^{\theta X}]\mathbb E[e^{\eta Y}],
\end{align}
$$

where the third equality uses $\operatorname{Cov}(X^m,Y^n) = 0$, which gives $\mathbb E[X^m Y^n] = \mathbb E[X^m]\mathbb E[Y^n]$. $\square$

To complete the proof, we apply the MGF characterization of independence from the previous section. By Lemma 2, the joint MGF factorizes:

$$
M_{(X,Y)}(\theta,\eta) = \mathbb E[e^{\theta X + \eta Y}] = \mathbb E[e^{\theta X}e^{\eta Y}] = M_X(\theta)M_Y(\eta)
$$

for all $(\theta,\eta)$ in a neighborhood of $(0,0)$. Since the MGF uniquely determines the distribution when finite near zero, this factorization implies the joint distribution factorizes: $\mu\_{(X,Y)} = \mu\_X \otimes \mu\_Y$. Therefore $X$ and $Y$ are independent. $\square$