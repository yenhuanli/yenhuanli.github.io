---
layout: post
title: "Weak Aggregation"
date: 2017-03-17 16:53:40 +0100
comments: true
categories: 
---

The *weak aggregating algorithm (WAA)* was introduced in the paper ["The weak aggregating algorithm and weak mixability"](http://dx.doi.org/10.1016/j.jcss.2007.08.003) by Kalnishkan and V'yugin. 
Unlike the aggregating algorithm (AA) by Vovk, the WAA does not require the mixability condition but a weaker convexity condition which will appear shortly. 

###Weak Aggregating Algorithm###

Recall the *prediction with expert advice (PEA)* game, which involves *Learner*, *Nature*, and a pool of experts indexed by a set $\Theta$. 
Let $\Omega$ be the *outcome space*, $\Gamma$ be the *prediction space*, and $\lambda: \Omega \times \Gamma \to [ 0, + \infty )$ be a *loss function*.
For each $t \in \mathbb{N}$, 

1. Each expert $\theta \in \Theta$ chooses a prediction $\gamma_t ( \theta ) \in \Gamma$.
2. Learner chooses a prediction $\gamma_t \in \Gamma$, which can be dependent on all previous predictions by himself and the experts, and previous outcomes chosen by Nature.
3. Nature chooses $\omega_t \in \Omega$.
4. Each expert $\theta \in \Theta$ suffers for the loss $\lambda ( \omega_t, \gamma_t ( \theta ) )$. Learner suffers for the loss $\lambda ( \omega_t, \gamma_t )$.

The WAA works as follows. 
Let $( \eta_t )_{t \in \mathbb{N}}$ be a given sequence of *learning rates*.
Let $P_0$ be a probability measure on $\Theta$.
For each $t \in \mathbb{N}$, Learner chooses as his prediction any $\gamma_t$ such that

$$
\lambda( \omega_t, \gamma_t ) \leq \int_\Theta \lambda ( \omega_t, \gamma_t (\theta) ) \, P_{t-1} ( \mathrm{d} \theta ) .
$$

After observing the losses, Learner constructs a probability measure $P_t$ such that for any $\mathcal{A} \subseteq \Theta$

$$
P_t ( \mathcal{A} ) = \int_{\mathcal{A}} \exp \left( - \eta_{t + 1} L_t ( \theta ) \right) \, P_0 ( \mathrm{d} \theta ) ,
$$

where $L_t ( \theta )$ denotes the *accumulative loss* of expert $\theta$, i.e., 

$$
L_t ( \theta ) := \lambda ( \omega_1, \gamma_1 ( \theta ) ) + \lambda ( \omega_2, \gamma_2 ( \theta ) ) + \cdots + \lambda ( \omega_t, \gamma_t ( \theta ) ) .
$$

Of course, the rule of choosing $\gamma_t$ does not apply to any PEA game.
Hence we need to require the *convexity* of the game.

**Definition.**
The game is said to be convex, if for any probability measure $P$ on $\Gamma$, there always exists some $\gamma^\star \in \Gamma$ such that

$$
\lambda ( \omega, \gamma^\star ) \leq \int_\Gamma \lambda ( \omega, \gamma ) \, P ( \mathrm{d} \gamma ) , \quad \text{for any } \omega \in \Omega . 
$$

By Jensen's inequality, if $\Gamma$ is convex and the mapping $\gamma \mapsto \lambda ( \omega, \gamma )$ is also convex for all $\omega \in \Omega$, then one can simply choose

$$
\gamma^\star = \int_\Gamma \gamma \, P ( \mathrm{d} \gamma ) . 
$$

If a game is $\eta$-mixable for some $\eta > 0$, then it is convex.

###Regret Analysis###

For any $T \in \mathbb{N}$, define the accumulative loss of Learner as

$$
L_T := \lambda ( \omega_1, \gamma_1 ) + \lambda ( \omega_2, \gamma_2 ) + \cdots + \lambda ( \omega_T, \gamma_T ) . 
$$

**Lemma.** Assume that the sequence $( \eta_t )_{t \in \mathbb{N}}$ is non-increasing. 
For every $T \in \mathbb{N}$, one has

$$
\exp \left( -\eta_T L_T \right) \geq \exp \left( - \eta_T \sum_{t \leq T} \delta_t \right) \int_{\Theta} \exp \left( - \eta_T L_T ( \theta ) \right) \, P_0 ( \mathrm{d} \theta ) , 
$$

where 

$$
\delta_t := \int_\Theta \lambda ( \omega_t, \gamma_t (\theta) ) \, P_{t - 1} ( \mathrm{d} \theta ) + \eta_t^{-1} \log \int_\Theta \exp \left( - \eta_t \lambda ( \omega_t, \gamma_t (\theta) ) \right) \, P_{t - 1} ( \mathrm{d} \theta ) . 
$$

**Proof.**
We prove by induction.
Assume the lemma holds for $t = T \in \mathbb{N}$.
For $t = T + 1$, one writes

$$
\exp \left( - \eta_{T + 1} L_{T + 1} \right) = \exp \left( - \eta_{T + 1} L_T \right) \exp \left( - \eta_{T + 1} \lambda ( \omega_{T + 1}, \gamma_{T + 1} ) \right) .
$$

Using the induction hypothesis, one can write

$$
\exp \left( - \eta_{T + 1} L_T \right) = \left[ \exp \left( - \eta_T L_T \right) \right]^{\frac{\eta_{T + 1}}{\eta_T}} \geq \left[ \exp \left( - \eta_T \sum_{t \leq T} \delta_T \right) \int_\Theta \exp \left( - \eta_T L_T ( \theta ) \right) \, P_0 ( \mathrm{d} \theta ) \right]^{\frac{\eta_{T + 1}}{\eta_T}} . 
$$

By Jensen's inequality, the RHS is bounded below by

$$
\exp \left( - \eta_{T + 1} \sum_{t \leq T} \delta_t \right) \int_\Theta \exp \left( - \eta_{T + 1} L_T (\theta) \right) \, P_0 (\mathrm{d} \theta) . 
$$

By the rule of choosing $\gamma_{T + 1}$, one has

$$
\exp \left( - \eta_{T + 1} \lambda (\omega_{T + 1}, \gamma_{T + 1}) \right) \geq \exp \left( - \eta_{T + 1} \int_\Theta \lambda (\omega_{T + 1}, \gamma_{T + 1} ( \theta )) \, P_T ( \mathrm{d} \theta ) \right) .
$$

One can write the RHS as

$$
\exp \left( - \eta_{T + 1} \delta_{T + 1} \right) \int_\Theta \exp \left( - \eta_{T + 1} \lambda ( \omega_{T + 1}, \gamma_{T + 1} ( \theta ) ) \right) \, P_T ( \mathrm{d} \theta ) . 
$$

The lemma follows, as

$$
\int_\Theta \exp \left( - \eta_{T + 1} \lambda ( \omega_{T + 1}, \gamma_{T + 1} ( \theta ) ) \right) \, P_T ( \mathrm{d} \theta ) = \frac{ \int_\Theta \exp \left( - \eta_{T + 1} \lambda ( \omega_{T + 1}, \gamma_{T + 1} (\theta) ) \right) \exp \left( - \eta_{T + 1} L_T ( \theta ) \right) \, P_0 ( \mathrm{d} \theta ) }{ \int_\Theta \exp \left( - \eta_{T + 1} L_T (\theta) \right) \, P_0 (\mathrm{d} \theta) } .
$$

*Q.E.D.*

Let us use the lemma to derive a regret bound under the following two assumptions.

1. The cardinality of $\Theta$ is finite.
2. The loss $\lambda ( \omega, \gamma )$ is bounded above by some $M > 0$, for all $\omega \in \Omega$ and $\gamma \in \Gamma$.

The lemma implies, for any $T \in \mathbb{N}$,

$$
L_T \leq L_T ( \theta ) + \eta_T^{-1} \log ( 1 / P_0 ( \theta ) ) + \sum_{t \leq T} \delta_t , \quad \text{for all } \theta \in \Theta.
$$

It remains to bound the term $\sum_{t \leq T} \delta_t$.
For every $t \in \mathbb{N}$, define the random variable $X_t := - \lambda ( \omega_t, \gamma_t ( \theta ) )$.
Using Hoeffding's lemma, one obtians

$$
\delta_t = - \mathbb{E}\, X_t + \eta_t^{-1} \log \mathbb{E}\, \exp ( \eta_t X_t ) \leq \frac{\eta_t M^2}{8} , \quad \text{for all } t \in \mathbb{N}. 
$$ 

Therefore, 

$$
L_T \leq L_T ( \theta ) + \eta_T^{-1} \log ( 1 / P_0 ( \theta ) ) + \frac{\eta_T M^2 T}{8} . 
$$

Minimizing the bound with respect to $\eta_T$, one obtains the following regret bound. 

**Theorem.** Suppose that $\vert \Theta \vert = N < + \infty$, and the loss function is bounded above by $M > 0$. Then the WAA with $P_0 ( \theta ) = 1 / N$ for all $\theta$ and $\eta_t = M^{-1} \sqrt{ t^{-1} 8 \log N }$ achieves, for all $T \in \mathbb{N}$, 

$$
L_T \leq L_T (\theta) + M \sqrt{ \frac{T \log N}{2} } , \quad \text{for all } \theta \in \Theta.
$$

**Remark.** We have used Hoeffding's lemma to simplify a little bit the original proof.
