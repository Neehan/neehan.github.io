---
title: "Manifold-Constrained Hyper-Connections: Generalized Residual Connections in DeepSeek-V4"
excerpt: "Residual connections force every layer to read from and write to one shared vector. Manifold-Constrained Hyper-Connections replace this vector with a matrix and constrain the mixing to be doubly stochastic, preventing gradient explosion at depth."
collection: blog
date: 2026-05-18
tags:
  - math
  - deep learning
  - transformers
---

The [residual connection](https://arxiv.org/abs/1512.03385) is one of the most important inventions in modern deep learning. Before ResNet introduced it in 2015, training networks deeper than about 20 layers was impractical because gradients either exploded or vanished on their way back through the network. The fix was simple. Instead of asking each layer to compute the next hidden state from scratch, let it compute a *correction* and add that to the input.

$$
x_{l+1} = x_l + F_l(x_l).
$$

This additive structure gives gradients a direct path from the loss back to early layers, bypassing all the intermediate nonlinearities. It works so well that every modern transformer uses it unchanged.

However, there is a fundamental bottleneck in residual connections. After $L$ layers, the hidden state is roughly $x\_0 + \sum\_{l=1}^{L} F\_l(x\_l)$. Since every layer reads from and writes to the same vector, a layer cannot selectively preserve its output for specific later layers or selectively read from specific previous layers.

This could be solved if each layer read and wrote to a matrix instead of a vector, with learnable gating rules to filter information. [Zhu et al. (2024)](https://arxiv.org/abs/2409.19606) proposed this idea as Hyper-Connections, and DeepSeek-V4 ([DeepSeek-AI, 2025](https://huggingface.co/deepseek-ai/DeepSeek-V4-Pro/resolve/main/DeepSeek_V4.pdf)) extended it with a manifold constraint to prevent gradient explosion at depth. In this blogpost, I will try to motivate what it is and how it works.

**Notations.** We denote the hidden dimension by $d$ and the number of rows in the expanded hidden state by $n$. The hidden state at layer $l$ is $X\_l \in \mathbb{R}^{n \times d}$. Each layer function $F\_l$ takes a single $d$-dimensional vector as input and returns a $d$-dimensional vector as output. This includes both attention and feed-forward layers. The set of $n \times n$ doubly stochastic matrices is denoted $\mathcal{M}$.

## From Vector to Matrix

As noted before, residual connections accumulate all layer outputs into one vector. In Hyper-Connections, we pass $n$ stacked vectors $x\_l \in \mathbb{R}^d$ between layers as a matrix $X\_l \in \mathbb{R}^{n \times d}$. (DeepSeek-V4 uses $n = 4$.) At initialization all rows are identical copies of the embedding vector. As training progresses, different layers learn to write to different rows, so the rows begin to carry different information. A later layer can then read from whichever rows are relevant to its computation. Three small, learnable matrices control the information flow. At the final layer, the first row of $X\_L$ is taken as the output vector for the prediction head.

## The Update Equation

At layer $l$, the update rule is

$$
X_{l+1} = B_l X_l + C_l F_l(A_l X_l)
$$

where $A\_l \in \mathbb{R}^{1 \times n}$, $B\_l \in \mathbb{R}^{n \times n}$, and $C\_l \in \mathbb{R}^{n \times 1}$.

This has the same structure as an RNN hidden state update. The first term $B\_l X\_l$ carries forward the existing state and determines what the network remembers. The second term is the new input. $A\_l$ selects what to feed from the hidden state to a layer, and $C\_l$ determines where to write the layer output back. When $n = 1$, all three become scalars, and setting them to 1 recovers the standard residual connection.

## Constraining $B\_l$ to Prevent Gradient Explosion

Note that, after $L$ layers, the residual component involves the product $B\_L B\_{L-1} \cdots B\_1$. If any eigenvalue exceeds 1 in magnitude, this product explodes. If all are strictly less than 1, it vanishes. This is the same problem that plagued RNNs for years, which LSTMs and GRUs solved with gating.

DeepSeek-V4 instead constrains $B\_l$ to be a [doubly stochastic matrix](https://en.wikipedia.org/wiki/Doubly_stochastic_matrix), which has the intriguing property that all of its eigenvalues have magnitude at most 1. Equivalently, a matrix $M \in \mathbb{R}^{n \times n}$ is doubly stochastic if all entries are non-negative, every row sums to 1, and every column sums to 1.

$$
\mathcal{M} = \left\{ M \in \mathbb{R}^{n \times n} \;\middle|\; M \geq 0, \; M \mathbf{1}_n = \mathbf{1}_n, \; \mathbf{1}_n^\top M = \mathbf{1}_n^\top \right\}
$$

The set $\mathcal{M}$ is also closed under multiplication, so the product $B\_L B\_{L-1} \cdots B\_1$ remains doubly stochastic regardless of depth. Explosion is impossible by construction. The vanishing problem is handled by the additive term $C\_l F\_l(A\_l X\_l)$, which injects fresh signal at every layer.

## Projecting $B\_l$ During Training

One concern with $B$ is that gradient descent updates it without respecting the doubly stochastic constraint. So before each forward pass, we project $B\_l$ back onto $\mathcal{M}$ using the [Sinkhorn-Knopp algorithm](https://en.wikipedia.org/wiki/Sinkhorn%27s_theorem): first exponentiate all entries to make them positive. Then alternate between normalizing rows to sum to 1 and normalizing columns to sum to 1.

$$
M^{(0)} = \exp(\tilde{B}_l), \qquad M^{(t)} = T_r(T_c(M^{(t-1)}))
$$

After enough iterations (DeepSeek-V4 uses 20), the result is approximately doubly stochastic. The entire operation is differentiable since it only involves exponentials and divisions.

## Input-Dependent Parameters

In practice, $A\_l$, $B\_l$, and $C\_l$ are not static, but are functions of the current hidden state.

$$
A_l, B_l, C_l = f(X_l; \theta_l)
$$

where $f$ is a small learned projection (flatten, RMSNorm, linear) with per-layer parameters $\theta\_l$. Since $n$ is small, the overhead is negligible.

## Conclusion

Manifold-Constrained Hyper-Connections replace the residual connection's single shared vector with a matrix of $n$ separate rows. Constraining $B\_l$ to be doubly stochastic prevents gradient explosion at any depth. The layers themselves are unchanged. mHC only changes how information flows between them.
