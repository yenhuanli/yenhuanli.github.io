---
layout: post
title: "Aggregating Algorithm"
date: 2017-03-02 18:57:06 +0100
comments: true
categories: 
---

This post is essentially a summary of some results in the classical paper [``A game of prediction with expert advice''](http://dx.doi.org/10.1006/jcss.1997.1556) by V. Vovk. 

**Definition.** A *game* is a triple $( \Omega, \Gamma, \lambda )$, where $\Omega$ is the *outcome space*, $\Gamma$ is the *prediction space*, and $\lambda: \Omega \times \Gamma \to [0, \infty]$ is a *loss function*.

Consider the standard *learning with expert advice* setting, in which a *learner* tries to predict the state of the *nature* sequentially, with the help of $n$ experts. 
Precisely speaking, at each trial $t \in \mathbb{N}$: 

1. Each expert $i$ makes a prediction $\gamma_t (i)$.
2. The learner makes a prediction $\gamma_t \in \Gamma$.
3. The nature chooses an outcome $\omega_t \in \Omega$.
4. The learner suffers for the loss $\lambda ( \omega_t, \gamma_t )$.

Before making his prediction at the trial $T$, the learner has access to the past history of the nature (up to trial $t - 1$) and all of the experts (up to trial $t$).

For every $t \in \mathbb{N}$, let us define the learner's accumulative loss as

$$
L_t := \lambda( \omega_1, \gamma_1 ) + \cdots + \lambda ( \omega_t, \gamma_t ), 
$$

and each expert's accumulative loss as

$$
L_t ( i ) := \lambda( \omega_1, \gamma_1 ( i ) ) + \cdots + \lambda( \omega_t, \gamma_t ( i ) ), \quad i = 1, \ldots, n .
$$

The learner's goal is to make predictions as well as the *best expert*, in the sense that $L_t \leq L_t(i) + \varepsilon$ for all $i \leq n$, $t \in \mathbb{N}$, and some *small enough* $\varepsilon$.

**Definition.** We say the game is *$\eta$-mixable* if for any probability distribution $P$ on $\Gamma$, there exists a $\gamma^\star \in \Gamma$, such that

$$
\exp ( - \eta \lambda ( \omega, \gamma^\star ) ) \geq \int \exp ( - \eta \lambda( \omega, \gamma ) ) \, P( \mathrm{d} \gamma ), \quad \text{for all } \omega \in \Omega . \notag
$$

**Remark.** By Jensen's inequality, if the mapping $\gamma \mapsto \exp ( - \eta \lambda ( \omega, \gamma ) )$ for all $\omega$ is concave, then the game is $\eta$-mixable.
Any such loss function is called *exp-concave*. 
Obviously, one can simply choose $\gamma^\star = \int \gamma \, P ( \mathrm{d} \gamma )$ if the loss is exp-concave.

**Proposition.** *If a game is $\eta$-mixable, there exists a prediction algorithm for the learner, such that*

$$
L_t \leq L_t ( i ) + \eta^{-1} \ln n , \quad \text{for all } i \leq n . 
$$

The prediction algorithm is called the *aggregating algorithm (AA)*, first introduced in the paper [``Aggregating strategies''](http://vovk.net/aa/index.html) by V. Vovk.
Let $( \pi_0 ( i ) )_{i \leq n}$ be a probability vector whose entries are all non-zero. 
At each trial $t \in \mathbb{N}$, the AA outputs a prediction $\gamma_t \in \Gamma$ satisfying

$$
\exp ( - \eta \lambda ( \omega_t, \gamma_t ) ) \geq \sum_{i \leq n} \pi_{t - 1} ( i ) \exp ( - \eta \lambda( \omega_t, \gamma_t ( i ) ) ) , 
$$

and after observing $\omega_t$, the AA updates the probability vector as

$$
\pi_t ( i ) := \pi_{t - 1} ( i ) \exp ( - \eta \, \lambda ( \omega_t, \gamma_t ( i ) ) ) , \quad \text{for all } i \leq n . 
$$

Because of the $\eta$-mixability assumption, the AA is well-defined.

**Lemma.** For each $t \in \mathbb{N}$, one has 

$$
\exp ( - \eta L_t ) \geq \sum_{i \leq n} \pi_0 ( i ) \, \exp ( - \eta L_t ( i ) ) , \quad \text{for all } i . 
$$

**Proof.** We prove by induction. 
Obviously, the inequality holds for $t = 0$ (for which we set $L_t = L_t (i) = 0$ for all $i$).
Assume the inequality to be prove holds for $t = T$.
First we write

$$
\exp ( - \eta L_{T + 1} ) = \exp ( - \eta L_T ) \exp ( - \eta \lambda ( \omega_{T + 1}, \gamma_{T + 1}(i) ) )
$$

By the definition of the AA, the RHS is bounded below by

$$
\exp ( - \eta L_T ) \frac{ \sum_{i \leq n} \exp ( - \eta \lambda ( \omega_T, \lambda_T ( i ) ) ) \, \pi_0 ( i ) \, \exp ( - \eta L_T ( i ) ) }{ \sum_{i \leq n} \pi_0 ( i ) \, \exp ( - \eta L_T ( i ) ) } . 
$$

By assumption, the RHS is bounded below by

$$
\sum_{i \leq n} \exp ( - \eta \lambda ( \omega_T, \lambda_T ( i ) ) ) \, \pi_0 ( i ) \, \exp ( - \eta L_T ( i ) ) = \sum_{i \leq n} \pi_0 ( i ) \exp ( \eta L_{T+1} ( i ) ) . 
$$

This proves the lemma. *Q.E.D.*

By the lemma, we have

$$
\exp ( - \eta L_t ) \geq \pi_0 ( i ) \, \exp ( - \eta L_t ( i ) ) , \quad \text{for all } i . 
$$

Hence we obtain

$$
L_t \leq L_t ( i ) + \frac{1}{ \eta } \log \left( \frac{1}{ \pi_0 ( i ) } \right) . 
$$

Choosing $\pi_0 ( i ) = 1 / n$ for all $i$ (the uniform distribution) proves the proposition.
