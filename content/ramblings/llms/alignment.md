---
title: Reinforcement Learning from Human Feedback
linktitle: RLHF
toc: true
type: docs
date: "2019-05-05T00:00:00+01:00"
draft: false
menu:
  llms:
    parent: 5) Alignment
    weight: 14

# Prev/next pager order (if `docs_section_pager` enabled in `params.toml`)
weight: 14
---
**Reinforcement Learning from Human Feedback** (RLHF) was introduced in the [InstructGPT](https://arxiv.org/abs/2203.02155) paper. Suppose that $\pi\_\theta$ is the distribution learned by a  language model after Unsupervised Pre-Training (UPT) and Supervised Fine-Tuning (SFT), that is
$$
\pi\_\theta(x) = \text{Categorical}\left(\text{softmax}\left(\text{LLM}\_{\theta}(x)\right)\right).
$$
We would like to *align* our model to human preferences (or rather, contracted human labelers). RLHF frames this task as reinforcement learning.

### Reinforcement Learning Recap
Reinforcement learning works as follows. In an environment with state $s_t$ there is an agent that can take actions $a_t$ in this environment according to a distribution $\pi_\theta(a_t\mid s_t)$, known as **policy**. After the agent performs an action, the environment then returns to the agent a state after the action $s_{t+1}$ and a reward $r_t$. 

<img src="/RL.png" alt="drawing" width="300"/>

The aim is to learn a policy such that the expected reward is maximized
$$
\max_{\theta} \\,\\,\mathbb{E}\_{\pi_\theta(a_t\mid s_t)}[R(S_t)]
$$
As we will see later, in the case of Language Modelling:

1. Agent: Pre-trained and fine-tuned Language model
2. State: Prompt
3. Policy: Next-token distribution given the prompt
4. Next-state: Sample from the categorical distribution represented by the LM

### Baseline, Policy and Reward Models
We take our SF-tuned language model $\pi\_{\theta}$ and create three identical copies:

1. **Baseline** $\pi_0$:  will have frozen parameters $\theta$ (unchanged through the entire RLHF process).
2. **Policy** $\pi_\theta$: Will end up being our aligned model, we change its parameters during RLHF.
3. **Reward Model** $r_\phi$: We copy $\pi_\theta$ a modification: we replace the last un-embedding layer: at the end of $\text{LLM}_\theta$ it will have one last linear layer that maps from $\mathbb{R}\^{n\_{emb}}$ to $\mathbb{R}\^{n\_{\text{vocab}}}$ so that we have one logit per token in the dictionary. This final linear layer is removed and replaced with a linear layer that maps $\mathbb{R}\^{n\_{emb}}$ to $\mathbb{R}$: we want a **scalar reward**. We call all of its parameters $\phi$.

*However*, in the original InstructGPT paper they said that for $r_\phi$ they used the same architecture as $\text{LLM}_\theta$, except that they made it much smaller ($6B$ parameters compared to $175B$). This is because it allows for faster reward modelling training and because using a larger reward model proved unstable. 

### Dataset of Human Preferences
For the next step, we focus exclusively on $r_\phi$. To generate a dataset of human preferences $\mathcal{D}\_{\text{preferences}}$ we follow these steps

1. For each prompt of interest $x_n$, sample twice from the policy $y\_{n, 1}, y\_{n, 2}\sim \pi\_{\theta}(\cdot\mid x_n)$
2. Ask human labelers to rank all of the outputs. The preferred output is now labelled $y\_{n, w}$ and the not-preferred output is labelled $y\_{n, l}$ where "w" and "l" stand for "winner" and "loser". Mathematically, we write $y\_{n, w} \succ y\_{n, l}$.

This gives us the following human preferences dataset
$$
\mathcal{D}\_{\text{preferences}} = \left\\{(x\_n, y\_{n, w}, y\_{n, l})\\,:\\, n=1, \ldots, n\_{\text{preferences}}\right\\}
$$

### Training the Reward Model



