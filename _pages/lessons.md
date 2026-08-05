---
layout: archive
title: "To Be Human"
permalink: /lessons/
author_profile: true
---

Here is a running list of lessons that I have learned over the years from my mistakes. This list isn't complete, as I am very capable at finding more spectacular ways to fail.

## Life Principles

1. Life is short; time is your most valuable currency.
2. Take full responsibility for your actions and own the outcomes.
3. In the grand scheme of things, your existence is utterly meaningless. Only the pursuit of dreams gives life its [meaning](https://en.wikipedia.org/wiki/Existentialism). 
4. Attachment is the cause of all [suffering](https://en.wikipedia.org/wiki/Four_Noble_Truths).
5. Be kind to others without ever expecting anything in return.

## Work Principles

1. Don't reinvent the wheel. Ask the best human (or AI) expert to identify the existing solutions.
2. Never micromanage, and never allow yourself to be micromanaged.
3. Be aware of your blind spots and collaborate with people who complement your weaknesses.
4. People think and work differently. Create a space for them to explain their perspectives.

## Machine Learning
1. When in doubt, always start with **[AdamW](https://docs.pytorch.org/docs/stable/generated/torch.optim.AdamW.html)** with learning rate between $[10^{-4}, 10^{-3}]$ and a linear warm-up.
2. Adam is a memory-hog because it tracks EMA estimates of the first two moments of the gradient.
3. Covariance is the dimension-scaled dot product of two mean-centered vectors and measures their directional alignment.
4. Multi-headed self-attention can be viewed as a dynamic CNN whose kernel spans the entire input, with each attention head acting as a separate channel.
5. When increasing the number of tokens per batch, a useful initial heuristic is: [$\mathrm{lr} \sim \sqrt{\mathrm{tokens}}$](https://arxiv.org/pdf/1705.08741). 
6. Larger models require more data and computational resources but not linearly. Your model might be [undertrained](https://arxiv.org/pdf/2001.08361) for its size.
7. Floating-point precision varies with magnitude, so final result of arithmetic depends on the order of operations, ie. $(a + b) + c \neq a + (b + c)$. This makes many algorithms [non-deterministic](https://thinkingmachines.ai/blog/defeating-nondeterminism-in-llm-inference/) on parallel GPU kernels. For example, as of 2025, [there is no deterministic CUDA kernel for `torch.cumsum`](https://github.com/pytorch/pytorch/issues/89492).
