---
title: "Why Does Normal Distribution Show Up in Central Limit Theorem?"
excerpt: "The Central Limit Theorem states that normalized sums of i.i.d random variables with finite mean and variance converge in distribution to a Gaussian. But why the normal distribution specifically? This post builds intuition by showing CLT is a direct consequence of the exponential limit $(1 + x/n)^n \\to e^x$."
collection: blog
date: 2026-02-16
tags:
  - math
  - theory
  - proofs
---

The classical Central Limit Theorem (CLT; Lindeberg-Lévy) states that the normalized sum of $n$ i.i.d. random variables with finite mean and variance must converge in distribution to $N(0,1)$ as $n \to \infty$. 

However, one might ask, why is this sum for any distribution normally distributed? Why does it not follow gamma, exponential, log normal, or any other distribution? In this blogpost, I will build intuition for why the normal distribution is inevitable. It is meant to be expository and not rigorous. For a rigorous proof, see [Wikipedia](https://en.wikipedia.org/wiki/Central_limit_theorem#Proof_of_classical_CLT). The key insight is this limit from calculus:

$$
\lim_{n \to \infty}\left(1 + \frac{x}{n}\right)^n = e^x.
$$

Since $e^x$ is the unique continuous function satisfying this limit, the limiting distribution of the above sum is forced to be normal.

**Notations.** We work on a fixed probability space $(\Omega, \mathcal{F}, \mathbb{P})$. For a random variable $X$, its density (when it exists) is $f\_X$. The [characteristic function](https://en.wikipedia.org/wiki/Characteristic_function_(probability_theory)) is $\phi\_X(t) = \mathbb{E}[e^{itX}]$, which is the Fourier transform of the distribution. We write $X\_1, X\_2, \ldots$ for i.i.d. copies of $X$, and $S\_n = X\_1 + \cdots + X\_n$ for their sum. Without loss of generality, we assume $X$ is standardized with $\mathbb{E}[X] = 0$ and $\operatorname{Var}(X) = 1$ (this step requires the finite mean and non-zero variance assumption).

## From Sums to Convolutions

We want to understand the distribution of $S\_n = X\_1 + \cdots + X\_n$. If $X$ and $Y$ are independent random variables with densities $f\_X$ and $f\_Y$, the density of their sum $X + Y$ is given by the [convolution](https://en.wikipedia.org/wiki/Convolution)

$$
f_{X+Y}(z) = \int_{-\infty}^{\infty} f_X(x) f_Y(z - x) \, dx = (f_X * f_Y)(z).
$$

For $n$ i.i.d. copies, the density of $S\_n$ is the $n$-fold convolution $f\_X * f\_X * \cdots * f\_X$. This is analytically intractable for most distributions.

## Characteristic Functions

The standard trick to simplify convolutions is to take Fourier transforms. In probability, the Fourier transform of a density is called the [characteristic function](https://en.wikipedia.org/wiki/Characteristic_function_(probability_theory))

$$
\phi_X(t) = \mathbb{E}[e^{itX}] = \int_{-\infty}^{\infty} f_X(x) e^{itx} \, dx.
$$

The characteristic function always exists because it is [dominated](https://en.wikipedia.org/wiki/Dominated_convergence_theorem) by $\|e^{itX}\| = 1$ for all real $t$ and $X$, so $\|\phi\_X(t)\| = \|\mathbb{E}[e^{itX}]\| \le \mathbb{E}[\|e^{itX}\|] = 1$. 

The [inversion formula](https://en.wikipedia.org/wiki/Characteristic_function_(probability_theory)#Inversion_formula) recovers $f\_X$ from $\phi\_X$, so the two representations carry exactly the same information.

Note that Fourier transforms convert convolution into multiplication. In particular, for independent $X$ and $Y$,

$$
\phi_{X+Y}(t) = \phi_X(t) \cdot \phi_Y(t).
$$

This means the characteristic function of $S\_n$ is simply $\phi\_{S\_n}(t) = [\phi\_X(t)]^n$, or applying the self-convolution $n$-times!

## Renormalized Self-Convolution Approaches a Fixed Point

Since $X$ has mean 0, the sum $S\_n$ also has mean 0. By linearity of variance for independent variables, $\operatorname{Var}(S\_n) = n$, so the standard deviation grows like $\sqrt{n}$. To keep the variance at 1, we rescale by defining

$$
Z_n = \frac{S_n}{\sqrt{n}}.
$$

The characteristic function of $Z\_n$ is

$$
\phi_{Z_n}(t) = \phi_{S_n/\sqrt{n}}(t) = \left[\phi_X\left(\frac{t}{\sqrt{n}}\right)\right]^n.
$$

To determine the limiting distribution, we need to understand the behavior of this expression as $n \to \infty$. Finite variance implies $\phi\_X(t)$ is twice differentiable at $t = 0$, so Taylor expansion gives

$$
\phi_X(s) = 1 + is\mathbb{E}[X] - \frac{s^2}{2}\mathbb{E}[X^2] + o(s^2).
$$

Since $\mathbb{E}[X] = 0$ and $\mathbb{E}[X^2] = 1$ for our standardized variable, this simplifies to $\phi\_X(s) = 1 - s^2/2 + o(s^2)$. Substituting $s = t/\sqrt{n}$ gives

$$
\phi_X\left(\frac{t}{\sqrt{n}}\right) = 1 - \frac{t^2}{2n} + o(1/n).
$$

Raising to the $n$-th power, we obtain

$$
\left[\phi_X\left(\frac{t}{\sqrt{n}}\right)\right]^n = \left(1 - \frac{t^2}{2n} + o(1/n)\right)^n.
$$

This is exactly the setting of the fundamental limit $\lim\_{n \to \infty}(1 + x/n)^n = e^x$. Setting $x = -t^2/2$, we get

$$
\lim_{n \to \infty} \phi_{Z_n}(t) = e^{-t^2/2}.
$$

By [Lévy's continuity theorem](https://en.wikipedia.org/wiki/L%C3%A9vy%27s_continuity_theorem), pointwise convergence of characteristic functions to a function continuous at 0 implies convergence in distribution. This shows that regardless of what $\phi\_X$ we start with, renormalized self-convolution always converges to a fixed point. Next, we will try to understand what this fixed point represents.

## The Gaussian is the Unique Fixed Point Under Finite Variance

We have shown that renormalized self-convolution forces convergence to $e^{-t^2/2}$ in the Fourier domain. But why must this correspond to a normal distribution, and could there be other fixed points?

It turns out the Gaussian is the unique probability density that equals its own Fourier transform (up to scaling). The [eigenfunctions of the Fourier transform](https://en.wikipedia.org/wiki/Hermite_polynomials#Hermite_functions) are the Hermite functions, with eigenvalues $(-i)^n$. Eigenvalue 1 occurs for $n = 0, 4, 8, \ldots$, but only the 0th Hermite function (the Gaussian) is nonnegative and integrates to 1, making it the unique such eigenfunction that is a valid probability density. We can verify that $e^{-t^2/2}$ is indeed the characteristic function of $N(0,1)$ by direct computation

$$
\phi_{N(0,1)}(t) = \int_{-\infty}^{\infty} \frac{1}{\sqrt{2\pi}} e^{-x^2/2} e^{itx} \, dx = e^{-t^2/2}.
$$

The Fourier transform of a Gaussian is again a Gaussian. No other distribution has this self-replicating property, which is why the CLT limit must be normal.

## Conclusion

The ubiquity of the normal distribution is ultimately a consequence of the ubiquity of $e^x$. Wherever sums accumulate, wherever averages stabilize, the exponential limit $(1 + x/n)^n \to e^x$ quietly enforces its will, and the bell curve emerges. The Gaussian is not an arbitrary choice but a mathematical inevitability, woven into the fabric of how quantities combine.
