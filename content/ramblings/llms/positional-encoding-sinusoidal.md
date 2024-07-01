---
title: Sinusoidal
linktitle: Sinusoidal
toc: true
type: docs
date: "2019-05-05T00:00:00+01:00"
draft: false
menu:
  llms:
    parent: 1) Positional Encoding
    weight: 2

# Prev/next pager order (if `docs_section_pager` enabled in `params.toml`)
weight: 2
---
The sinusoidal positional encoding was introduced in the original [Transformer](https://arxiv.org/pdf/1706.03762) paper. It is a **deterministic**, it is not learned and encodes **absolute positions** of tokens within the context. In the paper, they compare it with learned PE and find identical results, but hypothesize that sinusoidal encoding might:
1. learn to attend to *relative* positions
2. extrapolate to sequence lengths longer than the ones found during training.

Suppose $\text{pos}\in\{0, \ldots, \text{context-length}\}$ is the index of a token within the context and suppose that our embedding dimension is $d_{\text{embedding}}$. Then the position of the token, $i$, is embedded as a vector $\text{SPE}(\text{pos})$ of dimension $d_{\text{embedding}}$ whose even and odd entries are
\begin{align}
    \text{SPE}(\text{pos})_{i} = 
    \begin{cases}
        \sin\left(\frac{\text{pos}}{10\,000\^{i/d\_{\text{embedding}}}}\right) & \text{ if } i \text{ even} \\\\
        \\\\
        \cos\left(\frac{\text{pos}}{10\,000\^{i/d\_{\text{embedding}}}}\right) & \text{ if } i \text{ odd}
    \end{cases}
\end{align}

