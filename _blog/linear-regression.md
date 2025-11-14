---
title: "Linear Regression as a System of Linear Equations"
excerpt: "A systematic treatment of linear regression from the perspective of solving linear systems. We characterize when exact solutions exist based on rank conditions and derive the closed-form solution (X^T X)^{-1} X^T y. In the regression setting with noise, this formula emerges as both the least-squares minimizer and the maximum likelihood estimator."
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
X\theta = y.
$$

This system may have zero, one, or infinitely many solutions. The structure is governed entirely by the rank of $X$ and by whether $y$ lies in its column space.

## Existence and Uniqueness

A **unique solution** to $X\theta = y$ exists iff:

1. **At least one solution**:  
   There is at least one $\theta\in \mathbb{R}^d$ such that $X\theta = y$. In other words, $y$ lies in the column space of $X$, i.e.  
   $$\operatorname{rank}(X) = \operatorname{rank}([X \mid y]).$$

2. **At most one solution**: $X\theta = 0$ if and only if $\theta=0$, or equivalently,
   $$\operatorname{rank}(X) = d.$$
   To see why this ensures uniqueness, suppose $X\theta = X\theta' = y$. Then $X(\theta - \theta') = 0$, which implies $\theta - \theta' = 0$ by the injectivity condition.

If both conditions hold—equivalently,  
$$\operatorname{rank}(X) = \operatorname{rank}([X \mid y]) = d,$$  
then there is exactly one $\theta$ satisfying $X\theta = y$.

## Closed-Form Solution

A key observation is that $\operatorname{ker}(X) = \operatorname{ker}(X^TX)$, which follows from the equivalence

$$
X\theta = 0 \Leftrightarrow X^TX\theta = 0.
$$

To see the forward direction, note that if $X\theta = 0$, then $X^TX\theta = X^T \cdot 0 = 0$. For the reverse, if $X^TX\theta = 0$, then $\theta^T X^TX\theta = \|X\theta\|^2 = 0$, so $X\theta = 0$.

Since $\operatorname{ker}(X) = \operatorname{ker}(X^TX)$, we have $\operatorname{rank}(X) = \operatorname{rank}(X^TX)$. Therefore, if $\operatorname{rank}(X) = d$, then $X^TX$ is a $d \times d$ matrix with full rank, hence invertible. In this case the solution is

$$
\theta^* = (X^T X)^{-1} X^T y.
$$

This formula returns the **exact** $\theta$ whenever the system is consistent.  
It applies cleanly in cases:

- $n > d$ (tall, consistent),
- $n = d$ (square full-rank),
- but not $n < d$ (since $\operatorname{rank}(X) \le n < d$).

To verify that $\theta^*$ satisfies $X\theta^* = y$ when the system is consistent, observe that if $y = X\theta'$ for some $\theta'$, then 

$$
X\theta^* = X(X^TX)^{-1}X^Ty = X(X^TX)^{-1}X^TX\theta' = X\theta' = y.
$$

## The Regression Setting

In regression we typically have $n \gg d$ and almost never have an exact solution because $y \notin \operatorname{col}(X)$. Instead, we assume a noise model:

$$
y = X\theta + \varepsilon, \qquad \varepsilon \sim \mathcal{N}(0, \sigma^2 I).
$$

The negative log-likelihood (up to constants) is proportional to

$$
\|X\theta - y\|_2^2.
$$

Thus, maximum likelihood estimation is equivalent to solving the **least-squares problem**

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