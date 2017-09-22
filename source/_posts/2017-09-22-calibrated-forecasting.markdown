---
layout: post
title: "Calibrated Forecasting"
date: 2017-09-22 13:37:36 +0200
comments: true
categories: 
---

### Probability Forecasting

Can one predict the weather *ignorantly yet accurately*? 
To be concrete, suppose we are given the weather record of the past 100 days, 

$$
\text{wet}, \text{wet}, \text{dry}, \text{wet}, \text{dry}, \ldots; 
$$

we cannot access any other information (e.g., temperature, satellite images, etc.), and we know nothing about meteorology. 
Can we tell how likely it will rain tomorrow? 

This problem can be cast as a *probability forecasting* problem.
The protocol of probability forecasting is as follows. 
For $$t = 1, 2, \ldots$$, 

1. Predictor announces $$p_t \in [ 0, 1 ]$$. 
2. Reality announces $$y_t \in \{ 0, 1 \}$$.

The goal of Predictor is to make accurate predictions, such that $p_t$ is a good estimate of the likelihood of the event $$\{ y_t = 1 \}$$. 
As the protocol is deterministic, we do not say "probability of the event $$\{ y_t = 1 \}$$".

Thinking of '$0$' as 'dry', and '$1$' as 'wet', we recover the original weather forecasting problem.

### Calibration

What precisely are the requirements a good prediction strategy should satisfy? 
The requirements in general depend on the actual application scenario. 
In this post, we will only consider a minimal requirement---any good prediction strategy should be *calibrated*.

Roughly speaking, the notion of calibration requires the predictions to comply with the empirical frequency. 
Consider the following discretization argument. 
Fix some $\varepsilon > 0$. 
Let us decompose the interval $[ 0, 1 ]$ as 

$$
[ 0, 1 ] = \left[ 0, \varepsilon \right) \cup \left[ \varepsilon, 2 \varepsilon \right) \cup \cdots \cup \left[ (M - 2) \varepsilon, (M - 1) \varepsilon \right) \cup \left[ ( M - 1 ) \varepsilon, 1 \right] := I_1 \cup I_2 \cup \cdots \cup I_{M - 1} \cup I_M . 
$$

where $M := \lceil \varepsilon^{-1} \rceil$. 
We denote the center of $I_m$ by $c_m$.
Define

$$
\rho_m ( T ) := \frac{ \sum_{t = 1}^T \mathbb{I}_{ \{ p_t \in I_m, y_t = 1 \} } }{ \sum_{t = 1}^T \mathbb{I}_{ \{ p_t \in I_m \} } } , 
$$

if the divisor is non-zero, and $\rho_m := c_m$ otherwise. 
We would like, for all $m$, 

$$
\limsup_{T \to \infty} \left\vert \rho_m (T) - c_m \right\vert \leq \varepsilon, \quad \text{a.s.}
$$

We do not really want to care about those $m$'s such that $\sum_{t = 1}^T \mathbb{I}_{ \{ p_t \in I_m \} } = o( T )$, as they are irrelevant in the long run. 
A compact formulation of the requirement is

$$
\limsup_{T \to \infty} \sum_{m = 1}^M \left\vert \rho_m (T) - c_m \right\vert \frac{ \sum_{t = 1}^T \mathbb{I}_{ \{ p_t \in I_m \} } }{ T } \leq \varepsilon , \quad \text{a.s.}
$$

Some simple algebraic manipulations lead to the following equivalent defnition.

**Definition.** A (possibly randomized) prediction strategy is $$\varepsilon$$-calibrated, if the resulting sequence of predictions satisfy

$$
\limsup_{T \to \infty} \frac{1}{T} \sum_{m = 1}^M \sum_{t = 1}^T \mathbb{I}_{ \{ p_t \in I_m \} } \left\vert p_t - y_t \right\vert \leq \varepsilon , \quad \text{a.s.} 
$$

Surprisingly, even when one knows nothing about the mechanism generating the sequence $$( y_t )_{t \in \mathbb{N}}$$, calibration is achievable.

**Theorem.** Suppose that Reality, before announcing $y_t$, does not know the exact value of $p_t$. 
For any $\varepsilon > 0$, there exists an $\varepsilon$-calibrated probability forecasting strategy. 

Some quick observations: 

1. If $y_t$ can depend on the exact value of $p_t$, calibration is impossible.
For example, Reality can always choose $y_t = 1$ whenever $p_t \leq 0.5$, and $y_t = 0$ otherwise. 

2. The remark above also shows the neceesity of a randomized forecasting strategy.
If Forecaster uses a deterministic strategy, for which $p_t$ is a deterministic function of $p_1, y_1, p_2, y_2, \ldots, p_{t - 1}, y_{t - 1}$, then Reality can also compute the exact value of $p_t$. 

### An Achieving Strategy

A calibrated forecasting strategy can be derived by the idea of Blackwell approachability. 

The strategy only chooses $p_t$ from the set $$\{ c_m \}$$; effectively, Forecaster only needs to choose $$m_t \in \{ 1, 2, \ldots, M \}$$ for all $t$. 
Define, for all $m$, 

$$
f_t ( m, y ) := ( 0, \ldots, 0, c_m - y, 0, \ldots, 0 ) \in \mathbb{R}^M . 
$$

We already know that Forecaster must adopt a randomized strategy; in general, Reality's strategy can be also randomized. 
Suppose that Forecaster chooses $m_t$ randomly according to a probability distribution $P_t$ on $$\{ 1, 2, \ldots, M \}$$, and Reality chooses $y_t$ randomly according to a probabilty distribution $Q_t$ on $$\{ 0, 1 \}$$. 
Define

$$
f_t ( P_t, Q_t ) := \sum_{m = 1}^M P_t (m) Q_t (y) \, f_t ( m, y ), \quad \bar{f}_t := \frac{1}{t} \sum_{\tau = 1}^t f_t ( P_{\tau}, Q_{\tau} ) . 
$$

**Forecaster's Strategy.** For each $t$, Forecaster first computes some $P_t$ such that for any probability distribution $Q$ on $$\{ 0, 1 \}$$, 

$$
\left\langle \bar{f}_{t - 1} - \mathrm{proj} ( \bar{f}_{t - 1} ), f ( P_t, Q ) - \mathrm{proj} ( \bar{f}_{t - 1} ) \right\rangle \leq 0 , 
$$

where $\mathrm{proj}$ denotes projection onto the $1$-norm ball of radius $\varepsilon$ in $\mathbb{R}^M$, and then announces $m_t$ randomly according to $P_t$. 

**Lemma.** Such $P_t$ always exists.

**Remark.** In practice, one can compute $P_t$ by solving the saddle point problem: 

$$
P_t \in \mathrm{argmin}_P \mathrm{max}_Q \left\langle \bar{f}_{t - 1} - \mathrm{proj} ( \bar{f}_{t - 1} ), f ( P, Q ) - \mathrm{proj} ( \bar{f}_{t - 1} ) \right\rangle . 
$$

Notice that the objective function is bilinear; there are a variety of existing algorithms that solve this problem.

The strategy seems mysterious at first glance. 
I will introduce the theory of Blackwell approachability in the next post. 

### References 

1. A. P. Dawid. 1982. The well calibrated Bayesian. *J. Am. Stat. Assoc.*
2. D. P. Foster. 1999. A proof of calibration via Blackwell's approachability theorem. *Games Econ. Behav.*
3. N. Cesa-Bianchi and G. Lugosi. 2006. *Prediction, Learning, and Games*.
