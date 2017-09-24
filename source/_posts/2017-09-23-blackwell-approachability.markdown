---
layout: post
title: "Blackwell Approachability"
date: 2017-09-23 21:52:59 +0200
comments: true
categories: 
---

### Introduction

Suppose that Alice and Bob is playing a so-called two-player zero-sum game with perfect information. 
Alice chooses her action from a finite *action set* $$\mathcal{A}$$; Bob chooses his action from a finite action set $$\mathcal{B}$$.
There is a *payoff function* $f$, which assigns to each action pair $(a, b) \in \mathcal{A} \times \mathcal{B}$ a payoff $f ( a, b ) \in \mathbb{R}$; Bob's payoff is then simply $-f (a, b)$. 
In general, Alice and Bob may choose their actions randomly, so we consider the expected payoff: for any probability distribution $P \in \Delta ( \mathcal{A} )$ and $Q \in \Delta ( \mathcal{B} )$, define

$$
f ( P, Q ) := \sum_{a \in \mathcal{A}} \sum_{b \in \mathcal{B}} f ( a, b ) P ( a ) Q ( b ) . 
$$

If Alice does not have any additional information about Bob, it is reasonable to consider the very robust minimax strategy, i.e., to choose her action $$a \in \mathcal{A}$$ randomly according to 

$$
P^\star \in \mathrm{arg\, max}_{P} \mathrm{min}_Q f ( P, Q ) . 
$$

No matter what Bob's strategy is, Alice is guaranteed to achieve at least the minimax expected payoff.

