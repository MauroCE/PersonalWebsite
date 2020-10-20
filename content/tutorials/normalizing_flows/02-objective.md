---
title: Objective Function and Update Equations
linktitle: Example 1 - Objective
toc: true
type: docs
date: "2019-05-05T00:00:00+01:00"
draft: false
menu:
  normalizing-flows:
    parent: Building an Intuition
    weight: 2

# Prev/next pager order (if `docs_section_pager` enabled in `params.toml`)
weight: 2
---

## Data Log-Likelihood
Of course, we know what $p(x)$ looks like, it's just the pdf of a normal distribution. However, in practice, this could be a very complicated density and in that case, we can use the [univariate change of variable formula](https://en.wikipedia.org/wiki/Probability_density_function#Scalar_to_scalar) to find its pdf

$$
\log p(x) = \log p(g^{-1}(x)) + \log \left| \frac{\partial}{\partial x} g^{-1}(x)\right|
$$

In our case, the inverse transformation is given by $g^{-1}(x) = x - \mu$ and its derivative with respect to $x$ is $\frac{\partial}{\partial x} g^{-1}(x) = 1$. Since $\log(1) = 0$ we obtain:

$$
\begin{align}
\log p(x) 
&= -\frac{1}{2}\log(2\pi) -\frac{(x-\mu)^2}{2}
\end{align}
$$

Since the samples are i.i.d. we can estimate the log-likelihood very easily. 
$$
\begin{align}
\sum_{i=1}^n \log p(x^{(i)}) 
&= -\frac{n}{2}\log(2\pi) - \frac{1}{2}\sum^n\_{i=1} (x^{(i)} - \mu)^2
\end{align}
$$

Our objective function to minimize is then the negative log-likelihood. Often one uses the negative **average** log-likelihood instead, because this leads to more consistent gradients across different dataset sizes, you can read more about it [here](https://stats.stackexchange.com/questions/267847/motivation-for-average-log-likelihood)

$$
\frac{1}{n}\sum_{i=1}^n \log p(x^{(i)}) = -\frac{1}{2}\log(2\pi) - \frac{1}{2n}\sum^n\_{i=1} (x^{(i)} - \mu)^2
$$

## Log-Likelihood Gradient Estimates
We can now compute the gradient of the negative average log-likelihood with respect to $\mu$ 

$$
\begin{align}
  \frac{\partial}{\partial \mu} \frac{1}{n}\sum_{i=1}^n \log p(x^{(i)}) &= \frac{1}{n}\sum^n\_{i=1}x^{(i)} - \mu
\end{align}
$$

These can be easily computed since we know $n$, and $\mu$ so we compute $x^{(i)} = z^{(i)} + \mu$.

## Mean Update Equation
We can then update our mean using gradient descent with step size $\gamma$:
$$
\begin{align}
\mu_t \quad &\longleftarrow \mu\_{t-1} - \gamma \left\[\frac{1}{n}\sum^n\_{i=1}x^{(i)} - \mu\_{t-1}\right\]
\end{align}
$$

## Coding the Example
We will generate `n=10000` data points following the **true** normal distribution $\mathcal{N}(2, 1)$. Then, we will start from a random guess `mu = -1.0` and update it using the update equation above.

```python
# Import relevant libraries
import math
import numpy as np
import matplotlib.pyplot as plt
import scipy.stats as stats

# Generate data
n = 10000
mu_true = 2.0
x = np.random.normal(loc=mu_true, scale=1.0, size=n)

# Algorithm Settings
mu = -1.0        # Start from -1.0
gamma = 0.1      # Learning rate
n_iter = 100     # Number of iterations

# Loop through and update mu
mus = [mu]
for i in range(n_iter):
    mu = mu + gamma*(np.mean(x) - mu)
    mus.append(mu)

# Trajectory of mu
fig, ax = plt.subplots(figsize=(20, 10))
ax.plot(range(n_iter+1), mus, lw=3)
ax.hlines(y=mu_true, xmin=0, xmax=n_iter, 
          color='darkred', linestyle='dashed', lw=3)
ax.legend([r'$\mu_t$' + " Trajectory", r'$\mu_{true}$'], prop={'size': 29})
plt.show()
```

![Normalizing Flow Basic Example](/nf_trajectory_basic.png)

We can see that our algorithm reaches the correct answer. 


