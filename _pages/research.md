---
layout: archive
title: "Research"
permalink: /research/
author_profile: true
---

I build long-horizon agents that improve through [expert iteration](https://arxiv.org/abs/1705.08439) in context space, adapting a frozen model across many rounds by updating persistent state from verified reward signals rather than model weights.

Using this approach, my agents have discovered 150+ vulnerabilities in widely used OSS projects including Next.js, pnpm, MetaMask, and LiteLLM; improved the upper bound for [an open math problem](https://github.com/Neehan/zhang-zagier-82a); and topped the [Spider 2.0 dbt benchmark](https://github.com/SignalPilot-Labs/SignalPilot) for data science tasks. Earlier, my master's thesis work at MIT focused on model robustness under distribution shift.

{% if site.author.googlescholar %}
  <div class="wordwrap">My papers are below, and also on <a href="{{site.author.googlescholar}}">my Google Scholar profile</a>.</div>
{% endif %}

## Preprints

* Beyond the Model: How Harness Design Varies with LLM Performance on the 2026 International Mathematical Olympiad. <br>
  **Adib Hasan**, Akashnil Dutta, Tarik Adnan Moon. <br>
  *Preprint*, 2026. <br>
  [Code & Data](https://github.com/SignalPilot-Labs/AutoFyn)

* AutoFyn Technical Report: Non-Parametric Expert Iteration for Long-Horizon Agents. <br>
  **Adib Hasan**, Daniel Schaffield, Akashnil Dutta, Tarik Adnan Moon. <br>
  *Preprint*, 2026. <br>
  [Code](https://github.com/SignalPilot-Labs/AutoFyn)

## Publications

* VITA: Variational Pretraining of Transformers for Climate-Robust Crop Yield Forecasting. <br>
  **Adib Hasan**, Mardavij Roozbehani, Munther Dahleh. <br>
  *AAAI Conference on Artificial Intelligence (AAAI)*, 2026. <br>
  <span style="color: red;">Oral (Top 5%)</span> <br>
  [Arxiv](https://arxiv.org/abs/2508.03589) \| [GitHub](https://github.com/Neehan/VITA) \| [Blogpost](/blog/vita/) \| [Slides](https://github.com/Neehan/VITA/blob/main/docs/Variational_Transformer_slides.pdf)

* Pruning for Protection: Increasing Jailbreak Resistance in Aligned LLMs Without Fine-Tuning. <br>
  **Adib Hasan**, Ileana Rugina, Alex Wang. <br>
  *BlackboxNLP Workshop at EMNLP*, 2024. <br>
  [Arxiv](https://arxiv.org/abs/2401.10862) \| [Dataset](https://huggingface.co/datasets/notadib/harmful-tasks)

* Graphettes: Constant-time determination of graphlet and orbit identity including (possibly disconnected) graphlets up to size 8. <br>
  **Adib Hasan**, Po-Chien Chung, Wayne Hayes. <br>
  *PLoS ONE*, 2017. <br>
  [Journal](https://journals.plos.org/plosone/article?id=10.1371/journal.pone.0181570)


<!-- {% include base_path %}

{% for post in site.publications reversed %}
  {% include archive-single.html %}
{% endfor %} -->
