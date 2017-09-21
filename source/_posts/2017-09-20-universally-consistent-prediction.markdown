---
layout: post
title: "Universally Consistent Prediction"
date: 2017-09-20 18:48:19 +0200
comments: true
categories: 
---

### Introduction

Can one predict the future well, *without any subjective assumptions*? 
Indeed, there are actually some prediction strategies that work without any subjective assumption, and approach (arguably) the best possible performance in some sense.
This post introduces one such strategy described in [an article by V. Vovk](https://arxiv.org/abs/cs/0606093).  

Consider the following protocol of *online prediction*. 
For $t = 1, 2, \ldots$, 

1. Reality announces $x_t \in \mathcal{X}$. 
2. Predictor announces $\gamma_t \in \Gamma$. 
3. Reality announces $y_t \in \mathcal{Y}$. 

We assume the perfect information case; that is, all $x_t$'s, $\gamma_t$'s, and $y_t$'s, once announced, are known to both Reality and Predictor. 

**Exercise.** Find some real-life examples of this protocol. 
Notice that $\mathcal{X}$ can be the space of histories.

We measure the quality of prediction by a given loss function $\lambda: \mathcal{X} \times \Gamma \times \mathcal{Y} \to \mathbb{R}$, known to both Reality and Predictor. 
We will consider compact-type losses.

**Definition.** A loss function is called compact-type, if for any compact sets $A \subseteq \mathcal{X}$ and $B \subseteq \mathcal{Y}$ and any $M \in \mathbb{R}$, there exists some compact set $C \subset \Gamma$, such that $\lambda ( x, \gamma, y ) > M$ for any $x \in A$, $y \in B$, and $\gamma \notin C$. 

A (possibly randomized) prediction strategy is a function $\psi: \mathcal{X} \to \Delta ( \Gamma )$, where $\Delta ( \Gamma )$ denotes the set of all probabiity measures on $\Gamma$. 
For each $t$, Predictor computes $Q_t := \psi( x_t )$, and chooses $\gamma_t \in \Gamma$ randomly according to $Q_t$. 

**Definition.** We say that a prediction strategy is continuous, if the associated function $\psi$ is continuous.

**Theorem 1 ([Vovk](https://arxiv.org/abs/cs/0606093)).** Suppose that $\mathcal{X}$ and $\mathcal{Y}$ are locally compact metric spaces, and $\Gamma$ is a metric space. 
Suppose that $\lambda$ is continuous and compact-type. 
Then for any $\varepsilon > 0$, there exists a prediction strategy, such that if $$\{ x_t \}$$ and $$\{ y_t \}$$ are precompact, 

$$
\limsup_{T \to \infty} \frac{1}{T} \sum_{t = 1}^T \left( \lambda ( x_t, \gamma_t, y_t ) - \lambda ( x_t, \tilde{\gamma_t}, y_t ) \right) \leq \varepsilon, \quad \text{a.s.} , 
$$

where $( \tilde{\gamma_t} )_{t \in \mathbb{N}}$ is the sequence of predictions made by any continuous prediction strategy.

Any prediction strategy that achieves the theorem above is called universally consistent. 

### Ideas

The prediction strategy proposed by Vovk consists of two parts. 
Predictor first computes an estimate of the conditional probability distribution of $y_t$ given the history; then Predictor chooses the optimal $\gamma_t$ minimizing the estimated conditional expected loss.
Notice that, however, the online prediction protocol is completely deterministic; hence talking about the conditional probability and expected loss, rigorously speaking, is not legal in the sense of measure-theoretic probability. 

The first part is called *probability forecasting*. 
The protocol of probability forecasting is as follows. 
For $t = 1, 2, \ldots$, 

1. Reality announces $x_t \in \mathcal{X}$. 
2. Forecaster announces $P_t \in \Delta ( \mathcal{Y} )$. 
3. Reality announces $y_t \in \mathcal{Y}$.

**Theorem 2 ([Vovk](https://arxiv.org/abs/cs/0606093)).** Suppose that $\mathcal{X}$ and $\mathcal{Y}$ are compact metric spaces.
There exists some forecasting strategy, such that 

$$
\lim_{T \to \infty} \frac{1}{T} \sum_{t = 1}^T \left( f ( x_t, P_t, y_t ) - \int_{\mathcal{Y}} f ( x_t, P_t, y ) \, P_t ( \mathrm{d} y ) \right) = 0 , 
$$

for any continuous function $f: \mathcal{X} \times \Delta ( \mathcal{Y} ) \times \mathcal{Y} \to \mathbb{R}$.

The forecasting strategy behind Theorem 2 is based on the existence of a universal RKHS $\mathcal{H}$ on $\mathcal{X} \times \Delta ( \mathcal{Y} ) \times \mathcal{Y}$, i.e., an RKHS dense in $C ( \mathcal{X} \times \Delta ( \mathcal{Y} ) \times \mathcal{Y} )$.
Following the framework of defensive forecasting, one can construct a forecasting strategy such that the inequality in Theorem 2 holds for all functions in $\mathcal{H}$. 
As $\mathcal{H}$ is dense in $C ( \mathcal{X} \times \Delta ( \mathcal{Y} ) \times \mathcal{Y} )$, one obtains Theorem 2.

Naively speaking, the second part corresponds to finding the randomized action that minimizes the conditional expected loss, i.e., computing 

$$
\tilde{Q}_t \in \mathrm{argmin}_{Q \in \Delta( \Gamma )} \int_{\mathcal{Y}} \lambda ( x_t, Q, y ) P_t ( \mathrm{d} y ) , 
$$

where $P_t$ is the output of probability forecasting, and 

$$
\lambda ( x, Q, y ) := \int_{\Gamma} \lambda ( x, \gamma, y ) Q ( \mathrm{d} \gamma ) . 
$$

Then Predictor chooses $\gamma_t$ randomly according to $\tilde{Q}_t$. 

Theorem 2 considers continuous functions, while $\tilde{Q}_t$ may not be continuously dependent on $(x_t, P_t)$. 
Therefore, we need to slightly modify the second part.
What Predictor actually does in the second part is to compute $Q_t := G ( x_t, P_t ) \in \Delta ( \Gamma )$, for some continuous function $G$ satisfying

$$
\int_{\mathcal{Y}} \lambda ( x_t, G( x_t, P_t ), y ) P_t ( \mathrm{d} y ) \leq \mathrm{argmin}_{Q \in \Delta( \Gamma )} \int_{\mathcal{Y}} \lambda ( x_t, Q, y ) P_t ( \mathrm{d} y ) + \delta
$$

for some $\delta > 0$. 

**Lemma 1 ([Vovk](https://arxiv.org/abs/cs/0606093)).** Suppose that $\Gamma$ is a compact convex subset of a topological vector space. Such a continuous function $G$ exists for any $\delta > 0$. 

In Theorem 1, the conditions on $\mathcal{X}$, $\mathcal{Y}$, and $\Gamma$ are not as strict as those in Theorem 2 and Lemma 1.
This gap can be addressed by a doubling-like trick. 
Roughly speaking, Predictor starts with compact subsets $A \subseteq \mathcal{X}$ and $B \subseteq \mathcal{Y}$, and a compact convex subset $C \subseteq \Gamma$. 
Predictor implements the prediction strategy described above only on $A$, $B$, and $C$.
If Reality announces some $( x_t, y_t ) \notin A \times B$, Predictor enlarges $A$, $B$ and $C$ such that $( x_t, y_t )$ is contained. 

1. If Predictor is forced to enlarge the sets for inifinitely many times, $$\{ x_t \}$$ and $$\{ y_t \}$$ cannot be both precompact. 
2. Otherwise, ultimately Predictor is working with some $A \subseteq \mathcal{X}$, $B \subseteq \mathcal{Y}$, and $C \subseteq \Gamma$, to which Theorem 2 and Lemma 1 apply.
