---
# Course title, summary, and position.
linktitle: Approximate Bayesian Computation
summary: Learn the basics of Approximate Bayesian Computation and survey the recent methods in the literature.
weight: 1

# Page metadata.
title: Tutorial Overview
date: "2021-04-01T00:00:00Z"
lastmod: "2021-04-01T00:00:00Z"
draft: false  # Is this a draft? true/false
toc: true  # Show table of contents? true/false
type: docs  # Do not modify.

# Add menu entry to sidebar.
# - name: Declare this menu item as a parent with ID `name`.
# - weight: Position of link in menu.
menu:
  abc:
    name: Course Overview
    weight: 1
---

In this Approximate Bayesian Computation course you are going to learn about the key algorithms used in ABC, some of the ideas surrounding simulator-based models, how to code them, and you'll learn about the most recent advancements in the field.

- Chapter 1 - **Foundations of ABC**
    - [What is ABC](/ramblings/approximate-bayesian-computation/01-what-is-abc): Introduces the problem of estimating the parameter posterior with an intractable likelihood.
    - [ABC Distributions](/ramblings/approximate-bayesian-computation/02-abc-distributions): Sets the notation and nomenclature for the rest of the course. I explore the ABC likelihood, the ABC posterior, the augmented ABC posterior and the kernel. 
    - [ABC Kernel and Summary Statistics](/ramblings/approximate-bayesian-computation/03-kernel-properties-summary-statistics): Building on the previous section, I motivate the usage of the Kernel and why it is a good idea. I also briefly mention summary statistics. 
    - [Soft ABC](/ramblings/approximate-bayesian-computation/04-soft-abc): This is the first ABC algorithm you will encounter in this course. It resembles imporance sampling and sets the foundation for rejection ABC.
    - [Rejection ABC](/ramblings/approximate-bayesian-computation/05-rejection-abc): I frame Rejection ABC as a special case of Soft ABC with a uniform kernel. 
    
- Chapter 2 - **Sampling Schemes for ABC**
    - [MCMC-ABC](/ramblings/approximate-bayesian-computation/06-mcmc-abc): You will explore MCMC-ABC as a standard Metropolis-Hastings targeting the augmented ABC posterior and derive the pseudocode for it.
