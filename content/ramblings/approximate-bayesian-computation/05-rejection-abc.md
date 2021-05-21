---
title: An introduction to Rejection ABC
linktitle: Rejection ABC
toc: true
type: docs
date: "2019-05-05T00:00:00+01:00"
draft: false
menu:
  abc:
    parent: 1) Foundations of ABC
    weight: 5

# Prev/next pager order (if `docs_section_pager` enabled in `params.toml`)
weight: 5
---
$\newcommand{\ystar}{y^{\*}}
\newcommand{\Ycal}{\mathcal{Y}}
\newcommand{\isample}{^{(i)}}
\newcommand{\kernel}{p\_{\epsilon}(\ystar \mid y)}
\newcommand{\tkernel}{\tilde{p}\_{\epsilon}(\ystar \mid y)}
\newcommand{\jointABCpost}{p_\{\epsilon}(\theta, y \mid \ystar)}
\newcommand{\like}{p(y \mid \theta)}
\newcommand{\prior}{p(\theta)}
\newcommand{\truepost}{p(\theta \mid \ystar)}
\newcommand{\ABCpost}{p\_{\epsilon}(\theta \mid \ystar)}
\newcommand{\ABClike}{p\_{\epsilon}(\ystar \mid \theta)}
\newcommand{\kerneltilde}{\tilde{p}\_{\epsilon}(\ystar \mid y)}
\newcommand{\zkernel}{Z\_{\epsilon}}
\newcommand{\truelike}{p(\ystar \mid \theta)}$

### The Uniform Kernel: Rejection ABC
If we choose the ABC kernel to be a **uniform kernel** then we obtain Rejection-ABC. A uniform kernel is essentially an **indicator function** determining whether the true observation $\ystar$ is within an $\epsilon$-ball of the pseudo-observation $y\sim \like$ or not.
$$
\kernel=  \frac{1}{\text{Vol}(\epsilon)}\mathbb{I}\_{B\_{\epsilon}(y)}(\ystar) \propto 
\begin{cases}
1 & \ystar\in B\_{\epsilon}(y) \\ \newline
0 & \ystar \notin B\_{\epsilon}(y)
\end{cases}
$$

In the expression above $B\_{\epsilon}(y)$ is a ball of radius $\epsilon$ centered around the auxiliary data $y$: it contains all the data $y'\in \Ycal$ such that it is at a distance less than or equal to $\epsilon$, our tolerance. The distance is computed using a distance metric $\rho(\cdot, \cdot)$ which is usually the Euclidean one
$$
B\_{\epsilon}(y) = \left\\{y'\in\Ycal \\,\\,: \\,\\, \rho(y, y') \\,\leq \\, \epsilon\right\\}.
$$
The indicator function $\mathbb{I}\_{B\_{\epsilon}(y)}(\cdot)$ then is equal to $1$ whenever $\ystar$ is within an $\epsilon$-ball of $y$ and $0$ otherwise. Notice how we divide the indicator function by the $\text{Vol}(\epsilon)$, i.e. the volume of $B\_{\epsilon}(y)$ so that the kernel is normalized and is thus a valid probability density function.

To sample $N$ parameters from $\truepost$ then we can follow the Rejection-ABC algorithm. 

<img src="/rejectionabc.png" alt="Rejection ABC Pseudocode - Approximate Bayesian Computation" width="700"/>

Essentially the choice of an indicator function as our kernel implicitly defines an algorithm that either rejects or accepts samples. To see this, notice that this is one of those cases where in practice we can only evaluate an unnormalized kernel
$$
\tkernel = \mathbb{I}\_{B\_{\epsilon}(y)}(\ystar)
$$
so that each $\tilde{w}_n$ is either $1$ or $0$. Suppose out of $w_1, \ldots, w_N$ there are only $M \leq N$ non-zero weights. Then their sum is clearly $M$ and so this algorithm can be interpreted similarly to Algorithm $1$ where $w_n = \frac{1}{M}$ when $\rho(y_n, \ystar) \leq \epsilon$.

Of course, this is extremely inefficient in high dimensions as the probability of "hitting the data" $\ystar$ with our $\epsilon$-ball $B\_{\epsilon}(y)$ becomes negligible. If you compare this with Algorithm 1 you can see that accepted samples are those with weight of one $w_n=1$ while rejected samples have zero weight $w_n=0$.

As a side note, the posterior distribution targeted by Rejection Sampling is sometimes denoted $p(\theta \mid \rho(y, \ystar) \leq \epsilon)$ rather than the more general $\ABCpost$.