If the payoff function $f$ is vector-valued, talking about the minimax expected payoff becomes nonsense, as we cannot compare two vectors. 
This post introduces an analog for the vector-valued payoff case, proposed by David Blackwell in [this classic paper](https://projecteuclid.org/euclid.pjm/1103044235). 

### Blackwell Approachability

In the rest of this post, suppose that the payoff function $f$ takes values in $$\mathbb{R}^d$$. 

Blackwell considered the following protocol of repeated games. 
For $t = 1, 2, \ldots$, 

1. Alice announces $a_t \in \mathcal{A}$.
2. Bob announces $b_t \in \mathcal{B}$, without known $a_t$.

In general, Alice can use a randomized strategy. 
For convenience, let us say that Alice chooses $a_t$ randomly according to some probability distribution $$P_t \in \Delta ( \mathcal{A} )$$. 

Recall that Alice cannot achieve the "minimax expected payoff" in this vector-payoff case, as it is just not well-define. 
However, it might be possible to ensure that her average payoff will ultimately lies in some specific subset of $\mathbb{R}^d$.

Define the average payoff 

$$
\bar{f}_t := \frac{1}{t} \sum_{\tau = 1}^t f ( a_\tau, b_\tau ) . 
$$

**Definition.** Let $\mathcal{X} \subseteq \mathbb{R}^d$. The set $\mathcal{X}$ is said to be approachable by Alice, if for any sequence $$( b_t )_{t \in \mathbb{N}}$$, 

$$
\lim_{T \to \infty} \mathrm{dist} ( \bar{f}_T, \mathcal{X} ) = 0 , \quad \text{a.s.}, 
$$

where $\mathrm{dist} ( \bar{f}_T, \mathcal{X} )$ denotes the $2$-norm distance between $\bar{f}_T$ and the projection of $\bar{f}_T$ onto $\mathcal{X}$.

If we only consider convex sets, there is an exact characterization of approachable sets.

**Theorem 1.** A convex set $$\mathcal{X} \subseteq \mathbb{R}^d$$ is approachable to Alice, if and only if for any $Q \in \Delta ( \mathcal{B} )$, there exists some $P \in \Delta ( \mathcal{A} )$ such that $f ( P, Q ) \in \mathcal{X}$. 

**Exercise.** Suppose that for each $t$, Bob's action $b_t$ can be dependent on Alice's action $a_t$. 
Show that then even when the if and only if condition holds, $\mathcal{X}$ may not be approachable to Alice. 

The only if part of Theorem 1 is easy to check. 
Suppose that there exists some $Q^\star \in \Delta ( \mathcal{B} )$, such that $$f ( P, Q^\star ) \notin \mathcal{X}$$ for any $$P \in \Delta ( \mathcal{A} )$$. 
To guarantee that Alice fails to approach $\mathcal{X}$, Bob can always choose $b_t$ randomly according to $Q^\star$ for all $t$.

We will focus on the if part of Theorem 1 in the rest of this post. 

### Blackwell's Condition and Alice's Strategy

To prove the if part of Theorem 1, we need to show the existence of an approaching strategy. 
Indeed, the proof is constructive; there is an explicitly defined approaching strategy based on the following lemma. 

**Lemma 1.** Suppose that for any $Q \in \Delta ( \mathcal{B} )$, there exists some $P \in \Delta ( \mathcal{A} )$ such that $f ( P, Q ) \in \mathcal{X}$. 
Then for any $v \in \mathbb{R}^d$, there exists some $P \in \Delta ( \mathcal{A} )$, such that

$$
\left\langle v - \mathrm{proj} ( v ), f ( P, Q ) - \mathrm{proj} ( v ) \right\rangle  \leq 0, \quad \text{for all } Q \in \Delta ( \mathcal{B} ) , 
$$ 

where $\mathrm{proj}$ denotes the projection (w.r.t. the $2$-norm) onto $\mathcal{X}$. 

*Proof.* By definition, $\mathrm{proj} (v)$ is the minimizer of the $2$-norm distance to $v$ in $\mathcal{X}$. 
The optimality condition says

$$
\left\langle v - \mathrm{proj} ( v ), w - \mathrm{proj} ( v ) \right\rangle \leq 0, \quad \text{for all } w \in \mathcal{X} . 
$$

What we want to prove can be equivalently written as

$$
\mathrm{min}_{P} \mathrm{max}_{Q} \left\langle v - \mathrm{proj} ( v ), f ( P, Q ) - \mathrm{proj} ( v ) \right\rangle \leq 0 . 
$$

Applying von Neumann's minimax theorem, we can exchange the order of $\min$ and $\max$, and write

$$
\mathrm{max}_{Q} \mathrm{min}_{P} \left\langle v - \mathrm{proj} ( v ), f ( P, Q ) - \mathrm{proj} ( v ) \right\rangle \leq 0 . 
$$

The lemma follows from the optimality condition. $Q.E.D.$

We call the condition in Lemma 1 *Blackwell's condition*, following the convention in literature. 
Blackwell's condition ensures that the following strategy for Alice is well-defined. 

**Alice's Strategy.** Define the expected and average expected payoff: 

$$
f_t := f( P_t, b_t ) := \sum_{a \in \mathcal{A}} f( a, b_t ) P_t( a ), \quad \tilde{f}_t := \frac{1}{t} \sum_{\tau = 1}^t f ( P_t, b_t ) . 
$$

For each $t \in \mathbb{N}$, Alice computes some $P_t$ such that 

$$
\left\langle \tilde{f}_{t - 1} - \mathrm{proj} ( \tilde{f}_{t - 1} ), f ( P_t, Q ) - \mathrm{proj} ( \tilde{f}_{t - 1} ) \right\rangle  \leq 0, \quad \text{for all } Q \in \Delta ( \mathcal{B} ) , 
$$

and then announces $a_t$ randomly according to $P_t$.

**Remark.** In practice, $P_t$ can be computed as 

$$
P_t \in \mathrm{arg\, min}_{P} \max_{Q} \left\langle \tilde{f}_{t - 1} - \mathrm{proj} ( \tilde{f}_{t - 1} ), f ( P, Q ) - \mathrm{proj} ( \tilde{f}_{t - 1} ) \right\rangle . 
$$

The is a bilinear saddle point problem; there exists a variety of algorithms solving such a problem. 

### Proof of Theorem 1

Since $\mathcal{X}$ is approachable if and only if its closure is approachable, we assume that $\mathcal{X}$ is closed w.l.o.g. 
We will also assume that $\mathcal{X}$ is bounded for convenience; we will relax this unnecessary condition at the end of the proof. 

Theorem 1 is in fact a corollary of the following theorem. 

**Theorem 2.** Suppose that the if and only if condition in Theorem 1 holds. 
Suppose that Alice adopts the strategy described above.
Then for each $T \in \mathbb{N}$ and $\delta \in ( 0, 1 )$, with probability at least $$1 - \delta$$, it holds that 

$$
\mathrm{dist} ( \bar{f}_T, \mathcal{X} ) = O \left( \frac{M + R}{\sqrt{T}} + M \sqrt{ \frac{\log(1 / \delta)}{T} } \right) , 
$$

where $$M := \max_{a, b} \left\Vert f ( a, b ) \right\Vert_2$$, and $$R := \max_{w \in \mathcal{X}} \left\Vert w \right\Vert_2$$.

To prove Theorem 2, we start with the following lemma. 

**Lemma 2.** For all $T \in \mathbb{N}$, it holds that 

$$
\mathrm{dist} \left( \tilde{f}_T, \mathcal{X} \right) \leq \frac{M + R}{\sqrt{T}} .  
$$

*Proof.* 
Define $$\pi_t := \mathrm{proj} ( f_t )$$ and $$\tilde{\pi}_t := \mathrm{proj} ( \tilde{f}_t )$$.
By definition, we have 

$$
\left\Vert \tilde{f}_{t + 1} - \tilde{\pi}_{t + 1} \right\Vert_2^2 \leq \left\Vert \tilde{f}_{t + 1} - \tilde{\pi}_{t} \right\Vert_2^2. 
$$

Writing $$\tilde{f}_{t + 1} = [t / (t + 1)] \tilde{f}_t + [1 / (t+1)] f_{t + 1}$$, we have

$$
\mathrm{RHS} = \left( \frac{t}{t + 1} \right)^2 \left\Vert \tilde{f}_t - \tilde{\pi}_t \right\Vert_2^2 + \left( \frac{1}{t + 1} \right)^2 \left\Vert f_{t + 1} - \tilde{\pi}_t \right\Vert_2^2 + \frac{2 t}{(t + 1)^2} \left\langle \tilde{f}_t - \tilde{\pi}_t, f_{t + 1} - \tilde{\pi}_t \right\rangle . 
$$

Applying Lemma 1 and the triangle inequality, we obtain 

$$
\mathrm{RHS} \leq \left( \frac{t}{t + 1} \right)^2 \left\Vert \tilde{f}_t - \tilde{\pi}_t \right\Vert_2^2 + \left( \frac{1}{t + 1} \right)^2 ( M + R )^2 . 
$$

The lemma can be then easily checked by induction. *Q.E.D.*

*Proof of Theorem 2.* Recall that $$\bar{f}_t$$ denotes the average payoff; define $$\bar{\pi}_t := \mathrm{proj}( \bar{f}_t )$$. 
We start with the decomposition, 

$$
\mathrm{dist} ( \bar{f}_T, \mathcal{X} ) = \left\Vert \bar{f}_T - \bar{\pi}_T \right\Vert_2 \leq \left\Vert \bar{f}_T - \tilde{f}_T \right\Vert_2 + \left\Vert \tilde{f}_T - \tilde{\pi}_T \right\Vert_2 + \left\Vert \tilde{\pi}_T - \bar{\pi}_T \right\Vert_2
$$

By Lemma 2 and the non-expansiveness of projections, we have 

$$
\mathrm{RHS} \leq \frac{M + R}{\sqrt{T}} + 2 \left\Vert \bar{f}_T - \bar{\pi}_T \right\Vert_2 . 
$$

Define $$X_t := \sum_{\tau = 1}^t f ( a_\tau, b_\tau ) - f ( P_\tau, b_\tau )$$. 
Then $$( X_t )_{t \in \mathbb{N}}$$ forms a vector-valued martingale. 
Applying the Azuma inequality for Banach-space valued martingales, we obtain, for any $u > 0$, 

$$
\mathsf{P} \left( \left\Vert \bar{f}_T - \tilde{f}_T \right\Vert_2 \geq u \right) = O \left( \exp \left( - \frac{C T u^2 }{M^2} \right) \right) , 
$$

for some constant $C > 0$. *Q.E.D.*

In general, the set to be approached may not be bounded; then $R = + \infty$ if we directly apply the definition. 
Notice that, however, only sets lie in the convex hull of $$\{ f ( a, b ) \}$$ can be approached. 
Denote the convex hull by $\mathcal{F}$. 
Hence, it suffices to replace $\mathcal{X}$ by $$\mathcal{X} \cap \mathcal{F}$$, which is always bounded by construction; then it is easily checked that $R$ can be bounded above by $M$ to slightly simplify the expressions.

### Calibrated Forecasting

One might wonder why one should consider vector-valued payoffs. 
An interesting application is calibrated forecasting. 
With Theorem 1, it is an easy exercise to prove the theorem in the [previous post](http://yenhuanli.github.io/blog/2017/09/22/calibrated-forecasting/), showing the existence of an $\varepsilon$-calibrated forecasting strategy.

### References 

The strategy of Alice presented above is not exactly the original one proposed by Blackwell, as the original one does not lead to the $O ( 1 / \sqrt{T} )$ with-high-probability convergence rate in Theorem 2. 
The strategy presented above was described in the book by Cesa-Bianchi and Lugosi, without proof. 
The proof above is basically a mix of the arguments and results in the following references.


1. Blackwell, D., 1956. An analog of the minimax theorem for vector payoffs. *Pac. J. Math.*
2. Cesa-Bianchi, N. and Lugosi, G., 2006. *Prediction, Learning, and Games*.
2. Naor, A., 2012. On the Banach-space-valued Azuma inequality and small-set isoperimetry of Alon–Roichman graphs. *Combinatorics Probab. Comput.*
4. Sorin, S., 2002. *A First Course on Zero-Sum Repeated Games*.
