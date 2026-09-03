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

<style>
  .research-section { margin-top: 2.75rem; }
  .research-section h2 { margin-bottom: 1.35rem; }
  .paper-card {
    display: grid;
    grid-template-columns: minmax(180px, 26%) minmax(0, 1fr);
    gap: 1.75rem;
    align-items: center;
    margin-bottom: 2.25rem;
  }
  .paper-visual {
    box-sizing: border-box;
    display: flex;
    min-height: 150px;
    align-items: flex-end;
    padding: 1.25rem;
    overflow: hidden;
    border: 1px solid rgba(0, 0, 0, 0.08);
    border-radius: 10px;
    color: #fff;
    font-size: 0.72rem;
    font-weight: 700;
    letter-spacing: 0.08em;
    line-height: 1.35;
    text-transform: uppercase;
  }
  .paper-visual img {
    width: 100%;
    height: 150px;
    object-fit: contain;
  }
  .paper-visual--image { min-height: 0; padding: 0.5rem; background: #fff; }
  .paper-details { min-width: 0; }
  .paper-title { margin: 0 0 0.45rem; font-size: 1.05rem; line-height: 1.35; }
  .paper-authors, .paper-venue, .paper-links { margin: 0.3rem 0; line-height: 1.55; }
  .paper-venue { font-style: italic; }
  .paper-note { color: #c62828; font-style: normal; }
  .paper-links a { white-space: nowrap; }
  @media (max-width: 700px) {
    .paper-card { grid-template-columns: 1fr; gap: 1rem; }
    .paper-visual { min-height: 120px; }
  }
</style>

<section class="research-section">
  <h2>Test Time Scaling</h2>
  <article class="paper-card">
    <div class="paper-visual paper-visual--image"><img src="/images/autofyn_paper.png" alt="AutoFyn interface running a long-horizon security audit"></div>
    <div class="paper-details">
      <h3 class="paper-title">AutoFyn Technical Report: Non-Parametric Expert Iteration for Long-Horizon Agents</h3>
      <p class="paper-authors"><strong>Adib Hasan</strong>, Daniel Schaffield, Akashnil Dutta, Tarik Adnan Moon</p>
      <p class="paper-venue">Preprint, 2026</p>
      <p class="paper-links"><a href="https://github.com/SignalPilot-Labs/AutoFyn/blob/production/results/autofyn_technical_report.pdf">paper</a> / <a href="https://github.com/SignalPilot-Labs/autoFyn/">code</a></p>
    </div>
  </article>
</section>

<section class="research-section">
  <h2>Robustness Against Adversarial Distribution Shift</h2>
  <article class="paper-card">
    <div class="paper-visual paper-visual--image"><img src="/images/vita_paper.png" alt="Overview of VITA variational pretraining and crop-yield fine-tuning"></div>
    <div class="paper-details">
      <h3 class="paper-title">VITA: Variational Pretraining of Transformers for Climate-Robust Crop Yield Forecasting</h3>
      <p class="paper-authors"><strong>Adib Hasan</strong>, Mardavij Roozbehani, Munther Dahleh</p>
      <p class="paper-venue">AAAI Conference on Artificial Intelligence (AAAI), 2026 <span class="paper-note">— Oral (Top 5%)</span></p>
      <p class="paper-links"><a href="https://arxiv.org/abs/2508.03589">paper</a> / <a href="https://github.com/Neehan/VITA">code</a> / <a href="/blog/vita/">blog post</a> / <a href="https://github.com/Neehan/VITA/blob/main/docs/Variational_Transformer_slides.pdf">slides</a></p>
    </div>
  </article>

  <article class="paper-card">
    <div class="paper-visual paper-visual--image"><img src="/images/pruning_for_protection_paper.png" alt="Refusal rates of pruned and unpruned language models"></div>
    <div class="paper-details">
      <h3 class="paper-title">Pruning for Protection: Increasing Jailbreak Resistance in Aligned LLMs Without Fine-Tuning</h3>
      <p class="paper-authors"><strong>Adib Hasan</strong>, Ileana Rugina, Alex Wang</p>
      <p class="paper-venue">BlackboxNLP Workshop at EMNLP, 2024</p>
      <p class="paper-links"><a href="https://arxiv.org/abs/2401.10862">paper</a> / <a href="https://huggingface.co/datasets/notadib/harmful-tasks">dataset</a></p>
    </div>
  </article>
</section>

<section class="research-section">
  <h2>Algorithms and Data Structures</h2>
  <article class="paper-card">
    <div class="paper-visual paper-visual--image"><img src="/images/learned_binary_heap_paper.png" alt="Learned binary heap architecture and performance results"></div>
    <div class="paper-details">
      <h3 class="paper-title">Towards Learned Binary Heaps</h3>
      <p class="paper-authors"><strong>Adib Hasan</strong>, Angelos Pelecanos</p>
      <p class="paper-venue">MIT 6.890: Learning-Augmented Algorithms, 2019</p>
      <p class="paper-links"><a href="/files/6_890_Project.pdf">paper</a> / <a href="https://medium.com/@thankful_rose_ferret_864/how-we-sped-up-the-binary-heap-with-machine-learning-e10b3204e4e6">blog post</a></p>
    </div>
  </article>

  <article class="paper-card">
    <div class="paper-visual paper-visual--image"><img src="/images/graphette_paper.png" alt="Graph and bit-vector representations used by Graphettes"></div>
    <div class="paper-details">
      <h3 class="paper-title">Graphettes: Constant-time determination of graphlet and orbit identity including (possibly disconnected) graphlets up to size 8</h3>
      <p class="paper-authors"><strong>Adib Hasan</strong>, Po-Chien Chung, Wayne Hayes</p>
      <p class="paper-venue">PLoS ONE, 2017</p>
      <p class="paper-links"><a href="https://journals.plos.org/plosone/article?id=10.1371/journal.pone.0181570">paper</a></p>
    </div>
  </article>
</section>


<!-- {% include base_path %}

{% for post in site.publications reversed %}
  {% include archive-single.html %}
{% endfor %} -->
