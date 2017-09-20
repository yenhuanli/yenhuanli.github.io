---
layout: post
title: "Universally Consistent Prediction"
date: 2017-09-20 18:48:19 +0200
comments: true
categories: 
---

### Introduction

Can one predict the future well, *without any subjective assumptions*? 
Indeed, there are actually some prediction strategies that work without any subjective assumption, and can achive (arguably) the best possible prediction performance in some sense.
One such strategy can be found in [an article by V. Vovk](https://arxiv.org/abs/cs/0606093).  

Consider the following protocol of *online prediction*. 
For $t = 1, 2, \ldots$, 

1. Reality announces $x_t \in \mathcal{X}$. 
2. Then Predictor announces $\gamma_t \in \Gamma$. 
3. Then Reality announces $y_t \in \mathcal{Y}$. 

We assume the perfect information case; that is, all $x_t$'s, $\gamma_t$'s, and $y_t$'s, once announced, are known to both Reality and Predictor. 

**Exercise.** Find some real-life examples of this protocol. 
Notice that $\mathcal{X}$ can be the space of all possible histories.

We measure the quality of prediction by a given loss function $\lambda: \mathcal{X} \times \Gamma \times \mathcal{Y} \to \mathbb{R}$, known to both Reality and Predictor. 

**Definition.** A loss function is called compact-type, if for any compact sets $A \subseteq \mathcal{X}$ and $B \subseteq \mathcal{Y}$ and any $M \in \mathbb{R}$, there exists some compact set $C \subset \Gamma$, such that $\lambda ( x, \gamma, y ) > M$ for any $x \in A$, $y \in B$, and $\gamma \notin C$. 

A (possibly randomized) prediction strategy is a function $\psi: \mathcal{X} \to \Delta ( \Gamma )$, where $\Delta ( \Gamma )$ denotes the set of all probabiity measures on $\Gamma$. 
For each $t$, Predictor computes $Q_t := \psi( x_t )$, and chooses $\gamma_t \in \Gamma$ randomly according to $Q_t$. 

**Definition.** We say that a prediction strategy is continuous, if the associated function $\psi$ is continuous.

**Theorem ([Vovk](https://arxiv.org/abs/cs/0606093)).** Suppose that $\mathcal{X}$ and $\mathcal{Y}$ are locally compact metric spaces, and $\Gamma$ is a metric space. 
Suppose that $\lambda$ is continuous and compact-type. 
Then for any $\varepsilon > 0$, there exists a prediction strategy such that whenever $$\{ x_t \}$$ and $$\{ y_t \}$$ are precompact, 

$$
\limsup_{T \to \infty} \frac{1}{T} \sum_{t = 1}^T \left( \lambda ( x_t, \gamma_t, y_t ) - \lambda ( x_t, \tilde{\gamma_t}, y_t ) \right) \leq \varepsilon, \quad \text{a.s.} , 
$$

where $( \tilde{\gamma_t} )_{t \in \mathbb{N}}$ is the sequence of predictions made by any continuous prediction strategy.

Any prediction strategy that achieves the theorem above is called universally consistent. 

### Prediction Strategy
