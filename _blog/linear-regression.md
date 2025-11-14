---
title: "Linear Regression as a System of Linear Equations"
excerpt: "A systematic treatment of linear regression from the perspective of solving linear systems. We characterize when exact solutions exist based on rank conditions and derive the closed-form solution ($X^T X)^{-1} X^T y$. In the regression setting with noise, this formula emerges as both the least-squares minimizer and the maximum likelihood estimator."
collection: blog
date: 2025-10-10
tags:
  - theory
  - math
---

Linear regression is typically introduced as an optimization problem, but it can be understood more fundamentally as solving a linear system. This perspective clarifies when exact solutions exist and why the least-squares formula emerges naturally from maximum likelihood estimation under Gaussian noise.

# Setup

Let $X \in \mathbb{R}^{n \times d}$, $\theta \in \mathbb{R}^{d \times 1}$, and $y \in \mathbb{R}^{n \times 1}$. We are interested in solving the linear system

$$
X\theta = y.\qquad(1)
$$

This system may have zero, one, or infinitely many solutions. The structure is governed entirely by the rank of $X$ and by whether $y$ lies in its column space.

## Existence and Uniqueness

A **unique solution** to $X\theta = y$ exists iff:

1. **At least one solution**:  
   There is at least one $\theta\in \mathbb{R}^d$ such that $X\theta = y$. In other words, $y$ lies in the column space of $X$, i.e. $\operatorname{rank}(X) = \operatorname{rank}([X \mid y]).$

2. **At most one solution**: $X\phi = 0$ if and only if $\phi=0$, or equivalently,
   $$\operatorname{rank}(X) = d.$$
   To see why this ensures uniqueness, suppose $X\theta = X\theta' = y$. Then $X(\theta - \theta') = 0$, which implies $\theta - \theta' = 0$ by the injectivity condition.

If both conditions hold—i.e., $\operatorname{rank}(X) = \operatorname{rank}([X \mid y]) = d$—then there is *exactly one* $\theta$ satisfying $X\theta = y$.

## Closed-Form Solution

Multiply both sides of (1) by $X^T$ to obtain the **normal equations**:

$$
X^TX\theta = X^Ty.\qquad (2)
$$

Note that $X^TX$ is a $d \times d$ square matrix. To determine when this system has a unique solution, we need to understand when $X^TX$ is invertible.

A key observation is that $\operatorname{ker}(X) = \operatorname{ker}(X^TX)$, which follows from the equivalence

$$
X\theta = 0 \Leftrightarrow X^TX\theta = 0.
$$

To see the forward direction, note that if $X\theta = 0$, then $X^TX\theta = X^T \cdot 0 = 0$. For the reverse, if $X^TX\theta = 0$, then $\theta^T X^TX\theta = \\|X\theta\\|^2 = 0$, so $X\theta = 0$.

Since $\operatorname{ker}(X) = \operatorname{ker}(X^TX)$, we have $\operatorname{rank}(X) = \operatorname{rank}(X^TX)$. Therefore, if $\operatorname{rank}(X) = d$, then $X^TX$ has full rank and is invertible. In this case the solution of (2) is

$$
\theta^* = (X^T X)^{-1} X^T y.
$$

This formula returns the **exact** $\theta$ whenever the system is consistent.  
It applies cleanly in cases:

- $n > d$ (tall, consistent),
- $n = d$ (square full-rank),
- but not $n < d$ (since $\operatorname{rank}(X) \le n < d$).

To verify that $\theta^\*$ satisfies $X\theta^\* = y$ when the system is consistent, observe that if $y = X\theta^\dagger$ is the true solution for some $\theta^\dagger$, then 

$$
X\theta^* = X(X^TX)^{-1}X^Ty = X(X^TX)^{-1}X^TX\theta^\dagger = X\theta^\dagger = y.
$$

## The Regression Setting

In regression we typically have $n \gg d$ and almost never have an exact solution because $y \notin \operatorname{col}(X)$. Instead, we assume a noise model:

$$
y = X\theta + \varepsilon, \qquad \varepsilon \sim \mathcal{N}(0, \sigma^2 I).
$$

**Note:** The only source of uncertainty in this model is the *Gaussian* noise. Since $y = X\theta + \varepsilon$ with $\varepsilon \sim \mathcal{N}(0, \sigma^2 I)$, we have $y \sim \mathcal{N}(X\theta, \sigma^2 I)$. The negative log-likelihood is

$$
-\log p(y \mid X, \theta, \sigma^2) = \frac{n}{2}\log(2\pi\sigma^2) + \frac{1}{2\sigma^2}\|y - X\theta\|_2^2.
$$

Since $\sigma^2$ is a constant with respect to $\theta$, maximizing the log-likelihood (or minimizing the negative log-likelihood) over $\theta$ is equivalent to solving the **least-squares problem**

$$
\min_\theta \|X\theta - y\|_2^2.
$$

Setting the gradient to zero yields the normal equations:

$$
X^T X \theta = X^T y,
$$

and when $X$ has full column rank, the unique minimizer is

$$
\hat{\theta} = (X^T X)^{-1} X^T y.
$$

In the inconsistent case this does *not* satisfy $X\theta = y$ exactly, but it is the closest possible solution in squared-error terms and is also the MLE.