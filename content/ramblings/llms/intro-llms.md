---
title: High-Level Overview
linktitle: LLMs
toc: true
type: docs
date: "2019-05-05T00:00:00+01:00"
draft: false
menu:
  llms:
    parent: 1) High-Level Overview
    weight: 1

# Prev/next pager order (if `docs_section_pager` enabled in `params.toml`)
weight: 1
---
### <span style="color:#2AB7CA">Vocabulary</span>
I will write $\mathcal{V}$ to denote a **vocabulary**: a set of objects, known as **tokens**. For instance:
 1. $\mathcal{V}$ could contain the alphabet, digits $0$-$9$, and the remaining symbols on your keyboard. 
 2. $\mathcal{V}$ could additionally contain all the words in the Oxford dictionary.
 3. $\mathcal{V}$ could additionally contain all the suffixes and prefixes such as `"ing"` and `"un"`.
 
In practice, the number of tokens in the dictionary is around $50k$, although it varies a lot based on implementation. In code I will write `n_vocab` for the size of the vocabulary, in math formulas $n_{\text{vocab}}$, you can find a list of notation later in this section of the course.

I will use the symbol $\mathbf{t}$ to denote a token in the vocabulary, $\mathbf{t}\in\mathcal{V}$.

### <span style="color:#2AB7CA">Token Indices</span>
In reality the LLM does not work directly with the tokens, since these are typically *strings* and we want to work with **numbers**. Instead, we **sort the vocabulary** into a long sequence of tokens (the order is not important)
$$
\mathcal{S} := (\mathbf{t}_1, \ldots, \mathbf{t}\_{n\_{\text{vocab}}})
$$
and then we associate each token with each index, so that $i = 10$ identifies the $\mathbf{t}\_{10}$, the $10\^{\text{th}}$ token in the sorted vocabulary $\mathcal{S}$. I will call these indices simply **token indices**. The *set* of token indices is
$$
\mathcal{I} := \left\\{1, 2, 3, \ldots, n\_{\text{vocab}}\right\\}.
$$


> The input to the LLM are token indices.

### <span style="color:#2AB7CA">Embeddings</span>
I hope you agree that it is a bit awkward to work with **integers**. Internally, the LLM will associate to each of these indices a vector. This vectors will be known as **embeddings** of the tokens and they will have length $n_{\text{emb}}$, which will be chosen by the researcher. Therefore each token $s_i$ has an associated embedding vector $\mathbf{e}_i$. This happens *internally* the LLM, remember, the input to the LLM are the token indices.

### <span style="color: #FE4A49">Large Language Model</span>
> A Large Language Model (LLM) is a function that maps a sequence of tokens indices to a probability vector over the $\mathcal{S}$, representing the probability of the next token index in the sequence.

The LLM accepts sequences of token indices of length up to $T$, where $T$ is known as the **context size**, which is chosen by the researcher based on performance and hardware considerations. I will write
$$
\mathcal{S}\^{\leq T} := \left\\{(s_1, \ldots, s_t)\\,:\\, 1\leq t \leq T, \\, t\in \mathbb{Z}_+\right\\}
$$
for the domain of the Large Language Model. A LLM is a parametrized function $\text{LLM}\_\theta:\mathcal{V}\^{\leq T}\to [0, 1]^{n\_{\text{vocab}}}$.

### Training 
The parameters $\theta$ are initialized randomly using various heuristics and then they are learned in an unsupervised (or rather, self-supervised) fashion using back-propagation on a loss function, typically the **cross-entropy** loss. 

As usual, training is done using **batches** of data but for now we sweep that under the rag. Given a sequence of tokens $(s_1, \ldots, s_t)$ from the training data (e.g. a collection of documents) the LLM will obtain the probability vector $\boldsymbol{\alpha} = \text{LLM}\_\theta(s\_1, \ldots, s\_t)$ and then sample a token index from the corresponding categorical distribution $\widehat{s}\_{t+1}\sim \text{Categorical}(\boldsymbol{\alpha})$. These two will be compared using cross-entropy 

This will then be compared with the token that actually comes next in our data, which we denote $s_{t+1}$
$$

$$

The LLM function will be denoted $\text{LLM}_\theta$, where $\theta$ are the parameters of the model, and the input sequence of tokens $\mathbf{s}_t = (s_1, \ldots, s_t)^\top$, which is an element of $\mathcal{V}^t$. Importantly, the input sequence 

Mathematically, given a positive integer $T$ known as the **context size** , a LLM is a parametrized function $\text{LLM}\_\theta: \mathcal{V}\^{T}\to\mathbb{R}\_{\geq 0}^{n\_{\text{vocab}}}$ 

Mathematically, a LLM is a parametrized function $\text{LLM}_\theta: S_t \mapsto \boldsymbol{\alpha}$, where $S_t = (s_1, \ldots, s_t)^\top\in\mathcal{V}^t$ is a sequence of tokens from the vocabulary and $\boldsymbol{\alpha} = (\alpha\_1, \ldots, \alpha\_{n\_{\text{vocab}}})\^\top$ is a vector whose entries are non-negative and sum up to one
$$
\sum\_{i=1}\^n \alpha_i = 1, \\quad\text{and}\qquad \alpha_i \geq 0, \quad \forall i = 1, \ldots, n\_{\text{vocab}}.
$$
The 
