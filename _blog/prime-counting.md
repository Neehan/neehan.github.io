---
title: "Prime Counting Function and Chebyshev Bounds"
excerpt: "A simple proof bounding the prime counting function using Chebyshev's technique."
collection: blog
date: 2019-03-22
---

The distribution of primes plays a central role in number theory. The famous mathematician Gauss conjectured that the number of primes between \\(1\\) and \\(n\\) is roughly \\(n/\log n\\), with this estimation becoming increasingly accurate as \\(n\to \infty\\). We denote the number of primes up to \\(n\\) by \\(\pi(n)\\). Mathematically, Gauss's conjecture is equivalent to the claim

$$
\lim_{n\to\infty}\frac{\pi(n)}{n/\log n}=1
$$

This conjecture, now known as the [Prime Number Theorem](https://en.wikipedia.org/wiki/Prime_number_theorem), is notoriously difficult to prove—the first proof appeared nearly a century after it was stated.

While proving the exact asymptotic is hard, establishing that \\(\pi(n)\in O\left( \frac{n}{\log n} \right)\\) is much simpler. In this post, I present bounds showing that for \\(n\ge 12\\),

$$
(1+2\log 4)\frac{n}{\log n}\ge \pi(n)\ge \frac{\log 2}{2}\frac{n}{\log n}
$$

This technique originates from nineteenth-century mathematician Pafnuty Chebyshev, who used a more sophisticated version to prove tighter bounds:

$$
1.1\frac{n}{\log n}\ge \pi(n)\ge 0.92\frac{n}{\log n}
$$

for sufficiently large \\(n\\).

## The Lower Bound

Suppose \\(n\\) is even, i.e., \\(n=2k\\). Consider the prime factorization of the binomial coefficient \\(\binom{2k}{k}\\). Since all of its prime factors lie between \\(1\\) and \\(2k\\), we can write

$$
\binom{2k}{k}=p_1^{a_1}\cdots p_{\pi(2k)}^{a_{\pi(2k)}}
$$

**Claim:** For every prime \\(p\_i\\), we have \\(p\_i^{a\_i}\leq 2k\\).

**Proof:** The key observation is the simple identity 

$$
\lfloor a+b\rfloor-\lfloor a\rfloor-\lfloor b\rfloor\leq1
$$

Recall that the largest exponent of a prime \\(p\_i\\) dividing \\(r!\\) is

$$
\left\lfloor\frac{r}{p_i}\right\rfloor+\left\lfloor\frac{r}{p_i^2}\right\rfloor+\cdots
$$

Since \\(\binom{2k}{k}=\frac{(2k)!}{(k!)^2}\\), the exponent \\(a\_i\\) satisfies

$$
\begin{align}
a_i=&\left\lfloor\frac{2k}{p_i}\right\rfloor-2\left\lfloor\frac{k}{p_i}\right\rfloor+\left\lfloor\frac{2k}{p_i^2}\right\rfloor-2\left\lfloor\frac{k}{p_i^2}\right\rfloor+\cdots \\
&\leq 1+1+\cdots
\end{align}
$$

The sum contains as many \\(1\\)s as there are powers \\(p\_i^j \leq 2k\\). Therefore \\(a\_i \leq \log\_{p\_i}(2k)\\), which implies \\(p\_i^{a\_i}\leq 2k\\).

(Q.E.D.)

Combining the factorization with this bound, we obtain

$$
\binom{2k}{k}=p_1^{a_1}\cdots p_{\pi(2k)}^{a_{\pi(2k)}}\leq (2k)^{\pi(2k)}
$$

Since \\(\binom{2k}{k}\ge 2^k\\) (which follows from the binomial theorem), we have

$$
(2k)^{\pi(2k)}\ge 2^k\implies \pi(2k)\ge \frac{\log 2}{2}\cdot\frac{2k}{\log 2k}
$$

An analogous argument applies when \\(n\\) is odd by considering \\(\binom{2k+1}{k+1}\\). This establishes the lower bound:

$$
\boxed{\pi(n)\ge \frac{\log 2}{2}\cdot\frac{n}{\log n}}
$$

## The Upper Bound

**Claim:** For all \\(n\ge 2\\), 

$$
\prod_{p\leq n\\ p\ \text{prime}}p\leq 4^n
$$

**Proof:** We proceed by induction. The base case holds trivially. The step from \\(n=2k-1\\) to \\(n=2k\\) is immediate since no new primes appear. Thus we need only establish the step from \\(n=2k\\) to \\(n=2k+1\\):

$$
\begin{align}
\prod_{p \leq 2k+1\\ p\ \text{prime}}p &= \prod_{p \leq k+1\\ p\ \text{prime}}p \cdot\prod_{k+2\leq p \leq 2k+1\\ p\ \text{prime}}p\\
&\leq 4^{k+1}\cdot \binom{2k+1}{k}\quad (1)
\end{align}
$$

By the inductive hypothesis, the first product satisfies \\(\prod\_{p \leq k+1} p \leq 4^{k+1}\\). For the second product, observe that every prime between \\(k+2\\) and \\(2k+1\\) divides \\(\binom{2k+1}{k}\\), so their product is at most \\(\binom{2k+1}{k}\\).

The binomial coefficient satisfies \\(\binom{2k+1}{k}<4^k\\). Substituting into (1) yields

$$
\prod_{p \leq 2k+1\\ p\ \text{prime}}p \leq 4^{k+1}\cdot 4^k=4^{2k+1}\quad (\text{Q.E.D.})
$$

To convert this product bound into a bound on \\(\pi(n)\\), fix \\(2\leq m\leq n\\) and consider the primes between \\(m\\) and \\(n\\). There are \\(\pi(n)-\pi(m)\\) such primes, each at least \\(m\\), so

$$
m^{\pi(n)-\pi(m)}\leq \prod_{m\leq p \leq n\\ p\ \text{prime}}p\leq \prod_{1\leq p \leq n\\ p\ \text{prime}}p\leq 4^n
$$

Taking logarithms,

$$
(\pi(n)-\pi(m))\log m\leq n\log 4
$$

Rearranging,

$$
\pi(n)\leq \pi(m)+\frac{n\log 4}{\log m}\leq m+\frac{n\log 4}{\log m}
$$

Setting \\(m=\frac{n}{\log n}\\) gives

$$
\pi(n)\leq \frac{n}{\log n}\left(1+\frac{\log 4\cdot\log n}{\log(n/\log n)}\right)
$$

For \\(n\ge 12\\), one can verify that \\(\frac{\log n}{\log(n/\log n)}\leq 2\\) (and in fact much tighter bounds hold for large \\(n\\)). Therefore

$$
\boxed{\pi(n)\leq \left(1+2\log 4\right)\frac{n}{\log n}}
$$

