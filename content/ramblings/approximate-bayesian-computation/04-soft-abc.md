---
title: Soft ABC
linktitle: Soft ABC
toc: true
type: docs
date: "2019-05-05T00:00:00+01:00"
draft: false
menu:
  abc:
    parent: 1) Foundations of ABC
    weight: 4

# Prev/next pager order (if `docs_section_pager` enabled in `params.toml`)
weight: 4
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
\newcommand{\truelike}{p(\ystar \mid \theta)}
\newcommand{\Ebb}{\mathbb{E}}$

### A first ABC algorithm
The algorithm that I hinted at in the earlier sections can be summarized as follows: At each iteration we first sample from the prior, we then run the simulator with the new parameter value and finally feed the output of the simulator in the kernel which will give us a weight representing the similarity between $y$ and $\ystar$. Sometimes we can only evaluate the kernel up to a normalizing constant, that is we can only compute 
$$
\tkernel \propto \kernel
$$
In that case each weight will be un-normalized and so we will first compute all the weights and finally divide them by the sum of the weights, in a manner that resembles Importance Sampling. 

<img src="/generalabc.png" alt="General Approximate Bayesian Computation Algorithm with Kernel" width="700"/>

This algorithm defines a posterior approximation as a mixture of Dirac deltas
$$
\ABCpost = \sum\_{i=n}^N w_n \delta\_{\theta_n}(\theta)
$$
and it can then be used, for example, to approximate the expectation of a function of $\theta$ with respect to theparameter posterior
$$
\Ebb\_{\truepost}\left[g(\theta)\right] \approx \Ebb\_{\ABCpost}\left[g(\theta)\right] = \int g(\theta)\sum\_{n=1}^N w_n \delta\_{\theta_n}(\theta) d\theta  = \sum\_{n=1}^N g(\theta_n)w_n
$$

In the next section, we will see a special case of this framework: Rejection ABC, where we choose the indicator function as our kernel function so that samples are assigned either a weight of $1$ or a weight of $0$, i.e. they are either accepted or rejected. This explains the name of the algorithm described above. We call Soft ABC in contrast to Rejection ABC which works with "hard rejections", whereas Algorithm $1$ doesn't reject, it simply assigns a weight.


