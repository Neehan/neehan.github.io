---
title: "VITA: Variational Inference Transformer for Asymmetric Data"
excerpt: "A decoder-free variational pretraining framework for time-series forecasting under feature asymmetry."
collection: blog
date: 2025-08-01
tags:
  - theory
  - releases
---

In time-series forecasting, it’s common to have richer features during training than at deployment. Weather-based crop yield prediction exemplifies this: many satellite-derived datasets (e.g., NASA POWER) offer dozens of meteorological variables—31 in this case—for pretraining. However, operational yield datasets depend on fine-grained ground-station measurements, which typically include six or fewer variables. Despite how prevalent this setup is, most state-of-the-art time-series models (SimMTM, PatchTST, Chronos) assume *identical features across pretraining and deployment* and therefore fail to address this feature asymmetry.

VITA (Variational Inference Transformer for Asymmetric Data) solves this by learning latent representations that bridge the gap between training-time and deployment-time features. Unlike reconstruction-based pretraining, VITA trains a transformer encoder to output distributions over detailed features given only basic inputs, using a variational objective with a seasonal prior. Critically, no decoder is used as detailed weather serves directly as the target during pretraining. At fine-tuning, the encoder maps limited features to the same structured latent space for downstream prediction.

Applied to agricultural yield forecasting, VITA achieves \\(R^2 = 0.726\\) on extreme weather years (\\(p < 10^{-4}\\) vs. baselines), training in <2.5 hours on a single GPU with <2% parameter overhead.

## Problem Setup: Feature Asymmetry in Weather Forecasting

Let \\(\mathbf{x} \in \mathbb{R}^{T \times 6}\\) denote basic weather features (temperature, precipitation) observed over \\(T = 364\\) weeks, and \\(\mathbf{z}^\star \in \mathbb{R}^{T \times 31}\\) denote detailed satellite weather (radiation flux, surface albedo, cloud cover, wind speed, etc) available only during pretraining. Each sequence includes year and spatial coordinates.

**Datasets**

Consider \\( D\_w = \{(\mathbf{x}\_i, \mathbf{z}^\star\_i)\}\_{i=1}^{N\_w}\\) and \\(D\_y = \{(\mathbf{x}\_j, y\_j, \mathbf{y}^{\text{past}}\_j)\}\_{j=1}^{N\_y}\\) to pre-training and fine-tuning datasets, respectively, where \\(y\\) is crop yield and \\(\mathbf{y}^{\text{past}}\\) contains six years of historical yields (capturing soil quality and management practices).

**The asymmetry:** We have \\((\mathbf{x}, \mathbf{z}^\star)\\) pairs during pretraining but only \\(\mathbf{x}\\) at deployment. How do we learn representations from \\(\mathbf{x}\\) that generalize to downstream tasks?

## Architecture

VITA employs a transformer encoder that maps 364-week weather sequences into latent distributions for yield prediction:

$$
\begin{align}
\mathbf{x}_{\text{input}} &= \text{concat}(\mathbf{x}_{\text{weather}}, \text{year}, \text{coordinates}) \tag{1}\\
\mathbf{h}_{\text{weather}} &= E_\phi\big(\text{LinearProj}(\mathbf{x}_{\text{input}}) + \text{PosEmbed}(\cdot)\big) \tag{2}\\
[\boldsymbol{\mu}, \log \boldsymbol{\sigma}^2] &= \text{LinearProj}_{\mu,\sigma^2}(\mathbf{h}_{\text{weather}}) \tag{3}\\
\mathbf{z}_t &= \boldsymbol{\mu}_t + \boldsymbol{\sigma}_t \odot \boldsymbol{\epsilon}_t, \quad \boldsymbol{\epsilon}_t \sim \mathcal{N}(0, I) \tag{4}\\
\mathbf{z}_{\text{agg}} &= \sum_{k=1}^{364} \alpha_k \mathbf{z}_k, \quad \alpha_k = \text{softmax}\big(\text{MLP}_a(\mathbf{z}_k)\big) \tag{5}\\
\hat{y} &= \text{MLP}_y\big([\mathbf{z}_{\text{agg}}, \mathbf{y}^{\text{past}}]\big) \tag{6}
\end{align}
$$

