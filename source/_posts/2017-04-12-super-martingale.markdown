---
layout: post
title: "Defensive Forecasting"
date: 2017-04-12 18:37:39 +0200
comments: true
categories: 
---

*Defensive forecasting* is an approach to designing competitive on-line learning algorithms. 
The ideas was partially motivated by the game-theoretic framework of probability theory.
Although it seems that finally, defensive forecasting can be formulated without stating game-theoretic probability as in [this paper](https://doi.org/10.1016/j.tcs.2010.04.003), I find it insightful to understand the original formulation. 

### Game-Theoretic Probability

To get some idea about what the game-theoretic framework is, let us introduce an interesting result in this framework. 
Consider the following probabiilty prediction game. 
Let $K_0 = 1$.
For every $n \in \mathbb{N}$, 

1. *Forecaster* announces $p_n \in [ 0, 1 ]$.
2. *Skeptic* announces $s_n \in \mathbb{R}$.
3. *Reality* announces $y_n \in \lbrace 0, 1 \rbrace$.
4. $K_n = K_{n - 1} + s_n ( y_n - p_n )$.

This is in fact a game of betting from the perspective of Skeptic: 
Initially Skeptic has $1$ dollar. 
If $s_n > 0$, Skeptic is betting on the event $y_n = 1$; Skeptic gets $s_n y_n$ dollars in return if the event happens, and loses $s_n p_n$ dollars otherwise. 
Similarly, if $s_n < 0$, Skeptic is betting against the event $y_n = 1$.

The sequence $(K_n)$ is called a *martingale*.
If $K_0 = 1$ and $K_n \geq 0$ for all $n$, the sequence $( K_n )$ is called a *scoring martingale*.

Of course, the values of $K_n$ depend on the strategy of Skeptic.
Roughly speaking, in the game-theoretic framework, an event of small probabiilty corresponds to a large value of $K_n$.

**Theorem.**
(Weak law of large numbers) 
For any $N \in \mathbb{N}$, there exists a scoring martingale $( K_n )$, such that for any $\delta > 0$, $K_N \geq 1 / \delta$ unless

$$
\left\vert \frac{\sum_{n = 1}^N ( y_n - p_n )}{N} \right\vert \leq \frac{1}{\sqrt{ \delta N }}.
$$

*Proof.* Choose 

$$
s_n = \frac{2 \sum_{n = 1}^{n - 1} ( y_n - p_n )}{N}. 
$$

*Q.E.D.*

### Defensive Forecasting

Defensive forecasting aims at finding a strategy for Forecaster, such that Skeptic's capital does not grow, no matter what Reality's strategy is. 

If Skeptic and Reality cooperate, this aim is in general impossible.
For example, Skeptic and Reality can choose $s_n$ and $y_n$ in the following way: 

* If $p_n < 1 / 2$, they choose $s_n = 2$ and $y_n = 1$. 
* If $p_n \geq 1 / 2$, they choose $s_n = -2$ and $y_n = 0$. 

Then $K_n \geq K_{n - 1} + 1$ for every $n$.

However, if we slightly weaken Skeptic, this aim becomes surprisingly easy.
Notice that Skeptic keeps his wealth growing, by setting $s_n$ as a discontinuous function of $p_n$.
Let us impose a restriction that $s_n$ must be a continuous function of $p_n$.
Then we arrive at the following defensive porecasting protocol.

Let $K_0 = 1$.
For every $n \in \mathbb{N}$, 

2. *Skeptic* announces a continuous function $s_n: [ 0, 1 ] \to \mathbb{R}$.
3. *Forecaster* announces $p_n \in [ 0, 1 ]$.
4. Reality announces $y_n \in \lbrace 0, 1 \rbrace$.
5. $K_n = K_{n - 1} + s_n ( p_n ) ( y_n - p_n )$.

**Theorem.** 
There always exists a strategy for Forecaster, such that $K_0 \geq K_1 \geq K_2 \geq \cdots$. 

***Proof.***
If $s_n$ is strictly positive on $[ 0, 1 ]$, choose $p_n = 1$. 
If $s_n$ is strictly negative on $[ 0, 1 ]$, choose $p_n = 0$.
Otherwise, by continuity of $s_n$, there must exist some $p^\star$ such that $s_n ( p^\star ) = 0$; choose $p_n = p^\star$.
*Q.E.D.*

### K29

K29 is an algorithm for probability prediction, based on the defensive forecasting approach.
Consider the following protocol. 
For $n = 1, 2, \ldots, N$, 

1. Forecaster announces $p_n \in [ 0, 1 ]$. 
2. Reality announces $y_n \in \lbrace 0, 1 \rbrace$.

One can associate $y_n = 1$ to a rainy day; then $p_n$ dentoes Forecaster's estimate of the probability that a rainy day will happend. 

Let $\kappa$ be a kernel on $[ 0, 1 ]$.
The aim of K29 is to guarantee, for all $p \in [ 0, 1 ]$, 

$$
\left\vert \sum_{n = 1}^N \kappa ( p_n, p ) ( y_n - p_n ) \right\vert \leq \sqrt{N} .
$$

This inequality, roughly speaking, guarantees that the forecasting strategy is *calibrated*, i.e., for any $p \in [ 0, 1 ]$, among those days with $p_n \approx p$, there are about $( 100 \times p ) \%$ of them that are actually rainy. 
A precise intrepretation of the inequality is in fact much more complicated (for example, it is known that deterministic calibration is impossible), and hence skipped due to my lazziness ;)
Please find the references for details. 

**Theorem.**
Assume $\kappa ( p, p ) \leq 1$ for all $p \in [ 0, 1 ]$.
The inequality above can be achieved, if Forecaster applies the defensive forecasting strategy with respect to

$$
s_n: p \mapsto \sum_{i = 1}^n \kappa ( p, p_i ) ( y_i - p_i ) . 
$$

### References

1. G. Shafer and S. Ring, "Predicting bond yields using defensive forecasting," 2010.
2. V. Vovk, "Predictions as statements and decisions," 2006.
3. V. Vovk, A. Takemura, and G. Shafer, "Defensive forecasting," 2005.
