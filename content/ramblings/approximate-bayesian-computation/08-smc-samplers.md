---
title: Sequential Monte Carlo Samplers
linktitle: SMC Samplers
toc: true
type: docs
date: "2019-05-05T00:00:00+01:00"
draft: false
menu:
  abc:
    parent: 3) SMC-ABC
    weight: 8

# Prev/next pager order (if `docs_section_pager` enabled in `params.toml`)
weight: 8
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

### Problem Set-Up
This is all taken from [Sequential Monte Carlo samplers](http://www.cs.ubc.ca/labs/lci/papers/docs2006/doucet_sequentialmontecarlosamplers.pdf). We have a collection of target distributions 
$$
\pi_n(x) = \frac{\gamma_n(x)}{Z_n}
$$
and we would like to sample from them sequentially in order to approximate expectations.

### Importance Sampling
We write target expectations using the Importance Sampling (IS) trick for a proposal density $\eta_n$
$$
\Ebb\_{\pi_n}[\phi] = \int_E \phi(x) \pi_n(x) dx = \frac{1}{Z_n} \int_E \phi(x) \gamma_n(x) dx = \frac{1}{Z_n} \int_E \phi(x) w_n(x) \eta_n(x) dx
$$
$$
Z_n = \int_E \gamma_n(x) dx = \int_E w_n(x) \eta_n(x) dx
$$
where we have defined importance weights as 
$$
w_n(x) = \frac{\gamma_n(x)}{\eta_n(x)}
$$
Therefore importance sampling uses the following particle approximation
$$
\eta_n^N(dx) = \frac{1}{N} \sum\_{i=1}^N \delta\_{X_n^{(i)}}(dx)
$$
Plugging this into the two expressions above we obtain
\begin{align}
    \Ebb\_{\pi_n}[\phi] = \frac{\Ebb\_{\eta_n}[\phi w_n]}{\Ebb\_{\eta_n}[w_n]} \approx \frac{\Ebb\_{\eta_n^N}[\phi w_n]}{\Ebb\_{\eta_n^N}[w_n]} = \frac{\sum\_{i=1}^N \phi(X_n^{(i)}) w_n(X_n^{(i)})}{\sum\_{i=1}^N w_n(X_n^{(i)})} = \sum\_{i=1}^N \phi(X_n^{(i)}) W_n(X_n^{(i)})
\end{align}
where we have defined the _normalized_ importance weights
$$
W_n(X_n^{(i)}) = \frac{w_n(X_n^{(i)})}{\sum\_{j=1}^N w_n(X_n^{(j)})}
$$

### Sequential Importance Sampling
In importance sampling, for each different target $\pi_n$ we would sample the particles afresh from $\eta_n$. This assumes that we can sample from $\eta_n\approx \pi_n$ and that we can evaluate $\eta_n$ in order to evaluate the unnormalized IS weights
$$
w_n(x) = \frac{\gamma_n(x)}{\eta_n(x)}
$$
In Sequential Importance Sampling (SIS) we start using IS at $n=1$ but then we build $\eta_n$ from the previous iteration. Specifically we do this:

- At time $n=1$  our target is $\pi_1$ and we use an IS proposal $\eta_1$ which we choose to approximate well $\pi_1$ (often we choose $\eta_1 = \pi_1$). This means we sample particles $X_1^{(1:N)}$ from $\eta_1$ and then compute the IS unnormalized weights 
$$
w_1(X_1^{(i)}) = \frac{\gamma_1(X_1^{(i)})}{\eta_1(X_1^{(i)})}
$$
- Suppose that at time $n-1$ we had a set of particles $\\{X\_{n-1}^{(i)}\\}$ sampled from $\eta\_{n-1}$. Our target at time $n$ is $\pi_n$. In order to propose a new set of particles we use a Markov Kernel $K_n(x' \mid x)$. We call the resulting distribution $\eta_n$. Notice that this distribution can be found using the property that a kernel $K_n$ operates on measures on the left
$$
\eta_n  = \eta\_{n-1} K_n \implies \eta_n(x') = \int_E \eta\_{n-1}(x) K_n(x' \mid x) dx
$$
Once we have sampled from the kernel to move the particles forward $X_n^{(i)} \sim K_n(\cdot \mid X\_{n-1}^{(i)})$, we need to compute the weights to account for the discrepancy of sampling from $\eta_n$ rather than $\pi_n$
$$
w_n(X_n^{(i)}) = \frac{\gamma_n(X_n^{(i)})}{\eta_n(X_n^{(i)})}
$$
However notice we can only do this if we can evaluate $\eta_n$.

In general, we cannot evaluate $\eta_n$ because it is defined in terms of an integral with respect to $x\_{1:n-1}$. Indeed consider $\eta_3$

\begin{align}
    \eta\_3(x_3) 
    &= \int\_E \eta_2(x_2) K_3(x_3 \mid x_2) dx_2 \newline
    &= \int\_E \left[\int_E \eta_1(x_1) K_2(x_2 \mid x_1) dx_1\right] K_3(x_3 \mid x_2) dx_2 \newline
    &= \int\_{E^2} \eta_1(x_1) K_3(x_3 \mid x_2) K_2(x_2 \mid x_1) dx_1 dx_2
\end{align}

In general $\eta_n$ will be 
$$
\eta_n(x_n) = \int\_{E^{n-1}} \eta_1(x_1) \prod\_{k=1}^{n} K_k(x_k \mid x\_{k-1}) d x\_{1:k-1}
$$
which is clearly intractable. 

### SMC sampler
Since the problem is integration with respect to $x\_{1:k-1}$ we "open up" the integral and instead consider its integrand. Rather than considering $\eta_n(x_n)$ which proposes a new set of particles $\\{X_n^{(i)}\\}$ from $\\{X\_{n-1}^{(i)}\\}$ we consider the proposal distribution $\eta_n(x\_{1:n})$ defined as
$$
\eta_n(x\_{1:n}) = \eta_1(x_1) \prod\_{k=2}^{n} K_k(x_k \mid x\_{k-1})
$$
We would now like to perform importance sampling. To do this, we need to extend the target from $\pi_n(x_n)$ to $\widetilde{\pi}_n(x\_{1:n})$. We do this by introducing backward kernels $L\_{n-1}(x\_{n-1} \mid x_n)$
$$
\widetilde{\pi}_n(x\_{1:n}) = \frac{1}{Z_n} \widetilde{\gamma}_n(x\_{1:n}) =  \frac{1}{Z_n} \gamma_n(x_n) \prod\_{k=1}^{n-1} L_k(x\_{k} \mid x\_{k+1})
$$
The IS weights would then become
\begin{align}
    w_n(x\_{1:n})
    &= \frac{\widetilde{\gamma}_n(x\_{1:n})}{\eta_n(x\_{1:n})} \newline
    &= \frac{\gamma_n(x_n) \prod\_{k=1}^{n-1} L_k(x\_{k} \mid x\_{k+1})}{\eta_1(x_1) \prod\_{k=2}^{n} K_k(x_k \mid x\_{k-1})} \newline
    &= \frac{\gamma_n(x_n) L\_{n-1}(x\_{n-1} \mid x_n) L\_{n-2}(x\_{n-2} \mid x\_{n-1}) \cdots L_1(x_1 \mid x_2)}{\eta_1(x_1) K_n(x_n \mid x\_{n-1}) K\_{n-1}(x\_{n-1} \mid x\_{n-2}) \cdots K_2(x_2 \mid x_1)} \newline
    &= \frac{\gamma_n(x_n) L\_{n-1}(x\_{n-1} \mid x_n)}{\gamma\_{n-1}(x\_{n-1}) K_n(x_n \mid x\_{n-1})} \cdot \frac{ \gamma\_{n-1}(x\_{n-1}) L\_{n-2}(x\_{n-2} \mid x\_{n-1}) \cdots L_1(x_1 \mid x_2)}{  \eta_1(x_1) K\_{n-1}(x\_{n-1} \mid x\_{n-2}) \cdots K_2(x_2 \mid x_1)} \newline
    &= \frac{\gamma_n(x_n) L\_{n-1}(x\_{n-1} \mid x_n)}{\gamma\_{n-1}(x\_{n-1}) K_n(x_n \mid x\_{n-1})} \cdot w\_{n-1}(x\_{1:n-1}) \newline
    &= \widetilde{w}_n(x_n \mid x\_{n-1}) w\_{n-1}(x\_{1:n-1})
\end{align}

where we have defined the incremental weight as 
$$
\widetilde{w}_n(x_n \mid x\_{n-1}) = \frac{\gamma_n(x_n) L\_{n-1}(x\_{n-1} \mid x_n)}{\gamma\_{n-1}(x\_{n-1}) K_n(x_n \mid x\_{n-1})}
$$

To summarize:

- Importance Sampling at time $n$ targets $\pi_n$. It samples particles $\\{X_n^{(i)}\\}$ _afresh_ from a proposal $\eta_n$ and computes weights _afresh_ as $w_n(X_n^{(i)}) = \gamma_n(X_n^{(i)}) / \eta_n(X_n^{(i)})$. For this to work, however, we need to be able to find proposals $\eta_n \approx \pi_n$ which is in general very hard.
- Sequential Importance Sampling also targets $\pi_n$ at time $n$. It tries to fix the problem of finding $\eta_n$ by using a local Markov Kernel $K_n(\cdot \mid X\_{n-1}^{(i)})$ to sample a new set of particles $\\{X_n^{(i)}\\}$ starting from $\\{X\_{n-1}^{(i)}\\}$. This, at time $n$, gives rise to the following proposal distributions
$$
\eta_n(x_n) = \int\_{E^{n-1}} \eta_1(x_1) \prod\_{k=2}^n K_k(x_k \mid x\_{k-1}) dx\_{1:k-1}
$$
We can now sample from $\eta_n(x_n)$ but we cannot evaluate $\eta_n(\cdot)$ due to the integral with respect to $x\_{1:k-1}$. Evaluating $\eta_n$ is needed to compute the IS weights 
$$
w_n(X_n^{(i)}) = \frac{\gamma_n(X_n^{(i)})}{\eta_n(x_n)}
$$
- SMC Samplers overcomes the problem of integrating over $x\_{1:k-1}$ by working with the integrand directly. The proposal and the target distributions are then $\eta_n(x\_{1:n})$ and $\widetilde{\pi}_n(x\_{1:n})$. Notice the difference with respect to IS and SIS: In IS and SIS we get new particles at each time step, that is at time step $n-1$ we have $X\_{n-1}^{(1:N)}$ and at time step $n$ we have $X_n^{(1:N)}$. In an SMC sampler, instead, we _extend_ the particles at time $n-1$ $X\_{1:n-1}^{(1:N)}$ by sampling from a kernel $X_n^{(i)} \sim K_n(\cdot \mid X\_{n-1}^{(i)})$ and then appending this to the current particles to obtain $X\_{1:n}^{(1:N)}$. Since we have appended $X_n^{(1:N)}$ to the previous particles, we need to update the weights and these are updated using an incremental weight 
$$
\widetilde{w}_n(x_n \mid x\_{n-1}) = \frac{\gamma_n(x_n) L\_{n-1}(x\_{n-1} \mid x_n)}{\gamma\_{n-1}(x\_{n-1}) K_n(x_n \mid x\_{n-1})}
$$
Importantly, this requires us to introduce backwards kernels which essentially allow us to approach the problem from an auxiliary variable perspective. 

Since the variance of the weights increases as $\eta_n$ and $\widetilde{\pi}_n$ become further apart, one often resamples the particles according to 
$$
\widetilde{\pi}_n^N(dx\_{1:n}) = \sum\_{i=1}^N W_n^{(i)}\delta\_{X\_{1:n}^{(i)}}(d x\_{1:n})
$$


