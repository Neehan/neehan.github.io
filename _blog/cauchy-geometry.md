---
title: "Geometric Interpretration of the Cauchy–Schwarz Inequality"
excerpt: "How Pythagoras and positive definiteness geometrically prove the Cauchy–Schwarz inequality. The inequality simply states that projecting a vector onto another cannot increase its length. This geometric perspective reveals that Cauchy–Schwarz represents the cosine of an angle between vectors, connecting it to the law of cosines and Pearson correlation."
collection: blog
date: 2025-11-10
tags:
  - theory
---

The Cauchy–Schwarz inequality is usually introduced as a deep structural fact about inner product spaces. In standard courses, the inequality is proved algebraically from the axioms of an inner product. However, it is a more fundamental geometric fact.

In this blog post, I will show that the Cauchy–Schwarz inequality simply tells us *projecting a vector onto another cannot increase its length*. To make this work, we need introduce two properties to a vector space:
1. **Symmetric bilinear form** (enables orthogonal decomposition)
2. **Positive definiteness** (ensures all vectors have non-negative squared length)

From these alone, we can show Cauchy–Schwarz represents the **cosine of an angle** between two vectors.

## Setup

Let $V$ be a vector space with a symmetric, positive definite bilinear form $\langle \cdot, \cdot \rangle : V \times V \to \mathbb{R}$. Define the induced norm $\\|u\\| = \sqrt{\langle u, u \rangle}$.

**Positive definiteness** means:
- $\langle u, u \rangle \geq 0$ for all $u \in V$
- $\langle u, u \rangle = 0$ if and only if $u = 0$

**Symmetry and positive definiteness together imply Pythagoras' theorem**: if $\langle u, v \rangle = 0$ (orthogonality), then

$$
\|u + v\|^2 = \|u\|^2 + \|v\|^2
$$

This follows from expanding $\langle u+v, u+v \rangle = \langle u,u \rangle + \langle u,v \rangle + \langle v,u \rangle + \langle v,v \rangle$. By symmetry, $\langle u,v \rangle = \langle v,u \rangle$, so when $\langle u,v \rangle = 0$, the middle terms vanish and we get $\\|u+v\\|^2 = \\|u\\|^2 + \\|v\\|^2$.

## Orthogonal Projection and Pythagoras

For non-zero $v \in V$, define the **orthogonal projection** of $u$ onto $v$:

$$
\text{proj}_v(u) = \frac{\langle u, v \rangle}{\langle v, v \rangle} v = \frac{\langle u, v \rangle}{\|v\|^2} v
$$

This is the unique vector parallel to $v$ such that $u - \text{proj}_v(u)$ is orthogonal to $v$:

$$
\langle u - \text{proj}_v(u), v \rangle = \langle u, v \rangle - \frac{\langle u, v \rangle}{\|v\|^2}\langle v, v \rangle = 0
$$

We can decompose $u$ into parallel and perpendicular components:

$$
u = \text{proj}_v(u) + (u - \text{proj}_v(u))
$$

By **Pythagoras**:

$$
\|u\|^2 = \|\text{proj}_v(u)\|^2 + \|u - \text{proj}_v(u)\|^2
$$

## Deriving Cauchy–Schwarz

By **positive definiteness**, $\\|u - \text{proj}_v(u)\\|^2 \geq 0$, so:

$$
\|u\|^2 \geq \|\text{proj}_v(u)\|^2
$$

Compute the norm of the projection:

$$
\|\text{proj}_v(u)\|^2 = \left\|\frac{\langle u, v \rangle}{\|v\|^2} v\right\|^2 = \frac{|\langle u, v \rangle|^2}{\|v\|^4} \|v\|^2 = \frac{|\langle u, v \rangle|^2}{\|v\|^2}
$$

Therefore:

$$
\|u\|^2 \geq \frac{|\langle u, v \rangle|^2}{\|v\|^2}
$$

Rearranging:

$$
|\langle u, v \rangle|^2 \leq \|u\|^2 \|v\|^2
$$

This is the **Cauchy–Schwarz inequality**:

$$
|\langle u, v \rangle| \leq \|u\| \|v\|
$$

**Equality holds** if and only if $u - \text{proj}\_v(u) = 0$, meaning $u$ is parallel to $v$.

## The Cosine Interpretation

The projection naturally defines an angle between $u$ and $v$. Since the projection has a certain length and $u$ has length, define:

$$
\cos \theta = \frac{\|\text{proj}_v(u)\|}{\|u\|}
$$

This is the ratio of the projection's length to the original vector's length. From our earlier calculation:

$$
\|\text{proj}_v(u)\|^2 = \frac{|\langle u, v \rangle|^2}{\|v\|^2}
$$

so:

$$
\|\text{proj}_v(u)\| = \frac{|\langle u, v \rangle|}{\|v\|}
$$

Therefore:

$$
\cos \theta = \frac{|\langle u, v \rangle|}{\|u\| \|v\|}
$$

The Cauchy–Schwarz inequality is now transparent: it simply says the cosine is at most 1, i.e., **the projection cannot be longer than the original vector**.

Accounting for sign (parallel vs anti-parallel), we get:

$$
\langle u, v \rangle = \|u\| \|v\| \cos \theta
$$

This is the **law of cosines** in its inner product form. The bilinear form measures the "alignment" between vectors where the angle can be 0 (parallel), $\\pi/2$ (orthogonal), or $\\pi$ (anti-parallel).

## Implications
* If $u$ and $v$ have zero mean, then:
$$
\cos \theta = \frac{\langle u, v \rangle}{\|u\| \|v\|}
$$
is the **Pearson correlation coefficient** between $u$ and $v$.

* The Cauchy–Schwarz inequality becomes an equality when $u - \text{proj}\_v(u) = 0$, i.e., when $u$ is parallel to $v$ (or anti-parallel). This happens when:
- $\cos \theta \approx 1$ (high positive correlation), the vectors are nearly parallel and the bound is nearly saturated
- $\cos \theta \approx -1$ (high negative correlation), the vectors are nearly anti-parallel and the bound is nearly saturated

* When $\cos \theta = 0$ (orthogonality/no correlation), the bound becomes loose: the left side is zero while the right side can be arbitrarily large.

## Conclusion

We have shown that the Cauchy–Schwarz inequality is the geometric statement that **projecting a vector onto another cannot increase its length**.