The encoder \\(E\_\phi\\) is a standard transformer applied to weekly weather with positional embeddings. It outputs per-timestep Gaussians \\((\boldsymbol{\mu}\_t, \boldsymbol{\sigma}\_t)\\), samples latent states \\(\mathbf{z}\_t\\), then attention-pools them before predicting yield.

Notice that the architecture does not require input reconstruction (ie a decoder). The encoder directly outputs \\(q\_\phi(\mathbf{z} \mid \mathbf{x})\\), which will be matched against \\(\mathbf{z}^\star\\) during pretraining.

## Stage 1: Decoder-Free Variational Pretraining

Given \\((\mathbf{x}, \mathbf{z}^\star)\\) pairs, train the encoder to output a distribution over detailed weather from basic inputs:

$$
\mathcal{L}_{\text{pre}} = -\mathbb{E}_{(\mathbf{x}, \mathbf{z}^\star) \sim D_w}\big[\log q_\phi(\mathbf{z}^\star \mid \mathbf{x})\big] + \alpha \, \text{KL}\big[q_\phi(\mathbf{z} \mid \mathbf{x}) \,\|\, p_\theta(\mathbf{z})\big]
$$

**First term (variational likelihood):** Fit \\(q\_\phi\\) to predict observed detailed weather \\(\mathbf{z}^\star\\). Since \\(q\_\phi\\) is Gaussian, this is simply \\(\frac{1}{2\sigma^2} \|\mathbf{z}^\star - \boldsymbol{\mu}\_\phi(\mathbf{x})\|^2 + \text{const}\\).

**Second term (prior regularization):** Keep the latent space structured. Two choices:

**Standard prior:**

$$
p_\theta(\mathbf{z}) = \mathcal{N}(0, I)
$$

$$
\text{KL} = \frac{1}{2} \sum_{k} \left( \sigma_{\phi,k}^2 + \mu_{\phi,k}^2 - 1 - \log \sigma_{\phi,k}^2 \right)
$$

**Sinusoidal (seasonal) prior:**

$$
p_\theta(\mathbf{z}) = \mathcal{N}(\boldsymbol{\mu}_{\sin}, \text{diag}(\boldsymbol{\sigma}^2_{\sin}))
$$

$$
\mu_{\sin,k} = A_k \sin(\omega_k \cdot \text{week} + \theta_{0,k})
$$

$$
\text{KL} = \frac{1}{2} \sum_{k} \left( \frac{\sigma_{\phi,k}^2}{\sigma_{\sin,k}^2} + \frac{(\mu_{\phi,k} - \mu_{\sin,k})^2}{\sigma_{\sin,k}^2} - 1 - \log \frac{\sigma_{\phi,k}^2}{\sigma_{\sin,k}^2} \right)
$$

The seasonal prior bakes week-of-year structure into \\(\mathbf{z}\\) with learned parameters \\((A, \omega, \theta\_0, \sigma\_{\sin})\\).

**Feature masking:** To prevent trivial copying, randomly mask \\(10 \leq k \leq 25\\) of the 31 detailed variables during training. This forces robust latent representations.

**Result:** The encoder learns a seasonal, calibrated latent weather state from cheap inputs—without training a decoder.

## Stage 2: Variational Fine-Tuning for Yield

At fine-tuning, only \\((\mathbf{x}, y, \mathbf{y}^{\text{past}})\\) is available. Start from the pretrained encoder and optimize the ELBO for \\(p(y \mid \mathbf{x})\\):

$$
\mathcal{L}_{\text{yield}} = -\mathbb{E}_{q_\phi(\mathbf{z} \mid \mathbf{x})}\big[\log p_\theta(y \mid \mathbf{z})\big] + \beta \, \text{KL}\big[q_\phi(\mathbf{z} \mid \mathbf{x}) \,\|\, p_\theta(\mathbf{z})\big]
$$

Model yield as Gaussian: \\(p\_\theta(y \mid \mathbf{z}) = \mathcal{N}(y; \hat{y}, \sigma\_y^2)\\) where \\(\hat{y} = \text{MLP}\_y(\mathbf{z}\_{\text{agg}}, \mathbf{y}^{\text{past}})\\). This gives:

