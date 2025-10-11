---
title: "I looked into 16 Million Futures and Saw Argentina Win against Australia in 11 Million of Them"
excerpt: "Predicting FIFA World Cup 2022 match outcomes using Bayesian inference on Poisson processes."
collection: blog
date: 2022-12-01
---

*Note: After completing this work, I discovered [a similar model](https://eightyfivepoints.blogspot.com/2018/05/what-can-we-expect-from-21st-fifa-world.html) by Dixon and Coles. The key differences in my approach are the inclusion of a temporal drift term and full posterior estimation rather than maximum likelihood.*

I develop a probabilistic framework for predicting soccer match outcomes by modeling team goal-scoring rates as an autoregressive time series with Gaussian drift. The model treats each team's latent strength as evolving stochastically over time, with observations generated via a Poisson likelihood. Rather than fitting a neural network to learn these dynamics, I specify an interpretable Bayesian hierarchical model with explicit priors on drift variance and covariate effects. This design prioritizes posterior interpretability and uncertainty quantification over raw predictive performance.

## Problem Formulation

Let \\(g_n \in \mathbb{Z}_{\geq 0}\\) denote the number of goals scored by a team in match \\(n\\). We seek to model the conditional distribution \\(P(g_n \mid g_{1:n-1}, \mathbf{x}_{1:n})\\), where \\(\mathbf{x}_n\\) represents match-specific covariates (opponent strength, venue, etc.).

We adopt a Poisson likelihood for goal counts:

$$

g_n \mid \lambda_n \sim \text{Poisson}(\lambda_n)

$$

where \\(\lambda_n > 0\\) is the instantaneous goal-scoring rate for match \\(n\\). The key modeling challenge is to specify how \\(\lambda_n\\) depends on historical observations and covariates while accounting for temporal evolution in team quality.

We decompose \\(\lambda_n\\) into:
1. A latent team strength parameter \\(\iota_n \in \mathbb{R}\\) that evolves as a Gaussian random walk
2. A linear adjustment term \\(\boldsymbol{\alpha}^\top \mathbf{x}_n\\) capturing match-specific effects

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

Consider a simplified scenario where team A plays only team B repeatedly. We model team A's goal-scoring rate \\(\lambda_n\\) in match \\(n\\) as evolving according to:

$$

\begin{align}
g_n &\sim \text{Poisson}(\lambda_n)\\
\lambda_n &\sim \mathcal{N}(\lambda_{n-1}, \sigma^2)\\
P(\lambda_n \mid g_n, g_{n-1},\ldots) &\propto P(g_n\mid\lambda_n) \cdot P(\lambda_n \mid g_{n-1},\ldots)
\end{align}

$$

The drift term \\(\lambda_n \sim \mathcal{N}(\lambda_{n-1}, \sigma^2)\\) accounts for temporal changes in team quality due to lineup adjustments, player form, and experience accumulation. The posterior update can be computed numerically or via MCMC sampling.

For example, suppose Argentina scores 2 goals in match 1. Our posterior for \\(\lambda\\) concentrates around 2. In match 2, they score 1 goal; the posterior shifts left. In match 3, they score 3 goals; it shifts right, but with narrower variance as we accumulate more observations.

## Full Model: Variable Opposition

The simplified model fails when teams face different opponents. If Brazil plays South Korea and then Argentina, the observed goal-scoring rates cannot be directly compared without accounting for opponent strength.

We decompose team performance into:
1. **Intrinsic strength** \\(\iota_n\\): Team quality against a standardized baseline opponent
2. **Match-specific adjustments**: Covariates capturing opponent strength, match importance, venue effects, and player availability

Let \\(\mathbf{x}_n\\) denote the covariate vector for match \\(n\\), including:
- ELO rating difference \\(E_n\\)
- Match importance \\(m_n\\) (e.g., World Cup vs. friendly)
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

Prior specification rationale:
- **Coefficient prior** \\(\mathcal{N}(0, 2^2)\\): Appears weakly informative, but the exponential link function amplifies the effect. A coefficient two standard deviations from zero scales \\(\lambda_n\\) by a factor in \\([e^{-4}, e^4] \approx [0.02, 54.6]\\).
- **Drift variance** \\(\text{Gamma}(0.5, 1)\\): Mean and variance of 0.5 reflect the expectation of gradual evolution in team strength between consecutive matches.
- **Initial strength** \\(\mathcal{N}(0.4, 0.7^2)\\): World Cup matches average 2.8 goals total, suggesting approximately 1.4 goals per team. Setting \\(\iota_0 \approx \log(1.4) \approx 0.34\\) with moderate uncertainty.

## Results: Argentina vs. Australia

I fit separate models for Argentina and Australia using historical match data from [eloratings.net](http://eloratings.net). Posterior inference was performed over a discretized parameter space of 200 × 200 × 400 = 16 million configurations.

**Inferred model parameters for Argentina:**
- \\(\alpha_1\\) (ELO coefficient): Large positive value, confirming that rating differentials strongly predict goal-scoring rates.
- \\(\alpha_2\\) (match importance): Small negative coefficient, suggesting Argentina scores marginally fewer goals in high-stakes matches, likely due to facing stronger opposition in tournaments.
- \\(\iota_n\\) trajectory: The 2002 and 2006 national teams exhibited peak intrinsic strength in recent history.

**Match prediction:**
- Most likely scorelines: 1–0, 2–0
- \\(P(\text{Argentina win}) = 70.69\%\\)
- \\(P(\text{Australia win}) = 9.97\%\\)
- \\(P(\text{Draw}) = 19.34\%\\)

Interpretation: In 11.3 million of the 16 million sampled futures, Argentina outscored Australia.

## Model Validation: Brazil vs. Serbia

To assess predictive performance, I applied the model to Brazil's opening World Cup match against Serbia (training only on pre-match data). The model assigned near-equal probability to four scorelines: 1–0, 2–0, 1–1, and 2–1. The actual result was 2–0.

The model correctly predicted Brazil's victory with 57% probability. However, the near-uniform distribution over several scorelines indicates limited discriminative power for exact score prediction—a known limitation of Poisson-based match models without in-game contextual information.

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

Full code available at [github.com/notadib/soccer-wc-pred](https://github.com/notadib/soccer-wc-pred). The implementation uses JAX for JIT-compiled numerical integration, achieving approximately 15× speedup over naive NumPy implementations.

A more detailed exposition of this work is available in the [original Medium article](https://medium.com/@notadib/i-looked-into-16-million-futures-and-saw-argentina-win-against-australia-in-11-million-of-them-c336574ae574).
