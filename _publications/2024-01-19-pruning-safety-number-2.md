---
title: "Pruning for Protection: Increasing Jailbreak Resistance in Aligned LLMs Without Fine-Tuning"
collection: publications
permalink: /publication/2024-01-19-pruning-safety-number-2
excerpt: 'We demonstrated that moderate pruning of the llms result in higher resistance against jailbreaking prompts'
date: 2017-08-23
venue: 'Arxiv Preprint'
paperurl: 'https://arxiv.org/abs/2401.10862'
citation: '**Adib Hasan**, Ileana Rugina, & Alex Wang. (2024). Pruning for Protection: Increasing Jailbreak Resistance in Aligned LLMs Without Fine-Tuning.'
---
## Abstract
Large Language Models (LLMs) are vulnerable to 'Jailbreaking' prompts, a type of attack that can coax these models into generating harmful content. In this paper, we show that pruning up to 20% of the attention layer parameters with WANDA (Sun et al.) markedly increases their resistance to such attacks without fine-tuning and with negligible loss of performance in standard benchmarks. Intriguingly, we discovered that the safety gains post-pruning correlate to the initial safety training level of the model. This suggests WANDA pruning has a regularizing effect, which we reproduced with statistical significance for linear models. To systematically evaluate safety, we curate a dataset of 225 harmful tasks across five categories, inserted into ten different Jailbreaking prompts. Our analysis shows that pruning aids LLMs in concentrating attention on task-relevant tokens in jailbreaking prompts. This approach to safety is orthogonal and complementary to existing adversarial defense methods.