$$
\mathcal{L}_{\text{yield}} = (y - \hat{y})^2 + \beta \, \text{KL}\big[q_\phi(\mathbf{z} \mid \mathbf{x}) \,\|\, p_\theta(\mathbf{z})\big]
$$

The KL term keeps the latent space seasonal while fitting yields. Historical yields \\(\mathbf{y}^{\text{past}}\\) implicitly encode soil and management factors.

## Why This Works

Traditional time-series pretraining reconstructs masked features assuming the same features at deployment. VITA instead learns **what detailed weather would be** given basic observations, then uses that inferred state for prediction. The seasonal prior ensures \\(\mathbf{z}\\) captures week-of-year patterns; the KL penalty prevents collapse.

At deployment, only 6 basic weather variables are available, but the encoder maps them to the same 31-dimensional latent space learned during pretraining. The downstream MLP operates on this rich representation despite limited input features.

## Training Details

- **Windows:** 364 weeks (~7 years)
- **Pretraining:** 31 detailed variables; randomly mask 10–25
- **Fine-tuning:** 6 basic variables + 6 years historical yield
- **Loss weights:** \\(\alpha \approx 0.5\\) (pretrain), \\(\beta\\) tuned (fine-tune)
- **Architecture:** Transformer encoder with <2% parameter overhead
- **Compute:** Single L40S GPU, <2.5 hours

## Results

We evaluate on the five most extreme weather years across U.S. Corn Belt counties—where operational models fail most.

| Model | Mean \\(R^2\\) | Corn | Soybean |
|-------|---------|------|---------|
| **VITA-Sinusoidal** | **0.726** | **0.729±0.008** | 0.**722±0.005** |
| T-BERT (MSE; VITA arch.) | 0.677 | 0.660 ± 0.041 | 0.693 ± 0.011 |
| SimMTM | 0.665 | 0.642 ± 0.028 | 0.687 ± 0.018 |
| Chronos-Bolt-tiny | 0.573 | 0.525 ± 0.015 | 0.621 ± 0.017 |

All comparisons: \\(p < 10^{-4}\\) in paired tests. VITA's gains are largest in extreme years—precisely where accurate predictions matter for risk management.

**Generalization:**

- **Spatial:** Models pretrained on non-U.S. weather improve U.S. fine-tuning
- **Temporal:** Models trained on 1994–2009 remain accurate for 2014–2018

**Ablations:**

- No sinusoidal prior
- No pretraining
- Spatial Transfer: train on Non-US weather then test on Midwest Corn belt
- Year permutation: permute year labels on pretraining data to destroy temporal weather patterns so that model just cannot cheat by memorizing weather. 
- 4 years instead of 6 years of past data.


## Discussion

**Feature asymmetry as the learning signal:** VITA explicitly leverages the gap between rich training data and limited deployment data. This differs fundamentally from masked autoencoding, which assumes feature consistency and fails when fine-tuning on different features.

**Decoder-free design:** Using \\(\mathbf{z}^\star\\) directly as the target eliminates reconstruction artifacts and reduces compute. The encoder learns to infer detailed weather from basic inputs without decoding back to input space.

**Seasonal prior:** Baking week-of-year structure into \\(\mathbf{z}\\) via sinusoids provides inductive bias for agricultural cycles. The KL penalty keeps this structure even after fine-tuning on yield.

**Limitations:**

- Assumes yield is primarily weather-driven (management practices and pests matter too, though \\(\mathbf{y}^{\text{past}}\\) partially captures these)
- Performance depends on pretraining data covering analogous extreme events

Despite these constraints, VITA achieves state-of-the-art accuracy on extreme years using only public data (NASA POWER weather, USDA yields), with efficiency suitable for operational deployment.

## Conclusion

VITA demonstrates that decoder-free variational pretraining with seasonal priors effectively addresses feature asymmetry in time-series forecasting. By learning to infer rich latent states from limited inputs, VITA achieves robust performance under extreme conditions where operational models are most needed.

The broader lesson: many forecasting problems exhibit feature asymmetry between training and deployment. VITA's approach—using rich proxy targets during pretraining to learn transferable representations—offers a principled solution applicable across domains.
