---
title: "Adam and AdamW: How They Work and When They Fail"
excerpt: "Adam is an adaptive optimizer that rescales gradients coordinate-wise and maintains a momentum, addressing two major problems with Stochastic Gradient Descent. This post discusses why it is so effective and under what circumstances it can fail."
collection: blog
date: 2026-02-26
tags:
  - math
  - theory
  - proofs
---

[Adam](https://arxiv.org/abs/1412.6980) is the default optimizer in modern deep learning, often converging faster and more reliably than stochastic gradient descent. This improved performance comes at the cost of roughly three times the memory requirement for SGD. Moreover, when weight decay is involved, Adam does not optimize the same objective as SGD as you might expect. This post explains how Adam works, where its memory overhead comes from, and how the interaction with weight decay can cause unexpected behavior.

**Notations.** Throughout this post, $\theta \in \mathbb{R}^d$ denotes the model parameters, $\nabla\_\theta f$ denotes the gradient of $f$ with respect to $\theta$, and $g\_t$ denotes the minibatch gradient at step $t$. The learning rate is $\eta$, and the symbol $\odot$ denotes elementwise multiplication. Adam's hyperparameters are $\beta\_1$ and $\beta\_2$ for the exponential moving average decay rates, and $\epsilon$ for numerical stability. The regularization strength is $\lambda$.

## Motivation

In deep learning, we want to minimize a loss function over the parameters of a neural network. Given a dataset $\mathcal{D}$ and parameters $\theta \in \mathbb{R}^d$, we solve

$$
\min_\theta \; f(\theta) = \min_\theta \; \frac{1}{|\mathcal{D}|} \sum_{x \in \mathcal{D}} \ell(x; \theta),
$$

where $\ell(x; \theta)$ is the loss on a single example. Gradient descent updates the parameters by taking a step in the direction of steepest descent.

$$
\theta_{t+1} = \theta_t - \eta \nabla_\theta f(\theta_t).
$$


## Stochastic Gradient Descent and Its Drawbacks

In practice, modern datasets are too large to compute the full gradient $\nabla\_\theta f(\theta\_t)$ at each step. Instead, we approximate the gradient using a minibatch $\mathcal{B}\_t \subset \mathcal{D}$ sampled at each iteration.

$$
g_t = \frac{1}{|\mathcal{B}_t|} \sum_{x \in \mathcal{B}_t} \nabla_\theta \ell(x; \theta_t) \approx \nabla_\theta f(\theta_t).
$$

This minibatch gradient $g\_t$ is an unbiased estimate of the true gradient. Replacing the full gradient with this estimate gives stochastic gradient descent (SGD), which updates the parameters as

$$
\theta_{t+1} = \theta_t - \eta g_t.
$$

This update rule has two well-known problems.

The first problem is gradient noise. Minibatch gradients have high variance and can produce large outliers, causing the iterates to jitter rather than move smoothly toward the optimum.

The second problem is that SGD uses one learning rate for all coordinates. If the loss landscape is ill-conditioned and some directions require small steps while others require large steps, SGD struggles to converge.

## Adam

Adam addresses both problems by tracking two quantities in addition to the minibatch gradient.

To solve the first problem, Adam maintains an exponential moving average of the gradients.

$$
m_t = \beta_1 m_{t-1} + (1-\beta_1) g_t.
$$

Instead of using the raw minibatch gradient $g\_t$, Adam's update rule uses the smoothed estimate $m\_t$, which averages out noise across iterations. The hyperparameter $\beta\_1$ controls how long the memory of past gradients lasts. A larger $\beta\_1$ produces more smoothing but slower adaptation to changes in the gradient.

To solve the second problem, Adam maintains an exponential moving average of the elementwise squared gradients.

$$
v_t = \beta_2 v_{t-1} + (1-\beta_2) \, g_t \odot g_t.
$$

This quantity estimates the second moment of each coordinate. The update rule divides the gradient by $\sqrt{v\_t}$, giving per-coordinate step sizes that are inversely proportional to historical gradient magnitudes. Coordinates with large gradients receive smaller steps, while coordinates with small gradients receive larger steps.

Combining both fixes, the Adam update rule is

$$
\theta_{t+1} = \theta_t - \eta \frac{\hat{m}_t}{\sqrt{\hat{v}_t} + \epsilon}.
$$

The quantities $\hat{m}\_t$ and $\hat{v}\_t$ are bias-corrected versions of $m\_t$ and $v\_t$, which we discuss in the next section.

Adam's improved convergence comes at a memory cost. Vanilla SGD stores only the parameters $\theta$, whereas Adam additionally stores the momentum $m\_t$ and the second moment estimate $v\_t$. The optimizer state alone is twice the parameter memory, so the total memory footprint is roughly three times that of SGD. For large models, this can amount to tens or hundreds of gigabytes. 

## Bias Correction

The exponential moving averages are initialized at zero, which biases the early estimates downward. To see this, suppose $m\_0 = 0$ and the gradient has constant mean $\mathbb{E}[g\_t] = \mu$. Then

$$
\mathbb{E}[m_t] = (1 - \beta_1^t) \mu,
$$

so $m\_t$ underestimates $\mu$ by a factor of $(1 - \beta\_1^t)$. The same issue affects $v\_t$.

Adam corrects this bias by rescaling the estimates.

$$
\hat{m}_t = \frac{m_t}{1 - \beta_1^t}, \qquad \hat{v}_t = \frac{v_t}{1 - \beta_2^t}.
$$

These bias-corrected estimates are the ones that appear in the update rule.

## L2 Regularization and Weight Decay

In many machine learning algorithms, it is common to penalize large weights to prevent overfitting. The classic example is ridge regression, where we add a squared norm penalty to the loss.

$$
\min_\theta \; f(\theta) + \frac{\lambda}{2} \|\theta\|^2.
$$

The gradient of the regularizer is $\lambda \theta$. Adding this to the loss gradient, gradient descent becomes

$$
\theta_{t+1} = \theta_t - \eta (g_t + \lambda \theta_t) = (1 - \eta \lambda) \theta_t - \eta g_t.
$$

This equation admits a weight decay interpretation. The weights shrink by a factor of $(1 - \eta \lambda)$ at each step, followed by a gradient step. For vanilla SGD without momentum, L2 regularization and weight decay are equivalent.

For Adam, this equivalence breaks. The update direction is divided by $\sqrt{\hat{v}\_t} + \epsilon$, so if we add $\lambda \theta\_t$ to the gradient, the penalty term is also divided by this quantity.

$$
\theta_{t+1} = \theta_t - \eta \frac{\hat{m}_t + \lambda \theta_t}{\sqrt{\hat{v}_t} + \epsilon}.
$$

Coordinates with large $\hat{v}\_t$ receive weaker effective regularization because the penalty is divided by a larger number. Adding $\lambda \theta\_t$ to the gradient is not equivalent to uniform weight decay in Adam, because the penalty is preconditioned by $(\sqrt{\hat{v}\_t} + \epsilon)^{-1}$, making the effective shrinkage coordinate- and history-dependent.

## A Concrete Example

Ridge regression provides a clean setting to observe this effect. Consider an ill-conditioned design matrix $X$ where column 1 has scale 1 and column 2 has scale 1000. The gradient with respect to $\theta\_2$ is roughly 1000 times larger than the gradient with respect to $\theta\_1$, so Adam's estimate $\hat{v}\_{t,2}$ will be roughly $10^6$ times larger than $\hat{v}\_{t,1}$.

The effective regularization on coordinate $i$ is $\lambda \theta\_{t,i} / (\sqrt{\hat{v}\_{t,i}} + \epsilon)$. For the high-scale coordinate, $\sqrt{\hat{v}\_{t,2}}$ is large, so the regularization becomes weak. For the low-scale coordinate, $\sqrt{\hat{v}\_{t,1}}$ is small, so the regularization remains strong. Although the ridge solution itself depends on feature scaling, it is still the minimizer of a uniform $\lambda \|\theta\|^2$ penalty. Adam with L2 regularization adds an extra, optimizer-induced non-uniformity via the $(\sqrt{\hat{v}\_{t,i}} + \epsilon)^{-1}$ factor.

The stationary point of Adam with L2 regularization differs from the true ridge regression solution. SGD converges to the correct solution because it does not rescale the regularization term.

## AdamW

AdamW fixes this problem with a one-line conceptual change. Instead of adding the L2 penalty to the gradient, AdamW applies weight decay directly to the parameters after the Adam step.

$$
\theta_{t+1} = (1 - \eta \lambda) \theta_t - \eta \frac{\hat{m}_t}{\sqrt{\hat{v}_t} + \epsilon}.
$$

The weight decay term bypasses the adaptive denominator. The weights decay by a factor of $(1 - \eta \lambda)$ uniformly across all coordinates, then take a gradient step. This decoupling ensures that weight decay behaves as intended regardless of the gradient history.

## Conclusion

Adam addresses SGD's two main problems by combining momentum with adaptive per-coordinate learning rates. However, if you need weight decay, use AdamW. Adding L2 regularization directly to Adam's gradient produces non-uniform regularization that depends on gradient history, which is not the objective you intended to minimize.
