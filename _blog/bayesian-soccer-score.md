---
title: "I looked into 16 Million Futures and Saw Argentina Win against Australia in 11 Million of Them"
excerpt: "Predicting FIFA World Cup 2022 match outcomes using Bayesian inference on Poisson processes."
collection: blog
date: 2022-12-01
---

*For a less technical introduction to this work, see the [Medium article](https://medium.com/@notadib/i-looked-into-16-million-futures-and-saw-argentina-win-against-australia-in-11-million-of-them-c336574ae574).*

*Note: After completing this work, I discovered [a similar model](https://eightyfivepoints.blogspot.com/2018/05/what-can-we-expect-from-21st-fifa-world.html) by Dixon and Coles. The key differences in my approach are the inclusion of a temporal drift term and full posterior estimation rather than maximum likelihood.*

In this blogpost, I develop a probabilistic framework for predicting soccer match outcomes by modeling team goal-scoring rates as an autoregressive time series with Gaussian drift. The model treats each team's latent strength as evolving stochastically over time, with observations generated via a Poisson likelihood. Rather than fitting a neural network to learn these dynamics, I specify an interpretable Bayesian hierarchical model with explicit priors on drift variance and covariate effects. This design prioritizes posterior interpretability and uncertainty quantification over raw predictive performance.

**Full implementation:** [Colab notebook](https://colab.research.google.com/drive/1b-o\_ACHLYiweYk\_Ynp\_qxcWvZ2Am7-At) [GitHub repo](https://github.com/notadib/soccer-wc-pred)

## Problem Formulation

Let \\(g\_n \in \mathbb{Z}\_{\geq 0}\\) denote the number of goals scored by a team in match \\(n\\). We seek to model the conditional distribution \\(P(g\_n \mid g\_{1:n-1}, \mathbf{x}\_{1:n})\\), where \\(\mathbf{x}\_n\\) represents match-specific covariates (opponent strength, venue, etc.).

We adopt a Poisson likelihood for goal counts:

$$

g_n \mid \lambda_n \sim \text{Poisson}(\lambda_n)

$$

where \\(\lambda\_n > 0\\) is the instantaneous goal-scoring rate for match \\(n\\). The key modeling challenge is to specify how \\(\lambda\_n\\) depends on historical observations and covariates while accounting for temporal evolution in team quality.

We decompose \\(\lambda\_n\\) into:
1. A latent team strength parameter \\(\iota\_n \in \mathbb{R}\\) that evolves as a Gaussian random walk
2. A linear adjustment term \\(\boldsymbol{\alpha}^\top \mathbf{x}\_n\\) capturing match-specific effects

This yields the log-linear specification:

$$

\log \lambda_n = \iota_n + \boldsymbol{\alpha}^\top \mathbf{x}_n

$$

Inference proceeds via sequential Bayesian updating:

$$

P(\iota_n, \boldsymbol{\alpha}, \sigma \mid g_{1:n}) \propto P(g_n \mid \iota_n, \boldsymbol{\alpha}) \cdot P(\iota_n \mid \iota_{n-1}, \sigma) \cdot P(\iota_{n-1}, \boldsymbol{\alpha}, \sigma \mid g_{1:n-1})

$$

where \\(\sigma^2\\) parameterizes the drift variance between consecutive matches.

## Simplified Model: Fixed Opposition

Consider a simplified scenario where team A plays only team B repeatedly. We model team A's goal-scoring rate \\(\lambda\_n\\) in match \\(n\\) as evolving according to:

$$

\begin{align}
g_n &\sim \text{Poisson}(\lambda_n)\\
\lambda_n &\sim \mathcal{N}(\lambda_{n-1}, \sigma^2)\\
P(\lambda_n \mid g_n, g_{n-1},\ldots) &\propto P(g_n\mid\lambda_n) \cdot P(\lambda_n \mid g_{n-1},\ldots)
\end{align}

$$

The drift term \\(\lambda\_n \sim \mathcal{N}(\lambda\_{n-1}, \sigma^2)\\) accounts for temporal changes in team quality due to lineup adjustments, player form, and experience accumulation.

**Bayesian update mechanics:** To compute the posterior after observing \\(g\_n\\), we marginalize over the previous state:

$$
\begin{align}
P(\lambda_n \mid g_{n-1}, \ldots) &= \int_{\lambda_{n-1}} P(\lambda_n \mid \lambda_{n-1}) P(\lambda_{n-1} \mid g_{n-1}, \ldots) \, d\lambda_{n-1}
\end{align}
$$

After observing \\(g\_n\\):

$$
P(\lambda_n \mid g_n, g_{n-1}, \ldots) \propto P(g_n \mid \lambda_n) \cdot P(\lambda_n \mid g_{n-1}, \ldots)
$$

Expanding the prior recursively:

$$
P(\lambda_n \mid g_{n-1}, \ldots) \propto \int_{\lambda_{n-1}} P(\lambda_n \mid \lambda_{n-1}) P(g_{n-1} \mid \lambda_{n-1}) P(\lambda_{n-1} \mid g_{n-2}, \ldots) \, d\lambda_{n-1}
$$

This integral can be computed numerically or via MCMC sampling.

**Example trajectory:** Argentina scores 2 goals in match 1 → posterior for \\(\lambda\\) concentrates around 2. Match 2: 1 goal → posterior shifts left. Match 3: 3 goals → shifts right, but with narrower variance as observations accumulate.

## Full Model: Variable Opposition

The simplified model fails when teams face different opponents. If Brazil plays South Korea and then Argentina, the observed goal-scoring rates cannot be directly compared without accounting for opponent strength.

We decompose team performance into:
1. **Intrinsic strength** \\(\iota\_n\\): Team quality against a standardized baseline opponent
2. **Match-specific adjustments**: Covariates capturing opponent strength, match importance, venue effects, and player availability

Let \\(\mathbf{x}\_n\\) denote the covariate vector for match \\(n\\), including:
- ELO rating difference \\(E\_n\\)
- Match importance \\(m\_n\\) (e.g., World Cup vs. friendly)
- Key player availability indicators
- Home advantage

We model the observed goal rate as:

$$

\lambda_n = \exp(\iota_n + \boldsymbol{\alpha}^\top \mathbf{x}_n)

$$

The complete hierarchical model is:

$$

\begin{align}
\boldsymbol{\alpha} &\sim \mathcal{N}(0, 2^2 \mathbf{I})\\
\sigma &\sim \text{Gamma}(0.5, 1)\\
\iota_0 &\sim \mathcal{N}(0.4, 0.7^2)\\
\iota_n &\sim \mathcal{N}(\iota_{n-1}, \sigma^2)\\
\lambda_n &= \exp(\iota_n + \boldsymbol{\alpha}^\top \mathbf{x}_n)\\
g_n &\sim \text{Poisson}(\lambda_n)
\end{align}

$$

**Prior specification rationale:**
- **Coefficient prior** \\(\mathcal{N}(0, 2^2)\\): Appears weakly informative, but the exponential link function amplifies the effect. A coefficient two standard deviations from zero scales \\(\lambda\_n\\) by a factor in \\([e^{-4}, e^4] \approx [0.02, 54.6]\\).
- **Drift variance** \\(\text{Gamma}(0.5, 1)\\): Mean and variance of 0.5 reflect the expectation of gradual evolution in team strength between consecutive matches.
- **Initial strength** \\(\mathcal{N}(0.4, 0.7^2)\\): World Cup matches average 2.8 goals total, suggesting approximately 1.4 goals per team. Setting \\(\iota\_0 \approx \log(1.4) \approx 0.34\\) with moderate uncertainty.

## Bayesian Inference

The full sequential update equations are:

$$
\begin{align}
P(\iota_n, \boldsymbol{\alpha}, \sigma \mid g_{n-1}, g_{n-2}, \ldots) &= \int_{\iota_{n-1}} P(\iota_n, \boldsymbol{\alpha}, \sigma, \iota_{n-1} \mid g_{n-1}, g_{n-2}, \ldots) \, d\iota_{n-1}\\
&= \int_{\iota_{n-1}} P(\iota_n \mid \iota_{n-1}, \sigma) \, P(\boldsymbol{\alpha}, \sigma, \iota_{n-1} \mid g_{n-1}, g_{n-2}, \ldots) \, d\iota_{n-1}\\
&\propto \int_{\iota_{n-1}} P(\iota_n \mid \iota_{n-1}, \sigma) \, P(g_{n-1} \mid \iota_{n-1}, \boldsymbol{\alpha}) \, P(\iota_{n-1}, \boldsymbol{\alpha}, \sigma \mid g_{n-2}, \ldots) \, d\iota_{n-1}
\end{align}
$$

**Concrete examples for first few matches:**

$$
P(\iota_1, \boldsymbol{\alpha}, \sigma \mid g_0) \propto \int_{\iota_0} P(\iota_1 \mid \iota_0, \sigma) \cdot P(g_0 \mid \iota_0, \boldsymbol{\alpha}) \cdot P(\iota_0) \cdot P(\boldsymbol{\alpha}) \cdot P(\sigma) \, d\iota_0
$$

$$
P(\iota_2, \boldsymbol{\alpha}, \sigma \mid g_1, g_0) \propto \int_{\iota_1} P(\iota_2 \mid \iota_1, \sigma) \cdot P(g_1 \mid \iota_1, \boldsymbol{\alpha}) \cdot P(\iota_1, \boldsymbol{\alpha}, \sigma \mid g_0) \, d\iota_1
$$

**Implementation in log space:**

To avoid numerical underflow, all computations are performed in log space. Let \\(\Theta = (\iota, \boldsymbol{\alpha}, \sigma)\\) denote the parameter vector. We discretize the parameter space into a grid with \\(M\\) configurations.

**Algorithm:**

1. **Initialize** (match 0):
   $$
   \log P(\Theta_0^{(i)}) = \log P(\iota_0^{(i)}) + \log P(\boldsymbol{\alpha}^{(i)}) + \log P(\sigma^{(i)}), \quad i = 1, \ldots, M
   $$

2. **Posterior update after match 0**:
   $$
   \log P(\Theta_0^{(i)} \mid g_0) = \log P(g_0 \mid \Theta_0^{(i)}) + \log P(\Theta_0^{(i)}) - Z_0
   $$
   where \\(Z_0 = \text{logsumexp}_i[\log P(g_0 \mid \Theta_0^{(i)}) + \log P(\Theta_0^{(i)})]\\) and
   $$
   \log P(g_0 \mid \Theta_0^{(i)}) = g_0 \log \lambda_0^{(i)} - \lambda_0^{(i)} - \log(g_0!), \quad \lambda_0^{(i)} = \exp(\iota_0^{(i)} + \boldsymbol{\alpha}^{(i)\top} \mathbf{x}_0)
   $$

3. **For \\(n = 1, 2, \ldots, N\\) (where \\(N\\) is total number of matches):**

   **(a) Prediction** (marginalize over \\(\iota\_{n-1}\\)):

   For each configuration \\((\iota\_n^{(j)}, \boldsymbol{\alpha}^{(k)}, \sigma^{(l)})\\):
   $$
   \log P(\iota_n^{(j)}, \boldsymbol{\alpha}^{(k)}, \sigma^{(l)} \mid g_{0:n-1}) = \text{logsumexp}_{\iota_{n-1}^{(i)}} \left[ \log P(\iota_n^{(j)} \mid \iota_{n-1}^{(i)}, \sigma^{(l)}) + \log P(\iota_{n-1}^{(i)}, \boldsymbol{\alpha}^{(k)}, \sigma^{(l)} \mid g_{0:n-1}) \right]
   $$
   where
   $$
   \log P(\iota_n^{(j)} \mid \iota_{n-1}^{(i)}, \sigma^{(l)}) = -\frac{1}{2}\log(2\pi (\sigma^{(l)})^2) - \frac{(\iota_n^{(j)} - \iota_{n-1}^{(i)})^2}{2(\sigma^{(l)})^2}
   $$

   **(b) Posterior update** after observing \\(g_n\\):
   $$
   \log P(\Theta_n^{(i)} \mid g_{0:n}) = \log P(g_n \mid \Theta_n^{(i)}) + \log P(\Theta_n^{(i)} \mid g_{0:n-1}) - Z_n
   $$
   where \\(Z_n = \text{logsumexp}_i[\log P(g_n \mid \Theta_n^{(i)}) + \log P(\Theta_n^{(i)} \mid g_{0:n-1})]\\)

The discretization uses 200 values for \\(\iota\_n\\), 200 values for each \\(\alpha\_i\\), and 400 values for \\(\sigma\\), yielding \\(M = 200 \times 200 \times 400 = 16\\) million configurations.

## Results

**See the [Colab notebook](https://colab.research.google.com/drive/1b-o\_ACHLYiweYk\_Ynp\_qxcWvZ2Am7-At) for full derivations, model fitting code, and visualizations.**

I fit separate models for Argentina and Australia using historical match data from [eloratings.net](http://eloratings.net). Posterior inference was performed over a discretized parameter space of 200 × 200 × 400 = 16 million configurations.

**Key findings:**
- Argentina's inferred \\(\alpha\_1\\) (ELO coefficient) was large and positive, confirming rating differentials strongly predict outcomes
- Match importance (\\(\alpha\_2\\)) was small and negative—Argentina scores marginally fewer goals in high-stakes matches
- Argentina's \\(\iota\_n\\) trajectory revealed the 2002 and 2006 squads as peak strength in recent history

**Argentina vs. Australia prediction:**
- \\(P(\text{Argentina win}) = 70.69\%\\), \\(P(\text{Australia win}) = 9.97\%\\), \\(P(\text{Draw}) = 19.34\%\\)
- In 11.3 million of 16 million sampled futures, Argentina outscored Australia

**Model validation (Brazil vs. Serbia):**
The model correctly predicted Brazil's victory (57% probability) but assigned near-equal probability to four scorelines, indicating limited discriminative power for exact scores—a known limitation of Poisson models without in-game dynamics.

## Discussion and Limitations

**Stationarity assumption:** The model assumes constant goal-scoring rates throughout matches. In reality, teams adjust tactics based on score state—leading teams often play conservatively, while trailing teams adopt riskier strategies in late stages. A more sophisticated model would partition matches into temporal segments with score-dependent transition dynamics.

Implementing such a model requires granular in-game event data (e.g., goals with timestamps), which was unavailable for this analysis.

**Unmodeled covariates:** The current specification omits several potentially relevant factors:
- **Player-level information**: Individual player form, injuries, and substitutions are not explicitly modeled. Key absences (e.g., Neymar in Brazil's 2014 semifinal) can dramatically alter team strength.
- **In-game events**: Red/yellow cards that reduce team size or discipline are not accounted for in the pre-match rate parameter.
- **Venue effects**: While home advantage could be included as a covariate, fine-grained venue characteristics (pitch dimensions, altitude, climate) are not captured.
- **Psychological factors**: Team rivalries, coaching changes, morale, and tournament pressure effects remain latent.

Incorporating these would require richer feature engineering and potentially player-level hierarchical models, at the cost of increased model complexity and data requirements.

Despite these constraints, the model produces interpretable posterior distributions that align with domain intuition, making it suitable for quantifying uncertainty in match predictions.

## Implementation

The implementation uses JAX for JIT-compiled numerical integration, achieving approximately 15× speedup over naive NumPy implementations. See the [Colab notebook](https://colab.research.google.com/drive/1b-o\_ACHLYiweYk\_Ynp\_qxcWvZ2Am7-At) for interactive code and [GitHub repo](https://github.com/notadib/soccer-wc-pred) for the full source.
