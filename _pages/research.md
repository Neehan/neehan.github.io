---
layout: archive
title: "Research"
permalink: /research/
author_profile: true
---

I’m interested in test-time scaling and reasoning, especially how to make AI agents solve harder problems by working on them longer. This includes building agents that remain effective over many hours, understanding what limits their progress, and predicting when more compute will help. I like combining strong theoretical foundations with systems and experiments that test the underlying assumptions.

Recently, I built an autonomous agent system called [AutoFyn](/blog/autofyn/) that has discovered 150+ vulnerabilities in major open-source projects, including MetaMask, LiteLLM, and pnpm. I also co-developed [Discovery–Execution](/blog/discovery-execution/), a framework for predicting reasoning performance across compute allocations. Earlier, my master’s work at MIT focused on robustness under distribution shift. This included [VITA](/blog/vita/), selected for an oral presentation at AAAI 2026.

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
    color: #356aa0;
    font-size: 0.72rem;
    font-weight: 700;
    letter-spacing: 0.08em;
    line-height: 1.35;
    text-transform: uppercase;
  }
  .paper-visual:focus-visible { outline: 3px solid currentColor; outline-offset: 4px; }
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

{% for section in site.data.research %}
<section class="research-section">
  <h2>{{ section.title | escape }}</h2>
  {% for paper in section.papers %}
    {% include research-paper.html paper=paper %}
  {% endfor %}
</section>
{% endfor %}
