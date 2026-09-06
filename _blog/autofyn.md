---
title: "AutoFyn and Expert Iteration in Context Space"
excerpt: "AutoFyn is a long-horizon agent that learns across fresh context windows from verified outcomes. It combines bounded context, persistent memory, independent review, and external verification in one system."
collection: blog
date: 2026-08-13
tags:
  - agents
  - research
  - software
---

[Paper](/files/autofyn_technical_report.pdf) \| [GitHub](https://github.com/SignalPilot-Labs/AutoFyn)

Frontier LLMs can often solve a difficult problem in one attempt. Yet they are much less reliable at working on the same problem for a whole day. The main obstacle is not only the model's intelligence. It is also how the agent uses context and learns from failed attempts.

Most agents keep appending every action, observation, and conclusion to one conversation. The context eventually becomes noisy. Important facts get buried among old plans and dead ends. Starting a new conversation or compacting the chat fixes the noise, but now the agent forgets what it learned from that history.

Another problem is that a model's self-review is also noisy. It can reinforce the model's mistakes over time and degrade its performance. Long message histories compound this effect.

I built [AutoFyn](https://github.com/SignalPilot-Labs/AutoFyn) to address both problems. It works in rounds like a Ralph loop. Every round starts with a fresh model session. The new session receives only a compact state written to disk, the repository, and the relevant artifacts. It does not inherit the previous transcript.

Within each round, specialized agents explore different approaches, make a plan, build a candidate, and review it. The result is then checked against the environment. A security exploit must execute. A data science submission must improve the benchmark score. A mathematical claim is tested against small cases or symbolic calculations. The model cannot declare itself correct.

The verified result is distilled back into persistent state. Successful approaches become part of the incumbent solution. Failed approaches remain marked as failures. Repeated mistakes become rules for later rounds. The live context is then discarded and the process starts again.

![The AutoFyn loop: fresh context, multi-agent search, independent review, external verification, and persistent memory](/images/autofyn_loop.png)

## Expert Iteration Without Weight Updates

This loop is inspired by [Expert Iteration](https://arxiv.org/abs/1705.08439). In classical Expert Iteration, an apprentice proposes actions, a stronger search procedure acts as the expert, and the apprentice is trained on the expert's results. The updated apprentice then guides a better search in the next iteration.

AutoFyn has the same structure, but it never changes the model weights. The apprentice is the policy induced by the model and its current persistent state. Orchestrated search produces candidate artifacts. Independent review and a task-specific verifier provide the expert signal. The accepted result is distilled into memory instead of model parameters. The important distinction here is that a verifier agent runs a mechanical command, such as a unit test suite or the Lean compiler, and does not provide a subjective review.

In our loop, the model remains frozen, but its effective policy changes because the next round receives different context. We call this non-parametric expert iteration in context space.

## Prior Work

The individual ideas behind AutoFyn have strong precedents. [Reflexion](https://arxiv.org/abs/2303.11366) showed that agents can learn from feedback stored as text rather than weight updates. [ExpeL](https://arxiv.org/abs/2308.10144) extracted reusable natural-language knowledge from past experience. [MemGPT](https://arxiv.org/abs/2310.08560) and [HiAgent](https://arxiv.org/abs/2408.09559) treated memory management as a central systems problem. [Context-Folding](https://arxiv.org/abs/2510.11967) showed that long trajectories can be compressed while preserving the useful outcome of a subtask.

There is also good evidence that these mechanisms are necessary. [Lost in the Middle](https://arxiv.org/abs/2307.03172) showed that models use long contexts unevenly. Work on [self-correction](https://arxiv.org/abs/2310.01798) found that reasoning can get worse when a model revises itself without external feedback.

My contribution is turning these insights into one operational package. AutoFyn combines fresh contexts, explicit memory, multi-agent search, independent review, external verification, and incumbent retention in one user-facing system. A user gives it a repository, a verifiable goal, and a time budget. The system can then work across many rounds without requiring the user to manage its context or approve every step.

## Results

![AutoFyn performance on IMO 2026 and Spider 2.0 dbt](/images/autofyn_paper.png)

We tested the same loop in security, mathematics, and data science.

In security audits, AutoFyn found more than 150 vulnerabilities across thirteen widely used open-source projects. These include Next.js, MetaMask, pnpm, Warp, LiteLLM, Langflow, Open WebUI, and RAGFlow. We submitted 43 advisories from these findings. Sixteen had been confirmed by maintainers when we wrote the report.

On the six problems from the 2026 International Mathematical Olympiad, AutoFyn reached 42 out of 42 with GPT-5.6 Sol and Claude Fable 5. It also improved substantially weaker models without changing their weights. GLM 5.2 increased from 20.7 in its provider's coding agent to 32.0 in AutoFyn. Claude Sonnet 5 increased from 23.3 to 29.7.

For data science, AutoFyn built and optimized the SignalPilot Agent autonomously. That agent scored 65.60 on the Spider 2.0 dbt benchmark and held the top position on the public leaderboard when the report was written.

## Why It Matters

These results suggest that model capability is not a single fixed number. The harness determines how much useful work a model can do before its context degrades, how it responds to failure, and what evidence it accepts as progress.

The most important requirement is a verifiable goal. AutoFyn works when the environment can return a grounded signal. Here are some examples.

1. The model proposes an exploit against a live system. It either fires or it does not.
2. The model proposes changes to a data science harness. Its benchmark score either improves or it does not.
3. The model proposes code changes. The test suite either passes or it does not.

In each scenario, the signal may be narrow, but it is reproducible and harder to game than the model's opinion of its own work.

AutoFyn does not guarantee that every round improves. A verifier can be incomplete and memory distillation can discard useful information. The system instead makes progress measurable and makes hill climbing possible. It keeps the best verified artifact, records what happened, and gives the next fresh context a better starting point.

That's all for today. If you like the concept, please give [AutoFyn](https://github.com/SignalPilot-Labs/AutoFyn) a try.
