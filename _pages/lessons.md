---
layout: archive
title: "Life Lessons"
permalink: /lessons/
author_profile: true
---

Here is a running list of lessons that I have learned over the years from making embarrassing mistakes. This list isn't complete, as I always surprise myself by discovering new ways to fail.

## Work Principles

1. Don't reinvent the wheel—build the cart instead. Ask the best human (or AI) expert to identify existing solutions before building anything.
2. Never micromanage, and never allow yourself to be micromanaged.
3. Be aware of your blind spots and hire people who complement your weaknesses.
4. People think and work differently. Create a space for them to explain their perspectives.

## Life Principles

1. Life is short; time is your most valuable currency.
2. Accept that you will have shortcomings. Take full responsibility for your actions and own the outcomes.
3. In the grand scheme of things, your existence is utterly meaningless. Only the pursuit of dreams gives life its [meaning](https://en.wikipedia.org/wiki/Existentialism). 
4. Attachment is the cause of all [suffering](https://en.wikipedia.org/wiki/Four_Noble_Truths).
5. Kindness costs little, yields much.

## Machine Learning Training
1. When in doubt, always start with **[AdamW](https://docs.pytorch.org/docs/stable/generated/torch.optim.AdamW.html) (not Adam)** with learning rate between 1e-4 and 1e-3 and a linear warm-up. 
2. Scale your learning rate along with batch size: [_lr ~ sqrt(batch size)_](https://arxiv.org/pdf/1705.08741). **Note:** for small batch size like 32, 64 etc, linear scaling often leads to [better results](https://arxiv.org/pdf/1404.5997).
3. Larger models require more data and computational resources but not linearly. Your model might be [undertrained](https://arxiv.org/pdf/2001.08361) for its size.
